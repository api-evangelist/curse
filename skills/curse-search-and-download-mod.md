---
name: Search and download a CurseForge mod
description: Find a game, search its mods, pick a file, and resolve a download URL using the CurseForge Core API.
api: openapi/curse-core-openapi-original.yml
operations:
  - Get Games
  - Search Mods
  - Get Mod Files
  - Get Mod File Download URL
---

# Search and download a CurseForge mod

Use the CurseForge Core API (`https://api.curseforge.com`) to locate a mod for a game and resolve a downloadable file.

## Auth
Send an `x-api-key: <API_KEY>` header on every request. Keys are issued from the CurseForge for Studios developer console (https://console.curseforge.com/); third-party access requires an approved application.

## Steps
1. **Get Games** (`GET /v1/games`) — list supported games and find the target `gameId` (Minecraft is `432`).
2. **Search Mods** (`GET /v1/mods/search`) — pass `gameId` plus filters (`searchFilter`, `categoryId`, `gameVersion`, `sortField`). Read `pagination` (`index`/`pageSize`, max page size 50, `index + pageSize <= 10000`) to page results.
3. **Get Mod Files** (`GET /v1/mods/{modId}/files`) — list files for the chosen `modId`; select the `fileId` matching the game version / mod loader.
4. **Get Mod File Download URL** (`GET /v1/mods/{modId}/files/{fileId}/download-url`) — resolve the CDN download URL.

## Notes
- Responses are wrapped in a `{ "data": ... }` envelope; paginated responses add a `pagination` object.
- Errors are plain HTTP status codes (403 = missing/invalid key, 404 = unknown id). See `errors/curse-problem-types.yml`.
- No idempotency-key or rate-limit headers are documented; retry 5xx with backoff.
