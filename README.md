# PMG Field Audit

Static GitHub/Cloudflare application for auditing PMG stations and expected devices.

## Data architecture

- `data/stations.json`: 129 physical station-master records from the clean inventory workbook.
- `data/devices.json`: 457 expected device-slot records. Where a supported matched asset was available in the Station Audit Master excerpt, tag/serial/user/status are included. Otherwise the record is deliberately marked `Needs Audit`; it is not synthetic asset identification.
- `data/locations.json`: location summaries.
- `data/lookups.json`: filter values.

The workbook states that the station master contains 129 rows and uses one row per physical station. The app therefore treats stations as the source of truth and device records as editable expected slots.

## Deploy

Upload this complete folder structure to the repository root. Cloudflare's Git integration should serve `index.html`. Do not move the `css`, `js`, or `data` folders because `index.html` uses relative paths.

## Local testing

Because the app uses `fetch()` for JSON, opening `index.html` directly as a `file://` URL may be blocked by the browser. Test through Cloudflare or a local static server.

## Important persistence note

Edits are currently saved in the browser's `localStorage`. Export JSON regularly. GitHub JSON files do not update automatically when a user edits the app. A shared multi-user version will need a backend such as Supabase, Cloudflare D1, or an API.
