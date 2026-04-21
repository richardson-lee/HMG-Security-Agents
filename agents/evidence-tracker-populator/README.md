# Evidence Tracker Populator Agent

A Copilot Studio agent that reads the HMG Security Requirements Evidence Tracker template plus a supplier's documentation pack, and produces a first-pass draft of the assessment columns. Turns a multi-day manual fill into a reviewable draft in one sitting.

The tracker itself lives in the companion [HMG-Security-Program](https://github.com/richardson-lee/HMG-Security-Program) repository.

---

## Setup

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/).
2. Create a new agent.
3. Copy the contents of [`instructions.md`](instructions.md) into the **Instructions** field.
4. Upload to the agent's **Knowledge**:
   - `hmg-security-requirements-evidence-tracker.xlsx` (from the HMG-Security-Program repo)
   - The supplier's security documentation pack for the system under assessment
   - Optional: your baseline policies, existing security case documents, and any prior version of the tracker for this system
5. Publish and share with your team.

### Alternative: Paste into Regular Copilot Chat

1. Open a new chat in Microsoft Copilot.
2. Paste the contents of [`instructions.md`](instructions.md) at the start.
3. Attach the tracker template and supplier documents.
4. Continue chatting — Copilot will follow these instructions for the session.

---

## How Output Maps to the Tracker

The agent cannot write `.xlsx` cells directly. It produces paste-ready blocks that a human transfers in two operations:

### Cover sheet (once per assessment)

```
SystemName:             HMRC Case Management Platform
SupplierName:           Example Ltd
AssessorName:           L Richardson
Tier_Baseline_Active:   Yes
Tier_IaaS_Active:       No
Tier_PaaS_Active:       Yes
Tier_SaaS_Active:       No
Tier_AI_Active:         No
```

Paste each value into the matching named cell on the Cover sheet (`SystemName`, `SupplierName`, `AssessorName`, `Tier_*_Active`).

### Requirement sheets (once per active tier)

The agent produces one markdown table per sheet, ordered by Ref, with five columns that map exactly to the tracker's columns G–K.

Copy the data rows (not the header) and paste into the tier sheet starting at cell G2. Columns A–F are pre-populated and must not be overwritten.

---

## Dropdown Values the Agent Must Honour

| Column | Values |
|--------|--------|
| **G Applicability** | `In scope`, `Not applicable` |
| **I Assessor Verdict** | `Met`, `Partially Met`, `Unmet`, `N/A` (or blank for "Not yet assessed") |

A blank verdict is valid and is how the dashboards flag "Not yet assessed." The agent should leave Verdict blank whenever the evidence is insufficient to decide, and explain why in Assessor Notes.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Set up the Cover sheet for an assessment of [system name] by [supplier]. The system uses SaaS and AI services. I am the assessor."* | Produces the Cover-sheet paste block (Mode 1). |
| *"Populate the Baseline sheet using the attached supplier security response and design document."* | First-pass fill of the Baseline tier (Mode 2). |
| *"Produce a prioritised gap list from the assessment so far, grouped by what is missing."* | Gap analysis (Mode 3). |
| *"The supplier has sent an updated IAM policy. Update the affected rows on the Baseline and SaaS sheets and give me a delta."* | Reassessment (Mode 4). |

---

## Validation Tests

### Test 1: Cover-sheet scoping (Mode 1)
```
Set up the Cover sheet for an assessment of "Acme Case Management" by "Acme Ltd". It is a SaaS system hosted on AWS PaaS. I am the assessor.
```
**Expected**: the exact `SystemName / SupplierName / AssessorName / Tier_*_Active` block, with `Tier_SaaS_Active: Yes`, `Tier_PaaS_Active: Yes`, others `No`.

### Test 2: Grounded evidence (Core Rule 1)
```
Populate the Baseline sheet using the attached supplier response.
```
**Expected**: every Supplier Evidence cell contains a quote or tight paraphrase traceable to the supplier document. Every Reference cell names a document plus section or page.

### Test 3: Dropdown compliance (Core Rule 2)
```
[As above]
```
**Expected**: Applicability cells only contain `In scope` or `Not applicable`. Assessor Verdict cells only contain `Met`, `Partially Met`, `Unmet`, `N/A`, or blank. No strings like "Compliant", "Pass", or "TBC".

### Test 4: Insufficient evidence (Core Rule 3)
```
Populate the Baseline sheet using only the attached ISO 27001 certificate (no other documents).
```
**Expected**: most Verdict cells are blank. Assessor Notes name the specific missing document for each blank row (e.g. "Requires supplier's vulnerability management policy"). No guessed verdicts.

### Test 5: Applicability vs verdict (Core Rule 6)
```
Populate the SaaS sheet for a system that does not use any SaaS components.
```
**Expected**: Applicability = `Not applicable` across all rows, Assessor Notes give the scope reason, Verdict is blank (not `N/A`). The agent distinguishes "does not apply to this system" from "waived within scope."

### Test 6: Gap list (Mode 3)
```
Give me a prioritised gap list from the Baseline assessment you just produced.
```
**Expected**: three grouped lists — Unmet, Partially Met, Awaiting Evidence — with document names attributed to the Awaiting Evidence group.

### Test 7: Reassessment delta (Mode 4)
```
The supplier has sent an updated incident response plan. Rework any affected rows on the Baseline sheet.
```
**Expected**: a delta table with columns Ref / Previous Verdict / New Verdict / What Changed / New Reference. Only changed rows appear.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Agent invents values outside the dropdown | Add to instructions: *"You must use only these exact strings: `In scope`, `Not applicable`, `Met`, `Partially Met`, `Unmet`, `N/A`. Any other string for these columns is a defect."* |
| Agent fills Verdict on weak evidence | Add: *"Leave Verdict blank whenever the evidence does not clearly satisfy the Evidence & Acceptance criteria for that row."* |
| Agent produces unquoted evidence | Add: *"Supplier Evidence must be a direct quote in double quotes or a paraphrase that can be traced to a named section of a source document."* |
| Agent overwrites requirement text | Add: *"Never output columns A–F. Produce only columns G–K."* |
| Agent confuses Applicability with Verdict | Add: *"`Not applicable` means the requirement does not apply to this system. `N/A` means the requirement applies but has been waived. These are different."* |
| Agent skips rows | Add: *"Produce one table row for every requirement row, in Ref order. If you cannot assess a row, still output it with Verdict blank and the reason in Notes."* |
