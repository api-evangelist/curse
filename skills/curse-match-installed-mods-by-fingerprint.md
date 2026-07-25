---
name: Match installed mods by fingerprint
description: Identify locally installed mod files by their Murmur2 fingerprint and hydrate full mod metadata via the CurseForge Core API.
api: openapi/curse-core-openapi-original.yml
operations:
  - Get Fingerprints Matches
  - Get Mods
---

# Match installed mods by fingerprint

Given a set of local mod files, identify them against CurseForge and pull their metadata.

## Auth
Send `x-api-key: <API_KEY>` on every request.

## Steps
1. Compute the CurseForge Murmur2 fingerprint for each local file (whitespace-stripped hash, per CurseForge's fingerprinting convention).
2. **Get Fingerprints Matches** (`POST /v1/fingerprints`) — send `{ "fingerprints": [<uint>, ...] }`. The response's `exactMatches[]` map each fingerprint to its `modId` and `file`; unmatched hashes appear in `unmatchedFingerprints`.
3. **Get Mods** (`POST /v1/mods/get-mods-by-ids`) — send `{ "modIds": [...] }` to hydrate full mod records (name, summary, latest files, categories) for every matched `modId`.

## Notes
- For game-scoped matching use `POST /v1/fingerprints/{gameId}` instead.
- Responses use the `{ "data": ... }` envelope. See `conventions/curse-conventions.yml` for pagination and envelope details and `errors/curse-problem-types.yml` for status codes.
