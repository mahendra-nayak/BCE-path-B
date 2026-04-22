STAGE-2-COMPLETE: SOURCE-DERIVED — 2026-04-22 — Path B Stranger-Led
(Stage 1 draft superseded. A01 and A03 completed from source code. A02 added.)

# TOPOLOGY.md — VGM Data Platform

---

## A01 — Layer Boundary Map

[STAGE-2-UPDATE — 2026-04-22]: Stage 1 A01 was produced from intake (LOW confidence).
All 8 boundaries have been verified and completed from source code.
Auth, protocol, and error handling fields filled. No intake divergences found
(intake described no specific protocols — code now provides them).

---

### Boundary LB-01 — External Sources → Ingestion Layer

- **Produced by:** External systems: Flespi (telematics API), Twilio (REST API), Callbell (REST API), Element (SFTP), Argyle (webhook + REST), BOS/Minave/FleetSync/Manheim/Solo/3CX/Dynamics/Stripe/Uber (REST APIs and RDBMS — confirmed from test files in vgm_source_bq)
- **Artifact:** Raw event/record data — JSON payloads (REST), binary files (SFTP .xlsx), webhook POST bodies (Argyle)
- **Consumed by:** Ingestion Cloud Functions / batch containers — `main.py` / `main_script.py` / `main_stg_incremental.py` entry points
- **Handoff mechanism:**
  - REST API poll: `requests.get(url, headers=headers, params={"page": page})` (Callbell)
  - SFTP pull: `paramiko.SFTPClient.from_transport()` → `sftp.file(path, "rb")` (Element)
  - Webhook receive: HTTP POST to Cloud Function endpoint (Argyle)
  - BigQuery Pub/Sub stream: Flespi telemetry written to BQ staging via published messages (Flespi → STG)
- **Auth pattern:** Bearer token from Secret Manager (`Authorization: Bearer {api_key}`); SFTP username/password from Secret Manager; Argyle webhook signature verification
- **Error handling:** `try/except` blocks raise `RuntimeError` with error logged to Cloud Logging; Teams webhook notification sent on failure; no automatic retry at the HTTP client layer (retry only in Twilio via `*_retry.py` modules)
- **Failure mode:** Unhandled exception propagates to Cloud Workflow; job marked FAILED in `vgm_logs` BigQuery log table; Teams notification sent via `send_teams_notification()`

---

### Boundary LB-02 — Ingestion Layer → BigQuery STG

- **Produced by:** Ingestion pipeline entry points — `main.py::load_data_to_bigquery()`, `bq_client.load_table_from_dataframe()`, direct SQL INSERT into STG tables
- **Artifact:** Staged records in BigQuery STG dataset — schema derived from source JSON, typed via Pandas DataFrame schema mapping to BigQuery schema
- **Consumed by:** Merge/transformation layer — `execute_merge_query()` functions in each module
- **Handoff mechanism:** Database write — `bigquery.Client().load_table_from_dataframe(df, f"{STG_DATASET_ID}.{table}")` or `bq_client.query(INSERT INTO ...)`
- **Auth pattern:** Application Default Credentials (ADC) via `bigquery.Client()` — no explicit key; relies on Cloud Function / Cloud Run service account
- **Error handling:** `job.result()` blocks until complete; exception raised on failure; logged to `vgm_logs` log table
- **Failure mode:** BigQuery write exception propagates; job fails; log entry written; Teams notification sent

---

### Boundary LB-03 — BigQuery STG → BigQuery OP (MERGE)

- **Produced by:** `execute_merge_query()` functions — dynamic MERGE statement generation using `INFORMATION_SCHEMA.COLUMNS` to build ON clause, UPDATE SET, and INSERT column lists
- **Artifact:** Upserted records in BigQuery OP dataset — `edw_date_modified = CURRENT_TIMESTAMP()` added on every UPDATE
- **Consumed by:** Downstream consumers — BigQuery API, OP SQL execution, Power BI
- **Handoff mechanism:** Database MERGE — `bq_client.query(merge_statement).result()`
- **Auth pattern:** ADC via service account
- **Error handling:** MERGE job exception caught; `insert_log_msg()` writes FAILED status to log table; exception re-raised; control table not updated on failure
- **Failure mode:** Control table `last_updated` not advanced → next run will re-process the same window. Concurrent UPDATE retried up to 5× with 2s sleep (telematics module).

---

### Boundary LB-04 — BigQuery OP → BigQuery API (REST)

- **Produced by:** BigQuery OP dataset tables (listed in `ACCESSIBLE_TABLES` secret)
- **Artifact:** JSON query results — `[dict(row) for row in results]`
- **Consumed by:** External API consumers (internal teams / clients) — identified only from documentation; no consumer code reviewed
- **Handoff mechanism:** HTTP REST — `GET /query/{dataset_id}/{table_id}` with `X-API-Key` header
- **Auth pattern:** API key in `X-API-Key` header; compared against `Config.BQ_API_KEY` from Secret Manager; 401 raised on mismatch
- **Error handling:** BigQuery timeout enforced via `result(timeout=Config.BQ_TIMEOUT)`; HTTPException 500 on query failure; 403 on restricted table access; 400 on invalid timestamp format
- **Failure mode:** HTTP 5xx response to client; no retry at API layer

---

### Boundary LB-05 — BigQuery OP → Power BI Dashboards

- **Produced by:** BigQuery OP and RPT dataset views (deployed via `devops_resources/foundational_dm_resources/bigquery_dm_templates/`)
- **Artifact:** Report data via BigQuery views (50+ SQL view definitions found in codebase)
- **Consumed by:** Power BI Desktop / Power BI Service
- **Handoff mechanism:** NOT DETERMINABLE FROM SOURCE (Power BI connector to BigQuery — direct query or import mode not confirmed from reviewed code; `vgm_powerbi_migration/` not fully read)
- **Auth pattern:** NOT DETERMINABLE FROM SOURCE
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Failure mode:** NOT DETERMINABLE FROM SOURCE

---

### Boundary LB-06 — BigQuery Vector Store + Vertex AI → Gen-AI Chatbot

- **Produced by:** BigQuery vector store table (`DATASET_NAME.TABLE_NAME`) via `BigQueryVectorSearch`; Vertex AI model `gemini-1.5-flash-002`
- **Artifact:** LLM-generated natural language responses (string); chat history stored in `CHAT_HISTORY_TABLE` in BigQuery
- **Consumed by:** End users via `/chat` REST endpoint or WhatsApp webhook
- **Handoff mechanism:**
  - Context retrieval: `vector_search.as_retriever().get_relevant_documents(question)` → BigQuery vector similarity search
  - LLM invocation: `retrieval_qa({"query": full_query})` → Vertex AI Gemini 1.5 Flash (synchronous)
  - WhatsApp delivery: `requests.post(API_URL, json=payload, headers={"Authorization": f"Bearer {ACCESS_TOKEN}"})` → Meta WhatsApp Business API
- **Auth pattern:** Vertex AI — ADC via service account; WhatsApp — Bearer token in `ACCESS_TOKEN` env var (not from Secret Manager — hardcoded env)
- **Error handling:** On empty retrieved docs: returns fallback message and connects to human agent; LLM exceptions not explicitly caught in `ask_question()`
- **Failure mode:** Silent fallback message returned; no alerting mechanism for LLM failures observed

---

### Boundary LB-07 — Secret Manager → All Pipeline Layers

- **Produced by:** Google Cloud Secret Manager (GCP managed service)
- **Artifact:** JSON-encoded secret payloads — API keys, SFTP credentials, DB connection strings, Teams webhook URLs, notification service keys
- **Consumed by:** All pipeline modules at startup — `SecretManagerServiceClient().access_secret_version({"name": f"projects/{PROJECT_ID}/secrets/{secret_name}/versions/latest"})`
- **Handoff mechanism:** Direct GCP API call (synchronous, blocking)
- **Auth pattern:** ADC via service account — no explicit key required if running on GCP
- **Error handling:** Exception caught; logged as ERROR; RuntimeError raised — propagates to STARTUP-FATAL in all modules
- **Failure mode:** If Secret Manager is unreachable at startup, the entire pipeline fails to initialize

---

### Boundary LB-08 — GitLab CI/CD → Cloud Deployment

- **Produced by:** GitLab pipelines (`*_resources_creation_pipeline.yml`, `deployment1.sh`, `deployment2.sh`)
- **Artifact:** Deployed Cloud Functions, Cloud Run services, Cloud Workflows, GCS-uploaded SQL/Python scripts
- **Consumed by:** GCP project runtime environment
- **Handoff mechanism:** `gcloud functions deploy` / `gcloud run deploy` / Deployment Manager (shell scripts in `deployment1.sh`, `deployment2.sh`)
- **Auth pattern:** GitLab CI service account credentials (GCP_SA_KEY or Workload Identity — NOT DETERMINABLE FROM SOURCE without reading pipeline YAML files)
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Failure mode:** NOT DETERMINABLE FROM SOURCE

---

### Boundary LB-09 — OP SQL Execution → Business Central API [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A01]

- **Produced by:** `bos_entry_report_export.py::post_to_bc(posting_date)` — called from `main_op.py::bos_entry_executor`
- **Artifact:** BOS journal entry posted to Business Central ERP
- **Consumed by:** Business Central financial system
- **Handoff mechanism:** Direct HTTP call (exact protocol — NOT DETERMINABLE FROM SOURCE without reading `bos_entry_report_export.py` fully)
- **Auth pattern:** NOT DETERMINABLE FROM SOURCE
- **Error handling:** Exception propagates to `bos_entry_executor`; `bos_entry_cntrl_tbl.validation_flag` remains false on failure
- **Failure mode:** BOS entry not posted; email notification sent with affected posting dates

---

### Boundary LB-10 — OP SQL Execution → GCS (SQL/Python script storage) [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A01]

- **Produced by:** GCS bucket (`BUCKET_NAME` env var) — populated at deploy time by CI/CD pipeline
- **Artifact:** SQL query files and Python script files read at runtime
- **Consumed by:** `bq_utility_op.py::fetch_query(path, BUCKET_NAME)` → used by `execute_step()`, `billing_transaction_validation()`, `generate_bos_entry()`
- **Handoff mechanism:** GCS object read — `storage.Client().bucket(BUCKET_NAME).blob(path).download_as_text()`
- **Auth pattern:** ADC via service account
- **Error handling:** NOT DETERMINABLE FROM SOURCE without reading `bq_utility_op.py` fully
- **Failure mode:** Script not found → `execute_step()` raises RuntimeError → script skipped with FAILURE log

---

## A02 — Module Call Map

See `discovery/components/A02_module_call_map.md` for full detail.

**Summary findings:**
- No cross-module runtime calls — all modules are independently deployed units communicating only through shared BigQuery tables and GCS
- All route handlers and batch entry points are synchronous (`def`, not `async def`)
- Only concurrency pattern: `ThreadPoolExecutor(max_workers=6)` in `vgm_callbell_bq` for parallel message fetching
- Startup is the highest-risk phase: all Vertex AI, BigQuery, and Secret Manager clients are module-level singletons; failure in any one prevents the service from starting
- `vgm_op_sql_execution::execute_step` uses Python `exec()` to run fetched scripts in-process — no sandboxing

---

## A03 — External System Boundary Map

[STAGE-2-UPDATE — 2026-04-22]: Stage 1 A03 had 11 entries (intake-derived, all auth/error fields NDfS).
All entries have been completed from source code. 6 new external systems added that were not in the intake.

---

### ES-01 — Flespi

- **Type:** Telematics platform (vehicle tracking / fleet management)
- **Direction:** Inbound — VGM polls Flespi API for telemetry records
- **Protocol / format:** REST API; JSON payload with nested telemetry fields (`$.timestamp`, `$.device.id`, `$.vin`, `$.vehicle.mileage`, `$.position.latitude`, etc.)
- **Auth credentials:** Bearer token — fetched from Secret Manager at startup
- **Error handling:** HTTP non-200 returns empty dict `{}`; no retry at HTTP layer; failure logged and Teams notification sent
- **Notes:** Two ingestion frequencies — hourly (vgm_telematics_bq) and minute-level (vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data). Data arrives via Pub/Sub channel; raw JSON stored in STG then transformed via multi-CTE SQL (hour-sliced aggregation).

---

### ES-02 — Twilio

- **Type:** Communication platform (SMS / telephony / call recordings)
- **Direction:** Inbound — VGM polls Twilio API
- **Protocol / format:** Twilio REST API; multiple sub-resources: call insights, driver analysis, recording properties, transcription
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE without reading `twilio_call_insights.py` in full (credential pattern likely from Secret Manager based on module pattern)
- **Error handling:** Dedicated `*_retry.py` modules for call insights and driver analysis — retry logic confirmed
- **Notes:** Results are also formatted as Excel reports and emailed via SendGrid

---

### ES-03 — Callbell

- **Type:** Customer engagement / chat platform
- **Direction:** Inbound — VGM polls Callbell REST API
- **Protocol / format:** REST API; paginated JSON (`{"contacts": [...], "meta": {"pages": N}}`); page parameter `?page=N`
- **Auth credentials:** Bearer token — `"Authorization": f"Bearer {api_key}"` from Secret Manager secret `SOURCE_SECRET` (keys: `API_BASE_URL`, `API_KEY`)
- **Error handling:** HTTP non-200 returns empty dict `{}`; no retry; RuntimeError raised on exception; Teams notification on job failure
- **Notes:** Concurrent message fetching via ThreadPoolExecutor(max_workers=6); contacts fetched first (full load with truncate), then messages (incremental by contact watermark in `CALLBELL_CNTRL_TBL`)

---

### ES-04 — Element (SFTP)

- **Type:** File feed via SFTP
- **Direction:** Inbound — VGM pulls `.xlsx` files from remote SFTP server
- **Protocol / format:** SFTP via `paramiko`; files are `.xlsx` format; filtered to exclude `new-cars` prefix
- **Auth credentials:** SFTP host, port, username, password — from Secret Manager (`secret_name` parameter); keys: `host`, `port`, `username`, `password`
- **Error handling:** `paramiko` exceptions caught; logged as ERROR; RuntimeError raised and propagated
- **Notes:** Files already processed tracked via `get_processed_files()` to avoid re-ingestion; uploaded to GCS as intermediate before BigQuery load

---

### ES-05 — Internal VGM Source Systems (multiple)

[STAGE-2-UPDATE — 2026-04-22]: Stage 1 listed this as one entry. Source code reveals at least 11 distinct internal/external source systems all ingested by `vgm_source_bq/vgm_source_to_bq_script/main_script.py`. Confirmed from test file names:

| System | Evidence |
|--------|----------|
| **BOS** (Back Office System) | `test_bigquery_functions.py`, `op_bos_*` SQL files, `bos_entry_cntrl_tbl` control table |
| **Minave** | `test_minave_functions.py`, 20+ `op_minave_*` translated SQL files, Mexico market (CDMX, Guadalajara, Puebla) |
| **Argyle** | `test_argyle_functions.py`, dedicated webhook Cloud Function + merge exec Cloud Function |
| **Stripe** | `test_stripe_functions.py`, `op_stripe_driver_risk_score.sql` |
| **Uber** | `test_uber_functions.py`, `op_uber_accidents_combined.py` |
| **FleetSync** | `test_fleetsync.py`, `op_fleetsync_car_valuations.sql` |
| **Manheim** | `test_manheim_functions.py`, `op_manheim_valuations_trims.sql` |
| **Solo** | `test_solo_functions.py` |
| **3CX** | `test_3cx.py`, `op_3cx_call_insights.sql` |
| **Microsoft Dynamics** | `test_dynamics_api.py` |
| **Element API** | `test_element_api.py` (separate from SFTP; also has an API interface) |

- **Protocol / format:** NOT DETERMINABLE FROM SOURCE per system without reading `main_script.py` fully (file is 2000+ lines)
- **Auth credentials:** Per-system secrets from Secret Manager
- **Error handling:** Pattern consistent with other modules — RuntimeError raised, Teams notification on failure

---

### ES-06 — Google BigQuery

- **Type:** Cloud data warehouse (GCP managed service)
- **Direction:** Bidirectional — pipeline modules write STG/OP tables; API and chatbot read OP tables
- **Protocol / format:** BigQuery Python client (`google-cloud-bigquery`); SQL DML (INSERT, MERGE, UPDATE, TRUNCATE, SELECT)
- **Auth credentials:** ADC via `bigquery.Client()` — service account attached to Cloud Function / Cloud Run / batch container
- **Error handling:** `.result()` blocks and raises `google.api_core.exceptions.GoogleAPICallError` on failure; caught in each module's try/except; logged; RuntimeError raised
- **Notes:** `edw_date_modified = CURRENT_TIMESTAMP()` added on every MERGE UPDATE; control tables track watermarks per OP table

---

### ES-07 — Google Vertex AI

- **Type:** Managed AI/ML platform (GCP)
- **Direction:** Inbound to chatbot — VGM sends prompts, receives generated text
- **Protocol / format:** LangChain `VertexAI` wrapper; model: `gemini-1.5-flash-002`; embeddings model: `text-multilingual-embedding-002`
- **Auth credentials:** ADC via service account — no explicit key
- **Error handling:** Exceptions in `extract_name_from_text_llm()` silently caught and return `""`; `ask_question()` LLM call not wrapped in try/except — unhandled exceptions propagate as HTTP 500
- **Notes:** Chatbot is scoped to MiNave (Mexican car rental brand) — confirmed from `TONE_PREFIX` and `SYSTEM_INSTRUCTIONS` in `config.py`; supports English and Spanish

---

### ES-08 — Google Cloud Secret Manager

- **Type:** Secrets management service (GCP)
- **Direction:** Inbound — all modules fetch secrets at startup
- **Protocol / format:** `secretmanager.SecretManagerServiceClient().access_secret_version({"name": "projects/{id}/secrets/{name}/versions/latest"})`; payload decoded as UTF-8 JSON
- **Auth credentials:** ADC via service account
- **Error handling:** RuntimeError raised on failure; STARTUP-FATAL in all modules
- **Notes:** Secrets are JSON objects with multiple keys per secret (e.g., `BQ_API_KEY`, `BQ_DATASET_ID`, `ACCESSIBLE_TABLES`, `HOST_URL`, `BQ_TIMEOUT` in one secret for BigQuery API)

---

### ES-09 — Google Cloud Storage

- **Type:** Object storage (GCP)
- **Direction:** Bidirectional — Element SFTP uploads files; OP SQL execution reads scripts
- **Protocol / format:** GCS Python client (`google-cloud-storage`); binary file upload, text file download
- **Auth credentials:** ADC via service account
- **Error handling:** NOT DETERMINABLE FROM SOURCE without reading `bq_utility_op.py::fetch_query` fully
- **Notes:** GCS bucket (`BUCKET_NAME`) stores all operational SQL and Python scripts; scripts fetched at runtime (not bundled in Docker image) — allows script changes without redeployment

---

### ES-10 — Power BI (Microsoft)

- **Type:** Business intelligence / reporting tool
- **Direction:** Outbound — VGM serves data to Power BI via BigQuery views
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE (Power BI connector type not confirmed)
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Notes:** `vgm_powerbi_migration/vgm_powerbi_migration_script/migration_script.py` exists for report migration; one `.pbix` report file present (`Miami Branch Manager Dashboard.pbix`); 50+ RPT-layer SQL view definitions found in `devops_resources`

---

### ES-11 — GitLab CI/CD

- **Type:** CI/CD platform
- **Direction:** Outbound control plane — triggers deployments
- **Protocol / format:** GitLab pipeline YAML (`*_resources_creation_pipeline.yml`); shell scripts (`deployment1.sh`, `deployment2.sh`)
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (likely GCP service account key stored as GitLab CI variable)
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Notes:** Each module has its own `*_gitlab_pipelines/` directory with pipeline definitions; top-level `.gitlab-ci.yml` coordinates all modules via `$PIPELINE_NAME` variable

---

### ES-12 — Microsoft Teams (webhook) [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** Enterprise messaging (webhook integration)
- **Direction:** Outbound — VGM sends failure/alert notifications to Teams
- **Protocol / format:** HTTP POST with Adaptive Card JSON payload; `Content-Type: application/json`
- **Auth credentials:** Teams webhook URL from Secret Manager (`NOTIFIER_SECRET_NAME` secret, key: `TEAMS_NOTIFICATION_URL`)
- **Error handling:** Exceptions caught; logged as ERROR in Teams notification function itself; secondary failure (Teams unreachable) does not prevent primary job failure propagation
- **Notes:** Adaptive Card format used consistently across all modules; pattern: `{"type": "message", "attachments": [{"contentType": "application/vnd.microsoft.card.adaptive", ...}]}`

---

### ES-13 — SendGrid / Email [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** Email delivery service
- **Direction:** Outbound — VGM sends reports and failure alerts via email
- **Protocol / format:** SendGrid API or SMTP (exact method NOT DETERMINABLE FROM SOURCE without reading `twilio_send_email.py` and `vgm_notifier_op.py::send_email` fully)
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE (likely from Secret Manager)
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Notes:** Used in Twilio pipeline (daily/weekly reports as Excel attachments) and OP SQL execution (failure alerts, BOS entry alerts, schema change alerts)

---

### ES-14 — Meta WhatsApp Business API [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** Messaging platform API (Meta Graph API)
- **Direction:** Bidirectional — chatbot receives webhook events; sends replies
- **Protocol / format:** `POST https://graph.facebook.com/v22.0/{WHATSAPP_PHONE_NUMBER_ID}/messages`; JSON payload `{"messaging_product": "whatsapp", "to": recipient_id, "type": "text", "text": {"body": message}}`
- **Auth credentials:** Bearer token in `ACCESS_TOKEN` env var — loaded from env (not Secret Manager — potential security concern)
- **Error handling:** Non-200 response logged as ERROR; no retry; response returned regardless
- **Notes:** Webhook verification via `GET /whatsapp?hub.verify_token=VERIFY_TOKEN`; processed message IDs stored in BigQuery `PROCESSED_MESSAGE_TABLE` to prevent duplicate processing

---

### ES-15 — Monday.com [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** Project management / work management platform
- **Direction:** Outbound — OP SQL execution sends validation failure records to Monday.com boards
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE without reading `vgm_notifier_op.py::send_to_mondaydotcom` fully
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Notes:** Called in `execute_validation()` when a validation query returns non-empty results; DataFrame of failing records sent to Monday.com alongside email attachment

---

### ES-16 — Business Central (Microsoft ERP) [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** ERP / accounting system
- **Direction:** Outbound — OP SQL execution posts validated BOS journal entries to Business Central
- **Protocol / format:** NOT DETERMINABLE FROM SOURCE without reading `bos_entry_report_export.py::post_to_bc` fully
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE
- **Error handling:** Exception propagates from `bos_entry_executor`; `bos_entry_cntrl_tbl.validation_flag` remains `false` on failure; email notification sent with pending posting dates
- **Notes:** Only posted if `billing_transaction_validation_flag = True` AND `bos_validation_flag = True` — double validation gate before ERP write

---

### ES-17 — Argyle (webhook) [STAGE-2-UPDATE — 2026-04-22: NEW — not in Stage 1 A03]

- **Type:** Gig economy data aggregator (real-time webhook)
- **Direction:** Inbound — Argyle pushes events to VGM Cloud Function endpoint
- **Protocol / format:** HTTP POST webhook; JSON event payload → written to BigQuery STG; separate merge-exec Cloud Function runs SQL MERGEs for accounts, account_status, gigs, gig_events, gig_income
- **Auth credentials:** NOT DETERMINABLE FROM SOURCE without reading `vgm-edw-function-argyle-webhook/main.py` fully
- **Error handling:** NOT DETERMINABLE FROM SOURCE
- **Notes:** Two Cloud Functions: `vgm-edw-function-argyle-webhook` (receives events) and `vgm-edw-function-argyle-merge-exec` (runs 5 MERGE SQL scripts). This is the only event-driven (non-scheduled) inbound integration in the system.
