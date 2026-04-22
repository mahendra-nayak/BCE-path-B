STAGE-1-DRAFT: INTAKE-DERIVED — 2026-04-22 — Path B Stranger-Led

# A00 — Codebase Map (Template T0)
BCE Stage 2, Session A0 — Full directory traversal
Total files: 533 (excl. .git)

Format: `<file path> | <one-line purpose>`

---

## Root

`.gitignore` | Git ignore rules for the repository
`.gitlab-ci.yml` | Top-level GitLab CI/CD pipeline — orchestrates all module pipelines via $PIPELINE_NAME variable
`BCE_PATH_B_EXECUTION_PLAN.md` | BCE Path B execution plan — strangerled system extraction methodology and session plan
`bce_prompt_sequence_path_b.md` | BCE prompt sequences for each stage of Path B extraction
`README.md` | Project overview, architecture, module index, install and run instructions

---

## skills/

`skills/bce_core.md` | BCE core methodology v2.0 — artifact graph, ID schema, stage definitions
`skills/bce_signal.md` | BCE Signal pipeline v1.0 — non-code source extraction (docs, Teams, Git)
`skills/SKILL (2).md` | Additional skill documentation (purpose requires reading)

---

## discovery/

`discovery/INTAKE_SUMMARY.md` | Pre-extraction intake document — system purpose, known architecture, open questions
`discovery/TOPOLOGY.md` | Stage 1 BCE artifact — layer boundary map (A01) and external system map (A03)
`discovery/MODULE_CONTRACTS.md` | Stage 1 BCE artifact — T4 module contract skeletons (11 modules)
`discovery/INVARIANT_CATALOGUE.md` | Stage 1 BCE artifact — invariant catalogue (empty; to be populated in Stage 2)
`discovery/RISK_REGISTER.md` | Stage 1 BCE artifact — risk register (empty; to be populated in Stage 2)
`discovery/STAGE1_COMPLETION.md` | Stage 1 BCE completeness summary with engineer sign-off
`discovery/components/A00_codebase_map.md` | This file — Stage 2 Session A0 full codebase inventory

---

## devops_resources/

### foundational_dm_resources/

`devops_resources/foundational_dm_resources/deployment.py` | Deployment Manager Python template — top-level foundational resource deployment
`devops_resources/foundational_dm_resources/deployment.py.schema` | Schema definition for foundational deployment template
`devops_resources/foundational_dm_resources/foundational_deployment_config.yaml` | YAML config driving foundational Deployment Manager deployment

#### bigquery_dm_templates/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/bigquery_deployment_template.py` | Deployment Manager Python template — provisions BigQuery datasets
`devops_resources/foundational_dm_resources/bigquery_dm_templates/bigquery_deployment_template.schema` | Schema for BigQuery deployment template
`devops_resources/foundational_dm_resources/bigquery_dm_templates/one_time_ddl_statement_cntrl.sql` | One-time DDL to create the control (CNTRL) BigQuery dataset/tables
`devops_resources/foundational_dm_resources/bigquery_dm_templates/one_time_ddl_statement_logs.sql` | One-time DDL to create the logs BigQuery dataset/tables
`devops_resources/foundational_dm_resources/bigquery_dm_templates/one_time_ddl_statement_ops.sql` | One-time DDL to create the operational (OP) BigQuery dataset/tables
`devops_resources/foundational_dm_resources/bigquery_dm_templates/one_time_ddl_statement_rpt.sql` | One-time DDL to create the reporting (RPT) BigQuery dataset/tables
`devops_resources/foundational_dm_resources/bigquery_dm_templates/one_time_ddl_statement_stg.sql` | One-time DDL to create the staging (STG) BigQuery dataset/tables

##### dataops_views/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/dataops_views/vw_dataops_last_updated_ts.sql` | BigQuery view — last updated timestamp for DataOps monitoring
`devops_resources/foundational_dm_resources/bigquery_dm_templates/dataops_views/vw_dataops_monthly_failure.sql` | BigQuery view — monthly pipeline failure metrics for DataOps
`devops_resources/foundational_dm_resources/bigquery_dm_templates/dataops_views/vw_dataops_total_jobs_run.sql` | BigQuery view — total jobs run count for DataOps monitoring
`devops_resources/foundational_dm_resources/bigquery_dm_templates/dataops_views/vw_dataops_validation_metrics.sql` | BigQuery view — data validation metrics for DataOps
`devops_resources/foundational_dm_resources/bigquery_dm_templates/dataops_views/vw_dataops_weekly_failure.sql` | BigQuery view — weekly pipeline failure metrics for DataOps

##### finance_reports/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/finance_reports/bos_entry.sql` | BigQuery view/table — BOS (back-office system) entry report for finance
`devops_resources/foundational_dm_resources/bigquery_dm_templates/finance_reports/earned_not_invoiced.sql` | BigQuery view — revenue earned but not yet invoiced
`devops_resources/foundational_dm_resources/bigquery_dm_templates/finance_reports/invoiced_not_earned.sql` | BigQuery view — revenue invoiced but not yet earned
`devops_resources/foundational_dm_resources/bigquery_dm_templates/finance_reports/month_end_deposit.sql` | BigQuery view — month-end deposit reconciliation report

##### metric_views/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/metric_views/agreement_metrics.sql` | BigQuery view — fleet agreement KPI metrics
`devops_resources/foundational_dm_resources/bigquery_dm_templates/metric_views/driver_metrics.sql` | BigQuery view — driver performance KPI metrics
`devops_resources/foundational_dm_resources/bigquery_dm_templates/metric_views/fleet_composition_metrics.sql` | BigQuery view — fleet composition breakdown metrics
`devops_resources/foundational_dm_resources/bigquery_dm_templates/metric_views/fleet_metrics.sql` | BigQuery view — overall fleet performance metrics
`devops_resources/foundational_dm_resources/bigquery_dm_templates/metric_views/top_return_reasons.sql` | BigQuery view — top vehicle return reasons metric

##### report_views/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_car_model_year.sql` | BigQuery view — branch comparison by car model year
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_car_odometer.sql` | BigQuery view — branch comparison by car odometer reading
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_fleet.sql` | BigQuery view — branch comparison by fleet size
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_fleet_line_chart.sql` | BigQuery view — branch fleet comparison time series (line chart data)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_retention.sql` | BigQuery view — branch comparison by driver retention rate
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_comparison_retention_line_chart.sql` | BigQuery view — branch retention comparison time series (line chart data)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_car_and_driver_car.sql` | BigQuery view — branch performance report: car dimension
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_car_and_driver_driver.sql` | BigQuery view — branch performance report: driver dimension
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_community_driver.sql` | BigQuery view — branch performance for community driver segment
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_community_driver_list.sql` | BigQuery view — detailed list of community drivers by branch
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_fleet_retention_agreeement.sql` | BigQuery view — branch fleet retention by agreement
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_fleet_retention_car.sql` | BigQuery view — branch fleet retention by car
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_fleet_retention_driver.sql` | BigQuery view — branch fleet retention by driver
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_issues_car.sql` | BigQuery view — branch performance: car issues breakdown
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_issues_reason.sql` | BigQuery view — branch performance: issue reasons breakdown
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/branch_performance_top_return_reason.sql` | BigQuery view — branch performance: top return reasons
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/cars_revenue_profit_by_branch_estimated_profit_revenue_chart.sql` | BigQuery view — estimated profit/revenue by branch (chart data)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/report_views/cars_revenue_profit_by_branch_stage_10_car_chart.sql` | BigQuery view — stage-10 car revenue/profit by branch (chart data)

##### VGM R2 Reports SQL/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_action_program_tracking_high_hour_drivers_count.sql` | BigQuery view — count of high-hour drivers in action program tracking
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_action_program_tracking_high_hour_drivers_list.sql` | BigQuery view — list of high-hour drivers in action program tracking
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_model_year.sql` | BigQuery R2 view — branch comparison by car model year (R2 version)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_model_year_new_york_city.sql` | BigQuery R2 view — branch comparison car model year filtered to New York City
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_model_year_north_east.sql` | BigQuery R2 view — branch comparison car model year filtered to North East region
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_model_year_south_east.sql` | BigQuery R2 view — branch comparison car model year filtered to South East region
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_model_year_south_west.sql` | BigQuery R2 view — branch comparison car model year filtered to South West region
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_odometer.sql` | BigQuery R2 view — branch comparison by car odometer (R2 version)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_odometer_new_york_city.sql` | BigQuery R2 view — branch odometer comparison filtered to New York City
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_odometer_north_east.sql` | BigQuery R2 view — branch odometer comparison filtered to North East
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_odometer_south_east.sql` | BigQuery R2 view — branch odometer comparison filtered to South East
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_car_odometer_south_west.sql` | BigQuery R2 view — branch odometer comparison filtered to South West
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_composition.sql` | BigQuery R2 view — branch comparison by fleet composition (R2 version)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_composition_new_york_city.sql` | BigQuery R2 view — fleet composition comparison filtered to New York City
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_composition_north_east.sql` | BigQuery R2 view — fleet composition comparison filtered to North East
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_composition_south_east.sql` | BigQuery R2 view — fleet composition comparison filtered to South East
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_composition_south_west.sql` | BigQuery R2 view — fleet composition comparison filtered to South West
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_fleet_linechart.sql` | BigQuery R2 view — branch fleet comparison time series line chart
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_retention.sql` | BigQuery R2 view — branch comparison by retention rate (R2 version)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_branch_comparison_retention_linechart.sql` | BigQuery R2 view — branch retention comparison time series line chart
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_cars_revenue_profit_by_branch_cars_by_stages_linechart.sql` | BigQuery R2 view — cars by lifecycle stage over time (line chart)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_cars_revenue_profit_by_branch_estimated_profit_revenue_chart.sql` | BigQuery R2 view — estimated profit and revenue by branch (chart)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_cars_revenue_profit_by_branch_revenue_chart.sql` | BigQuery R2 view — revenue by branch (chart data)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_cars_revenue_profit_by_branch_stage_10_car_chart.sql` | BigQuery R2 view — stage-10 car revenue by branch (chart data)
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_agreement_duration_and_retention.sql` | BigQuery R2 view — data explorer: agreement duration and retention overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_agreement_duration_and_retention_agreement.sql` | BigQuery R2 view — data explorer: agreement duration/retention at agreement grain
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_agreement_duration_and_retention_driver.sql` | BigQuery R2 view — data explorer: agreement duration/retention at driver grain
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_avg_price.sql` | BigQuery R2 view — data explorer: average pricing overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_avg_price_table.sql` | BigQuery R2 view — data explorer: average pricing detail table
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_driver_behaviour_and_composition.sql` | BigQuery R2 view — data explorer: driver behaviour and fleet composition overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_driver_behaviour_and_composition_table.sql` | BigQuery R2 view — data explorer: driver behaviour and composition detail table
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_net_adds.sql` | BigQuery R2 view — data explorer: net fleet additions overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_net_adds_table_agreement.sql` | BigQuery R2 view — data explorer: net adds detail table at agreement grain
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_net_adds_table_driver.sql` | BigQuery R2 view — data explorer: net adds detail table at driver grain
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_net_adds_table_switches.sql` | BigQuery R2 view — data explorer: net adds detail for driver switches
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_repos_and_accident.sql` | BigQuery R2 view — data explorer: repossessions and accidents overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_repos_and_accident_table.sql` | BigQuery R2 view — data explorer: repos and accidents detail table
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_utilization.sql` | BigQuery R2 view — data explorer: fleet utilization overview
`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM R2 Reports SQL/vw_vgm_data_explorer_utilization_table.sql` | BigQuery R2 view — data explorer: fleet utilization detail table

##### VGM Reports/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/VGM Reports/vw_moic_report.sql` | BigQuery view — MOIC (Multiple on Invested Capital) report

##### validation_stored_procedures/

`devops_resources/foundational_dm_resources/bigquery_dm_templates/validation_stored_procedures/flespi_validation_checks_sp.sql` | BigQuery stored procedure — validates Flespi telematics data quality
`devops_resources/foundational_dm_resources/bigquery_dm_templates/validation_stored_procedures/profitability_validation_checks_sp.sql` | BigQuery stored procedure — validates profitability calculation data

#### function_dm_templates/

`devops_resources/foundational_dm_resources/function_dm_templates/function/main.py` | Deployment Manager Cloud Function template — generic Cloud Function scaffold
`devops_resources/foundational_dm_resources/function_dm_templates/function/requirements.txt` | Python dependencies for the generic Cloud Function template

#### pubsub_dm_templates/

`devops_resources/foundational_dm_resources/pubsub_dm_templates/pubsub_deployment_template.py` | Deployment Manager template — provisions Cloud Pub/Sub topics and subscriptions
`devops_resources/foundational_dm_resources/pubsub_dm_templates/pubsub_deployment_template.schema` | Schema for Pub/Sub deployment template

#### storage_buckets_dm_templates/

`devops_resources/foundational_dm_resources/storage_buckets_dm_templates/storage_deployment_template.py` | Deployment Manager template — provisions Cloud Storage buckets
`devops_resources/foundational_dm_resources/storage_buckets_dm_templates/storage_deployment_template.schema` | Schema for Cloud Storage deployment template

### foundational_resources_gitlab_pipelines/

`devops_resources/foundational_resources_gitlab_pipelines/deployment.sh` | Shell script — deploys foundational resources via Deployment Manager
`devops_resources/foundational_resources_gitlab_pipelines/foundational_resources_pipeline.yml` | GitLab CI pipeline definition for foundational resource deployment

### network_dm_resources/

`devops_resources/network_dm_resources/deploy_network_infra.sh` | Shell script — deploys VPC and network infrastructure
`devops_resources/network_dm_resources/vpc_network_dm_templates/deployment.py` | Deployment Manager template — provisions VPC network
`devops_resources/network_dm_resources/vpc_network_dm_templates/deployment.py.schema` | Schema for VPC network deployment template

### vgm_gitlab_runner_setup/

`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_deployment/deploy_gitlab_runner.sh` | Shell script — deploys the GitLab Runner VM
`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_deployment/deployment_template.py` | Deployment Manager template — provisions GitLab Runner VM
`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_deployment/deployment_template.py.schema` | Schema for GitLab Runner VM deployment template
`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_deployment/vm_deployment_config.yaml` | YAML config for GitLab Runner VM deployment
`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_docker_image/Dockerfile` | Dockerfile — custom Docker image for the GitLab Runner
`devops_resources/vgm_gitlab_runner_setup/gitLab_runner_docker_image/requirements.txt` | Python dependencies baked into the GitLab Runner Docker image

### vgm_jumpbox_server_setup/

`devops_resources/vgm_jumpbox_server_setup/deploy_jumpbox_server.sh` | Shell script — deploys the jumpbox (bastion) VM
`devops_resources/vgm_jumpbox_server_setup/deployment_template.py` | Deployment Manager template — provisions jumpbox VM
`devops_resources/vgm_jumpbox_server_setup/deployment_template.py.schema` | Schema for jumpbox VM deployment template
`devops_resources/vgm_jumpbox_server_setup/vm_deployment_config.yaml` | YAML config for jumpbox VM deployment

### vgm_sonarqube_server_setup/

`devops_resources/vgm_sonarqube_server_setup/deploy_sonarqube_server.sh` | Shell script — deploys the SonarQube code quality server VM
`devops_resources/vgm_sonarqube_server_setup/deployment_template.py` | Deployment Manager template — provisions SonarQube VM
`devops_resources/vgm_sonarqube_server_setup/deployment_template.py.schema` | Schema for SonarQube VM deployment template

---

## vgm_bigquery_api/

`vgm_bigquery_api/sonar-project.properties` | SonarQube project config for vgm_bigquery_api code quality scanning

### vgm_bigquery_api_gitlab_pipelines/

`vgm_bigquery_api/vgm_bigquery_api_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for BigQuery API (resource creation)
`vgm_bigquery_api/vgm_bigquery_api_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for BigQuery API (Cloud Function deploy)
`vgm_bigquery_api/vgm_bigquery_api_gitlab_pipelines/vgm_bigquery_api_resources_creation_pipeline.yml` | GitLab CI pipeline definition for BigQuery API resource creation and deployment

### vgm_bigquery_api_script/

`vgm_bigquery_api/vgm_bigquery_api_script/.env` | Environment variable file for local development of BigQuery API
`vgm_bigquery_api/vgm_bigquery_api_script/auth.py` | API key verification middleware for the BigQuery API
`vgm_bigquery_api/vgm_bigquery_api_script/config.py` | Configuration loader for BigQuery API (env vars, GCP project settings)
`vgm_bigquery_api/vgm_bigquery_api_script/Dockerfile` | Dockerfile — containerises the BigQuery API service
`vgm_bigquery_api/vgm_bigquery_api_script/main.py` | FastAPI application entry point with Mangum Cloud Function handler
`vgm_bigquery_api/vgm_bigquery_api_script/requirements.txt` | Python dependencies for the BigQuery API service
`vgm_bigquery_api/vgm_bigquery_api_script/routes/health.py` | FastAPI route — GET /health endpoint for BigQuery API
`vgm_bigquery_api/vgm_bigquery_api_script/routes/help.py` | FastAPI route — GET /help endpoint returning API usage documentation
`vgm_bigquery_api/vgm_bigquery_api_script/routes/metadata.py` | FastAPI route — GET /metadata endpoint for dataset and table metadata
`vgm_bigquery_api/vgm_bigquery_api_script/routes/query.py` | FastAPI route — POST /query endpoint for executing BigQuery queries
`vgm_bigquery_api/vgm_bigquery_api_script/tests/test_api.py` | Unit/integration tests for BigQuery API routes
`vgm_bigquery_api/vgm_bigquery_api_script/tests/test_incremental_capture.py` | Tests for incremental data capture logic in the BigQuery API

---

## vgm_callbell_bq/

`vgm_callbell_bq/sonar-project.properties` | SonarQube project config for vgm_callbell_bq code quality scanning

### vgm_callbell_to_bq_gitlab_pipelines/

`vgm_callbell_bq/vgm_callbell_to_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for Callbell ingestion pipeline
`vgm_callbell_bq/vgm_callbell_to_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for Callbell ingestion pipeline
`vgm_callbell_bq/vgm_callbell_to_bq_gitlab_pipelines/vgm_callbell_to_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for Callbell-to-BigQuery resource creation
`vgm_callbell_bq/vgm_callbell_to_bq_gitlab_pipelines/vgm_callbell_to_bq_workflow.yaml` | Google Cloud Workflows YAML — orchestrates Callbell ingestion execution

### vgm_callbell_to_bq_script/

`vgm_callbell_bq/vgm_callbell_to_bq_script/.env` | Environment variables for local development of Callbell ingestion
`vgm_callbell_bq/vgm_callbell_to_bq_script/bq_merge.py` | BigQuery MERGE operation helper — upserts Callbell data into OP tables
`vgm_callbell_bq/vgm_callbell_to_bq_script/Dockerfile` | Dockerfile — containerises the Callbell ingestion service
`vgm_callbell_bq/vgm_callbell_to_bq_script/main.py` | Main entry point — orchestrates concurrent Callbell API data fetch and BigQuery load
`vgm_callbell_bq/vgm_callbell_to_bq_script/tests/test_bq_merge.py` | Tests for BigQuery merge operations in Callbell pipeline
`vgm_callbell_bq/vgm_callbell_to_bq_script/tests/test_cloud_logger_source_bq.py` | Tests for Cloud Logging integration in Callbell pipeline
`vgm_callbell_bq/vgm_callbell_to_bq_script/tests/test_get_secret.py` | Tests for Secret Manager secret retrieval in Callbell pipeline
`vgm_callbell_bq/vgm_callbell_to_bq_script/tests/test_main.py` | Tests for main orchestration logic of Callbell ingestion

---

## vgm_chatbot_gen_ai/

`vgm_chatbot_gen_ai/Dockerfile` | Dockerfile — containerises the Gen-AI chatbot service
`vgm_chatbot_gen_ai/requirements.txt` | Python dependencies for the Gen-AI chatbot (FastAPI, LangChain, Vertex AI, etc.)
`vgm_chatbot_gen_ai/vgm_chatbot_gen_ai_resources_creation_pipeline.yml` | GitLab CI pipeline definition for Gen-AI chatbot resource creation and deployment

### app/

`vgm_chatbot_gen_ai/app/config.py` | Configuration loader for the Gen-AI chatbot (GCP project, Vertex AI settings)
`vgm_chatbot_gen_ai/app/main.py` | FastAPI application entry point for Gen-AI chatbot with CORS and route registration
`vgm_chatbot_gen_ai/app/models/chat_models.py` | Pydantic data models for chat request and response payloads
`vgm_chatbot_gen_ai/app/routes/chat.py` | FastAPI route — POST /chat endpoint for conversational AI queries
`vgm_chatbot_gen_ai/app/routes/health.py` | FastAPI route — GET /health endpoint for chatbot service
`vgm_chatbot_gen_ai/app/routes/whatsapp.py` | FastAPI route — POST /whatsapp endpoint for WhatsApp webhook integration
`vgm_chatbot_gen_ai/app/services/bigquery_client.py` | Service — BigQuery client for chatbot data retrieval queries
`vgm_chatbot_gen_ai/app/services/name_extractor.py` | Service — extracts entity names from natural language chat input
`vgm_chatbot_gen_ai/app/services/retrieval_qa.py` | Service — LangChain retrieval-augmented QA chain against BigQuery data
`vgm_chatbot_gen_ai/app/services/vector_search.py` | Service — vector similarity search for chatbot context retrieval
`vgm_chatbot_gen_ai/app/services/whatsapp_service.py` | Service — WhatsApp message handling and response formatting

---

## vgm_element_sftp_bq/

`vgm_element_sftp_bq/sonar-project.properties` | SonarQube project config for vgm_element_sftp_bq

### vgm_element_sftp_to_bq_gitlab_pipelines/

`vgm_element_sftp_bq/vgm_element_sftp_to_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_gitlab_pipelines/vgm_element_sftp_to_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for Element SFTP resource creation
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_gitlab_pipelines/vgm_element_sftp_to_bq_workflow.yaml` | Google Cloud Workflows YAML — orchestrates Element SFTP ingestion

### vgm_element_sftp_to_bq_script/

`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/.env` | Environment variables for local development of Element SFTP ingestion
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/Dockerfile` | Dockerfile — containerises the Element SFTP ingestion service
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/main.py` | Main entry point — orchestrates SFTP file retrieval and BigQuery load for Element data
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/src/cloud_logger.py` | Cloud Logging helper for Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/src/gcp_functions.py` | GCP utility functions (BigQuery, GCS operations) for Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/src/secret_manager_client.py` | Secret Manager client for Element SFTP pipeline credentials
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/src/sftp_operator.py` | SFTP connection and file download operator for Element data
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/src/vgm_notifier.py` | Notification helper (Teams/email alerts) for Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/tests/test_cloud_logger_sftp_bq.py` | Tests for Cloud Logging in Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/tests/test_gcp_functions_sftp_bq.py` | Tests for GCP utility functions in Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/tests/test_secret_manager_client_sftp_bq.py` | Tests for Secret Manager client in Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/tests/test_sftp_operator_sftp_bq.py` | Tests for SFTP operator in Element SFTP pipeline
`vgm_element_sftp_bq/vgm_element_sftp_to_bq_script/tests/test_vgm_notifier_sftp_bq.py` | Tests for notification helper in Element SFTP pipeline

---

## vgm_flespi_bq/

`vgm_flespi_bq/sonar-project.properties` | SonarQube project config for vgm_flespi_bq

### vgm_flespi_gs_files/

`vgm_flespi_bq/vgm_flespi_gs_files/vgm_gs_maintenance_history.csv` | Static CSV — vehicle maintenance history reference data for GCS upload
`vgm_flespi_bq/vgm_flespi_gs_files/vgm_gs_pepboys_invoices.csv` | Static CSV — Pep Boys maintenance invoices reference data for GCS upload

### vgm_flespi_sql_queries/flespi_historical_sql_queries/

`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/1_op_hourly_mileage_all_devices.sql` | SQL — computes hourly mileage across all Flespi devices (historical backfill)
`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/2_op_flespi_device_car_mapping.sql` | SQL — maps Flespi device IDs to vehicle records (historical)
`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/3_op_flespi_validation_mileage_records.sql` | SQL — validates mileage records for data quality (historical)
`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/4_op_flespi_validated_trackers.sql` | SQL — identifies validated/active Flespi tracker devices (historical)
`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/5_op_flespi_daily_mileage.sql` | SQL — aggregates daily mileage per vehicle from Flespi data (historical)
`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_historical_sql_queries/6_op_flespi_driver_profit.sql` | SQL — calculates driver profit using Flespi mileage data (historical)

### vgm_flespi_sql_queries/flespi_incremental_sql_queries/

`vgm_flespi_bq/vgm_flespi_sql_queries/flespi_incremental_sql_queries/1_op_incremental_hourly_mileage_all_devices.sql` | SQL — computes hourly mileage across all devices for incremental ingestion

### vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/

`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/.env` | Environment variables for Flespi OP historical data backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/bq_utility_op_historical.py` | BigQuery utility functions for Flespi OP historical data processing
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/cloud_logger_op_historical.py` | Cloud Logging helper for Flespi OP historical backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/Dockerfile` | Dockerfile — containerises Flespi OP historical backfill service
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/main_op_historical.py` | Entry point — executes Flespi operational SQL queries for historical backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/tests/test_execute_query_op_historical.py` | Tests for query execution in Flespi OP historical backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/tests/test_fetch_query_op_historical.py` | Tests for query fetching in Flespi OP historical backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/tests/test_log_to_bigquery_op_historical.py` | Tests for BigQuery logging in Flespi OP historical backfill
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_historical_data/tests/test_main_op_historical.py` | Tests for main orchestration in Flespi OP historical backfill

### vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/

`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/.env` | Environment variables for Flespi OP hourly incremental processing
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/bq_utility_op_incremental.py` | BigQuery utility functions for Flespi OP hourly incremental processing
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/cloud_logger_op_incremental.py` | Cloud Logging helper for Flespi OP hourly incremental processing
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/Dockerfile` | Dockerfile — containerises Flespi OP hourly incremental service
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/main_op_incremental.py` | Entry point — executes Flespi operational SQL queries on hourly incremental data
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/tests/test_execute_query_op_incremental.py` | Tests for query execution in Flespi OP hourly incremental
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/tests/test_fetch_query_op_incremental.py` | Tests for query fetching in Flespi OP hourly incremental
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/tests/test_log_to_bigquery_op_incremental.py` | Tests for BigQuery logging in Flespi OP hourly incremental
`vgm_flespi_bq/vgm_op_flespi_bq/vgm_op_flespi_bq_hourly_incremental_data/tests/test_main_op_incremental.py` | Tests for main orchestration in Flespi OP hourly incremental

### vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/

`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/.env` | Environment variables for Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/Dockerfile` | Dockerfile — containerises Flespi STG historical backfill service
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/main_stg_historical.py` | Entry point — fetches historical Flespi telemetry and stages into BigQuery STG
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_export_bq_to_gcs_stg_historical.py` | Tests for BigQuery-to-GCS export in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_fetch_all_device_ids_stg_historical.py` | Tests for fetching all Flespi device IDs in historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_fetch_historical_data.py` | Tests for Flespi API historical data fetch
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_get_secret_stg_historical.py` | Tests for secret retrieval in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_insert_data_into_bigquery_stg_historical.py` | Tests for BigQuery insert in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_insert_raw_data_stg_historical.py` | Tests for raw data insert in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_main_stg_historical.py` | Tests for main orchestration in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_process_all_devices_stg_historical.py` | Tests for multi-device processing in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_process_device_data_stg_historical.py` | Tests for per-device data processing in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_send_log_to_bigquery_stg_historical.py` | Tests for log shipping to BigQuery in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_transform_data_stg_historical.py` | Tests for data transformation in Flespi STG historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_historical_data/tests/test_transform_field_stg_historical.py` | Tests for field-level transformation in Flespi STG historical backfill

### vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/

`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/.env` | Environment variables for Flespi STG hourly incremental ingestion
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/Dockerfile` | Dockerfile — containerises Flespi STG hourly incremental service
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/get_secret_stg_incremental.py` | Secret Manager helper for Flespi STG hourly incremental ingestion
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/main_stg_incremental.py` | Entry point — fetches hourly incremental Flespi telemetry and stages into BigQuery STG
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/vgm_notifier_stg_incremental.py` | Notification helper for Flespi STG hourly incremental pipeline alerts
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_export_to_gcs_stg_incremental.py` | Tests for GCS export in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_extract_rows_stg_incremental.py` | Tests for row extraction in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_fetch_telemtry_data_stg_incremental.py` | Tests for Flespi telemetry API fetch in hourly incremental ingestion
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_get_last_updated_timestamp_stg_incremental.py` | Tests for watermark timestamp retrieval in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_get_secret_stg_incremental.py` | Tests for secret retrieval in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_insert_into_bigquery_stg_incremental.py` | Tests for BigQuery insert in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_insert_log_entry_stg_incremental.py` | Tests for log entry insert in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_insert_raw_data_stg_incremental.py` | Tests for raw data insert in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_main_stg_incremental.py` | Tests for main orchestration in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_transform_field_stg_incremental.py` | Tests for field transformation in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_transform_rows_stg_incremental.py` | Tests for row-level transformation in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_update_last_updated_timetstamp_stg_incremental.py` | Tests for watermark timestamp update in Flespi STG hourly incremental
`vgm_flespi_bq/vgm_stg_flespi_bq/vgm_stg_flespi_bq_hourly_incremental_data/tests/test_vgm_notifier_stg_incremental.py` | Tests for notification helper in Flespi STG hourly incremental

### vgm_stg_flespi_bq_gitlab_pipelines/

`vgm_flespi_bq/vgm_stg_flespi_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for Flespi STG pipeline
`vgm_flespi_bq/vgm_stg_flespi_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for Flespi STG pipeline
`vgm_flespi_bq/vgm_stg_flespi_bq_gitlab_pipelines/flespi_bq_historical_workflow.yaml` | Google Cloud Workflows YAML — orchestrates Flespi historical backfill
`vgm_flespi_bq/vgm_stg_flespi_bq_gitlab_pipelines/flespi_bq_hourly_incremental_workflow.yaml` | Google Cloud Workflows YAML — orchestrates Flespi hourly incremental ingestion
`vgm_flespi_bq/vgm_stg_flespi_bq_gitlab_pipelines/vgm_stg_flespi_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for Flespi STG resource creation and deployment

---

## vgm_op_sql_execution/

`vgm_op_sql_execution/sonar-project.properties` | SonarQube project config for vgm_op_sql_execution

### vgm_op_gs_files/

`vgm_op_sql_execution/vgm_op_gs_files/vgm_gs_pepboys.csv` | Static CSV — Pep Boys vendor reference data for GCS upload
`vgm_op_sql_execution/vgm_op_gs_files/vgm_gs_pepboys_workorderlines.csv` | Static CSV — Pep Boys work order line items reference data for GCS upload

### vgm_op_queries/operational_queries/

`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_3cx_call_insights.sql` | SQL — transforms 3CX call centre data into operational call insights table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_accident_cost_prediction.sql` | SQL — predicts accident costs for fleet vehicles
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_adaptive_edw_pl_ratio_calculation.sql` | SQL — calculates adaptive P&L ratio for EDW reporting
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_ar_report.sql` | SQL — generates accounts receivable operational report
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_argyle_daily_driver_account_status.sql` | SQL — daily driver account status from Argyle gig data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_argyle_daily_export_to_cf.py` | Python — exports Argyle daily data to Cloud Functions for downstream use
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_argyle_trip.sql` | SQL — transforms Argyle trip data into operational trip table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_billing_transaction.sql` | SQL — transforms Business Central billing transactions into OP table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_cars_agreement.sql` | SQL — transforms Business Central car agreement data into OP table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_cars_driver.sql` | SQL — transforms Business Central car-driver relationship data into OP table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_daily_car_driver_mapping.sql` | SQL — daily car-to-driver mapping from Business Central data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_daily_driver_profit.sql` | SQL — calculates daily driver profit from Business Central data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_minave_custledgerentry_translated.sql` | SQL — translates Business Central/Minave customer ledger entries
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_minave_genledgerentry_translated.sql` | SQL — translates Business Central/Minave general ledger entries
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_report_carcensus.sql` | SQL — car census report from Business Central source data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bc_revenue_fix_1.py` | Python — one-time revenue correction script for Business Central data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_billing_transaction.sql` | SQL — unified billing transaction operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_api_car_registration_insurance.sql` | SQL — car registration and insurance data from BOS API
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_billing_transaction_daily_incr.sql` | SQL — daily incremental billing transactions from BOS system
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_billing_transaction_t4_incr.sql` | SQL — T4 incremental billing transactions from BOS system
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_cars_odometer_reading_cleansed.py` | Python — cleansed odometer readings from BOS car data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_cars_odometer_reading_isolation_cleansed.py` | Python — isolation forest cleansing for BOS odometer anomalies
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_cars_odometer_reading_linear_cleansed.py` | Python — linear interpolation cleansing for BOS odometer readings
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_cars_odometer_reading_raw.sql` | SQL — raw BOS odometer readings staging query
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_car_driver_mapping.sql` | SQL — daily car-driver mapping from BOS system
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_driver_closing_balance.sql` | SQL — daily driver closing balance from BOS billing data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_driver_profit.sql` | SQL — daily driver profit calculation from BOS data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_driver_tenure.sql` | SQL — driver tenure calculation from BOS agreement data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_fleet_odometer_reading.sql` | SQL — daily fleet-level odometer readings from BOS
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_fleet_odometer_reading_intermediate.sql` | SQL — intermediate step in daily fleet odometer reading pipeline
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_fleet_odometer_reading_snapshot.sql` | SQL — daily snapshot of fleet odometer readings from BOS
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_daily_odometer_reading_anomaly_alerts.py` | Python — detects and alerts on anomalous odometer readings from BOS
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_entry_report.sql` | SQL — BOS entry report for finance reconciliation
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_bos_reversion_version_flattened.py` | Python — flattens BOS record version history for analysis
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_callbell_messages_aggregation.sql` | SQL — aggregates Callbell message data into operational summary
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_agreement.sql` | SQL — unified car agreement operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_carcondition.sql` | SQL — car condition records operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_carreturn.sql` | SQL — car return records operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_driver.sql` | SQL — unified car-driver relationship operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_driver_cross_reference.sql` | SQL — cross-reference table mapping drivers across source systems
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_driver_cross_reference_unmapped.sql` | SQL — identifies drivers not yet mapped across source systems
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_mileage_outlier_alerts.py` | Python — detects and alerts on mileage outliers across fleet
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_resale.sql` | SQL — car resale value operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_cars_reservation.sql` | SQL — car reservation operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_change_log_builder.py` | Python — builds change log from source record version history
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_collision.sql` | SQL — collision/accident records operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_crm_cars_car.sql` | SQL — car master data from CRM system into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_crm_workorders.sql` | SQL — work order data from CRM system into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_customers_lead.sql` | SQL — customer lead records operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_daily_car_distance_from_branch.sql` | SQL — daily distance of each car from its home branch (telematics-derived)
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_daily_car_driver_mapping.sql` | SQL — unified daily car-driver mapping across all sources
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_daily_device_tracking_status.sql` | SQL — daily status of Flespi tracking devices per vehicle
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_daily_driver_profit.sql` | SQL — unified daily driver profit operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_daily_fleet_odometer_reading_interpolated.py` | Python — interpolates missing daily fleet odometer readings
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_device_tracking_status_alerts.py` | Python — alerts on Flespi device tracking status anomalies
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_driver_ar_segment_color.sql` | SQL — assigns AR segment colour codes to drivers based on balance
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_driver_ar_segment_color_alerts.py` | Python — sends alerts when driver AR segment colour changes
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_driver_days_to_pay_transaction_grain.sql` | SQL — calculates days-to-pay metric at transaction grain for drivers
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_driver_retention_score.sql` | SQL — computes driver retention score from agreement history
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_fleetsync_car_valuations.sql` | SQL — car valuation data from FleetSync into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_fleetsync_report_carcensus.sql` | SQL — car census report from FleetSync source data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_flespi_branch_avg_percentile_thresholds.sql` | SQL — computes branch-level average and percentile thresholds for Flespi metrics
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_flespi_daily_driver_behavior_score.sql` | SQL — daily driver behaviour scoring from Flespi telematics
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_flespi_daily_mileage.sql` | SQL — daily mileage per vehicle from Flespi telematics
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_flespi_device_car_mapping.sql` | SQL — maps Flespi device IDs to vehicle records (operational incremental)
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_flespi_voyo_devices_raw.sql` | SQL — raw Flespi Voyo device data staging
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_maintenance.sql` | SQL — vehicle maintenance records operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_maintenance_costPerMile.sql` | SQL — cost-per-mile metric from maintenance records
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_maintenance_odometer_lookup.sql` | SQL — odometer lookup for maintenance event cost calculations
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_maintenance_workorder_line.sql` | SQL — work order line items from maintenance records
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_manheim_valuations_trims.sql` | SQL — Manheim vehicle valuation and trim data into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_accident_translated.sql` | SQL — Minave accident records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_agreement_status_translated.sql` | SQL — Minave agreement status records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_billing_transaction.sql` | SQL — Minave billing transactions into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_car_translated.sql` | SQL — Minave car records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cars_agreement.sql` | SQL — Minave car agreement records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cars_carcondition.sql` | SQL — Minave car condition records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cars_carreturn.sql` | SQL — Minave car return records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cars_driver.sql` | SQL — Minave car-driver relationship records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cars_reservation.sql` | SQL — Minave car reservation records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cdmx_2024_translated.sql` | SQL — Minave CDMX (Mexico City) 2024 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_cdmx_2025_translated.sql` | SQL — Minave CDMX 2025 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_customer_address_translated.sql` | SQL — Minave customer address records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_customer_translated.sql` | SQL — Minave customer records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_customers_lead.sql` | SQL — Minave customer lead records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_daily_car_driver_mapping.sql` | SQL — daily car-driver mapping from Minave source data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_gsheets_workorder.sql` | SQL — Minave work order data from Google Sheets into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_guadalajara_2022_translated.sql` | SQL — Minave Guadalajara 2022 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_guadalajara_2023_translated.sql` | SQL — Minave Guadalajara 2023 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_guadalajara_2024_translated.sql` | SQL — Minave Guadalajara 2024 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_pricing_agreementprice.sql` | SQL — Minave agreement pricing records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_puebla_2023_translated.sql` | SQL — Minave Puebla 2023 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_puebla_2024_translated.sql` | SQL — Minave Puebla 2024 records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_recovered_car_translated.sql` | SQL — Minave recovered car records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_return_car_translated.sql` | SQL — Minave car return records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_return_reason_translated.sql` | SQL — Minave return reason records translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_spreadsheet_processing.py` | Python — processes Minave spreadsheet uploads into BigQuery
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_tbl_autoordenservicio_translated.sql` | SQL — Minave auto work order service table translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_tbl_detalleordenservicio_translated.sql` | SQL — Minave work order detail table translated to English
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_minave_workorder.sql` | SQL — Minave work order records into operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_monday_board_subro_claims.py` | Python — fetches subrogation claims from Monday.com board into BigQuery
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_monthly_car_mileage.sql` | SQL — monthly mileage aggregation per car
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_monthly_vehicle_mileage_by_branch.sql` | SQL — monthly vehicle mileage aggregated by branch
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_pricing_agreementprice.sql` | SQL — unified agreement pricing operational table
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_repo_prediction.sql` | SQL — repossession risk prediction model query
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_repo_prediction_weekly_alerts.py` | Python — sends weekly repossession prediction alerts to stakeholders
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_report_carcensus.sql` | SQL — car census operational report (primary source)
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_report_carcensus_unified.sql` | SQL — unified car census combining all source systems
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_report_uw_score.sql` | SQL — underwriting score report for driver risk assessment
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_revenue_fix_1.sql` | SQL — revenue correction/adjustment query (version 1)
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_review_table_agreement.sql` | SQL — review/audit table for agreement records
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_review_table_driver.sql` | SQL — review/audit table for driver records
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_stripe_driver_risk_score.sql` | SQL — driver risk score derived from Stripe payment data
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_telematics_daily_all_devices.sql` | SQL — daily telematics summary across all tracked devices
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_temp_table_agreement_to_bos.sql` | SQL — temporary table mapping agreements to BOS system IDs
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_temp_table_driver_to_bos.sql` | SQL — temporary table mapping drivers to BOS system IDs
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_twilio_3cx_agents_mapped.sql` | SQL — maps Twilio agents to 3CX call centre agents
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_uber_accidents_combined.py` | Python — combines Uber accident data with internal fleet accident records
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_driver_percentile.sql` | SQL — weekly driver performance percentile ranking
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_driver_traffic_light_rating.sql` | SQL — weekly traffic light (RAG) rating per driver
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_high_risky_drivers.sql` | SQL — identifies high-risk drivers on a weekly basis
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_high_risky_drivers_alerts.py` | Python — sends weekly alerts for high-risk driver list
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_safe_driver_tracking.sql` | SQL — weekly tracking of safe driver programme participation
`vgm_op_sql_execution/vgm_op_queries/operational_queries/op_weekly_vehicle_mileage_by_branch.sql` | SQL — weekly vehicle mileage aggregated by branch

### vgm_op_queries/operational_queries_backup/

`vgm_op_sql_execution/vgm_op_queries/operational_queries_backup/op_bc_revenue_fix_1_bkp.py` | Python backup — previous version of Business Central revenue fix script
`vgm_op_sql_execution/vgm_op_queries/operational_queries_backup/op_maintenance_bkp.sql` | SQL backup — previous version of maintenance operational query

### vgm_op_queries/validation_queries/

`vgm_op_sql_execution/vgm_op_queries/validation_queries/finance_setting_validation.sql` | SQL — validates finance settings configuration in BigQuery
`vgm_op_sql_execution/vgm_op_queries/validation_queries/transaction_added.sql` | SQL — validates newly added billing transactions
`vgm_op_sql_execution/vgm_op_queries/validation_queries/transaction_hard_delete.sql` | SQL — validates hard-deleted billing transaction records
`vgm_op_sql_execution/vgm_op_queries/validation_queries/transaction_modified.sql` | SQL — validates modified billing transaction records
`vgm_op_sql_execution/vgm_op_queries/validation_queries/transaction_soft_delete.sql` | SQL — validates soft-deleted billing transaction records

### vgm_op_scheduled_queries/

`vgm_op_sql_execution/vgm_op_scheduled_queries/ar_report_lookup.sql` | SQL — scheduled AR report lookup query for accounts receivable monitoring
`vgm_op_sql_execution/vgm_op_scheduled_queries/op_flespi_validated_mileage_records.sql` | SQL — scheduled query: Flespi validated mileage records
`vgm_op_sql_execution/vgm_op_scheduled_queries/op_flespi_validated_trackers.sql` | SQL — scheduled query: Flespi validated tracker devices

### vgm_op_sql_execution_gitlab_pipelines/

`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for OP SQL execution pipeline
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for OP SQL execution pipeline
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/op_sql_execution_workflow.yaml` | Google Cloud Workflows YAML — orchestrates operational SQL execution
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_repo_prediciton_prod/main.py` | Cloud Function — production repossession prediction model Cloud Function entry point
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_repo_prediciton_prod/requirements.txt` | Python dependencies for the repossession prediction Cloud Function
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_sql_execution_resources_creation_pipeline.yml` | GitLab CI pipeline definition for OP SQL execution resource creation
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_translation_cloud_function/main.py` | Cloud Function — translates Minave (Spanish) records to English using Cloud Translate API
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_translation_cloud_function/requirements.txt` | Python dependencies for the translation Cloud Function
`vgm_op_sql_execution/vgm_op_sql_execution_gitlab_pipelines/vgm_op_translation_cloud_function/tests/test_main.py` | Tests for the translation Cloud Function

### vgm_op_sql_execution_script/

`vgm_op_sql_execution/vgm_op_sql_execution_script/.env` | Environment variables for local development of OP SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/bos_entry_report_export.py` | BOS entry report export helper — exports report data to downstream consumers
`vgm_op_sql_execution/vgm_op_sql_execution_script/bq_utility_op.py` | BigQuery utility functions for operational SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/cloud_logger_op.py` | Cloud Logging helper for operational SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/Dockerfile` | Dockerfile — containerises the operational SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/main_op.py` | Main entry point — fetches and executes operational SQL queries from GCS against BigQuery
`vgm_op_sql_execution/vgm_op_sql_execution_script/vgm_notifier_op.py` | Notification helper (Teams/email) for operational SQL execution pipeline
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_bos_entry_report_export.py` | Tests for BOS entry report export helper
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_execute_py.py` | Tests for Python-based operational query execution
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_execute_query.py` | Tests for SQL query execution in OP SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_fetch_query.py` | Tests for SQL query file fetch from GCS
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_insert_log_msg.py` | Tests for log message insert in OP SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_main_op.py` | Tests for main orchestration in OP SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_send_to_mondaycom.py` | Tests for Monday.com notification sending in OP SQL execution
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_validation.py` | Tests for data validation logic in OP SQL execution service
`vgm_op_sql_execution/vgm_op_sql_execution_script/tests/test_vgm_notifier_op.py` | Tests for notification helper in OP SQL execution service

---

## vgm_powerbi_migration/

### vgm_powerbi_migration_script/

`vgm_powerbi_migration/vgm_powerbi_migration_script/.env` | Environment variables for Power BI migration script
`vgm_powerbi_migration/vgm_powerbi_migration_script/migration_script.py` | Main script — automates Power BI report migration between tenants via GCS
`vgm_powerbi_migration/vgm_powerbi_migration_script/README.md` | Documentation — Power BI migration script functions and test guide
`vgm_powerbi_migration/vgm_powerbi_migration_script/sonar-project.properties` | SonarQube project config for vgm_powerbi_migration
`vgm_powerbi_migration/vgm_powerbi_migration_script/tests/test_migration_script.py` | Tests for Power BI migration script functions

### vgm_powerbi_reports/

`vgm_powerbi_migration/vgm_powerbi_reports/Miami Branch Manager Dashboard.pbix` | Power BI report file — Miami Branch Manager Dashboard (binary .pbix)

---

## vgm_source_bq/

`vgm_source_bq/sonar-project.properties` | SonarQube project config for vgm_source_bq

### vgm_op_flespi_bq_minute_incremental_data/

`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/.env` | Environment variables for Flespi minute-level incremental ingestion
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/cloud_logger.py` | Cloud Logging helper for Flespi minute-level incremental pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/Dockerfile` | Dockerfile — containerises Flespi minute-level incremental service
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/get_secret.py` | Secret Manager helper for Flespi minute-level incremental pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/main_minute_incremental.py` | Entry point — ingests Flespi telematics data at minute-level granularity into BigQuery
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/vgm_notifier.py` | Notification helper for Flespi minute-level incremental pipeline alerts
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_flespi_bq_bigquery_functions.py` | Tests for BigQuery functions in Flespi minute-level pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_flespi_bq_cloud_logger.py` | Tests for Cloud Logging in Flespi minute-level pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_flespi_bq_execute_query.py` | Tests for query execution in Flespi minute-level pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_flespi_bq_get_secret.py` | Tests for secret retrieval in Flespi minute-level pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_flespi_bq_vgm_notifier.py` | Tests for notification helper in Flespi minute-level pipeline
`vgm_source_bq/vgm_op_flespi_bq_minute_incremental_data/tests/test_main_minute_incremental.py` | Tests for main orchestration in Flespi minute-level pipeline

### vgm_source_to_bq_gitlab_pipelines/

`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for VGM source ingestion pipeline
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for VGM source ingestion pipeline
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/function/main.py` | Cloud Function entry point — generic VGM source data trigger function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/function/requirements.txt` | Python dependencies for generic VGM source trigger Cloud Function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/function/tests/test_main.py` | Tests for generic VGM source trigger Cloud Function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/vgm_source_to_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for VGM source-to-BigQuery resource creation
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/vgm_source_to_bq_workflow.yaml` | Google Cloud Workflows YAML — orchestrates VGM source ingestion

#### argyle_cloud_function/vgm-edw-function-argyle-merge-exec/

`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/.env` | Environment variables for Argyle merge execution Cloud Function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/main.py` | Cloud Function — executes BigQuery MERGE for Argyle gig economy data
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/requirements.txt` | Python dependencies for Argyle merge execution Cloud Function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/sql_scripts/op_argyle_accounts.sql` | SQL — MERGE statement for Argyle driver account records
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/sql_scripts/op_argyle_accounts_status.sql` | SQL — MERGE statement for Argyle driver account status records
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/sql_scripts/op_argyle_gigs.sql` | SQL — MERGE statement for Argyle gig records
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/sql_scripts/op_argyle_gigs_event.sql` | SQL — MERGE statement for Argyle gig event records
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-merge-exec/sql_scripts/op_argyle_gigs_income_alldatetime.sql` | SQL — MERGE statement for Argyle gig income across all datetimes

#### argyle_cloud_function/vgm-edw-function-argyle-webhook/

`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-webhook/.env` | Environment variables for Argyle webhook Cloud Function
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-webhook/main.py` | Cloud Function — receives Argyle webhook events and writes to BigQuery STG
`vgm_source_bq/vgm_source_to_bq_gitlab_pipelines/argyle_cloud_function/vgm-edw-function-argyle-webhook/requirements.txt` | Python dependencies for Argyle webhook Cloud Function

### vgm_source_to_bq_script/

`vgm_source_bq/vgm_source_to_bq_script/.env` | Environment variables for local development of VGM source ingestion
`vgm_source_bq/vgm_source_to_bq_script/bq_merge.py` | BigQuery MERGE helper — upserts VGM source data into OP tables
`vgm_source_bq/vgm_source_to_bq_script/cloud_logger.py` | Cloud Logging helper for VGM source ingestion pipeline
`vgm_source_bq/vgm_source_to_bq_script/Dockerfile` | Dockerfile — containerises the VGM source ingestion service
`vgm_source_bq/vgm_source_to_bq_script/get_secret.py` | Secret Manager helper for VGM source ingestion pipeline
`vgm_source_bq/vgm_source_to_bq_script/main_script.py` | Main entry point — orchestrates ingestion of multiple source systems (BOS, Argyle, Minave, Stripe, Uber, Element, Dynamics, FleetSync, Manheim, Solo, 3CX) into BigQuery
`vgm_source_bq/vgm_source_to_bq_script/vgm_notifier.py` | Notification helper for VGM source ingestion pipeline alerts
`vgm_source_bq/vgm_source_to_bq_script/tests/test_3cx.py` | Tests for 3CX call data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_argyle_functions.py` | Tests for Argyle gig data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_bigquery_functions.py` | Tests for BigQuery utility functions in source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_bq_merge.py` | Tests for BigQuery MERGE operations in source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_claims_functions.py` | Tests for insurance claims data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_cloud_logger_source_bq.py` | Tests for Cloud Logging in source ingestion pipeline
`vgm_source_bq/vgm_source_to_bq_script/tests/test_dynamics_api.py` | Tests for Microsoft Dynamics API data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_element_api.py` | Tests for Element API data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_export_bgq_to_gcs.py` | Tests for BigQuery-to-GCS export in source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_fleetsync.py` | Tests for FleetSync data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_get_secret.py` | Tests for secret retrieval in source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_load_to_bq.py` | Tests for BigQuery load operations in source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_main_script.py` | Tests for main orchestration in VGM source ingestion
`vgm_source_bq/vgm_source_to_bq_script/tests/test_manheim_functions.py` | Tests for Manheim vehicle valuation data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_minave_functions.py` | Tests for Minave (Mexico market) data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_rdbms.py` | Tests for relational database (RDBMS) source ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_solo_functions.py` | Tests for Solo platform data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_stripe_functions.py` | Tests for Stripe payment data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_uber_functions.py` | Tests for Uber gig data ingestion functions
`vgm_source_bq/vgm_source_to_bq_script/tests/test_vgm_notifier.py` | Tests for notification helper in VGM source ingestion

---

## vgm_telematics_bq/

`vgm_telematics_bq/sonar-project.properties` | SonarQube project config for vgm_telematics_bq

### vgm_telematics_to_bq_gitlab_pipelines/

`vgm_telematics_bq/vgm_telematics_to_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for telematics ingestion pipeline
`vgm_telematics_bq/vgm_telematics_to_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for telematics ingestion pipeline
`vgm_telematics_bq/vgm_telematics_to_bq_gitlab_pipelines/vgm_telematics_to_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for telematics-to-BigQuery resource creation
`vgm_telematics_bq/vgm_telematics_to_bq_gitlab_pipelines/vgm_telematics_to_bq_workflow.yaml` | Google Cloud Workflows YAML — orchestrates telematics hourly ingestion

### vgm_telematics_to_bq_script/

`vgm_telematics_bq/vgm_telematics_to_bq_script/.env` | Environment variables for local development of telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/cloud_logger.py` | Cloud Logging helper for telematics ingestion pipeline
`vgm_telematics_bq/vgm_telematics_to_bq_script/Dockerfile` | Dockerfile — containerises telematics ingestion service
`vgm_telematics_bq/vgm_telematics_to_bq_script/get_secret.py` | Secret Manager helper for telematics ingestion pipeline
`vgm_telematics_bq/vgm_telematics_to_bq_script/main.py` | Entry point — hourly incremental Flespi telematics data ingestion into BigQuery
`vgm_telematics_bq/vgm_telematics_to_bq_script/vgm_notifier.py` | Notification helper (Teams/email) for telematics ingestion pipeline alerts
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_bigquery_functions.py` | Tests for BigQuery functions in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_cloud_logger.py` | Tests for Cloud Logging in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_execute_query.py` | Tests for query execution in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_get_secret.py` | Tests for secret retrieval in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_main.py` | Tests for main orchestration in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_merge.py` | Tests for BigQuery MERGE operations in telematics ingestion
`vgm_telematics_bq/vgm_telematics_to_bq_script/tests/test_telematics_bq_vgm_notifier.py` | Tests for notification helper in telematics ingestion

---

## vgm_twilio_bq/

`vgm_twilio_bq/sonar-project.properties` | SonarQube project config for vgm_twilio_bq

### vgm_twilio_to_bq_gitlab_pipelines/

`vgm_twilio_bq/vgm_twilio_to_bq_gitlab_pipelines/deployment1.sh` | Shell script — first deployment step for Twilio ingestion pipeline
`vgm_twilio_bq/vgm_twilio_to_bq_gitlab_pipelines/deployment2.sh` | Shell script — second deployment step for Twilio ingestion pipeline
`vgm_twilio_bq/vgm_twilio_to_bq_gitlab_pipelines/vgm_twilio_to_bq_resources_creation_pipeline.yml` | GitLab CI pipeline definition for Twilio-to-BigQuery resource creation
`vgm_twilio_bq/vgm_twilio_to_bq_gitlab_pipelines/vgm_twilio_to_bq_workflow.yaml` | Google Cloud Workflows YAML — orchestrates Twilio ingestion

### vgm_twilio_to_bq_script/

`vgm_twilio_bq/vgm_twilio_to_bq_script/.env` | Environment variables for local development of Twilio ingestion
`vgm_twilio_bq/vgm_twilio_to_bq_script/bq_merge.py` | BigQuery MERGE helper — upserts Twilio data into OP tables
`vgm_twilio_bq/vgm_twilio_to_bq_script/bq_utilty.py` | BigQuery utility functions for Twilio ingestion pipeline
`vgm_twilio_bq/vgm_twilio_to_bq_script/Dockerfile` | Dockerfile — containerises Twilio ingestion service
`vgm_twilio_bq/vgm_twilio_to_bq_script/main_script.py` | Main entry point — orchestrates Twilio call/SMS data fetch and BigQuery load
`vgm_twilio_bq/vgm_twilio_to_bq_script/requirements.txt` | Python dependencies for Twilio ingestion service
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_call_insights.py` | Fetches and processes Twilio call insights data
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_call_insights_retry.py` | Retry logic for Twilio call insights data fetching
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_driver_analysis.py` | Analyses Twilio call data per driver for operational insights
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_driver_analysis_retry.py` | Retry logic for Twilio driver analysis data fetching
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_excel_wrap.py` | Wraps Twilio report data into Excel format for distribution
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_recording_properties.py` | Fetches and processes Twilio call recording metadata
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_report_generation.py` | Generates Twilio daily communication reports
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_send_email.py` | Sends generated Twilio reports via email (SendGrid)
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_transcription.py` | Fetches and stores Twilio call transcription data
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_weekly_driver_analysis.py` | Weekly Twilio driver call analysis
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_weekly_driver_analysis_retry.py` | Retry logic for weekly Twilio driver analysis
`vgm_twilio_bq/vgm_twilio_to_bq_script/twilio_weekly_report_generation.py` | Generates weekly Twilio communication reports
`vgm_twilio_bq/vgm_twilio_to_bq_script/vgm_notifier.py` | Notification helper (Teams/email) for Twilio ingestion pipeline alerts
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_bq_merge.py` | Tests for BigQuery MERGE in Twilio pipeline
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_bq_utilty.py` | Tests for BigQuery utility functions in Twilio pipeline
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_main_script.py` | Tests for main orchestration in Twilio ingestion
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_call_insights.py` | Tests for Twilio call insights processing
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_call_insights_retry.py` | Tests for Twilio call insights retry logic
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_driver_analysis.py` | Tests for Twilio driver analysis processing
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_driver_analysis_retry.py` | Tests for Twilio driver analysis retry logic
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_excel_wrap.py` | Tests for Twilio Excel report wrapping
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_recording_properties.py` | Tests for Twilio recording properties fetching
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_report_generation.py` | Tests for Twilio daily report generation
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_send_email.py` | Tests for Twilio email sending via SendGrid
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_transcription.py` | Tests for Twilio call transcription fetching
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_weekly_driver_analysis.py` | Tests for weekly Twilio driver analysis
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_weekly_driver_analysis_retry.py` | Tests for weekly Twilio driver analysis retry logic
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_twilio_weekly_report_generation.py` | Tests for weekly Twilio report generation
`vgm_twilio_bq/vgm_twilio_to_bq_script/tests/test_vgm_notifier.py` | Tests for notification helper in Twilio ingestion

---

## Notable observations for engineer review

1. **`vgm_source_to_bq_script/main_script.py`** is the highest-complexity entry point — test file names reveal it ingests from at least 11 sources: BOS, Argyle, Minave, Stripe, Uber, Element, Dynamics (Microsoft), FleetSync, Manheim, Solo, 3CX. This was not fully visible from the intake.
2. **Minave** appears to be a Spanish-language fleet management system (Mexico market) — a significant sub-system with translation pipelines, regional datasets (CDMX, Guadalajara, Puebla), and dedicated Cloud Functions. Not named in the intake.
3. **Argyle** (gig economy data aggregator) has a dedicated webhook Cloud Function and merge execution function — real-time inbound event integration not mentioned in intake.
4. **Microsoft Dynamics** appears as a source system (`test_dynamics_api.py`) — not mentioned in intake.
5. **FleetSync**, **Manheim**, **Solo**, **3CX** are additional source systems surfaced only from test file names — none named in intake.
6. **`vgm_op_sql_execution`** is larger than expected — 125+ SQL files and Python scripts covering fleet analytics, driver scoring, financial reporting, and ML-based predictions (repo prediction, odometer anomaly detection, mileage interpolation).
7. **`discovery/annotations/`** directory exists (created earlier) but contains no files yet — placeholder for Stage 2 annotations.
