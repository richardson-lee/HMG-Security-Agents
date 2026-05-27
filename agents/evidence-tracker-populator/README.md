# Evidence Tracker Populator

A prompt for Microsoft Copilot that reads the HMG Security Requirements Evidence Tracker template plus a supplier's documentation pack, and produces a first-pass draft of the assessment columns. Turns a multi-day manual fill into a reviewable draft in one sitting.

The tracker itself lives in the companion [HMG-Security-Program](https://github.com/richardson-lee/HMG-Security-Program) repository.

---

## How to use

1. Open a new Microsoft Copilot chat.
2. Paste the contents of [`instructions.md`](instructions.md) as your first message.
3. Attach:
   - `hmg-security-requirements-evidence-tracker.xlsx` (from the HMG-Security-Program repo).
   - The supplier's security documentation pack for the system under assessment.
   - Optional: prior version of the tracker, baseline policies, existing security case material.
4. Ask a question — see Starter Prompts below.

You'll need to paste the prompt again at the start of each new chat.

---

## How output maps to the tracker

The prompt produces paste-ready blocks that a human transfers in two operations:

### Cover sheet (once per assessment)
```
SystemName:           HMRC Case Management Platform
SupplierName:         Example Ltd
AssessorName:         L Richardson
Tier_Baseline_Active: Yes
Tier_IaaS_Active:     No
Tier_PaaS_Active:     Yes
Tier_SaaS_Active:     No
Tier_AI_Active:       No
```
Paste each value into the matching named cell on the Cover sheet.

### Requirement sheets (once per active tier)

A markdown table per sheet, ordered by Ref, with five columns matching columns G–K. Copy the data rows (not the header) and paste into the tier sheet starting at cell G2. Columns A–F are pre-populated and must not be overwritten.

---

## Dropdown values

| Column | Allowed values |
|--------|----------------|
| **G Applicability** | `In scope`, `Not applicable` |
| **I Assessor Verdict** | `Met`, `Partially Met`, `Unmet`, `N/A`, or blank ("Not yet assessed") |

Blank verdict is valid and is how the dashboards flag "Not yet assessed". The prompt should leave Verdict blank whenever evidence is insufficient, and explain why in Assessor Notes.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Set up the Cover sheet for an assessment of [system] by [supplier]. The system uses SaaS and AI services. I am the assessor."* | Mode 1: Scope |
| *"Populate the Baseline sheet using the attached supplier response and design document."* | Mode 2: Populate |
| *"Produce a prioritised gap list grouped by what's missing."* | Mode 3: Gap list |
| *"The supplier sent an updated IAM policy. Update the affected rows and give me a delta."* | Mode 4: Reassessment |

---

## Validation Tests

### Test 1: Cover-sheet scoping
```
Set up the Cover sheet for "Acme Case Management" by "Acme Ltd". SaaS on AWS PaaS. I am the assessor.
```
**Expected**: SystemName / SupplierName / AssessorName / Tier_*_Active block with Tier_SaaS_Active=Yes, Tier_PaaS_Active=Yes, others No.

### Test 2: Grounded evidence
```
Populate the Baseline sheet using the attached supplier response.
```
**Expected**: Every Supplier Evidence cell is traceable to the supplier document. Every Reference names a document plus section/page.

### Test 3: Dropdown compliance
```
[As Test 2]
```
**Expected**: Applicability only `In scope` or `Not applicable`. Verdict only `Met`/`Partially Met`/`Unmet`/`N/A`/blank. No strings like "Compliant", "Pass", or "TBC".

### Test 4: Insufficient evidence
```
Populate the Baseline sheet using only the attached ISO 27001 certificate.
```
**Expected**: Most Verdict cells blank. Notes name the specific missing document per blank row. No guessed verdicts.

### Test 5: Applicability vs verdict
```
Populate the SaaS sheet for a system with no SaaS components.
```
**Expected**: Applicability = `Not applicable` across all rows, Notes give scope reason, Verdict is blank (not `N/A`).

### Test 6: Gap list
```
Give me a prioritised gap list from the Baseline assessment.
```
**Expected**: Three grouped lists — Unmet, Partially Met, Awaiting Evidence — with document names attributed to Awaiting Evidence.

### Test 7: Reassessment delta
```
The supplier sent an updated incident response plan. Rework any affected rows.
```
**Expected**: Delta table — Ref / Previous Verdict / New Verdict / What Changed / New Reference. Only changed rows appear.

### Test 8: Conflict surfacing
```
Populate row B2-04. The Security Policy says "MFA enforced for all admin access". The IAM export shows two break-glass accounts with MFA disabled.
```
**Expected**: Supplier Evidence quotes both. Verdict is blank. Notes name the conflict and request clarification on the break-glass exemption.

### Test 9: Synthesis discipline
```
Populate the SaaS sheet using the attached marketing one-pager and SOC 2 Type II report.
```
**Expected**: Where both address the same control, Reference cites the SOC 2 (more authoritative). Marketing-only claims appear with Verdict blank and a call for an authoritative source.

### Test 10: Partial pack
```
Populate everything you can. I've only attached the supplier's ISO 27001 certificate and Statement of Applicability.
```
**Expected**: No refusal. Rows the SoA evidence supports are populated. Other rows have Verdict blank with Notes naming the specific missing document.

---

## Troubleshooting

| Problem | Add to the prompt |
|---------|-------------------|
| Agent invents values outside the dropdown | *"You must use only these exact strings: `In scope`, `Not applicable`, `Met`, `Partially Met`, `Unmet`, `N/A`."* |
| Agent fills Verdict on weak evidence | *"Leave Verdict blank whenever the evidence does not clearly satisfy the Evidence & Acceptance criteria."* |
| Agent produces unquoted evidence | *"Supplier Evidence must be a direct quote in double quotes or a paraphrase traceable to a named section of a source document."* |
| Agent overwrites requirement text | *"Never output columns A–F. Produce only columns G–K."* |
| Agent confuses Applicability with Verdict | *"`Not applicable` means the requirement does not apply to this system. `N/A` means it applies but has been waived."* |
| Agent skips rows | *"Produce one table row for every requirement row, in Ref order. If you cannot assess a row, output it with Verdict blank and the reason in Notes."* |
