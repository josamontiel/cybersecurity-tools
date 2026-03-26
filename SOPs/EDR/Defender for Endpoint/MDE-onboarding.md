# SOP: User Onboarding for Microsoft Defender for Endpoint (MDE)

## 1. Purpose and Scope
The purpose of this Standard Operating Procedure (SOP) is to provide a structured framework for enrolling and securing corporate-managed and BYOD assets—including Windows, macOS, iOS, and Android devices—into **Microsoft Defender for Endpoint**.

---

## 2. Prerequisites
Before beginning the onboarding sequence, ensure the following requirements are met:

### 2.1 Licensing & Access
* **Licenses:** Valid M365 E3/E5, Windows 10/11 Enterprise E3/E5, or Microsoft Business Premium.
* **Permissions:** Global Administrator or Security Administrator access to the [Microsoft Defender Portal](https://security.microsoft.com).
* **MDM Access:** Administrative access to **Microsoft Intune** (Microsoft Endpoint Manager).

### 2.2 Network Requirements
* Ensure devices have outbound access to MDE service URLs via port **443**.
* Verify that proxy or firewall settings do not inspect/terminate the SSL connection for Defender endpoints.

---

## 3. Onboarding Laptops (Windows & macOS)

### 3.1 Windows 10 & 11 (via Microsoft Intune)
1. **Navigate:** Open the [Microsoft Intune admin center](https://intune.microsoft.com).
2. **Path:** Go to **Endpoint security** > **Endpoint detection and response**.
3. **Create Policy:** Select **Create Policy**, set Platform to *Windows 10, 11, and Server*, and Profile to *Endpoint detection and response*.
4. **Configuration:**
    * Set **Microsoft Defender for Endpoint client configuration package type** to **Onboard**.
    * Enable **Sample Sharing** for all files.
5. **Assignments:** Assign the policy to the "All Laptops" or specific pilot device groups.

### 3.2 macOS Devices
1. **Download Package:** In the [Defender Portal](https://security.microsoft.com), go to **Settings** > **Endpoints** > **Onboarding**. Select **macOS** and download the Onboarding Package.
2. **Deployment:** Use Intune to push the `.pkg` installer.
3. **Configuration Profiles:** Deploy the following profiles via Intune/MDM to prevent user prompts:
    * **System Extensions**
    * **Full Disk Access**
    * **Network Filter**
4. **Verification:** Open Terminal on the Mac and run: `mdatp health`. Ensure `licensed` and `healthy` status are true.

---

## 4. Onboarding Mobile Devices (iOS & Android)

### 4.1 Android (Enterprise/Work Profile)
1. **App Deployment:** Add the **Microsoft Defender** app from the Managed Google Play Store in Intune.
2. **Configuration:** Create an **App configuration policy** for managed devices. 
    * Set `low_priority_configuration` to `false` to ensure high protection.
3. **User Action:** The user opens the Defender app, signs in, and accepts the requested permissions (VPN and Storage).

### 4.2 iOS/iPadOS
1. **App Deployment:** Add **Microsoft Defender** from the iOS App Store to Intune.
2. **Zero-Touch Setup:** Create a **Device Configuration** profile using the "VPN" template to automatically configure the loopback VPN for Web Protection.
3. **User Action:** The user launches the app and signs in with their corporate email. No further configuration is required if Zero-touch is active.

---

## 5. Verification and Validation
After deployment, confirm the security posture of the devices:

| Step | Action | Expected Result |
| :--- | :--- | :--- |
| **1** | Device Inventory | Device appears in **Microsoft Defender Portal** > **Assets** > **Devices**. |
| **2** | Sensor Health | Health state is listed as **Active**. |
| **3** | Detection Test | Run the [EICAR test file](https://www.eicar.org/?page_id=3950) or Microsoft’s DIY script to trigger a test alert. |

---

## 6. Troubleshooting
* **Sensor Latency:** It may take up to 20 minutes for a newly onboarded device to appear in the portal.
* **Co-existence:** If a third-party antivirus is active, Defender may enter **Passive Mode**. Ensure the previous AV is uninstalled to enable Active protection.
* **Connectivity Issues:** Use the [Client Analyzer tool](https://aka.ms/mdatpanalyzer) to diagnose communication gaps between the device and MDE cloud.

---
**Last Updated:** March 2026  
**Document Owner:** IT Security Department
