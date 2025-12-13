# RFC-0001: ECAS–AITX Governance Protocol

**Title:** Explicit Contextual Authority Switching (ECAS) integrated with AITX  
**Status:** Draft  
**Version:** 1.0  
**Authors:** Thomas et al.  
**Last Updated:** 2025-12-13  

---

## 1. Abstract

This RFC defines **Explicit Contextual Authority Switching (ECAS)**, a runtime governance protocol that enforces deliberate, user-acknowledged transitions between bounded AI operational roles. ECAS is integrated with **AITX Federated Governance** to provide identity, policy enforcement, auditability, and conflict resolution across distributed AI and robotic systems.

The protocol converts consent from a passive legal artifact into an **enforceable system state**, reducing authority ambiguity, unsafe reliance, and liability exposure.

---

## 2. Design Goals

- Prevent authority drift in AI systems
- Reduce reliance and misinterpretation through explicit role boundaries
- Convert consent into enforceable runtime state
- Maintain low-latency operation in distributed systems
- Support auditability and post-incident reconstruction

---

## 3. Explicit Non-Goals (v1)

This RFC intentionally does **not** attempt to:

- Define AI alignment theory
- Replace professional judgment
- Guarantee regulatory compliance
- Solve emotional dependency risks fully
- Govern autonomous weapons or lethal systems

Any claims beyond these bounds are out of scope for v1.

---

## 4. High-Level Architecture

```text
[ Identity ]
     ↓
[ ECAS Gatekeeper ]
     ↓
[ Policy / CRE ]
     ↓
[ Orchestrator ]
     ↓
[ Execution ]
     ↓
[ Immutable Ledger ]



ECAS executes before orchestration and before policy conflict resolution. Where both role enforcement and compliance conflict detection apply, ECAS establishes authority state first, and conflict adjudication follows.

5. Versioning & Compatibility

Backwards compatibility is not guaranteed across major versions

Consent State Objects (CSOs) are version-pinned to policy hashes and disclosure templates

Deprecated precedents and policies remain auditable (non-deletable), with explicit deprecation markers

6. Terminology

The following terms are used throughout this specification:

ECAS (Explicit Contextual Authority Switching): A runtime governance protocol that enforces deliberate, user-acknowledged transitions between bounded AI operational roles.

AITX: Federated governance infrastructure providing identity, policy enforcement, auditability, and conflict resolution.

Role Silo (RS): A bounded operational context with explicit capability allowlists and refusal rules.

EMI (ECAS Modal Intercept): A blocking interface that requires explicit acknowledgment and consent before entering a non-default Role Silo.

CSO (Consent State Object): A cryptographically signed, immutable record of user consent for a specific Role Silo and policy version.

BEL (Behavioral Enforcement Layer): A runtime policy engine that enforces Role Silo constraints mechanically.

CRE (Conflict Resolution Engine): A governance component that adjudicates conflicts between applicable policy frameworks.

PID (Persistent In-Silo Disclosure): A continuous UI indicator that communicates the active Role Silo and its limitations.

7. ECAS State Model (Conceptual)

ECAS operates as a finite state machine governing authority transitions.

States

DEFAULT: General-purpose, low-authority operation.

AWAITING_CONSENT: Blocking state pending explicit user acknowledgment and consent.

ACTIVE_SILO: Authorized operation within a bounded Role Silo.

EXPIRED: Consent invalidated due to timeout, policy change, or role exit.

EMERGENCY_OVERRIDE: Temporary elevated authority triggered by explicit emergency conditions.

Transitions

DEFAULT → AWAITING_CONSENT: User intent requires a non-default Role Silo.

AWAITING_CONSENT → ACTIVE_SILO: Valid CSO is generated.

ACTIVE_SILO → EXPIRED: CSO invalidation event occurs.

ANY → EMERGENCY_OVERRIDE: Emergency trigger is invoked.

EMERGENCY_OVERRIDE → DEFAULT: Post-incident resolution and review complete.

8. Conformance

An implementation is ECAS-conformant if and only if:

Role transitions are blocking and non-bypassable

Consent is explicit, user-acknowledged, and logged as a CSO

Runtime behavior is mechanically constrained by the active Role Silo

Consent invalidation results in automatic behavioral reversion

All Role transitions and consent artifacts are immutably logged

9. Conformance Testing (Non-Normative)

The following guidance is provided to assist implementers and auditors. This section does not introduce additional normative requirements.

A reference implementation SHOULD demonstrate:

Forced ECAS Modal Intercept (EMI) when transitioning from DEFAULT to any non-default Role Silo

Inability to generate Role-restricted outputs without a valid, active CSO

Automatic session invalidation and behavioral reversion upon CSO expiry or policy version change

Immutable logging of all Role transitions, CSO creation, and CSO invalidation events

Verifiable linkage between runtime behavior and the active Role Silo definition

10. Threat Model

Threats, mitigations, and residual risks are defined in the companion document:

threat-model.md

This document is considered normative context for ECAS–AITX v1 but does not introduce additional conformance requirements.


---

## 2) Mermaid diagrams (copy/paste into `diagrams/`)

### A) ECAS state machine (`diagrams/ecas_state_machine.mmd`)
```mermaid
stateDiagram-v2
  [*] --> DEFAULT

  DEFAULT --> AWAITING_CONSENT: intent_requires_silo
  AWAITING_CONSENT --> ACTIVE_SILO: cso_issued
  AWAITING_CONSENT --> DEFAULT: cancel

  ACTIVE_SILO --> EXPIRED: cso_invalidated
  EXPIRED --> DEFAULT: revert

  DEFAULT --> EMERGENCY_OVERRIDE: emergency_trigger
  AWAITING_CONSENT --> EMERGENCY_OVERRIDE: emergency_trigger
  ACTIVE_SILO --> EMERGENCY_OVERRIDE: emergency_trigger

  EMERGENCY_OVERRIDE --> DEFAULT: post_incident_complete

B) System architecture (diagrams/architecture.mmd)
flowchart TD
  A[Identity] --> B[ECAS Gatekeeper]
  B --> C[Policy Engine / CRE]
  C --> D[Orchestrator]
  D --> E[Execution: Apps / Cells / Robots]
  E --> F[Immutable Ledger (WORM)]
  B --> F
  C --> F

C) Request sequence (diagrams/request_sequence.mmd)
sequenceDiagram
  participant U as User
  participant UI as Client/UI
  participant G as ECAS Gatekeeper
  participant P as Policy/CRE
  participant O as Orchestrator
  participant X as Execution
  participant L as Ledger

  U->>UI: Submit request
  UI->>G: dispatch(request)
  G->>G: Determine required Role Silo
  alt No valid CSO
    G-->>UI: BLOCK + EMI payload
    UI->>U: Show modal (acknowledge + consent)
    U->>UI: Consent
    UI->>G: submit_consent()
    G->>L: append(CSO_CREATE)
    G-->>UI: CSO issued
  end

  UI->>G: dispatch(request with CSO)
  G->>L: append(ROLE_ACTIVE + BEL_EVAL)
  G->>P: conflict_check(request)
  alt Conflict
    P->>L: append(CRE_TICKET_CREATE)
    P-->>UI: PENDING_ADJUDICATION
  else Clear
    P-->>O: proceed
    O->>X: execute(tool/model calls)
    X->>L: append(EXECUTION_EVENT)
    X-->>UI: response
  end

3) GitHub release note (v1.0-draft)

Paste this into your GitHub “Release” description:

## v1.0-draft — ECAS–AITX Governance Protocol (RFC-0001)

This release publishes the initial draft of a runtime governance protocol combining:

- **ECAS (Explicit Contextual Authority Switching):** consent-as-state, role silo gating, and mechanical enforcement
- **AITX:** federated governance infrastructure for identity, policy enforcement, auditability, and conflict resolution

### What’s Included
- RFC-0001: ECAS–AITX Governance Protocol (Draft v1.0)
- Threat model document (v1)
- Diagrams (Mermaid): architecture, ECAS state machine, request sequence

### What’s Explicitly Out of Scope (v1)
- Model architectures and training requirements
- Regulatory compliance guarantees
- Emotional dependency mitigation beyond scope boundaries
- Autonomous weapons / lethal systems

### Intended Use
Implementation-ready pilot guidance for engineers, safety teams, and legal reviewers.