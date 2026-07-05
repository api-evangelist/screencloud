# ScreenCloud

ScreenCloud is a cloud [digital signage](https://screencloud.com) platform that
turns any screen into a managed display for content, dashboards, and apps. This
repository is an [APIs.json](https://apisjson.org) (0.19) catalog of ScreenCloud's
public developer surface, maintained as part of the API Evangelist
[`all/*`](https://apievangelist.com) network.

## Access model - be honest about what is public

ScreenCloud's documented public developer interface is the **ScreenCloud Studio
API**, a single **GraphQL** endpoint:

- **GraphQL, one endpoint, no versioning.** Anything you can do by hand in
  ScreenCloud Studio can be automated through queries and mutations.
- **Bearer-token auth.** Create a token in Studio under **Account Settings >
  DEVELOPER > New Token**, choose its permissions, and send it as
  `Authorization: Bearer <token>`.
- **Region-specific endpoint.** The GraphQL endpoint URL is *not* published as one
  fixed public host - you copy it from the same **DEVELOPER** tab, and it varies by
  organization region. Because of this, the `baseURL` in `apis.yml` points at the
  Studio platform host (`https://studio.screencloud.com`) rather than a fabricated
  GraphQL host.
- **Request/response only.** The Studio API is GraphQL over HTTPS. There is **no
  documented public WebSocket API and no documented GraphQL subscription / SSE
  transport** for third-party developers (see `review.yml`). Realtime delivery to
  players is internal and not published as a public interface.

Two related offerings sit alongside the Studio API:

- **Developer Platform / App framework** (`screencloud.github.io/developer`) - build
  custom signage apps in HTML/JavaScript. Apps are private by default; publishing to
  the public App Store is **approval-gated** ("just let us know and we'll make it
  happen"). This is a client-side app runtime, not a WebSocket API.
- **Legacy Router API** (`screencloud.docs.apiary.io`) - an older REST surface on
  Apiary, **superseded** by the Studio GraphQL API. Not the recommended integration
  path today.

The GraphQL **operation names** captured in this repo are taken **verbatim** from
ScreenCloud's public GraphQL reference (Studio Service `v2.103.0`). The object and
input **field bodies** in the curated SDL are modeled for readability and are not
exhaustive - the production schema has hundreds of operations.

## APIs in this catalog

The single GraphQL endpoint is organized here into six logical APIs:

| API | What it covers |
| --- | --- |
| **Studio Screens API** | Pair/depair devices, list/search screens, screen groups, assign content, send commands (refresh, clear cache, screenshot). |
| **Studio Playlists API** | Create/update playlists, add files/links/apps, publish drafts. |
| **Studio Media and Files API** | Upload and organize media, folders, tags, file processing jobs. |
| **Studio Channels and Casts API** | Multi-zone channels, publish/duplicate, add content to zones, start/stop casts. |
| **Studio Apps API** | App catalog, app instances, install/uninstall at org or space scope. |
| **Studio Playback Logs API** | Proof-of-play logs, exports, screen content history, QR metrics. |

## Repository layout

```
apis.yml                     APIs.json 0.19 catalog (six logical GraphQL APIs)
README.md                    This file
review.yml                   WebSocket review + confirmed GraphQL operations
graphql/
  screencloud-studio-schema.graphql   Curated SDL (verbatim operation names)
  screencloud-graphql.md              GraphQL usage guide + examples
collections/
  screencloud.opencollection.json     GraphQL request collection
plans/
  screencloud-plans-pricing.yml       Core / Pro / Enterprise per-screen pricing
rate-limits/
  screencloud-rate-limits.yml         Rate-limit posture (none published)
finops/
  screencloud-finops.yml              FinOps view (per-screen subscription)
```

## Pricing (per screen, per month, USD, +VAT)

- **Core** - $20/screen. 80+ apps and integrations, unlimited storage, templates,
  Quick Post. Free trial.
- **Pro** - $30/screen. Adds premium apps, dashboards, and QR-code metrics. Free
  trial.
- **Enterprise** - custom quote. Adds a free device on annual plans, professional
  design support, onboarding and training.

The Studio GraphQL API carries no separate API fee - it automates an account you
already pay for on a per-screen basis. See `plans/` and `finops/`. Verify current
numbers on [screencloud.com/pricing](https://screencloud.com/pricing).

## Links

- Website: https://screencloud.com
- Developer docs: https://developer.screencloud.com/
- GraphQL reference: https://screencloud.github.io/signage-next-graphql-docs/graphql-reference
- Developer Platform: https://screencloud.github.io/developer/
- Studio (sign in / get token): https://studio.screencloud.com
- Pricing: https://screencloud.com/pricing

---

Maintained by [Kin Lane](https://apievangelist.com) - kin@apievangelist.com
