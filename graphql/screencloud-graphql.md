# ScreenCloud Studio API (GraphQL)

ScreenCloud is a cloud digital signage platform. Its documented public developer
surface is the **ScreenCloud Studio API**, a single **GraphQL** endpoint. Anything
you can do by hand in ScreenCloud Studio - pair screens, build playlists and
channels, upload media, cast content, install apps, export playback logs - can be
automated through this one endpoint.

- **Type:** GraphQL (single endpoint, no versioning, schema evolves in place)
- **Transport:** HTTPS, request/response only (no WebSocket, no subscriptions)
- **Auth:** Bearer token - `Authorization: Bearer <API_TOKEN>`
- **Endpoint:** Region-specific. Copy the exact GraphQL endpoint URL from
  **ScreenCloud Studio > Account Settings > DEVELOPER**. It is not published as one
  fixed public host.
- **Schema version at review:** Studio Service `v2.103.0`
- **Reference:** https://screencloud.github.io/signage-next-graphql-docs/graphql-reference
- **Curated SDL:** [screencloud-studio-schema.graphql](screencloud-studio-schema.graphql)

## Getting an API token

1. In ScreenCloud Studio, open **Account Settings** from the profile menu.
2. Go to the **DEVELOPER** tab and click **New Token**.
3. Select permissions (a "Grant full access" option exists for testing), name the
   token, and create it.
4. Copy the token immediately - it is shown only once.
5. Copy the **GraphQL Endpoint** listed on the same DEVELOPER tab (region-specific).

Connectivity test query:

```graphql
query {
  currentOrgId
}
```

## Logical API areas

The single endpoint is organized in this catalog into six logical APIs:

| Logical API | Representative operations |
| --- | --- |
| Screens | `allScreens`, `screen`, `pairScreen`, `depairScreen`, `setScreenContent`, `refreshScreen`, `sendCommandToScreensByScreenIds` |
| Playlists | `allPlaylists`, `createPlaylist`, `addFileToPlaylist`, `addAppInstanceToPlaylist`, `publishDraftPlaylist` |
| Media & Files | `allFiles`, `createFile`, `searchFile`, `createFolder`, `folderHierarchy`, `filesByTagNames` |
| Channels & Casts | `allChannels`, `createChannel`, `publishDraftChannel`, `addPlaylistToChannel`, `castStart`, `castStop` |
| Apps | `allApps`, `appBySlug`, `createAppInstance`, `installSpaceApp`, `uninstallOrgApp` |
| Playback Logs | `getPlaybackLogs`, `exportPlaybackLogs`, `allScreenContentHistories`, `getQRMetrics`, `downloadQRMetrics` |

## Example: pair a screen

```graphql
mutation PairScreen($code: String!, $name: String, $spaceId: UUID) {
  pairScreen(pairingCode: $code, name: $name, spaceId: $spaceId) {
    id
    name
    deviceId
  }
}
```

## Example: assign a playlist to a screen

```graphql
mutation Assign($screenId: UUID!, $playlistId: UUID!) {
  setScreenContentByPlaylistId(screenId: $screenId, playlistId: $playlistId) {
    id
    content { contentType contentId }
  }
}
```

## Example: export playback logs

```graphql
query Logs($from: Datetime, $to: Datetime) {
  exportPlaybackLogs(from: $from, to: $to)
}
```

## Notes on scope and honesty

- The **Query** and **Mutation** field names in the curated SDL and the tables
  above are taken **verbatim** from ScreenCloud's public GraphQL reference. The
  production schema is much larger (hundreds of operations plus billing, SSO,
  notification, white-label, and feature-flag surfaces); object and input field
  bodies in the SDL are **modeled** for readability, not exhaustive.
- The GraphQL **endpoint host** is region-specific and not published as one fixed
  URL, so `baseURL` in `apis.yml` points at the Studio platform host
  (`https://studio.screencloud.com`) rather than a fabricated GraphQL host.
- A legacy **Router API** exists on Apiary (`screencloud.docs.apiary.io`) but is
  superseded by the Studio GraphQL API.
- ScreenCloud's **Developer Platform / App framework**
  (`screencloud.github.io/developer`) is a separate offering for building custom
  HTML/JavaScript signage apps; publishing to the public App Store is
  approval-gated.

## Tools

Use any GraphQL client - GraphiQL, Altair, Insomnia, or Postman - pointed at your
region's endpoint with the bearer token in the `Authorization` header.
