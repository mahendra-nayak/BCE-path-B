STAGE-1-DRAFT: INTAKE-DERIVED — 2026-04-22 — Path B Stranger-Led

# INVARIANT_CATALOGUE.md — VGM Data Platform

> Source: INTAKE_SUMMARY.md
> Confidence: LOW — intake was based solely on README documentation with no code or custodian validation.

No invariants identified from intake — to be derived from code in Stage 2.

The INTAKE_SUMMARY.md does not state any explicit invariants, data correctness rules,
operational constraints, or security requirements. The Known Architecture section
describes components at a high level only. The Open Questions section explicitly flags
that data schemas, transformation logic, failure handling, security model, and
module communication patterns are all unknown.

Stage 2 source code inspection is required to produce IC records.

---

## Placeholder Catalogue

| ID | Statement | Category | Scope | Why it matters | Currently enforced | Source |
|----|-----------|----------|-------|----------------|--------------------|--------|
| — | No invariants identified from intake | — | — | — | NOT DETERMINABLE FROM SOURCE | INTAKE_SUMMARY.md |

---

## Open Questions Flagged in Intake (potential invariant sources for Stage 2)

The following open questions from INTAKE_SUMMARY.md may surface invariants during Stage 2:

1. What are the exact data schemas and transformations between STG and OP datasets?
2. How are failures, retries, and data quality issues handled in the ETL pipelines?
3. How do different modules communicate (event-driven vs scheduled vs API-based)?
4. How is security handled across APIs and data access layers?
5. What are the performance and scaling characteristics of the system?

Stage 2 should inspect source code with these questions in focus to identify candidate invariants.
