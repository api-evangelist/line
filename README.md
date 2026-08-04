# LINE (line)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

LINE is a Japan-based messaging platform with over 200 million monthly active users across Japan, Taiwan, Thailand, and Indonesia, offering messaging, payments, news, and a broad ecosystem of services. The LINE Developers platform exposes public APIs for building chatbots, mini-apps, social login, and audience marketing, all documented as OpenAPI specifications. APIs use Bearer token authentication with channel access tokens issued per LINE channel.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/line/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/line/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Messaging
- Chatbots
- Social Login
- Mini Apps
- Marketing
- Japan

## Timestamps

- **Created:** 2026-05-11
- **Modified:** 2026-05-29

## APIs

### LINE Messaging API

Send and receive messages, manage rich menus, broadcast and narrowcast content, and handle webhooks for LINE chatbots and Official Accounts. Uses Bearer channel access tokens against api.line.me and api-data.line.me.

- **Human URL:** [https://developers.line.biz/en/docs/messaging-api/](https://developers.line.biz/en/docs/messaging-api/)
- **Base URL:** `https://api.line.me`

#### Tags

- Messaging
- Chatbots
- Webhooks
- Rich Menus

#### Properties

- [Documentation](https://developers.line.biz/en/reference/messaging-api/)
- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/messaging-api.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Webhook  Open A P I](https://raw.githubusercontent.com/line/line-openapi/main/webhook.yml)
- [AsyncAPI](https://raw.githubusercontent.com/api-evangelist/line/refs/heads/main/asyncapi/line-messaging-webhook.yml) — [AsyncAPI Specification](https://www.asyncapi.com/docs/reference/specification/latest)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### LINE Front-end Framework (LIFF) Server API

Server API for managing LINE Front-end Framework (LIFF) apps which embed web applications inside the LINE client. Authenticated with Bearer channel access tokens.

- **Human URL:** [https://developers.line.biz/en/docs/liff/](https://developers.line.biz/en/docs/liff/)
- **Base URL:** `https://api.line.me`

#### Tags

- LIFF
- Web Apps
- Channels

#### Properties

- [Documentation](https://developers.line.biz/en/reference/liff-server/)
- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/liff.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### LINE Channel Access Token API

Issue, verify, and revoke channel access tokens used to authenticate requests to other LINE APIs.

- **Human URL:** [https://developers.line.biz/en/docs/messaging-api/channel-access-tokens/](https://developers.line.biz/en/docs/messaging-api/channel-access-tokens/)
- **Base URL:** `https://api.line.me`

#### Tags

- Authentication
- Tokens

#### Properties

- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/channel-access-token.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### LINE Insight API

Retrieve analytics and engagement metrics for LINE Official Accounts, including message delivery, follower demographics, and reach statistics.

- **Human URL:** [https://developers.line.biz/en/reference/messaging-api/#get-insight](https://developers.line.biz/en/reference/messaging-api/#get-insight)
- **Base URL:** `https://api.line.me/v2/bot/insight`

#### Tags

- Analytics
- Insights
- Metrics

#### Properties

- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/insight.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### LINE Manage Audience API

Create and manage audience groups for targeted narrowcast messaging on LINE Official Accounts.

- **Human URL:** [https://developers.line.biz/en/reference/messaging-api/#manage-audience-group](https://developers.line.biz/en/reference/messaging-api/#manage-audience-group)
- **Base URL:** `https://api.line.me/v2/bot/audienceGroup`

#### Tags

- Audiences
- Marketing
- Targeting

#### Properties

- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/manage-audience.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### LINE Shop (Mission Stickers) API

Mission Stickers API for distributing LINE stickers to users who complete specific missions, integrated with LINE Official Accounts.

- **Human URL:** [https://developers.line.biz/en/docs/messaging-api/mission-stickers/](https://developers.line.biz/en/docs/messaging-api/mission-stickers/)
- **Base URL:** `https://api.line.me/shop`

#### Tags

- Stickers
- Promotions

#### Properties

- [OpenAPI](https://raw.githubusercontent.com/line/line-openapi/main/shop.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/line.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/line.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/linecorp)
- [Website](https://line.me)
- [Developer  Portal](https://developers.line.biz/en/)
- [Documentation](https://developers.line.biz/en/docs/)
- [Open A P I  Repository](https://github.com/line/line-openapi)
- [GitHub Organization](https://github.com/line)
- [Sign Up](https://developers.line.biz/console/)
- [Pricing](https://www.linebiz.com/jp-en/service/line-account-connect/)
- [M C P Server](https://github.com/line/line-bot-mcp-server)
- [L L Ms Txt](https://developers.line.biz/llms.txt)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
