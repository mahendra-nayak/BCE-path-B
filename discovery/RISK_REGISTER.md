STAGE-1-DRAFT: INTAKE-DERIVED — 2026-04-22 — Path B Stranger-Led

# RISK_REGISTER.md — VGM Data Platform

> Source: INTAKE_SUMMARY.md — Known Pain Points
> Confidence: LOW — intake was based solely on README documentation with no code or custodian validation.

No prior risk knowledge — to be populated from code extraction in Stage 2.

INTAKE_SUMMARY.md explicitly states: "No explicit pain points are mentioned in the README."

Stage 2 source code inspection is required to populate this register.

---

## Placeholder Register

| Risk ID | Description | Severity | Source Artifact | Mitigation | Recommended Action |
|---------|-------------|----------|-----------------|------------|--------------------|
| — | No pain points identified from intake | NOT DETERMINABLE FROM SOURCE | INTAKE_SUMMARY.md | NOT DETERMINABLE FROM SOURCE | NOT DETERMINABLE FROM SOURCE |

---

## Latent Risk Signals from Open Questions

The following open questions from INTAKE_SUMMARY.md represent areas of unknown risk
that Stage 2 should actively investigate:

1. **ETL failure handling** — "How are failures, retries, and data quality issues handled in the ETL pipelines?" — Risk of silent data loss or duplicate ingestion if not handled.
2. **Module communication model** — "How do different modules communicate (event-driven vs scheduled vs API-based)?" — Risk of coupling or missed triggers.
3. **Security model** — "How is security handled across APIs and data access layers?" — Risk of unauthorised data access.
4. **Schema governance** — "What are the exact data schemas and transformations between STG and OP datasets?" — Risk of schema drift breaking downstream consumers.
5. **Scaling characteristics** — "What are the performance and scaling characteristics of the system?" — Risk of capacity-related failures under load.
6. **Ownership and lifecycle** — "What is the ownership and lifecycle of each module?" — Risk of unmaintained modules with no clear owner.
7. **Consumer scope** — "Who are the primary consumers (internal teams, clients, or both)?" — Risk of unmanaged external dependencies on internal API contracts.

These are not confirmed risks — they are candidate investigation areas for Stage 2.
