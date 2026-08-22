# ScreenCloud

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
