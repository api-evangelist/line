# LINE (line)

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
