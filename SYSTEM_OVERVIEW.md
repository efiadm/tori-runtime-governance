# TORI System Overview

**Status:** Design overview. Not a production claim.

## IP / anti-theft statement
TORI is a **compositional system**, not a single prompt or hidden technique. Its value comes from **integration + sequencing** of governance, routing, policy enforcement, data handling, audit telemetry, and rollback controls. Any individual document is insufficient to reproduce TORI’s operational behavior or safety properties.

## Purpose
TORI provides a governed operating framework for model-assisted work where:
- system behavior is **bounded and explainable**
- tool access is **least-privilege and policy-mediated**
- outputs are **audit-friendly** (assumptions, unknowns, decision points)
- high-risk work **fails closed** and escalates rather than improvises

## Architectural components (conceptual)
The TORI architecture is organized into seven coupled components (see `MANIFEST.json`):

1. **Runtime Contract (`tori.runtime.contract`)**
   - Defines what TORI must always do (e.g., declare assumptions, honor limits).
   - Defines what TORI must never do (e.g., bypass controls, conceal uncertainty).

2. **Routing Gatekeeper (`tori.routing.gatekeeper`)**
   - Determines the handling path for a request.
   - Rejects or defers requests that exceed policy or context.

3. **Policy Enforcement (`tori.policy.enforcement`)**
   - Enforces tool boundaries, data minimization, escalation rules, and permissions.
   - Applies fail-closed defaults when inputs are ambiguous or high-stakes.

4. **Data Handling (`tori.data.handling`)**
   - Data classification, redaction, retention constraints, provenance expectations.

5. **Audit Telemetry (`tori.audit.telemetry`)**
   - Records the “why” behind outputs: policy decisions, routing class, model/tool identifiers, and operator-provided context.

6. **Risk & Alignment (`tori.risk.alignment`)**
   - Harm-limiting mechanisms and misuse resistance.
   - Uncertainty handling and escalation/shutdown triggers.

7. **Versioning & Change Control (`tori.versioning.control`)**
   - Defines review gates, compatibility expectations, and rollback pathways.

## Operating principles
TORI’s operating stance is conservative by design:

- **Bounded authority:** TORI does not expand scope implicitly; it requests missing inputs or declines.
- **Least privilege by default:** No tool or data access without explicit policy allowance.
- **Traceable reasoning artifacts:** Outputs must surface assumptions, constraints, and unknowns.
- **Fail closed under uncertainty:** Ambiguity or policy conflict yields refusal, deferral, or escalation.
- **Separation of concerns:** Routing, enforcement, and audit are distinct; no single layer can silently override others.
- **Operator accountability:** TORI treats operator-supplied context as authoritative but logs it as such for audit.

## High-level control flow (non-procedural)
A TORI session can be described as a set of coupled checks rather than a single prompt:

- **Intake & classification**: Determine request category and risk posture.
- **Policy shaping**: Narrow the permitted response/action space.
- **Response generation**: Produce an output constrained by the shaped space.
- **Audit emission**: Record what constraints were applied and why.
- **Termination rules**: Stop or escalate when limits or safety triggers are met.

This description is intentionally non-procedural to avoid enabling replication without the integrated system.

## Output contract (what reviewers should expect)
TORI outputs (when operating correctly) should:
- state assumptions and unknowns explicitly
- avoid hidden tool use or hidden data reliance
- refuse or narrow scope when safety/authority is unclear
- maintain internal consistency with stated constraints

## Non-goals
- Full autonomy, self-directed operation, or uncontrolled tool execution.
- Claims of correctness beyond provided evidence.
- Replacement for security reviews, privacy programs, or incident response.
