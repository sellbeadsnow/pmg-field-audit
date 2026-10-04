# PMG Field Audit: Clinical Auditor Field Guide

## 1. Introduction & Core Mission
The PMG Field Audit application is a mobile-first tool designed to accurately map physical IT assets to clinical locations across Prestige Medical Group facilities. This data ensures compliance, assists with hardware replacement cycles, and keeps software dependencies accurate.

As an Auditor, you are the eyes on the ground. Your mission is to ensure that what exists on your screen matches the physical equipment sitting on desks, mounted to walls, or stored in carts.

---

## 2. Preparing for a Field Walk
Before you head into a clinic area, perform this quick preparation checklist to ensure a smooth audit:

- **Check Your Device:** Ensure your iPad or phone has at least 50% battery life. 
- **Network Validation:** Verify that your cellular connection or internal PMG Wi-Fi is active.
- **Clean State Check:** Log in to the application and ensure your active location filters match your assigned physical route for the day (e.g., Canton Clinic -> 3rd Floor).

---

## 3. Step-by-Step Audit Workflow

### Step 3.1: Locate the Station
1. Walk to the physical workstation area.
2. Look at the primary name listed at the top of your app interface (e.g., "Canton, Check In, 3rd Floor"). 
3. *Note:* Do not look for the technical alphanumeric Station ID; focus on the descriptive human heading.

### Step 3.2: Check What Needs Attention
Before opening any details, look at the summary view. The system will highlight missing categories or discrepancies immediately under the "What needs attention" card panel.

### Step 3.3: Verify Devices (The Color System)
Look at the device checklist cards. They use a strict stoplight color system:
- **Green (Complete):** The asset tag and serial number are verified and correct. No action needed.
- **Amber (Needs Data):** A device is physically there, but critical metadata (like a phone extension or IP address) is blank. Tap the card to update.
- **Red (Missing/Unidentified):** An expected device was not found or has never been audited.

### Step 3.4: Resolve Device Discrepancies
- **If a device matches:** Tap the card, cross-check the physical asset tag label, and select **Confirm**.
- **If a device is missing entirely:** Do not invent placeholder tags. Update the device status dropdown to `Not Found` or `Moved`.
- **If a device is new/unlisted:** Tap **Add Device**, enter the verified physical serial number and asset tag, and assign it to the station.

### Step 3.5: Review, Save & Advance
Once all cards turn green or have logged exceptions, tap **Save & Next**. The app will automatically sync your updates with the database and seamlessly load the next station in your active clinic route.

---

## 4. Handling Field Edge Cases

| Scenario | Action to Take |
| :--- | :--- |
| **Asset Tag is missing/scratched off** | Leave the asset tag blank. Do not invent an ID. Enter the physical serial number and leave an audit note. |
| **Station is physically locked/inaccessible** | Select the `Flag Issue` control at the top header. Mark severity as `Medium` and write a note: "Room locked, keys unavailable." Skip to next. |
| **"Save Conflict" Alert pops up** | This means another auditor updated this station while you were looking at it. Review the changes on screen, select the correct physical data, and re-save. |
| **Network connection drops mid-walk** | Continue auditing normally. The UI will display a "Queued Offline" sync badge. Do not clear your browser cache or log out until you return to a connected zone and the badge clears. |
