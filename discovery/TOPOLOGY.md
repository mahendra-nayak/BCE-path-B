STAGE-1-DRAFT: INTAKE-DERIVED — 2026-04-22 — Path B Stranger-Led

# TOPOLOGY.md — VGM Data Platform

---

## A01 — Layer Boundary Map

> Source: INTAKE_SUMMARY.md — Known Architecture
> Note: Boundaries are implied from high-level component descriptions only.
> No custodian validation. Confidence: LOW.

---

### Boundary LB-01 — External Sources → Ingestion Layer

- **Produced by:** External systems: Flespi, Twilio, Callbell, Element SFTP, internal VGM systems (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Raw event/record data (type, format, schema — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** Cloud Functions / ETL pipeline entry points (module and function — NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** NOT DETERMINABLE FROM SOURCE (could be API call, SFTP pull, message queue — requires source code)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-02 — Ingestion Layer → BigQuery Staging (STG)

- **Produced by:** Cloud Functions / ETL pipelines (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Staged records in BigQuery STG datasets (schema — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** Transformation layer / Operational SQL execution (module and function — NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** Database (BigQuery write)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-03 — BigQuery Staging (STG) → BigQuery Operational (OP)

- **Produced by:** ETL transformation / Operational SQL execution layer (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Transformed records in BigQuery OP datasets (schema — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** REST API layer, Power BI dashboards, Gen-AI chatbot (module and function — NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** Database (BigQuery read/write)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-04 — BigQuery OP → REST API (BigQuery API Service)

- **Produced by:** BigQuery OP datasets (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Query results (format — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** API consumers — internal teams, clients (NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** Direct call (HTTP REST API)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-05 — BigQuery OP → Power BI Dashboards

- **Produced by:** BigQuery OP datasets (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Report data (schema — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** Power BI (consumer module — NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** NOT DETERMINABLE FROM SOURCE (direct connector or export — requires source code)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-06 — BigQuery OP / Vertex AI → Gen-AI Chatbot

- **Produced by:** BigQuery OP datasets and Vertex AI (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** LLM-generated responses to natural language queries (format — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** End users via chatbot interface (NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** NOT DETERMINABLE FROM SOURCE (API call — requires source code)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-07 — Secret Manager → All Layers

- **Produced by:** Google Cloud Secret Manager
- **Artifact:** Secrets / credentials (format — NOT DETERMINABLE FROM SOURCE)
- **Consumed by:** All pipeline modules and API services (module and function — NOT DETERMINABLE FROM SOURCE)
- **Handoff mechanism:** Direct call (GCP Secret Manager API)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### Boundary LB-08 — GitLab CI/CD → Cloud Deployment

- **Produced by:** GitLab pipelines / Deployment Manager (module and function — NOT DETERMINABLE FROM SOURCE)
- **Artifact:** Deployed Cloud Functions, infrastructure resources
- **Consumed by:** GCP project runtime environment
- **Handoff mechanism:** NOT DETERMINABLE FROM SOURCE (gcloud deploy / Deployment Manager — requires source code)
- **Failure mode:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

## A02 — Module Call Map

A02 — DEFERRED: Module Call Map requires source code inspection. Produced in Stage 2.

---

## A03 — External System Boundary Map

> Source: INTAKE_SUMMARY.md — Known Architecture / System Purpose
> Note: Only systems explicitly named in intake are listed.

---

### ES-01 — Flespi

- **Type:** Telematics platform (vehicle tracking / fleet management)
- **Direction:** Inbound — VGM consumes data from Flespi
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Notes:** Described as a primary telematics data source; hourly incremental ingestion mentioned in README

---

### ES-02 — Twilio

- **Type:** Communication platform (SMS / telephony)
- **Direction:** Inbound — VGM consumes call/SMS data
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-03 — Callbell

- **Type:** Customer engagement / chat platform
- **Direction:** Inbound — VGM consumes engagement data
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-04 — Element (SFTP)

- **Type:** File feed via SFTP
- **Direction:** Inbound — VGM pulls files
- **Protocol / format:** SFTP (file format — NOT DETERMINABLE FROM SOURCE)
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-05 — Internal VGM Source Systems

- **Type:** Internal operational system(s)
- **Direction:** Inbound — VGM consumes internal source data
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Notes:** Referenced as "internal VGM systems" — boundaries not further described in intake

---

### ES-06 — Google BigQuery

- **Type:** Cloud data warehouse (GCP managed service)
- **Direction:** Bidirectional — ingestion writes; API and dashboards read
- **Protocol / format:** BigQuery API / SQL
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-07 — Google Vertex AI

- **Type:** Managed AI/ML platform (GCP)
- **Direction:** Inbound to chatbot — VGM queries Vertex AI for LLM responses
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-08 — Google Cloud Secret Manager

- **Type:** Secrets management service (GCP)
- **Direction:** Inbound — VGM reads secrets at runtime
- **Protocol / format:** GCP Secret Manager API
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-09 — Google Cloud Storage

- **Type:** Object storage (GCP)
- **Direction:** Bidirectional — used for intermediate file storage and pipeline assets
- **Protocol / format:** GCS API
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-10 — Power BI

- **Type:** Business intelligence / reporting tool (Microsoft)
- **Direction:** Outbound — VGM serves data to Power BI dashboards
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)

---

### ES-11 — GitLab CI/CD

- **Type:** CI/CD platform
- **Direction:** Outbound control plane — triggers deployments to GCP
- **Protocol / format:** GitLab pipelines / YAML
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (requires source code)
- **Error handling:** NOT DETERMINABLE FROM SOURCE (requires source code)
