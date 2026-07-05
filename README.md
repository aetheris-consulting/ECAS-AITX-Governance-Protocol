## Repository Structure (v1)

- README.md
- RFC-0001-ECAS-AITX.md
- docs/
  - locus-local-ordinance-integration.md
- schemas/
  - locus_source.schema.json
- examples/
  - locus_source.example.json
- planned/
  - threat-model.md
  - schemas/cso.schema.json
  - schemas/role_silo.schema.json
  - diagrams/ecas_state_machine.mmd
  - diagrams/aitx_integration.mmd
  - openapi/ (optional v1.1)

# ECAS–AITX Governance Framework

**Explicit Contextual Authority Switching (ECAS)** integrated with **AITX Federated AI Governance**.

This repository defines a **runtime governance standard** for AI systems that prevents authority drift, unsafe reliance, and liability exposure by enforcing **explicit, user-acknowledged role transitions** with **cryptographic consent artifacts** and **mechanical behavioral constraints**.

This is not a policy document.  
This is an **engineering specification**.

---

## Why This Exists

Modern AI systems fail at governance for one core reason:

> Consent, roles, and disclaimers are treated as *text*, not *system state*.

As a result:
- AI systems drift into unintended authority
- Users form unreasonable reliance
- Liability chains remain unbroken
- Safety is enforced socially, not mechanically

**ECAS fixes this by turning consent into a runtime control primitive.**

AITX provides the **federated infrastructure** (identity, policy, audit, conflict resolution) that makes ECAS enforceable across organizations, devices, and robots.

---

## Core Idea (Plain English)

Before an AI system enters a high-risk mode (medical, robotics, companion, etc.):

1. The system **stops**
2. The user is shown a **non-bypassable disclosure**
3. The user must **explicitly acknowledge and consent**
4. The system generates a **signed Consent State Object (CSO)**
5. The AI’s behavior is **mechanically constrained**
6. All actions are **immutably logged**

If consent expires, behavior **reverts automatically**.

---

## What This Spec Covers (v1.0)

✅ Explicit role silos (modes)  
✅ Blocking consent modals (EMI)  
✅ Cryptographic consent artifacts (CSO)  
✅ Runtime behavioral enforcement (BEL)  
✅ Immutable audit logging  
✅ Integration with federated policy systems (AITX)  
✅ LOCUS-aware local ordinance source integration for AITX / CRE screening

🚫 This spec does **not** define:
- Model architectures
- Training data requirements
- Emotional companionship design
- Autonomous lethal systems
- Regulatory compliance guarantees
- Automatic legal advice or automatic local-law compliance determinations

Those are intentionally out of scope for v1.

---

## LOCUS Local Ordinance Source Layer

This repository now includes a draft integration path for **LOCUS v1.0**, the LocalLaws U.S. municipal and county ordinance corpus.

LOCUS is useful for ECAS-AITX because it can feed local-law source evidence into the **AITX Conflict Resolution Engine (CRE)** before an AI system acts in a location-sensitive role silo.

Primary integration points:

- `docs/locus-local-ordinance-integration.md` — architecture, governance mapping, runtime flow, risk controls, and conformance extension
- `schemas/locus_source.schema.json` — provenance-preserving source object for LOCUS-derived ordinance chunks
- `examples/locus_source.example.json` — example object for a city/county/state-scoped local source record

Important license boundary:

- The LOCUS dataset is published under `cc-by-nc-4.0`.
- This repository remains Apache-2.0.
- Do **not** vendor, mirror, or redistribute LOCUS data inside this repository without a separate license review.
- Treat LOCUS-derived records as source evidence until reviewed and promoted into validated policy constraints.

---

## Status

- **Version:** v1.0 (Draft)
- **Maturity:** Implementation-ready pilot
- **Audience:** Engineers, architects, safety teams, legal reviewers

---

## License

Apache-2.0 (permissive, patent-safe).
