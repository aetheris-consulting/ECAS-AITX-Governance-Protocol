# AI Agent 0 Gate Orientation Runbook

**Status:** Draft agent operating manual  
**Applies to:** ECAS-AITX Governance Protocol v1.0 Draft  
**Audience:** AI agents, agent orchestrators, compliance agents, implementation agents, human-AI teams  
**Purpose:** define how agents must use the 0 Gate / Orientation Gate before applying the ECAS-AITX compliance framework.

---

## 1. Core Rule

No AI agent may move directly from user request to compliance interpretation, policy enforcement, workflow execution, or legal/local-law retrieval.

The agent must first pass the **0 Gate / Orientation Gate**.

The 0 Gate is the bootloader for governed AI work. It answers:

```text
Who or what is acting?
For whom?
On what work?
In what environment?
Under what authority?
With what boundaries?
Using what evidence path?
With what success, failure, rollback, and audit rules?
```

If those answers are missing, the agent must either:

1. resolve them from trusted context,
2. ask for the smallest missing input, or
3. route the work to `HOLD`, `HUMAN_REVIEW`, or `INSUFFICIENT_ORIENTATION`.

---

## 2. Relationship to ECAS-AITX

ECAS-AITX turns authority, consent, policy state, and auditability into runtime controls.

The 0 Gate happens **before** ECAS role activation and before AITX policy / CRE evaluation.

```text
[User Request]
      ↓
[0 Gate / Orientation Gate]
      ↓
[ECAS Role Silo Selection]
      ↓
[CSO / Consent State Object when required]
      ↓
[AITX Policy + CRE]
      ↓
[Compliance Source Adapters]
      ↓
[Execution / Refusal / Human Review]
      ↓
[Ledger / Audit Output]
```

The 0 Gate does not replace ECAS or AITX. It orients the agent so ECAS-AITX can be applied correctly.

---

## 3. Agent Contract

An agent working inside this repository MUST:

1. identify the actor, user, work object, and authority boundary;
2. classify the work into the correct implementation track;
3. classify the industry grouping and business context when relevant;
4. resolve jurisdiction before using compliance sources;
5. separate source evidence from validated policy constraints;
6. invoke ECAS when the work requires a role silo or elevated authority;
7. route conflicts, ambiguity, and high-liability actions to CRE or human review;
8. produce an audit-ready orientation packet before execution.

An agent MUST NOT:

- treat retrieved law, ordinance text, standards, or dataset rows as final legal advice;
- silently convert source evidence into binding policy;
- execute high-impact work without authority, review owner, and rollback path;
- skip jurisdiction resolution for compliance-sensitive work;
- assume absence of evidence means absence of legal obligation;
- mix commercial Apache-2.0 repository content with non-commercial source data without license review.

---

## 4. 0 Gate Object Model

The 0 Gate produces a **Gate Orientation Packet (GOP)**.

The GOP is the minimum state object required before an agent may proceed.

```yaml
gate_orientation_packet:
  gop_version: "0.1.0"
  gate: "0_gate_orientation"
  request_id: null

  actor_locus:
    agent_id: null
    agent_type: "ai_agent | human | human_ai_team | tool_agent | orchestrator"
    acting_party: null
    delegated_by: null
    authority_basis: "direct_user_request | policy | contract | role | system_task | unknown"

  user_locus:
    user_or_org: null
    beneficiary: null
    stakeholder_scope: []
    reliance_risk: "low | medium | high | unknown"

  work_locus:
    work_object: null
    work_type: "analysis | planning | drafting | execution | compliance_screening | system_change | external_action"
    intended_output: null
    commit_boundary: "none | draft_only | recommendation | system_change | external_world_action"
    success_condition: null
    failure_condition: null
    rollback_path: null

  environment_locus:
    deployment_context: "local | cloud | enterprise | public_web | physical_world | robotics | unknown"
    data_classes: []
    systems_touched: []
    external_dependencies: []

  implementation_track:
    primary_track: null
    secondary_tracks: []
    track_rationale: null

  industry_orientation:
    industry_grouping_system: "NAICS | ISIC | local_taxonomy | custom | unknown"
    industry_group: null
    subsector: null
    business_type: "manufacturing | service | merchandising | hybrid | unknown"
    organization_type: null
    business_function: null

  jurisdiction_orientation:
    country: null
    state_or_region: null
    county: null
    city: null
    international_context: false
    jurisdiction_confidence: "confirmed | inferred | ambiguous | missing"

  compliance_orientation:
    compliance_shells: []
    source_adapters: []
    candidate_sources: []
    validated_constraints: []
    open_questions: []
    human_review_required: false

  ecas_orientation:
    role_silo_required: false
    role_silo_id: null
    consent_required: false
    cso_required: false
    disclosure_update_required: false

  execution_boundary:
    locus_type: "internal | hybrid | external"
    seam_controls: []
    permitted_actions: []
    forbidden_actions: []

  audit:
    evidence_path: []
    policy_hashes: []
    decision: "PROCEED | HOLD | HUMAN_REVIEW | REFUSE | NEEDS_MORE_ORIENTATION"
    decision_reason: null
```

---

## 5. The Orientation Chain

The organization orientation architecture is a chained Gate 0 system.

Agents should resolve it in this order:

```text
0. Actor / Authority Orientation
1. Implementation Track
2. Industry Grouping
3. Jurisdiction Split
4. Compliance Shell
5. Final Industry Orientation
6. Business Type
7. Work Orientation
8. ECAS Role Silo / CSO Requirements
9. AITX CRE / Policy Evaluation
10. Execution, Hold, Refusal, or Human Review
```

This order matters because compliance can modify the final interpretation of the industry grouping.

Example:

```text
Initial industry grouping: food service
Jurisdiction: city + county + state
Compliance shell: health, zoning, licensing, local ordinance
Final industry orientation: regulated food-service storefront with local permitting constraints
Business type: service / merchandising hybrid
Work orientation: AI-assisted permit workflow or operational policy review
```

---

## 6. Implementation Tracks

Every governed work item must bind to at least one implementation track.

| Track | Name | Use When |
|---|---|---|
| T1 | Operational Optimization | Improving workflows, dashboards, processes, staffing, throughput, cost, quality, or internal operations |
| T2 | Production / Manufacturing | Accelerating physical production, manufacturing, logistics, robotics, or industrial throughput |
| T3 | Scientific / Discovery Acceleration | Supporting research, experiment design, literature reasoning, hypothesis generation, or discovery workflows |
| T4 | Education / Human Capability | Training, learning, curriculum, tutoring, enablement, workforce development, or knowledge transfer |
| T5 | Medical / High-Liability Support | Medical, healthcare, patient, clinician, payer, hospital, safety-critical, or high-liability administrative support |
| T6 | Compliance / Governance Overlay | Cross-cutting overlay for regulatory, legal, audit, policy, consent, safety, or governance controls |

T6 is usually a secondary or overlay track. It should not replace the primary work track unless the work itself is compliance/governance work.

---

## 7. Industry and Business Orientation

Industry grouping and business type are separate data groupings.

### 7.1 Industry Grouping

Industry grouping answers:

```text
What domain does this organization or workflow belong to?
```

Use a recognized taxonomy when possible:

- U.S. context: NAICS or another applicable U.S. sector/subsector system
- international context: ISIC or another applicable international system
- domain-specific context: regulated sector taxonomy, organizational taxonomy, or custom implementation taxonomy

### 7.2 Business Type

Business type answers:

```text
How does the business create and deliver value?
```

Use:

- `manufacturing`
- `service`
- `merchandising`
- `hybrid`
- `unknown`

Business type is a cluster grouping, not the same thing as industry grouping.

Example:

```text
Industry grouping: pet care / animal services
Business type: service
Business function: in-home pet care operations
Workflow: customer intake + scheduling + safety verification
Compliance shell: local business licensing + animal care + consumer protection
```

---

## 8. Jurisdiction Split

Compliance-sensitive work must resolve jurisdiction before source retrieval.

Minimum jurisdiction fields:

```yaml
jurisdiction_orientation:
  country: "US"
  state_or_region: null
  county: null
  city: null
  international_context: false
  jurisdiction_confidence: "confirmed | inferred | ambiguous | missing"
```

If jurisdiction is missing, the agent may still produce general planning guidance, but must not claim compliance completion.

For U.S. local-law work, the normal path is:

```text
country → state → county → city → site / operational footprint
```

For international work, use:

```text
country → region/province → municipality/local authority → site / operational footprint
```

---

## 9. Compliance Framework Use

The compliance framework is not a single static rulebook. It is a source-routed governance process.

Agents must distinguish five layers:

| Layer | Meaning | Agent Handling |
|---|---|---|
| Source Evidence | Raw law, ordinance, policy, standard, dataset row, or citation | Preserve provenance; do not enforce directly |
| Candidate Constraint | A source appears relevant to the work | Route to CRE / review |
| Validated Constraint | Reviewed and approved as applicable | Bind to policy hash and runtime control |
| Runtime Control | A concrete ECAS-AITX behavior change | Enforce mechanically |
| Audit Artifact | Record of source, decision, authority, and action | Ledger / export / preserve |

Agents must never collapse all five layers into one.

---

## 10. LOCUS Adapter Use

The LOCUS adapter is a local ordinance source adapter.

Use LOCUS when all of the following are true:

1. the work has U.S. local-law relevance;
2. city/county/state context is known or can be inferred with confidence;
3. local ordinances could affect the workflow, role silo, disclosure, permitted action, refusal rule, or review requirement;
4. the agent can preserve source provenance and review status.

Relevant LOCUS fields should be mapped into `schemas/locus_source.schema.json`.

### 10.1 LOCUS Function Mapping

| LOCUS Function | CRE Route | Meaning |
|---|---|---|
| `Context` | `source_context` | Background, definitions, scope, authority framing |
| `Process` | `workflow_requirement` | Filing, approval, documentation, timing, procedural requirement |
| `Rules` | `candidate_local_constraint` | Possible obligation, prohibition, permission, or standard |
| `Enforcement` | `candidate_local_constraint` | Penalty, inspection, enforcement, remedy, or consequence |

### 10.2 LOCUS Topic Mapping

| LOCUS Topic | Likely Governance Surface |
|---|---|
| `Business` | licensing, local operations, commercial activity |
| `Buildings` | facilities, housing, construction, built-environment deployment |
| `Zoning` | land use, physical deployment, site constraints |
| `Nuisance` | noise, public order, neighbor/public impact |
| `Other` | manual CRE triage |

### 10.3 LOCUS Safety Boundary

LOCUS-derived records are **source evidence** until reviewed.

They become runtime constraints only after:

```text
candidate_source → needs_review → validated_constraint → policy_hash → ECAS/AITX runtime control
```

---

## 11. ECAS Trigger Rules

After 0 Gate orientation, the agent must determine whether ECAS is required.

ECAS is required when the work involves:

- medical, legal, financial, housing, employment, identity, safety, or other high-impact domains;
- physical-world execution, robotics, or site deployment;
- compliance-sensitive recommendations;
- user reliance risk above low;
- authority transition from general assistance into expert, operational, or compliance role;
- external action such as sending, filing, purchasing, configuring, deploying, deleting, or changing a system.

When ECAS is required, the agent must identify:

```yaml
ecas_orientation:
  role_silo_required: true
  role_silo_id: "example: compliance_screening_agent"
  consent_required: true
  cso_required: true
  disclosure_update_required: false
```

---

## 12. Execution Boundary Locus

The agent must tag the work as internal, hybrid, or external.

| Locus | Meaning | Examples | Required Controls |
|---|---|---|---|
| `internal` | Work stays inside the reasoning/workspace boundary | drafting, analysis, schema design | normal audit and source tracking |
| `hybrid` | Internal work calls tools, APIs, agents, or repositories | GitHub PR, dataset query, calendar read | tool permission, provenance, diff review |
| `external` | Work changes outside state or waits on outside response | sending email, filing form, deploying service | explicit authorization, rollback path, ledger event |

External work has the highest control burden.

---

## 13. Decision Rules

The 0 Gate returns one of five decisions.

| Decision | Meaning |
|---|---|
| `PROCEED` | Orientation is sufficient; continue to ECAS/AITX or execution |
| `HOLD` | Orientation is incomplete but may be resolved |
| `HUMAN_REVIEW` | Authority, compliance, liability, or ambiguity requires reviewer |
| `REFUSE` | Work is outside allowed scope or violates hard constraints |
| `NEEDS_MORE_ORIENTATION` | Agent must collect missing locus, jurisdiction, authority, or work details |

Default to `HUMAN_REVIEW` when:

- source evidence conflicts;
- jurisdiction is ambiguous and execution would create risk;
- local-law, medical, legal, financial, housing, employment, or identity consequences are material;
- no accountable review owner exists;
- a policy would alter user rights, access, eligibility, safety, or external-world status.

---

## 14. Minimal Agent Procedure

Agents should run this sequence for every compliance-sensitive task:

```text
1. Parse user intent.
2. Create Gate Orientation Packet.
3. Resolve actor, work, authority, environment, and execution boundary.
4. Bind implementation track.
5. Bind industry grouping and business type.
6. Resolve jurisdiction.
7. Identify compliance shells and source adapters.
8. Retrieve only relevant source evidence.
9. Normalize source evidence into schemas.
10. Route candidate constraints to CRE or human review.
11. Determine ECAS role silo and CSO requirements.
12. Produce decision: PROCEED, HOLD, HUMAN_REVIEW, REFUSE, or NEEDS_MORE_ORIENTATION.
13. If proceeding, ledger evidence path and policy hashes.
14. Execute only inside the approved boundary.
```

---

## 15. Example: Local Business AI Deployment

```yaml
gate_orientation_packet:
  gate: "0_gate_orientation"
  actor_locus:
    agent_type: "ai_agent"
    authority_basis: "direct_user_request"
  work_locus:
    work_object: "Evaluate whether an AI-enabled service robot can operate at a specific storefront"
    work_type: "compliance_screening"
    commit_boundary: "recommendation"
  implementation_track:
    primary_track: "T1 Operational Optimization"
    secondary_tracks: ["T2 Production / Physical Operations", "T6 Compliance / Governance Overlay"]
  industry_orientation:
    industry_grouping_system: "NAICS"
    industry_group: "retail or food service, pending confirmation"
    business_type: "service / merchandising hybrid"
  jurisdiction_orientation:
    country: "US"
    state_or_region: "OH"
    county: "Franklin"
    city: "Columbus"
    jurisdiction_confidence: "confirmed"
  compliance_orientation:
    compliance_shells:
      - "local ordinance"
      - "zoning"
      - "business licensing"
      - "public safety"
    source_adapters:
      - "LOCUS"
    human_review_required: true
  ecas_orientation:
    role_silo_required: true
    role_silo_id: "local_compliance_screening"
    consent_required: true
    cso_required: true
  execution_boundary:
    locus_type: "hybrid"
    seam_controls:
      - "source provenance required"
      - "no legal advice claim"
      - "human review before binding enforcement"
  audit:
    decision: "HUMAN_REVIEW"
    decision_reason: "LOCUS can provide source evidence, but local applicability must be validated before runtime enforcement."
```

---

## 16. Example: Repository Documentation Update

```yaml
gate_orientation_packet:
  gate: "0_gate_orientation"
  actor_locus:
    agent_type: "ai_agent"
    acting_party: "repository assistant"
    authority_basis: "direct_user_request"
  work_locus:
    work_object: "Create Markdown documentation for AI agents"
    work_type: "drafting"
    commit_boundary: "system_change"
    rollback_path: "revert commit or close PR"
  environment_locus:
    deployment_context: "GitHub repository"
    systems_touched:
      - "aetheris-consulting/ECAS-AITX-Governance-Protocol"
  implementation_track:
    primary_track: "T6 Compliance / Governance Overlay"
    secondary_tracks:
      - "T1 Operational Optimization"
  execution_boundary:
    locus_type: "hybrid"
    seam_controls:
      - "branch update"
      - "pull request review"
      - "diff inspection"
  audit:
    decision: "PROCEED"
    decision_reason: "User authorized repository documentation update; change remains reviewable through PR."
```

---

## 17. Output Requirements

A compliant agent response should include:

1. orientation assumptions;
2. selected implementation track;
3. jurisdiction and industry status if relevant;
4. source adapters used;
5. whether ECAS/CSO is required;
6. whether human review is required;
7. what changed or what action was taken;
8. remaining gaps.

For repository changes, include:

- branch name;
- files changed;
- PR link or commit reference;
- merge status if known;
- known limitations.

---

## 18. Non-Negotiable Guardrails

Agents must preserve these boundaries:

- Orientation precedes execution.
- Jurisdiction precedes compliance claims.
- Source evidence precedes candidate constraint.
- Candidate constraint precedes validated policy.
- Validated policy precedes runtime enforcement.
- Runtime enforcement must be bound to audit state.
- External-world action requires explicit authority and rollback path.

In short:

```text
No orientation → no governance.
No jurisdiction → no compliance claim.
No provenance → no source trust.
No review → no binding legal/local-law control.
No policy hash → no runtime enforcement.
No audit → no accountable execution.
```
