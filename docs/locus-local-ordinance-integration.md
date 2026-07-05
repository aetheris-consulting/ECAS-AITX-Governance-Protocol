# LOCUS Local Ordinance Integration for ECAS-AITX

**Status:** Draft extension  
**Applies to:** ECAS-AITX Governance Protocol v1.0 Draft  
**Source:** LocalLaws/LOCUS-v1 and the paper *Freeing the Law with LOCUS: A Local Ordinance Corpus for the United States*

---

## 1. Purpose

LOCUS provides a machine-readable source layer for U.S. municipal and county ordinances. ECAS-AITX can use this layer as **jurisdictional policy evidence** for Conflict Resolution Engine (CRE) review, local compliance screening, and jurisdiction-aware role-silo constraints.

LOCUS must not be treated as automatic legal authority. It is a **source and classification layer**, not a final legal determination.

---

## 2. Source Facts

- **Dataset:** `LocalLaws/LOCUS-v1`
- **Publisher:** LocalLaws
- **Dataset page:** https://huggingface.co/datasets/LocalLaws/LOCUS-v1
- **Organization page:** https://huggingface.co/LocalLaws
- **Paper:** https://arxiv.org/abs/2606.19334
- **License:** `cc-by-nc-4.0`
- **Modality / format:** tabular text data in Parquet form
- **Task category:** text classification

Operational consequence: because LOCUS is licensed under `cc-by-nc-4.0`, this repository should reference and adapt to the dataset, but should **not vendor, mirror, or redistribute LOCUS data** inside the Apache-2.0 repo without a separate licensing review.

---

## 3. Why LOCUS Matters to ECAS-AITX

ECAS governs explicit authority transitions. AITX governs federated policy enforcement and auditability. LOCUS strengthens the AITX side by adding a local-law evidence source that can be queried before execution.

This matters because many AI deployments are affected by local rules, including:

- zoning and land-use constraints
- business licensing and permits
- building and housing rules
- nuisance, noise, and public-order ordinances
- local enforcement and penalty structures
- municipal/county procedural requirements

In ECAS-AITX terms, LOCUS becomes a **jurisdictional policy source adapter** feeding the CRE.

```text
[ User Intent ]
      ↓
[ ECAS Gatekeeper ]
      ↓
[ Role Silo + CSO ]
      ↓
[ AITX Policy / CRE ] ← [ LOCUS Local Ordinance Adapter ]
      ↓
[ Orchestrator ]
      ↓
[ Execution ]
      ↓
[ Immutable Ledger ]
```

---

## 4. Dataset Fields Relevant to Governance

A LOCUS-derived governance adapter should preserve, at minimum, the following fields when available:

| Field | Governance Use |
|---|---|
| `header` | Ordinance heading / local code section context |
| `content` | Source text chunk for review and retrieval |
| `is_substantive` | Candidate filter for rules that may affect execution |
| `function` | Maps chunk to Context, Rules, Process, or Enforcement |
| `topic` | Maps substantive law to Buildings, Business, Nuisance, Zoning, or Other |
| `source_jurisdiction_type` | Distinguishes city/county source type |
| `state` | State-level jurisdiction key |
| `city` | City-level jurisdiction key, when available |
| `county` | County-level jurisdiction key, when available |
| `enforcement_discretion` | Risk signal for enforcement variability |
| `opacity` | Risk signal for interpretability / clarity burden |
| `paternalism` | Policy-analysis signal; not a direct compliance control |
| `problem_salience` | Policy-analysis signal; not a direct compliance control |

---

## 5. Governance Mapping

### 5.1 Function Mapping

| LOCUS `function` | ECAS-AITX Interpretation | Default Handling |
|---|---|---|
| `Context` | Background, definitions, scope, authority framing | Store as source context; do not enforce directly |
| `Process` | Filing, approval, procedure, timing, documentation | Route to workflow / procedural checks |
| `Rules` | Substantive obligation, permission, prohibition, standard | Route to CRE as candidate local constraint |
| `Enforcement` | Penalty, inspection, remedy, enforcement mechanism | Route to CRE as candidate local constraint and audit risk |

### 5.2 Topic Mapping

| LOCUS `topic` | Likely AITX Surface |
|---|---|
| `Business` | business operations, licensing, permits, storefront deployment |
| `Buildings` | facilities, housing, construction, robotics in built environments |
| `Zoning` | site selection, physical deployment, land-use constraints |
| `Nuisance` | noise, public-order, neighbor-impact, local conduct limits |
| `Other` | CRE triage queue for manual classification |

---

## 6. Recommended Runtime Use

### Step 1 — Resolve jurisdiction

Before consulting LOCUS, the system should resolve the operational footprint:

```text
state → county → city → business/site footprint → role silo → intended action
```

If the location is ambiguous, ECAS-AITX should treat local-law screening as incomplete and log the ambiguity.

### Step 2 — Retrieve candidate ordinance chunks

Query LOCUS by jurisdiction and topic. Prefer restrictive retrieval over broad retrieval:

```text
state = "oh"
county = "franklin"
city = "columbus"
topic in ["Business", "Buildings", "Zoning", "Nuisance"]
function in ["Rules", "Enforcement", "Process"]
```

### Step 3 — Normalize into a Local Policy Source Object

Each candidate chunk should be converted into a `LocalPolicySourceObject` conforming to `schemas/locus_policy_source.schema.json`.

### Step 4 — Route to CRE

Only `Rules` and `Enforcement` chunks with `is_substantive=true` should default to **candidate constraint** status. `Process` chunks should default to **workflow requirement** status. `Context` chunks should default to **source context** status.

### Step 5 — Human/legal review gate

A LOCUS chunk can support governance screening, but it should not become an enforceable policy constraint until a reviewer, jurisdictional rule pack, or trusted legal update service confirms applicability.

Recommended review states:

```text
candidate_source → needs_review → validated_constraint → deprecated_or_superseded
```

### Step 6 — Hash and bind to CSO / audit ledger

If a local rule changes the active role silo, capability boundary, refusal rule, or disclosure text, the resulting policy hash should be bound to the Consent State Object (CSO) and ledgered.

---

## 7. Required Safety Controls

1. **No legal-advice substitution**  
   LOCUS output must be framed as research/compliance-screening evidence, not legal advice.

2. **No completeness guarantee**  
   Missing LOCUS coverage must not be interpreted as absence of local legal restrictions.

3. **No silent enforcement**  
   A model must not silently convert a retrieved ordinance into a binding runtime rule without provenance, review status, and policy hash.

4. **License isolation**  
   LOCUS-derived data should remain external or generated at deployment time unless licensing permits redistribution.

5. **Staleness handling**  
   Local ordinances can change. Each source object must preserve retrieval date, source version, and review status.

6. **OCR / extraction uncertainty**  
   Because source documents may involve OCR or heterogeneous formats, high-impact decisions require source verification.

---

## 8. Example Governance Flow

```text
User: "Can my business deploy an outdoor service robot at this location?"

1. ECAS detects a physical-world / business deployment request.
2. ECAS selects the relevant Role Silo.
3. AITX resolves operational footprint: state, county, city, site type.
4. LOCUS adapter retrieves local chunks tagged Zoning, Business, Buildings, and Nuisance.
5. CRE separates Context, Process, Rules, and Enforcement chunks.
6. Candidate constraints are queued for review or matched against validated local rule packs.
7. If local constraints affect behavior, ECAS requires a revised disclosure / CSO.
8. Execution proceeds only under the validated policy hash.
9. All source objects, policy decisions, and role transitions are ledgered.
```

---

## 9. Conformance Extension

An ECAS-AITX implementation claiming LOCUS-aware operation SHOULD demonstrate:

- jurisdiction resolution before local-law screening
- retrieval of LOCUS candidate chunks by state/county/city where available
- separation of source evidence from validated policy constraints
- preservation of dataset version, license, source URL, retrieval timestamp, and review status
- CRE handling for `Rules`, `Process`, and `Enforcement`
- CSO policy-hash update when local rules alter the role silo
- immutable audit logging of local-law source use

---

## 10. Citation

```bibtex
@article{peskoff2026freeing,
  title={Freeing the Law with LOCUS: A Local Ordinance Corpus for the United States},
  author={Peskoff, Denis and Barrow, Joe and Vu, Christopher and Davenport, Diag},
  journal={arXiv preprint arXiv:2606.19334},
  year={2026}
}
```
