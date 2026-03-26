# SOP: Creating Custom Detection Rules in Microsoft Defender for Endpoint

## 1. Purpose and Scope
This Standard Operating Procedure (SOP) defines the workflow for developing, validating, and deploying custom detection rules. Custom detections allow the security team to proactively identify specific behaviors, persistence mechanisms, or known indicators of compromise (IoCs) unique to the organization's threat landscape.

---

## 2. Prerequisites
* **Permissions:** Must have 'Manage Security Settings' or 'Advanced Hunting' permissions in the [Microsoft Defender Portal](https://security.microsoft.com).
* **Language:** Proficiency in **Kusto Query Language (KQL)**.
* **Data Latency:** Acknowledge that Advanced Hunting data typically has a 1–15 minute delay from the event occurrence.

---

## 3. Workflow: Rule Creation

### Step 1: Query Development
1. Navigate to **Hunting** > **Advanced hunting** in the Defender portal.
2. Draft a KQL query to identify the specific malicious activity.
3. **Requirement:** The query must include the following columns to trigger a detection:
    * `Timestamp`
    * `DeviceId`
    * `ReportId`
   
> **Example Query (Detecting Unsigned PowerShell Scripts):**
> ```kusto
> DeviceProcessEvents
> | where FileName =~ "powershell.exe"
> | where IsRooted == false
> | where ProcessCommandLine has "-ExecutionPolicy Bypass"
> | project Timestamp, DeviceId, DeviceName, ReportId, ProcessCommandLine
> ```

### Step 2: Query Validation
1. Run the query over the last **24 hours** or **7 days** to check for false positives.
2. Refine the logic (using `where` filters) until the results only represent actionable security concerns.

### Step 3: Create Detection Rule
1. Once the query is finalized, click **Create detection rule** at the top of the query editor.
2. **Alert Details:**
   * **Detection Name:** Provide a clear, unique name (e.g., *Suspicious PowerShell Execution*).
   * **Frequency:** Select how often the query runs (e.g., Every 1 hour, Every 24 hours).
   * **Alert Title:** The name that will appear in the Incidents queue.
   * **Severity:** Informational, Low, Medium, or High.
   * **Category:** Align with MITRE ATT&CK (e.g., Persistence, Execution).

### Step 4: Choose Impacted Entities
1. Map the query columns to the entities Defender tracks:
   * **Devices:** Map to `DeviceId`.
   * **Users:** Map to `AccountName` or `AccountSid` (if applicable).

### Step 5: Define Actions
Select automated actions to take when a match is found:
* **Devices:** Isolate device, Run antivirus scan, or Collect investigation package.
* **Files:** Move to quarantine.
* **Users:** Mark user as compromised (requires Entra ID integration).

---

## 4. Maintenance and Review
* **False Positive Reduction:** Review alerts weekly. If a rule triggers too many false positives, refine the KQL logic using `| where AccountName !~ "service_account_name"`.
* **Rule Audit:** Conduct a quarterly review of all custom detections to ensure they are still relevant and not redundant with Microsoft’s built-in detections.

---

## 5. Performance Constraints
* **Query Limits:** Custom detection queries are limited to a 10-minute execution time. If the query exceeds this, it will be disabled.
* **Resource Quota:** Organizations are limited to **500 custom detection rules**.

---
**Last Updated:** March 2026  
**Document Owner:** SOC Manager / Security Engineering
