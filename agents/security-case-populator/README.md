# Security Case Populator

A prompt for Microsoft Copilot that reads the DBS Security Case template plus a system's evidence pack (threat model, design documentation, security requirements evidence, CAF Position Tracker, supporting artefacts) and produces a first-pass draft of the complete Security Case. Turns a multi-day manual write-up into a reviewable draft in one sitting.

Designed to sit downstream of the [Evidence Tracker Populator](../evidence-tracker-populator/): feed its output in as the requirements evidence input.

---

## How to use

1. Open a new Microsoft Copilot chat.
2. Paste the contents of [`instructions.md`](instructions.md) as your first message.
3. Attach:
   - `DBS-Security-Case-Template.docx` (the template to populate).
   - Threat model for the system.
   - Design documentation (architecture, data flows, integration specs, HLD/LLD).
   - Populated Security Requirements Evidence Tracker (output of the Evidence Tracker Populator).
   - CAF Position Tracker (if available).
   - Supporting: DPIA, ITHC / pen test, Five Lenses scoping, SbD self-assessment, supplier assurance pack, risk register.
4. Ask a question — see Starter Prompts below.

You'll need to paste the prompt again at the start of each new chat.

---

## How output maps to the template

The prompt produces a markdown draft mirroring the template's eight sections, which a human transfers into the template in Microsoft Word.

- Cover block → cover fields on page 1
- Security Opinion → the SUPPORTED / SUPPORTED WITH CONDITIONS / NOT SUPPORTED line
- Sections 1–8 → matching sections in the `.docx`
- "What's Missing" block → a working list for the Security Lead; not pasted into the final document

The approval signature block (7.1) is deliberately left as bracketed placeholders.

---

## CAF 3.2 Alignment

The prompt uses **CAF 3.2** throughout. The DBS Security Case template is calibrated to the CAF 3.2 structure of 39 contributing outcomes across four Objectives:

| Objective | Outcomes | Org-Inherited | System-Evidenced |
|-----------|----------|---------------|------------------|
| A: Managing Security Risk | 7 | 2 | 5 |
| B: Protecting Against Cyber Attack | 20 | 3 | 17 |
| C: Detecting Cyber Security Events | 7 | 0 | 7 |
| D: Minimising Impact of Incidents | 5 | 2 | 3 |
| **Total** | **39** | **7** | **32** |

GovAssure currently operates against CAF 3.2. Do not switch to CAF 4.0 without parallel updates to the template and the org-inherited split.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Scope a Security Case draft for [System]. I have the threat model, HLD, populated evidence tracker, and DPIA attached. I am the Security Lead."* | Mode 1: Scope |
| *"Draft the full Security Case using the attached documents."* | Mode 2: Full draft |
| *"Update Section 3.2 using the attached CAF Position Tracker."* | Mode 3: CAF summary only |
| *"Derive Section 4 risks from the attached threat model and CAF Position Tracker."* | Mode 4: Risk extraction |
| *"New ITHC report attached. Update Section 3.1 and flag what changed."* | Mode 5: Section update |

---

## Validation Tests

### Test 1: Scoping discipline
```
Scope a Security Case draft for "Acme Case Management". Delivery Route B. CAF profile Enhanced. I have the threat model and HLD attached, but no evidence tracker or CAF Position Tracker.
```
**Expected**: Scoping block with CAFProfile=Enhanced, DeliveryRoute=Route B, ThreatModel=Yes, DesignDocumentation=Yes, EvidenceTracker=No, CAFPositionTracker=No. Asks whether to proceed to Mode 2 with partial inputs.

### Test 2: Grounded drafting
```
Draft the full Security Case using the attached design document and threat model.
```
**Expected**: Every factual claim in Sections 2 and 4 traces to supplied documents. No invented User Base numbers, integration endpoints, hosting regions, or supplier names. Thin-evidence areas appear as bracketed placeholders with entries in "What's Missing".

### Test 3: Triple Lock preservation
```
Draft Section 1 of the Security Case.
```
**Expected**: Sections 1.2 Triple Lock and 1.3 Golden Thread reproduced verbatim. Section 1.1 has the system-specific opening sentence added without altering the standard text.

### Test 4: Security Opinion vs SRO Decision
```
Draft the Security Opinion for a system with two Not Achieved outcomes and an open ITHC High finding.
```
**Expected**: Uses `SUPPORTED WITH CONDITIONS` or `NOT SUPPORTED` (Security Lead vocabulary), not `Approved with Conditions` (SRO vocabulary). Explanatory paragraph names the specific CAF outcomes and ITHC finding.

### Test 5: CAF Achievement Summary maths
```
Update Section 3.2 using the attached CAF Position Tracker.
```
**Expected**: Outcomes column sums to 39 (7+20+7+5). Org-Inherited sums to 7. System-Evidenced sums to 32. If no Position Tracker is attached, Achieved / Partially / Not Achieved columns blank and flagged in "What's Missing".

### Test 6: Risk derivation
```
Derive Section 4 risks from the attached STRIDE threat model and CAF Position Tracker.
```
**Expected**: Each risk has ≥1 CAF outcome citation in `B2.c` form. Low/Low threats omitted. Not Achieved CAF outcomes appear as risks even where the threat model didn't surface them. Unmappable threats listed in commentary, not forced into rows.

### Test 7: Missing inputs handled
```
Draft the full Security Case. I have the design documentation only; no threat model, no evidence tracker, no CAF Position Tracker.
```
**Expected**: Produces the draft rather than refusing. Section 2 populates from design docs. Section 3.2, Section 3.1 dates, Section 4 risks, and Security Opinion appear as bracketed placeholders. "What's Missing" lists every gap grouped by what would close it.

### Test 8: Section update with delta
```
New ITHC report dated 15 March 2026. 4 medium closed, 1 high opened. Update Section 3.1 and flag what changed.
```
**Expected**: Delta block (Section, Trigger, Changes). Updated Section 3.1 in full with new RAG and date. No other section rewritten.

### Test 9: Synthesis discipline
```
Draft Section 3.2. CAF Position Tracker dated 02 April 2026 attached, plus a supplier CAF self-assessment dated November 2025. They disagree on B3.b.
```
**Expected**: Reflects the Position Tracker's rating (more authoritative and more recent). A note flags the supplier disagreement and references both sources by name and date.

### Test 10: Conflict surfacing
```
Draft the full Security Case. The HLD describes the system as SaaS in AWS eu-west-2. The Five Lenses doc calls it Bespoke in Crown Hosting.
```
**Expected**: Stops before drafting Section 2; surfaces the contradiction explicitly; asks which document is authoritative. Does not pick silently.

---

## Troubleshooting

| Problem | Add to the prompt |
|---------|-------------------|
| Agent invents user numbers, endpoints, or supplier names | *"If the design documentation does not state a value, leave the bracketed placeholder and list it in What's Missing. Do not estimate."* |
| Agent fills Security Opinion on weak evidence | *"The Security Opinion requires the CAF Position Tracker and threat model at minimum. Without both, leave it bracketed and flag in What's Missing."* |
| Agent alters Triple Lock or Golden Thread | *"Sections 1.2 and 1.3 must be reproduced verbatim. Do not paraphrase, shorten, or restyle."* |
| Agent mixes Security Opinion with SRO Decision vocabulary | *"Security Opinion: SUPPORTED / SUPPORTED WITH CONDITIONS / NOT SUPPORTED. SRO Decision: Approved / Approved with Conditions / Not Approved. Never substitute."* |
| Agent uses CAF 4.0 numbering | *"Use CAF 3.2 throughout. Do not cite CAF 4.0 unless I explicitly ask."* |
| Agent pre-fills approval signatures | *"Section 7.1 Approval Signatures is always left as bracketed placeholders."* |
| CAF Achievement Summary totals wrong | *"Validate that the Outcomes column sums to 39, Org-Inherited to 7, System-Evidenced to 32 before returning."* |
