STAGE-1-DRAFT: INTAKE-DERIVED — 2026-04-22 — Path B Stranger-Led

# MODULE_CONTRACTS.md — VGM Data Platform

> T4 skeletons derived from INTAKE_SUMMARY.md Known Architecture only.
> No source code has been read. All implementation-level fields are NOT DETERMINABLE FROM SOURCE.
> Stage 2 will complete these skeletons from source code inspection.

---

## S1-01_vgm_telematics_bq

- **Module:** vgm_telematics_bq
- **Layer:** Data Ingestion
- **Primary Responsibility:** Ingest telematics data from Flespi into BigQuery (hourly incremental)
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Flespi (telematics platform), Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete — module identified from intake architecture description.

---

## S1-02_vgm_flespi_bq

- **Module:** vgm_flespi_bq
- **Layer:** Data Ingestion / Staging
- **Primary Responsibility:** Flespi staging and operational data processing (STG and OP datasets)
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Flespi, Google BigQuery, Google Cloud Storage
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-03_vgm_twilio_bq

- **Module:** vgm_twilio_bq
- **Layer:** Data Ingestion
- **Primary Responsibility:** Ingest Twilio call and SMS communication data into BigQuery
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Twilio, Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-04_vgm_callbell_bq

- **Module:** vgm_callbell_bq
- **Layer:** Data Ingestion
- **Primary Responsibility:** Ingest Callbell customer engagement platform data into BigQuery
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Callbell, Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-05_vgm_element_sftp_bq

- **Module:** vgm_element_sftp_bq
- **Layer:** Data Ingestion
- **Primary Responsibility:** Pull files from Element via SFTP and load into BigQuery
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Element (SFTP), Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-06_vgm_source_bq

- **Module:** vgm_source_bq
- **Layer:** Data Ingestion
- **Primary Responsibility:** Ingest data from internal VGM source systems into BigQuery; supports minute-level incremental capture
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Internal VGM systems, Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-07_vgm_op_sql_execution

- **Module:** vgm_op_sql_execution
- **Layer:** Data Transformation
- **Primary Responsibility:** Execute scheduled operational SQL queries to transform STG data into OP datasets
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Google BigQuery, Google Cloud Storage
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-08_vgm_bigquery_api

- **Module:** vgm_bigquery_api
- **Layer:** Data Serving / API
- **Primary Responsibility:** FastAPI REST service exposing BigQuery query capability to consumers with API key authentication
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** HTTP REST endpoints — `/health`, `/help`, `/metadata`, `/query` (implementation details NOT DETERMINABLE FROM SOURCE)
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-09_vgm_chatbot_gen_ai

- **Module:** vgm_chatbot_gen_ai
- **Layer:** Data Serving / AI Interface
- **Primary Responsibility:** LangChain + Vertex AI conversational chatbot supporting natural language querying; includes WhatsApp integration
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Google Vertex AI, Google BigQuery, Google Cloud Secret Manager
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** HTTP REST endpoints — `/health`, `/chat`, `/whatsapp` (implementation details NOT DETERMINABLE FROM SOURCE)
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-10_vgm_powerbi_migration

- **Module:** vgm_powerbi_migration
- **Layer:** Data Serving / Reporting
- **Primary Responsibility:** Automated Power BI report migration between tenants using GCS as intermediate storage
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Power BI (Microsoft), Google Cloud Storage
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.

---

## S1-11_devops_resources

- **Module:** devops_resources
- **Layer:** Infrastructure / DevOps
- **Primary Responsibility:** Infrastructure-as-Code templates for GCP resource provisioning (BigQuery datasets, Cloud Functions, Pub/Sub, Cloud Storage, VPC, VMs) via Deployment Manager and GitLab pipelines
- **Inputs:** NOT DETERMINABLE FROM SOURCE
- **Outputs:** NOT DETERMINABLE FROM SOURCE
- **External Dependencies:** Google Cloud Deployment Manager, GitLab CI/CD, GCP APIs
- **Internal Dependencies:** NOT DETERMINABLE FROM SOURCE
- **Public Interface:** NOT DETERMINABLE FROM SOURCE
- **Error Behaviour:** NOT DETERMINABLE FROM SOURCE
- **Known Fragility:** NOT DETERMINABLE FROM SOURCE
- **Change Impact:** NOT DETERMINABLE FROM SOURCE

> Stage 2 will complete.
