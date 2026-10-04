# Lessons Learned and Mistakes to Avoid

## 1. Do not hardcode operational data in the HTML

Hardcoded sample arrays made early prototypes quick, but every data correction required code changes. Station and device data now belong in JSON or a database.

## 2. Do not treat expected devices as confirmed devices

A station can require a printer without having a verified printer tag. Expectation and physical asset assignment must be separate concepts.

## 3. Do not fabricate missing identifiers

Stress testing must never generate plausible-looking AFPMG tags, serial numbers, phone extensions, MAC addresses, IP addresses, assigned users, or audit outcomes unless the data is explicitly labeled synthetic and isolated from production.

## 4. Do not use station ID as the primary human heading

Auditors recognize “Canton, Check In, 3rd Floor,” not an encoded identifier. The station name should be prominent; ID should remain visible as technical metadata.

## 5. Do not show the complete edit form by default

Large station and device forms were visually overwhelming. The better pattern is summary → attention cards → focused device card → optional expanded details.

## 6. Do not force desktop layout onto phone

A wide table and split queue/workspace caused the browser to shrink the interface. Phone needs cards, full-width audit, drawer-based queue/filter pane, and one focused editor.

## 7. Avoid nested scrolling on mobile

A scrolling dialog containing a scrolling body and a scrolling device form is difficult to operate. Maintain one primary vertical content scroll on phone.

## 8. Do not let decorative content dominate the work area

Large KPI and hero areas pushed inventory below the fold. Operational content should be visible quickly; KPIs should remain compact.

## 9. Do not make filters single-select when the real workflow is comparative

Users need combinations such as Canton + Jasper, Provider + MA, Needs Review + In Progress. Multi-select filter categories are essential.

## 10. Define filter logic explicitly

Values within a category use OR. Categories use AND. This must remain consistent on the main page, breadcrumbs, and station queue.

## 11. Do not rely on element IDs becoming JavaScript global variables

That behavior differs across environments and previously broke Inspect/Edit. Always bind DOM elements explicitly.

## 12. Remove references when UI elements are removed

A prior redesign deleted heading elements but left JavaScript assignments to those elements. Execute syntax and runtime checks after structural changes.

## 13. Do not report save success before persistence succeeds

LocalStorage allowed immediate saves, but a future API can fail. Keep the station open and show failure until the server confirms the transaction.

## 14. Save & Next must preserve context

The next station should come from the active route/filter context, not blindly from all stations.

## 15. GitHub-hosted JSON is not a multi-user database

Browser edits cannot safely rewrite repository JSON. Shared editing requires a backend, authentication, conflict control, and an audit trail.

## 16. Count and identity checks matter

Before and after imports, verify station counts, device counts, unique IDs, orphan devices, and duplicates. A visually correct page can still contain corrupted relationships.

## 17. Test real viewport sizes

Desktop browser resizing is not enough. Verify on actual or emulated phone portrait, iPad portrait, and iPad landscape, including browser chrome and on-screen keyboard behavior.

## 18. Documentation must change with the code

Architecture, schema, tests, deployment steps, and known limitations should be updated in the same pull request as significant features.
