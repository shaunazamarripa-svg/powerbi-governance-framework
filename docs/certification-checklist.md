# Certification checklist

A semantic model or report earns **Certified** status only when every required item is met. Items marked (R) are required; the rest are strongly recommended.

## 1. Ownership and purpose

- [ ] (R) A named business owner and a named technical owner
- [ ] (R) A written purpose, intended audience, and known limitations
- [ ] (R) Support path: where a consumer asks a question or reports an issue

## 2. Data quality and lineage

- [ ] (R) Sourced from the governed gold layer, not from ad hoc files or personal connections
- [ ] (R) Key totals reconciled to a system of record, with the reconciliation documented and dated
- [ ] (R) Data-quality checks in the pipeline, with alerting on failure
- [ ] Refresh schedule matches how the business uses the data, and failure notifications go to a monitored mailbox

## 3. Model quality

- [ ] (R) Best Practice Analyzer scan run and results reviewed; exceptions documented
- [ ] (R) Star schema; relationships single-direction unless justified
- [ ] (R) Every visible measure has a format string and a business definition
- [ ] Unused columns and tables removed; performance acceptable at expected concurrency

## 4. Security and privacy

- [ ] (R) Access granted through groups; workspace roles reviewed
- [ ] (R) Row-level security implemented where users should not see all data, and tested with "View as"
- [ ] (R) Sensitivity label applied
- [ ] (R) Reviewed for direct identifiers and minimum necessary data
- [ ] Audit and access review scheduled

## 5. Lifecycle

- [ ] (R) Deployed through the Dev, Test, Prod pipeline; source stored in Git
- [ ] (R) Release checklist completed
- [ ] Documentation page up to date (purpose, definitions, lineage, RLS, refresh)
- [ ] Recertification date set (for example, every 12 months)

## Decision

| Outcome | Meaning |
|---|---|
| Certified | All required items met; badge applied; recertification date recorded |
| Conditional | Minor gaps with dates to close them; keep Promoted until closed |
| Not certified | Feedback returned to the owner; may resubmit |

The designated certifier and the date go in the release record.
