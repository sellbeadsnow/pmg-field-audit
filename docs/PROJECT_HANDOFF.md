# PMG Field Audit Project Handoff

## 1. Project purpose

PMG Field Audit is a responsive inventory-audit application for reviewing physical workstations and their expected devices across Prestige Medical Group locations. The app supports two related jobs:

1. **Inventory administration**: search, filter, sort, and review stations across all locations.
2. **Field auditing**: walk through a location station by station, correct station/device records, flag issues, and continue to the next station.

The most important design principle is that this is not primarily a spreadsheet editor. It is a guided physical audit tool used while standing at a workstation, often on an iPad or phone.

## 2. Current source of truth

The workbook used during development defined:

- One row per physical station.
- 129 station-master records.
- Expected device slots attached to each station.
- Separate matched/reference device data.
- A station audit master containing available tag, serial, user, phone, printer, and audit fields.

The application currently separates the data into JSON files so the UI is not coupled to a hardcoded JavaScript array.

## 3. Current repository structure

```text
/
├── index.html
├── README.md
├── css/
│   └── app.css
├── js/
│   └── app.js
└── data/
    ├── stations.json
    ├── devices.json
    ├── locations.json
    └── lookups.json
```

### Responsibility of each file

- `index.html`: semantic application structure and accessible controls.
- `css/app.css`: desktop, iPad, and phone layout and visual states.
- `js/app.js`: data loading, derived metrics, filtering, sorting, audit navigation, local persistence, and export.
- `data/stations.json`: one record per physical station.
- `data/devices.json`: expected and identified devices connected by `stationId`.
- `data/locations.json`: location summaries.
- `data/lookups.json`: business lines, departments, statuses, and device types.

## 4. Current deployment

- Source control: GitHub.
- Hosting: Cloudflare Workers Git integration.
- Entry point: repository-root `index.html`.
- The CSS, JavaScript, and data folders must remain at the paths referenced by `index.html`.
- A GitHub commit triggers the connected Cloudflare deployment.

## 5. Current application behavior

### Main inventory page

- Loads station and device data from JSON using `fetch()`.
- Shows station, expected-device, attention, and location KPIs.
- Uses a Microsoft Lists-style filter pane.
- Supports multi-select filter categories.
- Supports search across station and device values.
- Supports sortable table headings on desktop.
- Uses touch-friendly cards instead of a wide table on phones.
- Shows completion and missing-data indicators.

### Inspection/audit view

- Opens full screen.
- Makes the recognizable station name the primary heading.
- Shows the station ID as secondary technical information.
- Uses clickable breadcrumb chips for location, facility, business line, and department.
- Uses a searchable station queue.
- Supports route navigation and Save & Next.
- Shows station completion and device status cards.
- Keeps the long station form collapsed until explicitly opened.
- Opens only one device editor at a time.

### Current persistence

- Station edits are stored in the browser with `localStorage`.
- JSON files in GitHub are not updated by browser edits.
- Changes are not automatically shared between people or devices.
- Export must be used to preserve and reconcile audit work before moving to a database.

## 6. Known limitations

1. **No shared database**: each browser has an isolated set of edits.
2. **No authentication or authorization**: anyone with site access can open the tool.
3. **No concurrency control**: two auditors could work on the same station without detection.
4. **No server-side audit history**: local changes overwrite the latest browser state.
5. **No attachment storage**: photos and documents are not yet implemented.
6. **No transactional save**: station and device changes are not committed atomically to a backend.
7. **No offline reconciliation strategy**: localStorage is not a complete offline synchronization solution.
8. **Expected-device records are not all confirmed physical assets**: blank tag/serial values intentionally mean “verify during walk-through.”

## 7. Product principles that must not be lost

- Physical station name must be more prominent than the station ID.
- A user should always know which station is being audited.
- A user should always know what still needs attention.
- The audit UI should reveal details progressively, not show every field at once.
- Phone UI should use cards and one focused editor, not a horizontally scrolling table.
- iPad landscape can use queue + workspace, but phone portrait should use a hidden drawer and full-width content.
- Desktop is optimized for inventory administration; iPad for field walks; phone for rapid verification.
- Never invent asset tags, serials, MAC addresses, extensions, IP addresses, assignment, or verification status.
- Preserve stable `stationId` and `deviceId` keys through imports and migrations.

## 8. Recommended next step

Move persistence from `localStorage` to a shared database through an API. Keep JSON import/export as a migration and recovery mechanism, not as the primary multi-user database.
