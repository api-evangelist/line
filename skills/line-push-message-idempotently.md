---
name: line-push-message-idempotently
description: >-
  Send a LINE push, multicast or broadcast message that can be safely retried
  after a 5xx or a timeout, using the X-Line-Retry-Key idempotency header, and
  check the monthly message quota before spending it.
api: LINE Messaging API
version: '1.0.0'
generated: '2026-08-13'
method: generated
source: >-
  openapi/line-messaging-api-openapi.yml,
  https://developers.line.biz/en/docs/messaging-api/retrying-api-request/,
  conventions/line-conventions.yml, rate-limits/line-rate-limits.yml
operations:
  - getMessageQuota
  - getMessageQuotaConsumption
  - pushMessage
  - multicast
  - broadcast
  - validatePush
---

# Push a LINE message idempotently

## Why this matters

A LINE send that returns `500` or times out **may still have been delivered**.
Retrying blind duplicates the message to the user. `X-Line-Retry-Key` is the
only defence, and it must be present on the **first** request — a request sent
without it can never be retried safely.

Idempotency is supported on exactly four operations: `pushMessage`,
`multicast`, `narrowcast`, `broadcast`. Sending the header anywhere else
returns `400`.

## Step 1 — Check the quota before spending it

```
GET https://api.line.me/v2/bot/message/quota
GET https://api.line.me/v2/bot/message/quota/consumption
```

`getMessageQuota` returns the plan's monthly allowance; `getMessageQuotaConsumption`
returns `totalUsage`. Push, multicast, broadcast and narrowcast are metered
**per recipient**, not per request — one push into a five-person chat is five
messages. Reply messages are free.

Exhausting the allowance returns `429` with
`"message": "You have reached your monthly limit."` — the same status code as a
rate-limit breach, so always read the message string.

## Step 2 — Generate a retry key

A hexadecimal UUID you generate. LINE does not issue it.

```
X-Line-Retry-Key: 123e4567-e89b-12d3-a456-426614174000
```

Persist it with the outbound job. It stays valid for **24 hours** from the
first request, so a retry must happen inside that window.

## Step 3 — Send

```
POST https://api.line.me/v2/bot/message/push
Authorization: Bearer {CHANNEL_ACCESS_TOKEN}
Content-Type: application/json
X-Line-Retry-Key: 123e4567-e89b-12d3-a456-426614174000

{
  "to": "{userId}",
  "messages": [ { "type": "text", "text": "Hello, user" } ]
}
```

`multicast` (`POST /v2/bot/message/multicast`) takes `to[]` of up to 500 user
IDs. `broadcast` (`POST /v2/bot/message/broadcast`) takes no recipient at all
and goes to every friend of the Official Account — treat it as irreversible.

## Step 4 — Decide on the response

| Status | Retry? | Why |
|---|---|---|
| 2xx | No | Accepted. Further retries are refused. |
| Timeout | Yes | May or may not have landed; the retry key makes it safe. |
| 500 | Yes | Same. Use exponential backoff. |
| 409 | No | This retry key was already accepted. |
| Other 4xx | No | The request is wrong; retrying will not change it. |
| 429 | No, back off | Rate limit, concurrency limit, or monthly quota. |

On a `409` the response carries `x-line-accepted-request-id` — the
`x-line-request-id` of the call that actually won — and, for push, repeats the
original `sentMessages[].id` and `sentMessages[].quoteToken`. Record those
rather than treating the `409` as a failure.

## Step 5 — Do not change the request between retries

Same body, same recipient, same retry key. Changing content while reusing the
key produces undefined behaviour.

## Rate limits to respect

- `pushMessage`, `broadcast` default bucket: **2,000 req/s** per channel per
  endpoint — except `broadcast`, which is **60 requests per hour**.
- `multicast`: **200 req/s**.
- A retry consumes rate limit like any other request.
- LINE returns **no** `RateLimit-*` or `Retry-After` headers. Budget locally.

See `rate-limits/line-rate-limits.yml`.

## Validate expensive payloads first

`validatePush` (`POST /v2/bot/message/validate/push`) accepts the same
`messages[]` array and returns `200` or a `400` with `details[].property`,
without sending and without spending quota. Always run it before a broadcast.
