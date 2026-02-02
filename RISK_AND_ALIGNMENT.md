# TORI Risk and Alignment Model

**Status:** Design-level risk controls. Not a guarantee of safety.

## IP / anti-theft statement
TORI is a **compositional system**, not a single prompt. Risk reduction depends on the **integration + sequencing** of routing, enforcement, data handling, audit, and version control. The harm-limiting properties described here are not recoverable from any single file in isolation.

## Design intent
TORI aims to reduce three common classes of risk in model-assisted systems:

1. **Capability overreach** (system takes actions beyond its authority or context)
2. **Untraceable outcomes** (no reliable record of why a decision was made)
3. **Misuse enablement** (system outputs materially increase harmful capability)

TORI does not claim to eliminate these risks; it seeks to **bound** and **make them auditable**.

## Threat model (representative)
TORI assumes realistic adversarial and failure pressures:

- **Prompt injection / instruction conflict**: attempts to override system constraints.
- **Data exfiltration**: requests that solicit secrets, credentials, or proprietary content.
- **Policy laundering**: reframing restricted actions as benign analysis.
- **Tool misuse**: attempts to cause unsafe external actions via integrated tools.
- **Model instability**: drift, regression, or inconsistent behavior across versions.
- **Over-trust**: operators relying on confident outputs without evidence.

## Harm-limiting mechanisms (conceptual)
### 1) Risk-aware routing and refusal
- Requests are classified into paths with different permissions and expected outputs.
- High-risk or ambiguous requests trigger **deferral, narrowing, or refusal** rather than improvisation.

### 2) Least-privilege policy enforcement
- Tool access and data access are explicitly bounded.
- Default posture is **deny-by-default** where policy does not explicitly allow.

### 3) Evidence discipline and uncertainty handling
TORI outputs should:
- identify assumptions as assumptions
- separate evidence-backed claims from speculative inferences
- prefer “unknown” over fabricated completion

### 4) Misuse resistance
TORI is designed to avoid materially enabling wrongdoing by:
- declining requests that facilitate harm (including proceduralization of dangerous capabilities)
- limiting actionable operational detail in sensitive domains
- preventing “capability uplift” where context is insufficient to establish legitimacy

### 5) Auditability as a safety feature
TORI treats audit telemetry as a primary control:
- logs which policy constraints were active
- records the routing category and applicable limits
- supports retrospective review and regression detection

## Predictability and stability targets (bounded)
TORI targets predictability through constraints, not through claims of perfect correctness:
- stable response structure (assumptions, constraints, answer, unknowns)
- consistent refusal modes under similar risk conditions
- explicit disclosure of limitation triggers

## Residual risk and known limitations
TORI explicitly acknowledges:
- A malicious operator can still misuse a system within whatever access they hold.
- No static policy can anticipate all harmful queries; conservative defaults are required.
- Audit logs support detection and accountability but do not prevent all harm.

## Escalation expectations
When TORI detects conditions it cannot safely resolve (policy conflict, missing authority, potential harm), it should:
- reduce scope,
- request operator clarification where safe,
- or stop and trigger human review pathways.

(Operational stop conditions are enumerated in `OPERATING_LIMITS.md`.)
