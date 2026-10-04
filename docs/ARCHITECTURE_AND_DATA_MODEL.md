# Architecture and Data Model

## Target architecture

```text
Browser UI
   |
   | HTTPS JSON API
   v
Cloudflare Worker API or Supabase API
   |
   v
Shared relational database
   |
   +-- file/photo object storage
   +-- authentication provider
   +-- audit/event history
```

## Why a separate database is now necessary

The JSON architecture was the correct prototype step because it separated data from UI code and allowed all stations to be stress-tested. JSON in GitHub is not a safe write target for concurrent users. A database is needed for shared state, validation, audit history, permissions, and conflict handling.

## Recommended database options

### Option A: Supabase

Recommended when the project needs authentication, relational data, row-level security, object storage, and a manageable admin interface in one platform.

### Option B: Cloudflare D1 + Worker API

Recommended when keeping the application mostly within Cloudflare is the priority. The Worker should expose controlled endpoints rather than allowing the browser to communicate directly with unrestricted tables.

### Selection criteria

Choose based on:

- Identity provider requirements.
- Whether photo/file storage is needed immediately.
- Who will administer the database.
- Required reporting and export workflows.
- Need for offline synchronization.
- Expected integration with Microsoft identity or internal systems.

## Core relational schema

### locations

```text
id                text primary key
name              text not null
display_order     integer
active            boolean
created_at        timestamp
updated_at        timestamp
```

### facilities

```text
id                text primary key
location_id       text references locations(id)
name              text not null
address           text nullable
active            boolean
created_at        timestamp
updated_at        timestamp
```

A location can contain more than one operational facility, such as primary care and pediatrics in the same city.

### stations

```text
id                     text primary key
source_sequence        integer
facility_id            text references facilities(id)
location_id            text references locations(id)
floor                  text nullable
suite                  text nullable
room_area              text nullable
business_line          text nullable
department             text nullable
station_name           text not null
expected_slots_text    text nullable
status                 text not null
assigned_auditor_id    text nullable
last_audit_at          timestamp nullable
notes                  text nullable
version                 integer not null default 1
active                  boolean not null default true
created_at              timestamp
updated_at              timestamp
```

### station_device_requirements

```text
id                text primary key
station_id        text references stations(id)
device_type       text not null
required          boolean not null
quantity_expected integer not null default 1
notes             text nullable
```

This separates the expectation that a station needs a device from the physical asset currently assigned to it.

### devices

```text
id                text primary key
asset_tag         text nullable unique
serial_number     text nullable
manufacturer      text nullable
model             text nullable
device_type       text not null
assigned_user     text nullable
status            text nullable
ip_address        text nullable
phone_extension   text nullable
phone_mac         text nullable
phone_display_name text nullable
warranty_status   text nullable
warranty_date     date nullable
purchase_date     date nullable
active            boolean not null default true
created_at        timestamp
updated_at        timestamp
```

### station_device_assignments

```text
id                text primary key
station_id        text references stations(id)
device_id         text references devices(id)
requirement_id    text nullable
assigned_at       timestamp
unassigned_at     timestamp nullable
is_current        boolean not null default true
```

Do not store station ownership only on the device row. Assignment history should be preserved.

### audits

```text
id                uuid primary key
station_id        text references stations(id)
auditor_id        text references users(id)
status            text not null
started_at        timestamp
completed_at      timestamp nullable
completion_percent integer
notes             text nullable
client_id         text nullable
created_at        timestamp
updated_at        timestamp
```

### audit_checks

```text
id                uuid primary key
audit_id          uuid references audits(id)
check_code        text not null
result            text not null
notes             text nullable
```

Suggested `result` values: `confirmed`, `failed`, `not_applicable`, `not_checked`.

### issues

```text
id                uuid primary key
station_id        text references stations(id)
device_id         text nullable references devices(id)
audit_id          uuid nullable references audits(id)
issue_type        text not null
severity          text not null
status            text not null
description       text not null
assigned_to       text nullable
created_by        text not null
created_at        timestamp
resolved_at       timestamp nullable
resolution_notes  text nullable
```

### attachments

```text
id                uuid primary key
audit_id          uuid nullable
issue_id           uuid nullable
station_id         text nullable
object_path        text not null
mime_type          text
caption            text nullable
created_by         text
created_at         timestamp
```

### users

```text
id                text primary key
display_name      text not null
email             text unique
role              text not null
active            boolean
```

Suggested roles: `viewer`, `auditor`, `manager`, `administrator`.

### audit_events

```text
id                uuid primary key
entity_type       text not null
entity_id         text not null
action            text not null
actor_id          text not null
before_json       json nullable
after_json        json nullable
created_at        timestamp
```

## ID rules

- `stationId` must remain stable, even if station name, department, or location display text changes.
- `deviceId` must not use array position.
- A device can exist without a current station assignment.
- Do not use asset tag as the only primary key because tags can be missing, duplicated in dirty source data, or replaced.
- Every update sent to the API should include a version or updated timestamp for conflict checking.

## API outline

```text
GET    /api/locations
GET    /api/stations
GET    /api/stations/:id
POST   /api/stations
PATCH  /api/stations/:id
GET    /api/stations/:id/devices
POST   /api/stations/:id/audits
PATCH  /api/audits/:id
POST   /api/issues
PATCH  /api/issues/:id
POST   /api/attachments
GET    /api/exports/inventory
```

## Validation rules

- Location, facility, and station name are required.
- Status values must come from controlled lookup values.
- Asset tag should be unique when present.
- Serial number should be unique when present unless a documented exception exists.
- Phone MAC must be normalized before comparison.
- IP addresses must be validated if entered.
- Audit completion cannot be set to complete while required checks remain `not_checked`, unless a manager override is recorded.
- Deleting a station should normally set `active=false`; do not erase history.
