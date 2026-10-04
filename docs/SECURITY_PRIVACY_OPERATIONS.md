# Security, Privacy, and Operations

## Repository exposure

Before committing data, confirm whether the repository is public or private. Inventory information can reveal internal locations, workstation names, usernames, network information, device identifiers, and infrastructure details. Production operational data should not be placed in a public repository.

## Authentication

A production shared version should require authentication. Do not rely on possession of the workers.dev URL as authorization.

## Authorization

Define which roles can:

- View all stations.
- Audit assigned locations.
- Change station structure.
- Reassign devices.
- Resolve issues.
- Manage users/lookups.
- Export the full inventory.

## Server-side enforcement

Client-side hiding is not security. Permissions and field validation must be enforced by the API/database layer.

## Secrets

Never place privileged database keys, service-role keys, storage secrets, or API secrets in:

- `index.html`
- Browser JavaScript
- JSON data files
- Public GitHub commits

## Logging

Log operationally useful events without logging secrets. Recommended events include:

- Login/logout.
- Station opened.
- Audit started/completed.
- Station updated.
- Device assigned/unassigned.
- Issue created/resolved.
- Export produced.
- Permission denied.
- Save conflict.

## Backups and recovery

- Schedule database backups.
- Test restoration, not only backup creation.
- Preserve periodic exports in a controlled location.
- Document who can perform recovery.
- Keep migration and rollback instructions with each release.

## Monitoring

Monitor:

- API error rate.
- Failed saves.
- Authentication failures.
- Conflict rate.
- Slow queries.
- Upload failures.
- Client JavaScript errors.
- Data-integrity check failures.

## Data retention

Define retention for:

- Audit history.
- Issue history.
- Photos and attachments.
- Deactivated stations/devices.
- Export files.
- Client-side queued/offline data.
