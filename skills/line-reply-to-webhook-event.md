---
name: line-reply-to-webhook-event
description: >-
  Receive a LINE Messaging API webhook, verify its HMAC signature, and reply to
  the originating chat using the single-use reply token. This is the core loop
  every LINE bot is built on, and the only sending method that does not consume
  the Official Account's monthly message quota.
api: LINE Messaging API
version: '1.0.0'
generated: '2026-08-13'
method: generated
source: >-
  openapi/line-messaging-api-openapi.yml,
  openapi/line-webhook-openapi.yml,
  conventions/line-conventions.yml, errors/line-problem-types.yml
operations:
  - callback
  - replyMessage
  - validateReply
  - getProfile
---

# Reply to a LINE webhook event

## Preconditions

- A Messaging API channel with a **channel secret** and a **channel access token**.
- A publicly reachable HTTPS webhook URL registered on the channel
  (`setWebhookEndpoint`, `PUT /v2/bot/channel/webhook/endpoint`).
- Base host: `https://api.line.me`.

## Step 1 — Verify the signature before parsing

LINE signs every webhook delivery. Compute
`Base64(HMAC-SHA256(channelSecret, rawRequestBody))` over the **raw bytes** and
compare it in constant time with the `x-line-signature` request header.

Do this before JSON parsing. Re-serialising the body changes the bytes and the
signature will never match.

Reject with `401` and stop if the comparison fails.

## Step 2 — Acknowledge fast, then work

Return `200` to LINE immediately. The webhook `callback` operation
(`POST /callback` in `openapi/line-webhook-openapi.yml`) declares only a `200`
response — LINE does not wait for your business logic. Do the work
asynchronously.

## Step 3 — Read the event envelope

The body is a `CallbackRequest`: a `destination` (the bot's own user ID) and an
`events[]` array. Each event carries:

- `type` — `message`, `follow`, `unfollow`, `join`, `postback`, `beacon`, and
  ~20 others.
- `mode` — `active` or `standby`. **Do not reply when `mode` is `standby`**;
  another module holds chat control.
- `source` — `{type: user|group|room}` plus `userId`, `groupId` or `roomId`.
- `webhookEventId` — dedupe key. LINE may redeliver.
- `deliveryContext.isRedelivery` — `true` on a redelivery; treat the event as
  possibly already handled.
- `replyToken` — present only on replyable events.

## Step 4 — Reply

```
POST https://api.line.me/v2/bot/message/reply
Authorization: Bearer {CHANNEL_ACCESS_TOKEN}
Content-Type: application/json

{
  "replyToken": "{replyToken from the event}",
  "messages": [ { "type": "text", "text": "Hello" } ]
}
```

Rules that are easy to get wrong:

- The reply token is **single use** and **short lived**. Reusing or reusing-late
  returns `400` with `"message": "Invalid reply token"`.
- Maximum **5 message objects** per request.
- Reply messages are **not counted** against the monthly message quota — push,
  multicast, broadcast and narrowcast are. Prefer reply wherever the
  conversation allows it.
- `X-Line-Retry-Key` is **not** supported on `replyMessage`; sending it returns
  `400`. The reply token itself is the de-duplication mechanism.

## Step 5 — Validate first when the payload is generated

If the message object is assembled dynamically (Flex JSON in particular), call
`validateReply` (`POST /v2/bot/message/validate/reply`) with the same
`messages[]` body. It returns `200` or a `400` with `details[].property`
pointing at the offending field, and it spends no quota.

## Enriching the reply

`getProfile` (`GET /v2/bot/profile/{userId}`) returns `displayName`,
`pictureUrl`, `statusMessage` and `language` for a user who has added the
Official Account. A `404` here is normal and means one of: the user does not
exist, has not consented, is not a friend, or has blocked the account — handle
it, do not retry it.

## Errors to expect

| Status | Meaning | Action |
|---|---|---|
| 400 `Invalid reply token` | Token expired or already used | Fall back to `pushMessage`, or drop |
| 400 `The request body has X error(s)` | Message object invalid | Read `details[].property` |
| 401 | Channel access token invalid | Re-issue via the Channel Access Token API |
| 429 | Rate limit, concurrency limit, or monthly quota | Discriminate on the `message` string |
| 500 | LINE-side failure | Safe to retry only with a retry key — which reply does not support |

Read `errors/line-problem-types.yml` for the full catalogue. Note there is no
numeric error code: the `message` string is the only discriminator.
