# Database Migration Plan

## Goal

Replace browser-only localStorage persistence with a secure, shared, auditable data service without breaking the existing field workflow.

## Phase 0: Freeze and inventory

- Tag the last JSON/localStorage version in Git.
- Export the current station and device working set.
- Document controlled values and required fields.
- Identify which fields came from station master versus unverified references.
- Decide whether existing browser edits need collection from multiple devices.

## Phase 1: Database foundation

- Create development and production projects/databases.
- Create schema from `ARCHITECTURE_AND_DATA_MODEL.md`.
- Add unique constraints and foreign keys.
- Add migration tooling.
- Create service roles and user roles.
- Seed controlled lookup values.

## Phase 2: Import

- Import locations and facilities first.
- Import stations using stable `stationId`.
- Import device requirements from expected slots.
- Import known physical devices separately.
- Create current station-device assignments only when supported by source data.
- Log unmatched references for review; do not force a match.
- Produce reconciliation counts and exception files.

## Phase 3: Read-only API integration

- Add an API client module.
- Replace station/device JSON fetches with API GET requests behind a feature flag.
- Keep JSON fallback temporarily.
- Compare API-derived counts with JSON-derived counts.
- Verify filters, sorting, queue, and completion calculations.

## Phase 4: Shared writes

- Add authenticated PATCH/POST endpoints.
- Save station updates and device updates transactionally.
- Add loading, success, and error states.
- Do not close a station or navigate next until the save succeeds.
- Add optimistic-concurrency checks using `version` or `updated_at`.
- Display a conflict-resolution screen when a newer version exists.

## Phase 5: Audits and issues

- Save each field visit as an audit record.
- Save checklist items as audit checks.
- Create issues separately from general notes.
- Add issue status, severity, ownership, and resolution.
- Add server-generated audit events.

## Phase 6: Photos and attachments

- Add object storage.
- Restrict file types and sizes.
- Store metadata in `attachments`.
- Associate attachments with a station, audit, and/or issue.
- Confirm retention and privacy requirements before rollout.

## Phase 7: Offline strategy

Do not assume a service worker alone solves offline auditing. Define:

- What data is downloaded for a route.
- How offline edits are queued.
- How client-generated IDs are reconciled.
- Conflict rules.
- Retry behavior.
- User-visible sync state.
- Recovery/export if synchronization fails.

## Cutover checklist

- Database record counts reconciled.
- All foreign keys valid.
- Authentication tested.
- Authorization tested.
- Read-only comparison passed.
- Multi-user write test passed.
- Conflict test passed.
- Backup and restore tested.
- Export tested.
- Monitoring enabled.
- Rollback plan documented.

## Rollback

- Preserve the last working JSON release.
- Keep database migration scripts reversible where feasible.
- Do not delete source JSON until production has passed acceptance testing.
- Maintain a database export path compatible with the application's recovery process.
