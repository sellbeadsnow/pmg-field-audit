# UX and Product Guide

## Primary users and modes

### Inventory administrator

Needs broad searching, multi-select filters, sort, export, data-health review, and comparison across locations.

### Field auditor on iPad

Needs a route/queue, recognizable station heading, clear progress, issue visibility, and Save & Next.

### Field auditor on phone

Needs station cards, full-width screens, one focused editor, large touch targets, and minimal nested scrolling.

## Non-negotiable UX rules

1. The station name is the primary title.
2. Station ID is secondary metadata.
3. Location context is always visible while auditing.
4. Every screen should answer “what should I do next?”
5. Missing information must be shown before the user opens a form.
6. Long forms must be progressive or collapsed.
7. Only one device editor opens at a time.
8. Save & Next must preserve the selected route/filter context.
9. Mobile must not render the desktop table.
10. A full page must never be scaled down to fit a desktop minimum width.
11. Avoid nested scroll regions on phone. The auditable content should have one primary vertical scroll.
12. Touch targets should be at least approximately 44 pixels high.
13. Form text should be at least 16 pixels on phones to avoid browser zoom.
14. Destructive actions must be visually separated and confirmed.
15. Quick Pass must never silently invent missing identifiers.

## Main inventory behavior

### Desktop

- Persistent left filter pane.
- Multi-select filters.
- Search across station and device values.
- Sortable headers.
- Sticky headers when the table scrolls.
- Completion and data-health columns.
- Clear filter control visible at all times.

### Phone

- Filter pane becomes a drawer.
- Inventory table becomes vertically stacked station cards.
- Cards show station name, ID, location, department, completion, and status.
- Opening a card launches a full-width audit view.

## Filter semantics

- Values within one category use **OR** logic. Example: Canton OR Jasper.
- Different categories use **AND** logic. Example: (Canton OR Jasper) AND Provider AND Needs Review.
- Active filters should display a count.
- Clear Filters should clear search and every selected filter.
- Breadcrumb filters inside the audit view should update the queue and main page consistently.

## Audit view hierarchy

### Header

- Clickable location/facility/business-line/department breadcrumbs.
- Large station name.
- Secondary station ID.
- Quick Pass, Flag, and Close controls.

### Summary

- Completion percent.
- Devices identified versus expected.
- Missing categories.
- Current status.

### Overview

- “What needs attention” cards.
- Full station form collapsed under a disclosure control.

### Devices

- Device status cards.
- Green: required identifiers complete.
- Amber: identified device missing important fields.
- Red: expected device not identified.
- Selecting a card opens a single device editor.

### Verify

- Plain-language checklist.
- Notes/issue description.
- Photo control when attachments are implemented.

### Review & Save

- Missing required values.
- Issues that will remain open.
- Save and Save & Next.

## Avoided mistakes

- Do not show every station and device field in one giant form.
- Do not use a technical station ID as the dominant title.
- Do not create a large decorative hero that pushes the working inventory below the fold.
- Do not use a wide table on phone.
- Do not hide the current station or route when a device editor opens.
- Do not make filters single-select when users need cross-location or cross-department review.
- Do not let quick actions bypass required audit evidence without explicit rules.
