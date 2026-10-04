# QA and Release Checklist

Use this checklist before every merge or Cloudflare deployment.

## A. Repository and file integrity

- [ ] `index.html` exists at repository root.
- [ ] All relative paths in `index.html` resolve.
- [ ] `css/app.css` loads without a 404.
- [ ] `js/app.js` loads without a 404.
- [ ] `data/stations.json` loads and parses.
- [ ] `data/devices.json` loads and parses.
- [ ] `data/locations.json` loads and parses if used.
- [ ] `data/lookups.json` loads and parses if used.
- [ ] JavaScript passes a syntax check.
- [ ] JSON files pass a JSON parser.
- [ ] Browser console has no uncaught errors.
- [ ] Network panel shows no failed required requests.

## B. Data integrity

- [ ] Every station has a unique, non-empty `stationId`.
- [ ] Every device has a unique, non-empty `deviceId`.
- [ ] Every device `stationId` resolves to an existing station, or is intentionally unassigned.
- [ ] No asset tag, serial, IP, extension, MAC, user, or status was fabricated.
- [ ] Missing values remain blank or explicitly marked for audit.
- [ ] Controlled fields use supported values.
- [ ] Station count matches the expected source count after imports.
- [ ] Duplicate station IDs are rejected.
- [ ] Duplicate non-empty asset tags are reviewed.
- [ ] Duplicate non-empty serial numbers are reviewed.
- [ ] Deactivated stations are not accidentally deleted with their history.

## C. Main inventory page

- [ ] Initial page renders with all stations.
- [ ] KPI counts match the loaded data.
- [ ] Search finds station name.
- [ ] Search finds station ID.
- [ ] Search finds device tag.
- [ ] Search finds device serial.
- [ ] Search finds assigned user.
- [ ] Multi-select values within one filter category use OR logic.
- [ ] Different filter categories use AND logic.
- [ ] Clearing filters restores all stations.
- [ ] Active filter count is correct.
- [ ] Every sortable heading sorts ascending and descending.
- [ ] Sort remains correct on numeric completion values.
- [ ] Empty-result state is readable.
- [ ] Clicking a row opens the correct station.
- [ ] Clicking Inspect opens the correct station once, without duplicate events.

## D. Audit view

- [ ] Station name is the largest title.
- [ ] Station ID is visible and correct.
- [ ] Breadcrumb values match the selected station.
- [ ] Breadcrumb filtering updates the queue.
- [ ] Queue search works.
- [ ] Queue department filters work.
- [ ] Current station is visibly highlighted.
- [ ] Route progress uses the current queue, not all stations.
- [ ] Previous navigates within the current route.
- [ ] Skip navigates without unintentionally saving.
- [ ] Save commits the selected station.
- [ ] Save & Next commits and opens the correct next station.
- [ ] Completion percentage recalculates after edits.
- [ ] Overview attention cards match missing data.
- [ ] Full station form is collapsed initially.
- [ ] Device-card color matches device completeness.
- [ ] Selecting one device opens only that device editor.
- [ ] Device edits do not overwrite another device.
- [ ] Verification checks persist after save.
- [ ] Notes persist after save.
- [ ] Quick Pass behavior matches written business rules.
- [ ] Flag Issue opens the correct workflow and preserves notes.
- [ ] Close does not silently discard edits; warn when needed.

## E. Persistence and export

- [ ] Local changes survive refresh in the same browser.
- [ ] Existing local edits merge with the correct station ID.
- [ ] Clearing browser storage produces a predictable clean state.
- [ ] Export creates valid JSON.
- [ ] Export contains all stations and devices.
- [ ] Export includes edited values.
- [ ] Import or migration scripts reject malformed records.
- [ ] Database save failures display a clear visible error after backend migration.
- [ ] No UI reports success before the server confirms the write after backend migration.

## F. Responsive checks

### Desktop

- [ ] Filter sidebar remains usable at 1366×768.
- [ ] Table header remains visible while scrolling.
- [ ] No important column is clipped without horizontal-scroll affordance.
- [ ] Audit queue and workspace use available width.

### iPad landscape

- [ ] Queue and workspace are both usable.
- [ ] Touch targets are large enough.
- [ ] Audit footer remains accessible.
- [ ] Keyboard opening does not hide the active field permanently.

### iPad portrait

- [ ] Filter and queue drawers open and close.
- [ ] Form remains readable.
- [ ] Save controls remain reachable.

### Phone portrait

- [ ] No desktop table is rendered.
- [ ] Station cards fill width.
- [ ] Audit uses the full viewport.
- [ ] There is no horizontal page scroll.
- [ ] There is one primary content scroll, not multiple confusing nested scrolls.
- [ ] Inputs use a readable size and do not trigger page zoom.
- [ ] Device cards are easily tappable.
- [ ] Bottom browser chrome does not cover save controls.

## G. Accessibility

- [ ] Every input has a visible label.
- [ ] Buttons have meaningful text or accessible names.
- [ ] Keyboard focus is visible.
- [ ] Filters and audit navigation work by keyboard.
- [ ] Color is not the only indication of status.
- [ ] Text/background contrast is readable.
- [ ] Dialog focus is trapped appropriately.
- [ ] Closing a dialog returns focus to the triggering station.
- [ ] Error messages are associated with the relevant field.

## H. Security and privacy after database migration

- [ ] Authentication is required where appropriate.
- [ ] Authorization rules are tested for every role.
- [ ] Users cannot alter another location's data unless permitted.
- [ ] API validates all fields server-side.
- [ ] Database credentials are never stored in browser code.
- [ ] Row-level security or API authorization is enabled.
- [ ] Uploaded files are type- and size-validated.
- [ ] Audit log records actor, action, and timestamp.
- [ ] Sensitive operational data is not exposed in public repositories.
- [ ] Production database backups are configured and restore-tested.

## I. Deployment smoke test

- [ ] GitHub commit is on the branch connected to Cloudflare.
- [ ] Cloudflare deployment reports success.
- [ ] Deployed revision matches the intended commit.
- [ ] Production URL loads in a private browser window.
- [ ] Production data requests return 200 responses.
- [ ] Desktop inspect flow works.
- [ ] Phone station-card flow works.
- [ ] One edit/save/export test succeeds.
- [ ] Old cached assets do not mask the new release; asset versioning is used when necessary.
