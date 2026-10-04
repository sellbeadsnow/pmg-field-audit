# Roadmap and Backlog

## Priority 0: Reliability

- Add automated data-integrity checks.
- Add browser error logging.
- Add explicit unsaved-change handling.
- Add empty/loading/error states.
- Add cache-busting/versioned assets for deployments.
- Add a test dataset separate from production data.

## Priority 1: Shared database

- Select Supabase or Cloudflare D1/API architecture.
- Create schema and migrations.
- Import stations, requirements, devices, and assignments.
- Add authentication and roles.
- Replace localStorage writes with API transactions.
- Add audit/event history.

## Priority 2: Field audit improvements

- Add explicit route creation and assignment.
- Allow route order to be customized.
- Add “Not found,” “Moved,” and “Not applicable” outcomes.
- Add previous/skip/revisit behavior.
- Add issue severity and ownership.
- Add photo attachments.
- Add barcode/QR scanning after testing browser/device support.

## Priority 3: Data quality

- Duplicate asset-tag review.
- Duplicate serial-number review.
- Device station-assignment history.
- Data normalization for MAC, IP, extensions, and usernames.
- Facility/floor/suite controlled values.
- Merge/reconcile unmatched reference feeds.

## Priority 4: Reporting

- Completion by location.
- Completion by department.
- Missing-data categories.
- Open issues by severity and owner.
- Devices by lifecycle/warranty status.
- Audit throughput and aging.
- Export to CSV/XLSX.

## Priority 5: Administration

- Manage lookups.
- Manage active locations and facilities.
- Manage users and roles.
- Configure required device types by station category.
- Configure checklist templates.
- Archive/deactivate stations.

## Backlog acceptance template

Every feature should include:

```text
Problem:
User:
Primary workflow:
Out of scope:
Data changes:
Permission changes:
Mobile behavior:
Empty/loading/error behavior:
Acceptance criteria:
Regression checks:
Analytics/logging:
Rollback:
```

## Definition of done

A feature is not done until:

- It works with 129+ stations and hundreds of devices.
- It works on desktop, iPad landscape, iPad portrait, and phone portrait.
- It has loading, empty, success, and failure states.
- It does not introduce horizontal phone scrolling.
- It passes relevant items in `QA_RELEASE_CHECKLIST.md`.
- It preserves stable IDs and data integrity.
- It includes updated documentation.
