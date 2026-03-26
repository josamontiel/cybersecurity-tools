# SOP: Asset Health Monitoring for Microsoft Defender for Endpoint (MDE)

## 1. Purpose and Scope
The goal of this SOP is to ensure the continuous health and operational status of the Defender for Endpoint sensor across all managed assets. This procedure outlines how to identify "silent" devices, resolve communication errors, and maintain high visibility within the security console.

---

## 2. Health Monitoring Frequency
* **Daily:** Review the "Device Health" dashboard for critical connectivity alerts.
* **Weekly:** Perform a deep dive into "Inactive" or "Can't be reached" devices.
* **Monthly:** Audit the "Configuration Discovery" and "Vulnerability Management" status for all OS versions.

---

## 3. Monitoring Procedures

### 3.1 Reviewing the Device Health Dashboard
1.  Log in to the [Microsoft Defender Portal](https://security.microsoft.com).
2.  Navigate to **Assets** > **Devices**.
3.  Apply the **Sensor Health State** filter to identify devices in the following states:
    * **Active:** Normal operations.
    * **Inactive:** Device has not communicated with the service for more than 7 days.
    * **Misconfigured:** Sensor is active but may have communication or policy issues.
    * **No Sensor Data:** Device is known to Intune/AD but the MDE sensor is not reporting.

### 3.2 Advanced Hunting: Identifying "Silent" Assets
Use the following KQL query in **Hunting > Advanced hunting** to find devices that have not reported a heartbeat in the last 7 days. This is more precise than the dashboard for identifying decommissioned or broken sensors.

```kusto
// Query to find devices that haven't reported in the last 7 days
DeviceInfo
| summarize LastSeen = max(Timestamp) by DeviceName, DeviceId, OSPlatform, JoinType
| where LastSeen < ago(7d)
| extend DaysSilent = datetime_diff('day', now(), LastSeen)
| order by DaysSilent desc
```

---

## 4. Remediation Steps

### 4.1 Resolving Inactive Devices
If a device is inactive but still in use by an employee:
1.  **Restart the Sensor:** On the local machine, ensure the `SENSE` service (Windows) or `mdatp` (macOS/Linux) is running.
2.  **Verify Connectivity:** Run the [MDE Client Analyzer](https://aka.ms/mdatpanalyzer) on the affected machine to check for blocked URLs or proxy issues.
3.  **Check Certificates:** Ensure the device has the required root certificates for TLS inspection/communication.

### 4.2 Handling Antivirus Health Issues
1.  Check the **Antivirus status** column in the Device Inventory.
2.  If **"Disabled"** or **"Not up to date"**:
    * Trigger a **Signature Update** remotely from the Device Action menu.
    * Run a **Quick Scan** to force a heartbeat update.

---

## 5. Automation of Health Alerts
To avoid manual checking, set up an **Action Center** notification:
1.  Go to **Settings** > **Endpoints** > **General** > **Alert notifications**.
2.  Create a notification rule for **"Sensor health"** issues.
3.  Set the recipient to the IT Security/SOC email alias.

---

## 6. Metrics & Reporting (KPIs)
The health of the asset fleet is measured by:
* **Sensor Coverage:** (Number of Onboarded Devices / Total Managed Assets) x 100. *Target: >98%*
* **Mean Time to Reconnect (MTTR):** Time taken to return an "Inactive" device to "Active" status. *Target: <48 hours*
* **Signature Freshness:** Percentage of devices with signatures less than 48 hours old. *Target: >95%*

---

## 7. Troubleshooting Resources
* **Client Analyzer Tool:** Use to generate a detailed HTML report of local sensor health.
* **Event Viewer:** Check `Applications and Services Logs` > `Microsoft` > `Windows` > `SENSE`.

---
**Last Updated:** March 2026  
**Document Owner:** Endpoint Engineering Team
