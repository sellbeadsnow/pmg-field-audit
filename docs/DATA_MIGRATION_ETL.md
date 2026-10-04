# PMG Field Audit: Data Migration & ETL Specification

## 1. Scope & Execution Goal
This specification outlines the data cleaning, transformation, and structural loading pipeline required to move static source records (`stations.json`, `devices.json`) into the production relational database schema.

---

## 2. Extraction Source Matrix

| Source File | Input Entities | Record Count Base | Data State Notes |
| :--- | :--- | :--- | :--- |
| `data/stations.json` | Baseline layout configurations | 129 records | Contains text fields for business units and facilities. |
| `data/devices.json` | Physical assets, requirements | Variable | High rate of missing tags and unverified fields. |

---

## 3. Transformation & Entity Mapping Rules

### Rule 3.1: Geographic De-duplication (`locations` & `facilities`)
- **Extraction Logic:** Scan all unique combinations of facility name and physical address string inside `stations.json`.
- **Transformation:** Programmatically generate stable UUID strings for distinct combinations. Map descriptive text to `facilities.name`.

### Rule 3.2: Splitting Expectations from Physical Assets
Legacy tracking combined station needs with actual serial items. The migration execution splits these into separate targets:

1. **Requirements Mapping (`station_device_requirements`):**
   - Parse column configurations like `expected_slots_text`.
   - If a station record notes a requirement for a printer, insert a requirement row linked to that stable `station_id`.
2. **Asset Mapping (`devices`):**
   - Read unverified serial listings. If an asset tag is present, sanitize and normalize the text entry.
   - If asset tag and serial number are entirely blank, **do not generate dummy data**. Create a null-placeholder association row.

---

## 4. Source-to-Target Column Field Mapping

```text
LEGACY SOURCE FILE            TRANSFORMATION ENGINE             RELATIONAL TARGET DB FIELD
===================           =====================             ==========================
stations.json -> stationId -> Direct Stable Injection ------->  stations.id (PK)
stations.json -> facility  -> Map to unique string table ---->  facilities.id (FK)
stations.json -> status    -> Validate lookup constraints --->  stations.status
devices.json  -> MAC       -> Strip punctuation, uppercase ->  devices.phone_mac
devices.json  -> serialNo  -> Check uniqueness pool --------->  devices.serial_number
```

---

## 5. Pre- and Post-Migration Data Validation Logs
To guarantee zero data loss during cutover execution, the extraction script must execute and log the following explicit balance checks:

- **Row Reconciliation Balance:** Total input source records must exactly balance target rows (`Total Input Stations (129) == Total Relational Target Rows Added`).
- **Orphan Constraints Validation:** Flag any asset record whose `stationId` reference does not link back to a parent station row. Do not force matching; log instances to an exception file for auditor manual cleanup.
- **Null Safety Check:** Verify that fields defined as `NOT NULL` (such as `station_name` or `device_type`) do not contain empty string conversions. If found, apply structural fallback defaults (`"Unnamed Target Room"`) and flag for physical validation.
