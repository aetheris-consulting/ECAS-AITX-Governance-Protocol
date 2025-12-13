# ECAS-AITX-Governance-Protocol
Runtime governance protocol for AI systems using explicit consent, role silos, and enforceable authority boundaries.
## Repository Structure (v1)

- README.md
- RFC-0001-ECAS-AITX.md
- threat-model.md
- schemas/
  - cso.schema.json
  - role_silo.schema.json
- diagrams/
  - ecas_state_machine.mmd
  - aitx_integration.mmd
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

🚫 This spec does **not** define:
- Model architectures
- Training data requirements
- Emotional companionship design
- Autonomous lethal systems
- Regulatory compliance guarantees

Those are intentionally out of scope for v1.

---

## Status

- **Version:** v1.0 (Draft)
- **Maturity:** Implementation-ready pilot
- **Audience:** Engineers, architects, safety teams, legal reviewers

---

## License

Apache-2.0 (permissive, patent-safe).
