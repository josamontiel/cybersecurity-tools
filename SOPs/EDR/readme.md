# Microsoft Defender for Endpoint (MDE) | SOP Documentation Library

## 1. Overview
This repository contains the Standard Operating Procedures (SOPs) for managing the lifecycle of assets within Microsoft Defender for Endpoint. These documents are designed to ensure consistent security posture, proactive threat hunting, and operational hygiene across Windows, macOS, and mobile platforms.

---

## 2. Documentation Directory

| Document Title | Purpose | Target Audience |
| :--- | :--- | :--- |
| **[Onboarding SOP](SOPs/EDR/MDE/MDE-onboarding.md)** | Step-by-step guide for enrolling new Laptops (Win/Mac) and Mobile (iOS/Android) devices. | IT Support / Desktop Eng |
| **[Asset Health Monitoring SOP](SOPs/EDR/MDE/health-monitoring.md)** | Procedures for identifying silent sensors, fixing communication gaps, and KQL heartbeat monitoring. | SOC / Security Admin |
| **[Custom Detection Rules SOP](SOPs/EDR/MDE/detection-rule-creation.md)** | Framework for writing KQL queries and converting them into automated security alerts. | SOC / Threat Hunters |
| **[Offboarding SOP](SOPs/EDR/MDE/mde-offboarding.md)** | Secure decommissioning of assets to prevent stale data and reclaim licenses. | IT Ops / HR Exit Team |

---

## 3. The Asset Lifecycle Flow

1.  **Onboarding:** Device is registered via Intune/MDM; MDE sensor is activated.
2.  **Monitoring:** Weekly health checks ensure the sensor is "Active" and signatures are up to date.
3.  **Detection:** Custom KQL rules run 24/7 to catch behaviors specific to our environment.
4.  **Offboarding:** Upon hardware retirement or employee exit, the device is gracefully removed.

---

## 4. Key Global Contacts
* **Primary Administrator:** [Insert Name/Email]
* **Security Operations Center:** [Insert DL/Teams Channel]
* **Escalation Path:** Microsoft Premier Support (via Admin Center)

---

## 5. Quick Links & Tools
* [Microsoft Defender Security Portal](https://security.microsoft.com)
* [MDE Client Analyzer Tool](https://aka.ms/mdatpanalyzer)
* [Kusto Query Language (KQL) Documentation](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)

---
**Last Updated:** March 2026  
**Status:** Version 1.0 (Production)
