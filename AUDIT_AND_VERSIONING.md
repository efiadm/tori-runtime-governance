# TORI Audit and Versioning

**Status:** Governance and change control model (design-level).

## IP / anti-theft statement

TORI is a **compositional system**: its safety and audit properties arise from **integration + sequencing** across components (routing, enforcement, data handling, telemetry, and release control), not from any single file.
TORI is a **compositional system**. Auditability and stability come from the coupled operation of routing, enforcement, data handling, telemetry, and change control. No single document provides a replicable blueprint; the value is in the integrated discipline and enforcement.

## Audit objectives
TORI’s audit model is intended to support:
- **Attribution:** what inputs, constraints, and routing decisions shaped the output
- **Reproducibility (bounded):** ability to understand outcomes given the same declared context (not guaranteed determinism)
- **Accountability:** clear ownership for policies, changes, and exceptions
- **Regression detection:** identify behavior drift across model/tool/policy versions

## What should be auditable (conceptual record)
A TORI-compliant record should be able to answer:
- What request was made (with appropriate redaction)?
- Which routing category applied and why?
- Which policies/limits were active?
- Which model/tool identifiers were used (if applicable)?
- What assumptions and unknowns were declared?
- Was any escalation/refusal triggered?

This is a governance requirement, not an implementation spec.

## Versioning model
### System versioning
- TORI uses semantic versioning (e.g., `MAJOR.MINOR.PATCH`) as recorded in `MANIFEST.json`.
- **MAJOR:** breaking changes to operating contract or audit expectations.
- **MINOR:** new components/capabilities that preserve contract.
- **PATCH:** bug fixes, clarifications, and non-breaking policy refinements.

### Component versioning
Each component in `MANIFEST.json` has its own version to support:
- targeted review
- staged rollout
- rollback of a specific layer (e.g., routing rules) without rewriting the full system

## Change control (review gates)
TORI changes should be gated by:
- **Technical review** (runtime maintainers)
- **Safety review** (harm and misuse implications)
- **Security review** (data handling, tool permissions, threat model alignment)
- **Audit/controls review** (telemetry sufficiency, traceability impact)

Approvals are role-based per `MANIFEST.json` ownership fields.

## Rollback strategy (principles)
Rollback is treated as a first-class safety tool:
- any change must have a defined rollback path
- rollbacks prioritize restoring **policy enforcement** and **audit integrity** first
- if audit integrity cannot be restored, TORI should operate in a restricted safe mode (as described in `OPERATING_LIMITS.md`)

## Release artifacts and integrity
A TORI release should include:
- a canonical `MANIFEST.json` (component ledger)
- human-readable design constraints (this artifact set)
- signed/attested provenance for versions and approvals (organization-specific mechanism)

Details of cryptographic signing and CI/CD integration are intentionally omitted here to avoid providing a replication recipe without the full governance context.

## Audit limitations
- Audit logs support accountability and debugging but do not guarantee safe outcomes.
- Redaction and minimization requirements can reduce forensic detail; this is a deliberate privacy tradeoff.
- A determined insider with sufficient privileges may still cause harm; governance reduces but does not eliminate this risk.
