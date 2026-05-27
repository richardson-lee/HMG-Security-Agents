# Security Requirements Analyst

A prompt for GRC work in Microsoft Copilot: comparing security requirements, generating new requirements mapped to frameworks, and assessing supplier compliance.

---

## How to use

1. Open a new Microsoft Copilot chat.
2. Paste the contents of [`instructions.md`](instructions.md) as your first message.
3. Attach any baseline requirements, supplier responses, or other documents you want assessed.
4. Ask a question — see Starter Prompts below.

You'll need to paste the prompt again at the start of each new chat.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Compare the attached supplier's security documentation against our baseline requirements and highlight any gaps."* | Gap analysis |
| *"Generate security requirements for a new cloud-hosted system, mapped to the CAF framework."* | Requirements generation (technical format) |
| *"Convert those into commercial language for an RFP."* | Requirements generation (commercial format) |
| *"Review the attached supplier security response against our baseline."* | Compliance assessment |
| *"Show me how CAF Objective B maps to ISO 27002:2022 and CIS Controls v8."* | Cross-framework mapping |

---

## Validation Tests

### Test 1: Basic Comparison
```
Compare these supplier requirements against our baseline. Identify any gaps.
[Paste 5-10 supplier requirements]
```
**Expected**: Table with baseline mapping, status, evidence citations. Asks which is baseline if unclear.

### Test 2: Framework Citation
```
What CAF requirements apply to cloud-hosted CRM systems?
```
**Expected**: Specific CAF clause references (e.g., "CAF B2.a"), not vague statements.

### Test 3: Technical Requirements
```
Generate draft security requirements from the CAF for assessing Salesforce as a SaaS CRM. Technical format.
```
**Expected**: Table with ID, requirement, CAF clause mapping, rationale.

### Test 4: Commercial Requirements
```
Convert those into commercial language for an RFP.
```
**Expected**: Plain language, outcome-focused, no jargon.

### Test 5: Compliance Assessment
```
Assess whether the attached supplier response meets our baseline. Flag gaps needing clarification.
[Attach supplier doc]
```
**Expected**: Summary count, table with evidence quotes, prioritised gap list, clarification questions.

### Test 6: Cross-Framework Mapping
```
Show me how CAF Objective B maps to ISO 27002:2022 and CIS Controls v8.
```
**Expected**: Mapping table with specific clause numbers, notes where mappings are approximate.

### Test 7: Uncertainty Handling
```
Does the supplier's proposal meet our requirements for incident response?
[Attach doc that doesn't mention IR]
```
**Expected**: Clear "unable to assess" — no fabrication.

### Test 8: Ambiguous Request
```
Compare these requirements.
[Provide only one set]
```
**Expected**: Asks which is baseline; requests the comparison set.

---

## See Also

Once you've assessed a supplier with this prompt, use the [Evidence Tracker Populator](../evidence-tracker-populator/) to produce a first-pass fill of the HMG Security Requirements Evidence Tracker.

---

## Troubleshooting

| Problem | Add to the prompt |
|---------|-------------------|
| Not citing specific clauses | *"You must include the specific clause number for every framework reference."* |
| Too verbose | *"Keep responses concise. Use tables for structured data."* |
| Speculating when data is missing | *"Never speculate. If you cannot find evidence, state 'Unable to assess'."* |
| Wrong framework | *"Use CAF as the primary framework unless I specify otherwise."* |
