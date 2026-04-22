STAGE-2-COMPLETE: SOURCE-DERIVED — 2026-04-22 — Path B Stranger-Led

# A02 — Module Call Map
BCE Stage 2, Session A

> Scope: serving layer and pipeline layer source files.
> All calls are intra-module (within a deployed unit). No cross-module runtime calls
> exist — each module deploys independently as a Cloud Function, Cloud Run container,
> or batch job. "Cross-module" interactions happen only through shared BigQuery tables.

---

## Section 1 — Internal Call Table

Legend: S = synchronous (blocking), A = concurrent (ThreadPoolExecutor), P = polling loop

### vgm_bigquery_api

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main.py::app` | `routes/health.py::router` | `app.include_router(health_router)` — module init | S |
| `main.py::app` | `routes/help.py::router` | `app.include_router(help_router)` — module init | S |
| `main.py::app` | `routes/metadata.py::router` | `app.include_router(metadata_router)` — module init | S |
| `main.py::app` | `routes/query.py::router` | `app.include_router(query_router)` — module init | S |
| `routes/metadata.py::list_tables` | `auth.py::verify_api_key` | `api_key: str = Depends(verify_api_key)` — per request | S |
| `routes/query.py::query_table` | `auth.py::verify_api_key` | `api_key: str = Depends(verify_api_key)` — per request | S |
| `routes/query.py::query_table` | `routes/query.py::build_where_clause` | line ~54 | S |
| `routes/query.py::query_table` | `routes/query.py::validate_timestamps` | line ~55 | S |
| `routes/query.py::query_table` | `routes/query.py::get_total_row_count` | line ~56 | S |
| `routes/query.py::query_table` | `routes/query.py::execute_data_query` | line ~58 | S |
| `auth.py::verify_api_key` | `config.py::Config.BQ_API_KEY` | attribute read | S |
| `routes/help.py::api_help` | `config.py::Config.HOST_URL` | attribute read | S |
| `routes/metadata.py::list_tables` | `config.py::Config.ACCESSIBLE_TABLES` | attribute read | S |
| `routes/query.py::*` | `config.py::Config / client` | attribute + module-level BQ client | S |

---

### vgm_chatbot_gen_ai

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `app/main.py::app` | `routes/health.py::router` | `app.include_router(health.router)` | S |
| `app/main.py::app` | `routes/chat.py::router` | `app.include_router(chat.router)` | S |
| `app/main.py::app` | `routes/whatsapp.py::router` | `app.include_router(whatsapp.router)` | S |
| `routes/chat.py::start_chat` | `services/bigquery_client.py::bq_client` | BigQuery INSERT chat session row | S |
| `routes/chat.py::ask_question` | `services/bigquery_client.py::bq_client` | BigQuery SELECT chat history + UPDATE | S |
| `routes/chat.py::ask_question` | `services/retrieval_qa.py::retrieval_qa` | `retrieval_qa.retriever.get_relevant_documents(question)` | S |
| `routes/chat.py::ask_question` | `services/retrieval_qa.py::retrieval_qa` | `retrieval_qa({"query": full_query})` — LLM invocation | S |
| `routes/whatsapp.py::*` | `services/bigquery_client.py::bq_client` | BigQuery reads/writes for session and processed-message dedup | S |
| `routes/whatsapp.py::*` | `services/whatsapp_service.py::send_whatsapp_message` | HTTP POST to Meta WhatsApp Business API | S |
| `routes/whatsapp.py::*` | `services/name_extractor.py::extract_name_from_text_llm` | Vertex AI LLM prompt for name extraction | S |
| `routes/whatsapp.py::*` | `services/retrieval_qa.py::retrieval_qa` | LLM invocation for response generation | S |
| `services/retrieval_qa.py` (module init) | `services/vector_search.py::vector_search` | `vector_search.as_retriever()` — startup | S |

---

### vgm_callbell_bq

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main.py::__main__` | `main.py::connect_to_api` | line ~461 | S |
| `main.py::__main__` | `main.py::fetch_callbell_contacts` | line ~463 | S |
| `main.py::fetch_callbell_contacts` | `main.py::fetch_data_from_callbell` | paginated loop | S |
| `main.py::fetch_callbell_contacts` | `main.py::process_data` | per page | S |
| `main.py::__main__` | `main.py::transform_df_to_match_bq_schema` | line ~465 | S |
| `main.py::__main__` | `main.py::truncate_table` | line ~468 | S |
| `main.py::__main__` | `main.py::load_data_to_bigquery` | line ~469 | S |
| `main.py::__main__` | `bq_merge.py::execute_merge_query` | line ~472 | S |
| `main.py::__main__` | `main.py::update_cntrl_tbl` | line ~475 (contacts) and ~504 (messages) | S |
| `main.py::__main__` | `main.py::get_callbell_metadata` | line ~485 | S |
| `main.py::__main__` | `main.py::ingest_callbell_messages` | line ~487 | S |
| `main.py::ingest_callbell_messages` | `main.py::process_messages` | ThreadPoolExecutor(max_workers=6), batches of 100 | **A** |
| `main.py::process_messages` | `main.py::fetch_data_from_callbell` | paginated per contact | S |
| `main.py::process_messages` | `main.py::process_data` | per page | S |
| `main.py::ingest_callbell_messages` | `main.py::transform_df_to_match_bq_schema` | after batch completes | S |
| `main.py::ingest_callbell_messages` | `main.py::load_data_to_bigquery` | after batch completes | S |
| `main.py::connect_to_api` | `main.py::get_secret` | Secret Manager fetch | S |
| `main.py::send_teams_notification` | `main.py::get_secret` | NOTIFIER_SECRET_NAME fetch | S |

---

### vgm_telematics_bq

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main.py` (module init) | `cloud_logger.py::get_logger` | `logger = get_logger(JOB_ID)` | S |
| `main.py::main` | `main.py::truncate_table` | line ~425 (STG) and ~418 (OP, full load) | S |
| `main.py::main` | `main.py::get_dynamic_query` | line ~422 | S |
| `main.py::main` | `main.py::execute_dynamic_query` | line ~426 | P (polls with sleep 10s) |
| `main.py::main` | `main.py::execute_merge_query` | line ~429 | S |
| `main.py::execute_merge_query` | `main.py::get_columns_info` | line ~335 | S |
| `main.py::execute_merge_query` | `main.py::insert_log_msg` | on success and failure | S |
| `main.py::execute_merge_query` | `main.py::update_op_cntrl_tbl` | line ~390 | S |
| `main.py::update_op_cntrl_tbl` | `main.py::get_max_datetime` | line ~300 | S |
| `main.py::update_op_cntrl_tbl` | BigQuery UPDATE (inline) | retries up to 5× on concurrent update | S |
| `main.py::main` | `main.py::get_max_timestamp` | line ~432 | S |
| `main.py::main` | `main.py::update_last_updated_timestamp` | line ~433 | S |

---

### vgm_element_sftp_bq

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main.py::__main__` | `sftp_operator.py::SFTPOperations.__init__` | instantiation | S |
| `SFTPOperations.__init__` | `secret_manager_client.py::SecretManagerClient.get_secret` | credential fetch | S |
| `SFTPOperations.__init__` | `gcp_functions.py::GCPFunctions.__init__` | parent init | S |
| `main.py::__main__` | `SFTPOperations.connect` | paramiko SFTP connect | S |
| `main.py::__main__` | `SFTPOperations.upload_files` | list remote dir, filter, upload new files | S |
| `SFTPOperations.upload_files` | `gcp_functions.py::GCPFunctions.upload_to_gcs` | per new file | S |
| `SFTPOperations.upload_files` | `gcp_functions.py::GCPFunctions.get_processed_files` | check already-processed list | S |
| `main.py::__main__` | `gcp_functions.py::GCPFunctions.*` | BigQuery load, merge, control updates | S |
| `main.py::__main__` | `SFTPOperations.disconnect` | close SFTP | S |
| `main.py::__main__` (on error) | `vgm_notifier.py::*` | Teams/email alert | S |

---

### vgm_op_sql_execution

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main_op.py` (module init) | `cloud_logger_op.py::get_logger` | `logger = get_logger(__name__)` | S |
| `main_op.py` (module init) | `bigquery.Client()` | `bq_client = bigquery.Client()` | S |
| `main_op.py::__main__` | BigQuery MERGE (inline) | update `stg_bos_cntrl_tbl` from intermediate table | S |
| `main_op.py::__main__` | BigQuery UPDATE (inline) | set load_type to incremental | S |
| `main_op.py::__main__` | `bq_utility_op.py::recreate_table` | recreate intermediate control table | S |
| `main_op.py::__main__` | `main_op.py::get_script_metadata` | fetch op_script_exec_cntrl_tbl | S |
| `main_op.py::__main__` | `main_op.py::script_executor` | per script in SCRIPTS_INFO | S |
| `main_op.py::script_executor` | `main_op.py::dependency_check` | checks log table for dep status | S |
| `main_op.py::script_executor` | `main_op.py::execute_step` | dispatches to SQL or Python executor | S |
| `main_op.py::execute_step` | `bq_utility_op.py::fetch_query` | fetch script from GCS bucket | S |
| `main_op.py::execute_step` | `bq_utility_op.py::execute_query` | BigQuery job execution | S |
| `main_op.py::execute_step` | `bq_utility_op.py::execute_py` | Python `exec()` of fetched script | S |
| `main_op.py::__main__` | `main_op.py::insert_cur_date_cntrl_entry` | idempotent BOS control insert | S |
| `main_op.py::__main__` | `main_op.py::get_bos_entry_metadata` | pending BOS posting dates | S |
| `main_op.py::__main__` | `main_op.py::dependency_check` | bos_entry dependency | S |
| `main_op.py::__main__` | `main_op.py::billing_transaction_validation` | 4 validation queries | S |
| `main_op.py::billing_transaction_validation` | `bq_utility_op.py::fetch_query` | per validation SQL from GCS | S |
| `main_op.py::billing_transaction_validation` | `main_op.py::execute_validation` | per validation query | S |
| `main_op.py::execute_validation` | `vgm_notifier_op.py::send_to_mondaydotcom` | on validation failure | S |
| `main_op.py::execute_validation` | `vgm_notifier_op.py::send_email` | with xlsx attachment on failure | S |
| `main_op.py::__main__` | `main_op.py::bos_entry_executor` | per pending posting date | S |
| `main_op.py::bos_entry_executor` | `main_op.py::bos_entry_validation` | finance setting check | S |
| `main_op.py::bos_entry_executor` | `main_op.py::generate_bos_entry` | runs BOS entry SQL | S |
| `main_op.py::bos_entry_executor` | `bos_entry_report_export.py::post_to_bc` | HTTP POST to Business Central API | S |
| `main_op.py::__main__` | `main_op.py::insert_cur_date_cntrl_ar` | idempotent AR control insert | S |
| `main_op.py::__main__` | `main_op.py::get_ar_metadata` | pending AR report dates | S |
| `main_op.py::__main__` | `main_op.py::ar_executor` | per pending AR date | S |
| `main_op.py::ar_executor` | `main_op.py::generate_bos_entry` | runs AR lookup SQL | S |
| `main_op.py::__main__` | `main_op.py::check_dataset_execution` | end-of-run audit | S |
| `main_op.py::check_dataset_execution` | `main_op.py::get_schema_change_metadata_for_email_notification` | schema drift check | S |
| `main_op.py::check_dataset_execution` | `main_op.py::get_meta_data_for_email_notification` | failure summary | S |
| `main_op.py::check_dataset_execution` | `vgm_notifier_op.py::send_email_notification_for_job_failure` | on failures or schema changes | S |

---

### vgm_source_bq (main_script.py)

> Note: full file is 2000+ lines. The test file names confirm it orchestrates at least 11 source
> systems. Internal call structure follows the same pattern as callbell/telematics:
> module-level bq_client + logger, per-source fetch functions, bq_merge for upserts.

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main_script.py` (module init) | `cloud_logger.py::get_logger` | logger init | S |
| `main_script.py` (module init) | `bigquery.Client()` | bq_client init | S |
| `main_script.py::__main__` | `get_secret.py::get_secret` | per source credential fetch | S |
| `main_script.py::__main__` | source-specific fetch functions (BOS, Argyle, Minave, Stripe, Uber, FleetSync, Manheim, Solo, 3CX, Dynamics, Element API) | per source | S |
| `main_script.py::__main__` | `bq_merge.py::execute_merge_query` | STG → OP upsert per table | S |
| `main_script.py::__main__` (on error) | `vgm_notifier.py::*` | Teams/email alert | S |

---

### vgm_flespi_bq / vgm_source_bq minute-incremental

> Follows same pattern as vgm_telematics_bq. Main functions:
> module-level logger + env vars → main() → get_secret → BQ client →
> Flespi API fetch → STG load → MERGE → control table update.

---

### vgm_twilio_bq

| Caller | Callee | Call site | Type |
|--------|--------|-----------|------|
| `main_script.py::__main__` | `bq_utilty.py::*` (BQ load, log) | per Twilio data type | S |
| `main_script.py::__main__` | `bq_merge.py::execute_merge_query` | STG → OP upsert | S |
| `main_script.py::__main__` | `twilio_call_insights.py::*` | fetch call records | S |
| `main_script.py::__main__` | `twilio_call_insights_retry.py::*` | retry on failure | S |
| `main_script.py::__main__` | `twilio_driver_analysis.py::*` | per-driver analysis | S |
| `main_script.py::__main__` | `twilio_recording_properties.py::*` | recording metadata | S |
| `main_script.py::__main__` | `twilio_transcription.py::*` | call transcriptions | S |
| `main_script.py::__main__` | `twilio_report_generation.py::*` | daily report | S |
| `main_script.py::__main__` | `twilio_excel_wrap.py::*` | Excel formatting | S |
| `main_script.py::__main__` | `twilio_send_email.py::*` | SendGrid email dispatch | S |
| `main_script.py::__main__` | `twilio_weekly_driver_analysis.py::*` | weekly analysis | S |
| `main_script.py::__main__` | `twilio_weekly_report_generation.py::*` | weekly report | S |
| `main_script.py::__main__` (on error) | `vgm_notifier.py::send_teams_notification` | Teams alert | S |

---

## Section 2 — Startup Sequence

### vgm_bigquery_api

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `config.py` imported; `load_dotenv()` executes | STARTUP-FATAL in local dev if missing; NON-FATAL in Cloud Function (env already set) |
| 2 | `Config` class body: `PROJECT_ID`, `SECRET_NAME` read from env | NON-FATAL (empty strings allowed) |
| 3 | `Config.get_secret(PROJECT_ID, SECRET_NAME)` → Secret Manager API call → returns JSON with `BQ_DATASET_ID`, `BQ_API_KEY`, `ACCESSIBLE_TABLES`, `HOST_URL`, `BQ_TIMEOUT` | **STARTUP-FATAL** — RuntimeError if Secret Manager unreachable or secret missing |
| 4 | `Config.validate()` → raises RuntimeError if any class attribute is falsy | **STARTUP-FATAL** |
| 5 | `bigquery.Client()` created at module level (config.py) | **STARTUP-FATAL** — fails if GCP auth unavailable |
| 6 | FastAPI app instance created; Mangum handler created | NON-FATAL |
| 7 | Routers included (health, help, metadata, query) | NON-FATAL |

---

### vgm_chatbot_gen_ai

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `app/config.py` imported; all `os.getenv()` reads; `API_URL` constructed from `WHATSAPP_PHONE_NUMBER_ID` | NON-FATAL (silent empty strings if env missing) |
| 2 | `services/bigquery_client.py` imported → `bigquery.Client(project=PROJECT_ID)` | **STARTUP-FATAL** |
| 3 | `services/vector_search.py` imported → `VertexAIEmbeddings(model_name="text-multilingual-embedding-002", project=PROJECT_ID)` | **STARTUP-FATAL** |
| 4 | `services/vector_search.py` → `BigQueryVectorSearch(project_id, dataset_name, table_name, embedding)` | **STARTUP-FATAL** |
| 5 | `services/name_extractor.py` imported → `VertexAI(model_name="gemini-1.5-flash-002", project=PROJECT_ID)` | **STARTUP-FATAL** |
| 6 | `services/retrieval_qa.py` imported → `VertexAI(model_name="gemini-1.5-flash-002")` + `ConversationBufferMemory` | **STARTUP-FATAL** |
| 7 | `services/retrieval_qa.py` → `RetrievalQA.from_chain_type(llm, retriever=vector_search.as_retriever())` | **STARTUP-FATAL** |
| 8 | FastAPI app created; CORSMiddleware added (`allow_origins=["*"]`); routers included | NON-FATAL |

> Note: All Vertex AI and BigQuery service objects are module-level singletons created at import time.
> Any failure in steps 2–7 will prevent the FastAPI app from starting at all.

---

### vgm_callbell_bq (batch)

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `load_dotenv()` | NON-FATAL |
| 2 | `os.getenv()` reads for all env vars | NON-FATAL |
| 3 | `bigquery.Client()` — module-level | **STARTUP-FATAL** |
| 4 | `get_logger(JOB_ID)` → `google.cloud.logging.Client()` | **STARTUP-FATAL** |
| 5 | `__main__`: `connect_to_api(SOURCE_SECRET)` → `get_secret()` → Secret Manager | **STARTUP-FATAL** |
| 6 | `connect_to_api`: extracts `API_BASE_URL` + `API_KEY` from secret JSON | **STARTUP-FATAL** |

---

### vgm_telematics_bq (batch)

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `load_dotenv()`, `os.getenv()`/`os.environ.get()` reads | NON-FATAL |
| 2 | `get_logger(JOB_ID)` → `google.cloud.logging.Client()` | **STARTUP-FATAL** |
| 3 | `main()` called → `bigquery.Client()` | **STARTUP-FATAL** |
| 4 | `main()`: reads `source`, `source_table`, `stg_table`, `op_table`, `primary_key`, `timestamp_column`, `load_type`, `last_updated` from env | NON-FATAL (values may be None) |

---

### vgm_element_sftp_bq (batch)

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `get_logger()` → `google.cloud.logging.Client()` | **STARTUP-FATAL** |
| 2 | `SFTPOperations.__init__` → `SecretManagerClient.get_secret()` → Secret Manager | **STARTUP-FATAL** |
| 3 | `SFTPOperations.__init__` → extracts SFTP host, port, username, password from secret | **STARTUP-FATAL** |
| 4 | `SFTPOperations.connect()` → `paramiko.Transport((host, port))` → SFTP handshake | **STARTUP-FATAL** |

---

### vgm_op_sql_execution (batch)

| Step | Action | Fatal? |
|------|--------|--------|
| 1 | `cloud_logger_op.get_logger(__name__)` — module-level | **STARTUP-FATAL** |
| 2 | `bigquery.Client()` — module-level | **STARTUP-FATAL** |
| 3 | `os.getenv()` reads (CNTRL_DATASET_ID, STG_CNTRL_TBL, STG_INTERMEDIATE_CNTRL_TBL, ENV_TYPE) | NON-FATAL |
| 4 | `__main__`: `load_dotenv()`, remaining env vars | NON-FATAL |
| 5 | `bq_client.query(merge_query).result()` — merge intermediate control table | **STARTUP-FATAL** (pipeline cannot proceed without up-to-date control state) |
| 6 | `bq_client.query(update_query).result()` — update load_type | **STARTUP-FATAL** |
| 7 | `recreate_table(CNTRL_DATASET_ID, STG_INTERMEDIATE_CNTRL_TBL)` | **STARTUP-FATAL** |

---

## Section 3 — Async Boundaries

> Finding: No true async/await boundaries exist in any module. All route handlers and
> batch entry points use synchronous `def`. The only concurrency pattern is
> ThreadPoolExecutor in vgm_callbell_bq.

| Location | Pattern | What is awaited / concurrent | Notes |
|----------|---------|------------------------------|-------|
| `vgm_callbell_bq/main.py::ingest_callbell_messages` | `ThreadPoolExecutor(max_workers=6)` + `as_completed()` | `process_messages()` calls run concurrently per contact UUID, in batches of 100 | Collected synchronously via `as_completed()`; not fire-and-forget |
| `vgm_telematics_bq/main.py::execute_dynamic_query` | Polling loop `while not query_job.done(): time.sleep(10)` | BigQuery async job polled synchronously | Blocking poll; not async/await |
| `vgm_bigquery_api/routes/query.py::execute_data_query` | `client.query(...).result(timeout=BQ_TIMEOUT)` | BigQuery job — blocks until complete or timeout | Synchronous blocking |
| `vgm_chatbot_gen_ai/routes/chat.py::ask_question` | `bq_client.query(...).result()` | BigQuery history fetch and update | Synchronous blocking |
| `vgm_chatbot_gen_ai/routes/chat.py::ask_question` | `retrieval_qa({"query": full_query})` | Vertex AI LLM call via LangChain | Synchronous blocking — no streaming |
| `vgm_chatbot_gen_ai/services/whatsapp_service.py::send_whatsapp_message` | `requests.post(API_URL, ...)` | Meta WhatsApp Business API HTTP call | Synchronous; no retry on failure |
| `vgm_op_sql_execution/main_op.py::execute_step → execute_py` | `exec(python_script, ...)` | Executes fetched Python script in-process | Synchronous; no isolation |

**Fire-and-forget:** None found.
**Queued work:** None found. No Pub/Sub producers or consumers in reviewed modules.
**True async/await:** None found in any reviewed module.
