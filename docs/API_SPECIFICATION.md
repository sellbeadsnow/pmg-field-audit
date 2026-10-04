# PMG Field Audit: System Interface & API Specification

## 1. Overview & Base Architecture
This document details the contract between the frontend interface and the core backend service layer. All API requests require a content type of `application/json`. The base path for all resource access is `/api/v1`.

---

## 2. Global Request Security & Headers
Every request to a mutating endpoint must provide user authentication metadata validated at the server layer.

```http
Authorization: Bearer <jwt_access_token>
X-Client-Version: 2.4.0
Content-Type: application/json
```

---

## 3. Core Resource Endpoints

### 3.1: Fetch Filtered Stations
* **Endpoint:** `GET /api/stations`
* **Query Parameters:**
  - `location_id` (string, optional) - Filter by geographic region.
  - `status` (string, optional) - Filter by audit life cycle state.
  - `search` (string, optional) - Text string matching station names or device parameters.

#### Response (`200 OK`)
```json
[
  {
    "id": "stn_01j9x4",
    "station_name": "Canton Clinic Check-In 1",
    "facility_id": "fac_canton_main",
    "location_id": "loc_canton",
    "floor": "3",
    "department": "Pediatrics",
    "status": "needs_attention",
    "completion_percent": 65,
    "version": 4,
    "updated_at": "2026-10-03T18:22:00Z"
  }
]
```

### 3.2: Update Station Audit Record
* **Endpoint:** `PATCH /api/stations/:id`
* **Validation Rule:** Must pass an optimistic-concurrency `version` matching the server's record state.

#### Request Payload
```json
{
  "version": 4,
  "status": "complete",
  "notes": "Verified all clinical peripherals standing at station.",
  "audit_checks": [
    {"check_code": "phys_verify", "result": "confirmed"},
    {"check_code": "label_match", "result": "confirmed"}
  ]
}
```

#### Response (`200 OK`)
```json
{
  "success": true,
  "id": "stn_01j9x4",
  "version": 5,
  "updated_at": "2026-10-04T04:30:12Z"
}
```

---

## 4. Server-Side Data Validation Rules
The API layer must handle, validate, and sanitize the following structures before committing data transactions:

- **IP Addresses:** Checked against regex constraints for valid IPv4 strings (`^([0-9]{1,3}\.){3}[0-9]{1,3}$`).
- **Phone MAC Addresses:** Must be stripped of punctuation and capitalized before comparison (e.g., inputting `00:AA:11:BB:22:CC` normalizes to `00AA11BB22CC`).
- **Uniqueness Fields:** Any provided `asset_tag` or `serial_number` must run a uniqueness check across the active database pool, returning an explicit block list exception on conflict entries.

---

## 5. System Error Contract
When a validation rule breaks or an active concurrency conflict is detected, the API returns a structured error object.

### Example: Concurrency Edit Conflict (`409 Conflict`)
```json
{
  "error": {
    "code": "CONCURRENCY_CONFLICT",
    "message": "The station record was modified by another user since you loaded it.",
    "target_field": "version",
    "context": {
      "client_version": 4,
      "server_version": 5,
      "modified_by": "J. Doe",
      "modified_at": "2026-10-04T04:28:11Z"
    }
  }
}
```
