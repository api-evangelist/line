---
name: line-narrowcast-to-audience
description: >-
  Build a LINE audience from user IDs, wait for it to become ready, send a
  narrowcast message to it, and track delivery — the marketing path across the
  Manage Audience, Messaging and Insight APIs.
api: LINE Manage Audience API + LINE Messaging API + LINE Insight API
version: '1.0.0'
generated: '2026-08-13'
method: generated
source: >-
  openapi/line-manage-audience-openapi.yml,
  openapi/line-messaging-api-openapi.yml,
  openapi/line-insight-openapi.yml,
  rate-limits/line-rate-limits.yml
operations:
  - createAudienceGroup
  - addAudienceToAudienceGroup
  - getAudienceData
  - getAudienceGroups
  - narrowcast
  - getNarrowcastProgress
  - validateNarrowcast
  - getNumberOfMessageDeliveries
  - deleteAudienceGroup
---

# Narrowcast to a LINE audience

Everything below runs against `https://api.line.me` with a channel access
token, except the by-file audience uploads, which go to
`https://api-data.line.me`.

## Step 1 — Create the audience

```
POST https://api.line.me/v2/bot/audienceGroup/upload
Authorization: Bearer {CHANNEL_ACCESS_TOKEN}

{
  "description": "spring-campaign-2026",
  "isIfaAudience": false,
  "audiences": [ { "id": "{userId}" } ]
}
```

Returns `202` with `audienceGroupId`, `type: UPLOAD`, `description` and
`created`. The `202` is the point: this is an **asynchronous job**, not a
completed write.

`createAudienceForUploadingUserIds` (`POST /v2/bot/audienceGroup/upload/byFile`)
is the same thing for a file upload and must be sent to `api-data.line.me`.

Rate limit on all Manage Audience endpoints: **60 requests per minute**.

## Step 2 — Add more IDs in batches

```
PUT https://api.line.me/v2/bot/audienceGroup/upload
{ "audienceGroupId": 12345678, "audiences": [ { "id": "{userId}" } ] }
```

**Concurrency limit: 10.** Across `createAudienceGroup`,
`createAudienceForUploadingUserIds`, `addAudienceToAudienceGroup` and
`addUserIdsToAudience`, no more than ten jobs may be QUEUED or WORKING for a
single `audienceGroupId` at once. Exceeding it returns `429`.

## Step 3 — Wait for READY

```
GET https://api.line.me/v2/bot/audienceGroup/{audienceGroupId}
```

Poll `audienceGroup.status` until it is `READY`. Other values: `IN_PROGRESS`,
`FAILED`, `EXPIRED`, `INACTIVE`. The `jobs[]` array carries each upload job's
`jobStatus` (`QUEUED`, `WORKING`, `FINISHED`, `FAILED`) and `failedType` — this
is also how you count against the concurrency limit.

**Do not narrowcast to an audience that is not READY.**

## Step 4 — Validate, then narrowcast

Dry-run first — it costs nothing:

```
POST https://api.line.me/v2/bot/message/validate/narrowcast
```

Then send:

```
POST https://api.line.me/v2/bot/message/narrowcast
X-Line-Retry-Key: {hex UUID}

{
  "messages": [ { "type": "text", "text": "..." } ],
  "recipient": {
    "type": "audience",
    "audienceGroupId": 12345678
  },
  "filter": { "demographic": { ... } },
  "limit": { "max": 100, "upToRemainingQuota": true }
}
```

Notes that matter:

- `narrowcast` supports `X-Line-Retry-Key`. Use it.
- Rate limit: **60 requests per hour**. This is a campaign endpoint, not a
  transactional one.
- `limit.upToRemainingQuota: true` caps the send at whatever monthly quota is
  left instead of failing — the safest setting.
- `recipient` supports boolean composition (`and`/`or`/`not`) over multiple
  audience groups.

## Step 5 — Track it

The response returns `X-Line-Request-Id`. Feed it to:

```
GET https://api.line.me/v2/bot/message/progress/narrowcast?requestId={X-Line-Request-Id}
```

`phase` moves `waiting` → `sending` → `succeeded` or `failed`, with
`successCount`, `failureCount`, `targetCount` and `acceptedTime`. A narrowcast
to a small audience can legitimately report `failed` with
`failedDescription` when the audience is below LINE's minimum size.

## Step 6 — Measure

- `getNumberOfMessageDeliveries` (`GET /v2/bot/insight/message/delivery?date=YYYYMMDD`)
  — broadcast/targeting/auto-response counts for a day.
- `getMessageEvent` (`GET /v2/bot/insight/message/event?requestId=...`) —
  impressions, clicks and per-link breakdown for that specific send.

Both are limited to **60 requests per hour**, and insight data is only
available from the day after the send.

## Clean up

`deleteAudienceGroup` (`DELETE /v2/bot/audienceGroup/{audienceGroupId}`).
Audiences expire on their own but count against the channel's audience limits
until then.
