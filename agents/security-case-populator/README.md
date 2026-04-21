# Security Case Populator Agent

A Copilot Studio agent that reads the DBS Security Case template plus a system's evidence pack (threat model, design documentation, security requirements evidence, CAF Position Tracker, supporting artefacts) and produces a first-pass draft of the complete Security Case. Turns a multi-day manual write-up into a reviewable draft in one sitting.

Designed to sit downstream of the [Evidence Tracker Populator](../evidence-tracker-populator/) agent: feed its output in as the requirements evidence input.

---

## Setup

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/).
2. Create a new agent.
3. Copy the contents of [`instructions.md`](instructions.md) into the **Instructions** field.
4. Upload to the agent's **Knowledge**:
   - `DBS-Security-Case-Template.docx` (the template to populate)
   - Threat model for the system under assessment
   - Design documentation (architecture, data flows, integration specs, HLD/LLD)
   - Populated Security Requirements Evidence Tracker (output of the Evidence Tracker Populator agent)
   - CAF Position Tracker (if available)
   - Supporting: DPIA, ITHC / penetration test report, Five Lenses scoping, Secure by Design self-assessment, supplier assurance pack, risk register extracts
5. Publish and share with your team.

### Alternative: Paste into Regular Copilot Chat

1. Open a new chat in Microsoft Copilot.
2. Paste the contents of [`instructions.md`](instructions.md) at the start.
3. Attach the template and the evidence pack.
4. Continue chatting — Copilot will follow these instructions for the session.

---

## How Output Maps to the Template

The agent cannot write `.docx` cells directly. It produces a markdown draft mirroring the template's eight sections, which a human transfers into the template in Microsoft Word.

- Cover block → cover fields on page 1
- Security Opinion → the SUPPORTED / SUPPORTED WITH CONDITIONS / NOT SUPPORTED line
- Sections 1–8 → matching sections in the `.docx`
- "What's Missing" block → a working list for the Security Lead; not pasted into the final document

The approval signature block (7.1) is deliberately left as bracketed placeholders. The agent does not pre-fill names.

---

## CAF 3.2 Alignment

This agent uses **CAF 3.2** throughout. The DBS Security Case template is calibrated to the CAF 3.2 structure of 39 contributing outcomes across four Objectives:

| Objective | Outcomes | Org-Inherited | System-Evidenced |
|-----------|----------|---------------|------------------|
| A: Managing Security Risk | 7 | 2 | 5 |
| B: Protecting Against Cyber Attack | 20 | 3 | 17 |
| C: Detecting Cyber Security Events | 7 | 0 | 7 |
| D: Minimising Impact of Incidents | 5 | 2 | 3 |
| **Total** | **39** | **7** | **32** |

This alignment is deliberate: GovAssure currently operates against CAF 3.2. Do not switch the agent to CAF 4.0 without a parallel update to the template and the org-inherited split.

---

## Agent Modes

| Mode | Use when |
|------|----------|
| **1. Scope** | Starting a new draft. Captures cover-block metadata and confirms inputs. |
| **2. Draft Full Security Case** | Primary mode. Produces the full eight-section markdown draft plus a What's Missing block. |
| **3. CAF Achievement Summary Only** | Refreshing Section 3.2 after a CAF Position Tracker update. |
| **4. Risk Extraction** | Deriving Section 4 risks from a threat model plus CAF gaps. |
| **5. Section Update** | Refreshing a single section after new evidence arrives, with a delta note. |

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Scope a Security Case draft for [System Name]. I have the threat model, HLD, populated evidence tracker, and DPIA attached. I am the Security Lead."* | Produces the scoping block (Mode 1). |
| *"Draft the full Security Case using the attached documents."* | Produces the eight-section draft plus What's Missing (Mode 2). |
| *"Update Section 3.2 using the attached CAF Position Tracker."* | Refreshes the CAF Achievement Summary (Mode 3). |
| *"Derive Section 4 risks from the attached threat model and CAF Position Tracker."* | Produces the Key Risks table (Mode 4). |
| *"New ITHC report attached. Update Section 3.1 and flag what changed."* | Section update with delta note (Mode 5). |

---

## Validation Tests

### Test 1: Scoping discipline (Mode 1)
```
Scope a Security Case draft for "Acme Case Management". Delivery Route B. CAF profile Enhanced. I have the threat model and HLD attached, but no evidence tracker or CAF Position Tracker yet.
```
**Expected**: the scoping block with `CAFProfile: Enhanced`, `DeliveryRoute: Route B (Bespoke)`, `ThreatModel: Yes`, `DesignDocumentation: Yes`, `EvidenceTracker: No`, `CAFPositionTracker: No`. The agent should stop and ask whether to proceed to Mode 2 with partial inputs.

### Test 2: Grounded drafting (Core Rule 1)
```
Draft the full Security Case using the attached design document and threat model.
```
**Expected**: every factual claim in Section 2 (System Context) and Section 4 (Key Risks) traces to the supplied documents. No invented User Base numbers, integration endpoints, hosting regions, or supplier names. Sections where evidence is thin appear as bracketed placeholders with entries in the What's Missing block.

### Test 3: Triple Lock preservation (Core Rule 3)
```
Draft Section 1 of the Security Case.
```
**Expected**: Sections 1.2 Triple Lock and 1.3 Golden Thread reproduced verbatim from the template. Section 1.1 has the system-specific opening sentence added without altering the standard text about organisational-level evidence.

### Test 4: Security Opinion vs SRO Decision (Core Rule 4)
```
Draft the Security Opinion for a system with two Not Achieved outcomes and an open ITHC High finding.
```
**Expected**: the opinion uses `SUPPORTED WITH CONDITIONS` or `NOT SUPPORTED` (Security Lead vocabulary), not `Approved with Conditions` (SRO vocabulary). The explanatory paragraph names the specific CAF outcomes and the ITHC finding. The template's guidance paragraph distinguishing the two remains intact.

### Test 5: CAF Achievement Summary maths (Mode 3)
```
Update Section 3.2 using the attached CAF Position Tracker.
```
**Expected**: the Outcomes column sums to 39 (7+20+7+5). The Org-Inherited column sums to 7. The System-Evidenced column sums to 32. The Achieved / Partially / Not Achieved columns sum across to the System-Evidenced total for each objective, not to the Outcomes total. If a Position Tracker is not attached, those three columns are left blank and flagged in What's Missing.

### Test 6: Risk derivation from threat model (Mode 4)
```
Derive Section 4 risks from the attached STRIDE threat model and CAF Position Tracker.
```
**Expected**: each risk has at least one CAF outcome citation in `B2.c` form. Threats with residual likelihood Low and impact Low do not appear. CAF outcomes assessed Not Achieved appear as risks even where the threat model did not surface them. Threat model entries that do not map to a CAF outcome are listed in commentary below the table, not forced into a row.

### Test 7: Missing inputs handled (Handling Missing Inputs)
```
Draft the full Security Case. I have the design documentation only; no threat model, no evidence tracker, no CAF Position Tracker.
```
**Expected**: the agent produces the draft rather than refusing. Sections 2 and 2.4 populate from the design documentation. Section 3.2, Section 3.1 assurance dates, Section 4 risks, and the Security Opinion appear as bracketed placeholders. The What's Missing block lists every gap grouped by what would close it (Threat Model, Evidence Tracker, CAF Position Tracker, Security Lead judgement).

### Test 8: Section update with delta (Mode 5)
```
New ITHC report dated 15 March 2026 attached. 4 medium findings closed, 1 high finding opened. Update Section 3.1 and flag what changed.
```
**Expected**: a delta block naming the section, the trigger, and the specific changes. The updated Section 3.1 follows in full, with the IT Health Check row reflecting the new RAG and date. No other section is rewritten.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Agent invents user numbers, endpoints, or supplier names | Add to instructions: *"If the design documentation does not state a value, leave the template's bracketed placeholder and list it in What's Missing. Do not estimate."* |
| Agent fills the Security Opinion on weak evidence | Add: *"The Security Opinion requires the CAF Position Tracker and the threat model at minimum. Without both, leave the Security Opinion as a bracketed placeholder and flag in What's Missing."* |
| Agent alters the Triple Lock or Golden Thread text | Add: *"Sections 1.2 and 1.3 must be reproduced verbatim. Do not paraphrase, shorten, or restyle these passages."* |
| Agent merges Security Opinion with SRO Decision vocabulary | Add: *"The Security Opinion uses SUPPORTED / SUPPORTED WITH CONDITIONS / NOT SUPPORTED. The SRO Decision uses Approved / Approved with Conditions / Not Approved. Never substitute one vocabulary for the other."* |
| Agent uses CAF 4.0 principle numbering | Add: *"This Security Case aligns to CAF 3.2 and the GovAssure 39-outcome structure. Do not cite CAF 4.0 unless the user explicitly requests it."* |
| Agent pre-fills approval signatures | Add: *"Section 7.1 Approval Signatures is always left as bracketed placeholders. The Security Lead, System Owner, SRO, and SIRO sign in person."* |
| CAF Achievement Summary totals do not sum to 39 | Add: *"Validate that the Outcomes column sums to 39, the Org-Inherited column sums to 7, and the System-Evidenced column sums to 32 before returning the table."* |
