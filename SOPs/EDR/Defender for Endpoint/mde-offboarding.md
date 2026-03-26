# SOP: Asset Offboarding for Microsoft Defender for Endpoint (MDE)

## 1. Purpose and Scope
This Standard Operating Procedure (SOP) ensures that devices are formally and securely removed from the Microsoft Defender for Endpoint environment. This prevents stale data from impacting security posture scores and ensures compliance with licensing limits.

---

## 2. Retention and Data Privacy
* **Data Retention:** By default, Defender for Endpoint retains device data for **180 days** after offboarding for forensic purposes.
* **Alerts:** Any existing alerts associated with the device will remain in the portal until the retention period expires.

---

## 3. Offboarding Procedures by OS

### 3.1 Windows 10 & 11 (via Microsoft Intune)
1.  Navigate to the [Microsoft Intune admin center](https://intune.microsoft.com).
2.  Go to **Endpoint security** > **Endpoint detection and response**.
3.  Create a new **Configuration Profile** or edit an existing offboarding policy.
4.  Set the **Microsoft Defender for Endpoint client configuration package type** to **Offboard**.
5.  **Important:** Assign this policy to a specific "Offboarding" group. Move the target device into this group.
6.  Once the device syncs, the SENSE service will stop and the device will no longer report data.

### 3.2 macOS Offboarding
1.  In the [Defender Portal](https://security.microsoft.com), go to **Settings** > **Endpoints** > **Offboarding**.
2.  Select **macOS** as the operating system and download the **Offboarding package**.
3.  The package contains `Python` scripts or `.mobileconfig` files.
4.  Deploy the offboarding script via your MDM (Intune/Jamf) or run it locally on the machine with sudo privileges.
5.  **Command:** `sudo path/to/offboarding_script.sh`

### 3.3 Mobile Devices (iOS & Android)
1.  **Retire/Wipe:** In Microsoft Intune, locate the device and select **Retire** (removes corporate data/management) or **Wipe** (factory reset).
2.  **Manual Removal:** If the device is BYOD and not fully managed, the user must uninstall the Microsoft Defender app.
3.  **Cleanup:** Once the MDM management profile is removed, the device will stop communicating with the MDE backend.

---

## 4. Portal Cleanup (Administrative)

### 4.1 Manual Status Update
If a device was physically destroyed or lost and cannot run an offboarding script:
1.  Go to **Assets** > **Devices**.
2.  Select the device(s).
3.  Click the **Exclude device** button. (Excluded devices do not count toward your security score or appear in active reports).

### 4.2 Checking for Successful Offboarding
1.  Verify the **Sensor health state** in the Device Inventory.
2.  An offboarded device will eventually move to **Inactive** and then disappear from the active "All Devices" view based on your portal filters.

---

## 5. Security Checklist
* [ ] Ensure all local "Detection and Response" (EDR) capabilities are disabled on the asset.
* [ ] Verify the device has been removed from any "Automated Investigation" groups.
* [ ] Reclaim the Microsoft 365 E5/Business Premium license if the user is leaving the organization.

---
**Last Updated:** March 2026  
**Document Owner:** IT Operations / Security Admin
