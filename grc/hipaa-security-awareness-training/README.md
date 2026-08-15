# HIPAA Security Awareness & Compliance Training Project

![Domain](https://img.shields.io/badge/Domain-GRC%20%26%20Healthcare%20Compliance-blue)
![Focus](https://img.shields.io/badge/Focus-Security%20Awareness%20%26%20Training-green)
![Scope](https://img.shields.io/badge/Data%20Scope-PII%20%7C%20PHI%20%7C%20ePHI-orange)
![Standard](https://img.shields.io/badge/Standard-45%20CFR%20Part%20164-purple)

## Project Overview

This project delivers a **HIPAA Security & Privacy Awareness Training Package** designed to educate healthcare personnel on federal regulatory mandates, data handling standards, and safeguard procedures. 

Developed as part of coursework for UMGC (CMIT 320), the project translates complex federal regulations—specifically the **HIPAA Privacy and Security Rules (45 CFR Parts 160 and 164)**—into actionable, workforce-ready operational guidance.

---

## Key Skills & Competencies Demonstrated

* **Governance, Risk & Compliance (GRC):** Mapping federal regulatory requirements to internal operational controls.
* **Data Classification & Handling:** Defining boundary metrics between PII, PHI, and ePHI.
* **Safeguard Control Analysis:** Categorizing Administrative, Physical, and Technical controls under 45 CFR § 164.
* **Security Awareness Education:** Communicating technical cybersecurity principles to non-technical staff.
* **Policy Research & Documentation:** Interpreting federal code and structuring compliance deliverables.

---

## Project Deliverables

1. **Executive Presentation Deck:** A 10-slide compliance training deck tailored for hospital staff and clinical personnel.
2. **Speaker Narration Script:** Complete voiceover scripts providing context and detailed instruction for each module.
3. **Regulatory Reference Guide:** Cites authoritative sources, including HHS guidance and eCFR standards.

---

## Technical & Regulatory Breakdown

### 1. Data Classification Matrix

| Data Type | Scope & Definition | Examples | Primary Safeguard Standard |
| :--- | :--- | :--- | :--- |
| **PII** | Identifies or traces an individual's identity. | SSN, Driver's License #, personal address. | DOL Privacy Guidelines |
| **PHI** | Identifiable data linked to health status, care provision, or payment. | Medical Record Numbers (MRN), lab results, billing history. | HIPAA Privacy Rule |
| **ePHI** | Any PHI created, stored, maintained, or transmitted digitally. | EHR database files, encrypted emails containing patient info, PACS images. | HIPAA Security Rule (Technical Controls) |

### 2. The Three Safeguard Pillars (45 CFR § 164)

* **Administrative Safeguards (§ 164.308):** Formal risk assessments, Role-Based Access Control (RBAC), least privilege enforcement, and mandatory annual workforce recertifications.
* **Physical Safeguards (§ 164.310):** Facility access controls, biometric/keycard perimeters, workstation screen timeout locks, and privacy filters.
* **Technical Safeguards (§ 164.312):** Multi-Factor Authentication (MFA), Unique User Identifiers (UID), audit log integrity, and AES-256 / TLS 1.3 encryption for data at rest and in transit.

### 3. Disclosure Logic & Exceptions


```
+------------------------------------------------------------------+
|                       Data Release Request                       |
+------------------------------------------------------------------+
/---------Is there written authorization from the patient?---------/                   

YES                      NO                       

[Process Authorized Release]
/----------Does request fall under TPO or legal mandates?----------/

YES            NO

[Apply Minimum Necessary]  [DENY RELEASE]  [Permissible Release]
```


## References & Authoritative Sources

* **U.S. Department of Health & Human Services (HHS):** [Summary of the HIPAA Security Rule](https://www.hhs.gov/hipaa/for-professionals/security/index.html)
* **Electronic Code of Federal Regulations (eCFR):** [Title 45, Subtitle A, Subchapter C, Part 164](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164)
* **U.S. Department of Labor (DOL):** [Guidance on Protection of Personally Identifiable Information (PII)](https://www.dol.gov/general/ppii)
