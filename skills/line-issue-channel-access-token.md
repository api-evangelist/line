---
name: line-issue-channel-access-token
description: >-
  Obtain and rotate a LINE channel access token — the credential every
  Messaging API, Insight, Manage Audience and LIFF call carries. Covers all
  four token generations LINE ships and which one to pick.
api: LINE Channel Access Token API
version: '1.0.0'
generated: '2026-08-13'
method: generated
source: >-
  openapi/line-channel-access-token-openapi.yml,
  authentication/line-authentication.yml,
  https://developers.line.biz/en/docs/basics/channel-access-token/
operations:
  - issueChannelTokenByJWT
  - verifyChannelTokenByJWT
  - revokeChannelTokenByJWT
  - getsAllValidChannelAccessTokenKeyIds
  - issueStatelessChannelToken
  - issueChannelToken
  - verifyChannelToken
  - revokeChannelToken
---

# Issue a LINE channel access token

## Pick the right generation

LINE ships four, all live on `https://api.line.me` simultaneously.

| Generation | Operation | TTL | Rotatable | Revocable |
|---|---|---|---|---|
| Long-lived | Console only, no API | none | no | reissue invalidates the old one |
| v2.0 short-lived | `issueChannelToken` | 30 days | no | `revokeChannelToken` |
| **v2.1 JWT assertion** | `issueChannelTokenByJWT` | up to 30 days | **yes** | `revokeChannelTokenByJWT` |
| v3 stateless | `issueStatelessChannelToken` | 15 minutes | n/a | not revocable |

**Choose v2.1 for any production service.** It is the only generation that lets
multiple tokens be valid at once, which is what makes zero-downtime rotation
possible. Choose v3 stateless for short-lived automation where you would rather
not store a credential at all.

## v2.1 — JWT assertion flow

### 1. Register an assertion signing key

In the LINE Developers Console, generate a key pair and register the public JWK
on the channel. You keep the private key.

### 2. Build and sign the JWT

Claims: `iss` and `sub` are the channel ID, `aud` is `https://api.line.me`,
`exp` no more than 30 minutes ahead, `token_exp` the desired token lifetime in
seconds (max 2,592,000 = 30 days). Header carries `alg: RS256`, `typ: JWT` and
the `kid` of the registered key.

### 3. Exchange it

```
POST https://api.line.me/oauth2/v2.1/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion={SIGNED_JWT}
```

The response carries `access_token`, `token_type: Bearer`, `expires_in` and
`key_id`. Store the `key_id` — it is how you identify this token later.

### 4. Rotate without downtime

- `getsAllValidChannelAccessTokenKeyIds`
  (`GET /oauth2/v2.1/tokens/kid`) lists the `key_id`s currently valid.
- Issue the new token, deploy it, confirm traffic has moved, then call
  `revokeChannelTokenByJWT` (`POST /oauth2/v2.1/revoke`) on the old one.
- `verifyChannelTokenByJWT` (`GET /oauth2/v2.1/verify?access_token=...`)
  reports the remaining lifetime and the owning channel.

## v3 — stateless

```
POST https://api.line.me/oauth2/v3/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id={channel ID}
&client_secret={channel secret}
```

Returns a 15-minute token. There is no revocation endpoint — expiry is the only
lifecycle. Rate limited to **370 requests per second**, which is the highest
published limit on the platform and a signal that LINE expects it to be called
per-request rather than cached.

## Using the token

```
Authorization: Bearer {CHANNEL_ACCESS_TOKEN}
```

The token is scoped to **one channel**. It carries no OAuth scope: entitlement
is a property of the channel and the Official Account plan, so an unauthorized
call returns `403` with `"Access to this API is not available for your
account"` rather than an `insufficient_scope` error.

Do not confuse this with LINE Login — that is a separate, end-user OAuth 2.0 /
OpenID Connect surface at `access.line.me` with the scopes `openid`, `profile`
and `email`. See `scopes/line-scopes.yml`.

## The exception

`attachModule` on the Module Attach API uses **HTTP Basic** against
`https://manager.line.biz`, not a Bearer channel token. It is the only
operation in the nine published specs that does.
