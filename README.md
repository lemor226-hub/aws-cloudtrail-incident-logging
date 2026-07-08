# Centralized Cloud Auditing & Incident Response Logging Pipeline

## 📌 Project Overview
This project establishes an automated cloud logging and forensic data transformation pipeline designed to maintain audit readiness and operational visibility within an enterprise AWS environment. By capturing raw cloud API data, parsing nested identity indicators, and structuring metrics into a dedicated security triage matrix, this framework directly fulfills continuous monitoring and incident documentation requirements.

## 🛡️ Control Objectives & GRC Alignment
This architecture is engineered to align directly with core industry compliance guardrails:
* **NIST CSF V2.0:** Governance (GV.OC) and Oversight, ensuring all management events are logged for operational accountability.
* **SOC 2 Type II (Trust Services Criteria):** CC6.1 & CC7.2 (Access Control and Cyber Threat Detection), establishing the security baseline required to identify and track unauthorized configuration attempts.
* **ISO/IEC 27001:2022:** Control A.8.12 (Data Leakage Prevention) and Control A.8.16 (Monitoring Activities).

## 🛠️ Tech Stack & Tools Used
* **Cloud Auditing Environment:** AWS CloudTrail, Amazon S3 (SSE-S3 Encrypted Log Storage Vault)
* **Threat Actor Profiling:** AWS IAM (Isolated Identity Simulations)
* **Data Parsing & Pipeline Engineering:** Microsoft Excel Power Query, Advanced M-Code JSON Transformations

---

## 📈 Pipeline Architecture & Step-by-Step Execution

### Phase 1: Cryptographic Event Auditing
Continuous log recording was initiated via a multi-region AWS CloudTrail configuration. Management events tracking read/write API requests were structured to stream in near real-time directly into a secure, access-controlled Amazon S3 bucket utilizing standard server-side encryption guardrails.

### Phase 2: Threat Actor Vector Simulation
To validate logging fidelity, a simulated corporate breach event was triggered utilizing a restricted testing identity (`intern-alex`). Multiple consecutive API calls were executed targeting a core data asset (`Prod-Core-Database`) to simulate a persistent attacker attempting to bypass structural security boundaries. 

### Phase 3: Raw Forensic Ingestion
Following the simulated threat vector execution, the raw cryptographic audit trail events were extracted directly from the system logs. A breakdown of the raw forensic telemetry footprint captured:
* **Event Target:** `ec2:StopInstances`
* **Enforced Outcome:** Access Denied (API Security Error: `Client.UnauthorizedOperation`)
* **Vulnerability Remediation Status:** Confirmed. The custom structural IAM policy successfully blocked the malicious execution payload.

### Phase 4: ETL Transformation Pipeline (Power Query)
Rather than executing manual text compilation, the unformatted, heavily nested JSON logging blocks were programmatically ingested into an ETL transformation pipeline leveraging Power Query. 
1.  **JSON Schema Unpacking:** The multi-layered array roots were flattened into standardized tabular rows.
2.  **Relational Identity Drilling:** Nested record subsets within `userIdentity` and `resources` were programmatically extracted to isolate identity names (`userName`) and asset resource paths (`ARN`) without dropping record fidelity.
3.  **Data Typing Normalization:** Log timestamps (`eventTime`) were transformed from standard Z-string format into clean local Date/Time stamps to enable rapid sequential time-series indexing during a live forensic investigation.

---

## 📊 Incident Triage Matrix Template

The resulting output presents a clean, operational dashboard optimized for executive reporting and GRC compliance review:

| Timestamp | Assumed Identity | API Action Attempted | Target Asset ID / ARN | Outcome Status | Source IP Address |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 7/7/2026 15:19| intern-alex | ec2:StopInstances | arn:aws:ec2:us-east-2:756418066268:instance/i-02dcde28da1123f7d | Access Denied (Policy Enforced) | 38.32.143.170 |
