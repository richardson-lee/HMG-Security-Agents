# Security Requirements Analyst Agent

A Copilot Studio agent for GRC work: comparing security requirements, generating new requirements mapped to frameworks, and assessing supplier compliance.

---

## Setup

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create a new agent
3. Copy the contents of [`instructions.md`](instructions.md) into the **Instructions** field
4. (Optional) Upload your baseline security requirements to the agent's **Knowledge**
5. Publish and share with your team

### Alternative: Paste into Regular Copilot Chat

If agents are disabled in your environment:

1. Open a new chat in Microsoft Copilot
2. Paste the contents of [`instructions.md`](instructions.md) at the start
3. Continue chatting — Copilot will follow these instructions for the rest of the session

> **Note**: You'll need to paste the instructions again each time you start a new chat.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Compare our baseline security requirements against a supplier's security documentation and highlight any gaps."* | Runs a gap analysis (Mode 1) |
| *"Generate security requirements for a new cloud-hosted system, mapped to the CAF framework."* | Creates requirements with framework mapping (Mode 2) |
| *"Show me how our CAF-based requirements map to ISO 27002:2022 controls."* | Cross-framework mapping (Mode 4) |

---

## Modes

| Mode | Purpose |
|------|---------|
| **Compare Requirements** | Gap analysis between two requirement sets |
| **Generate Requirements** | Create new requirements mapped to frameworks (technical or commercial format) |
| **Assess Compliance** | Check if supplier docs meet your baseline |
| **Map Frameworks** | Cross-reference between CAF, ISO 27002, CIS Controls, etc. |

---

## Validation Tests

### Test 1: Basic Comparison (Mode 1)
```
Compare the following supplier security requirements against our baseline requirements. Identify any gaps.

[Paste a short set of 5-10 supplier requirements]
```
**Expected**: Table with baseline mapping, status assessment, evidence citations. Should ask which is baseline if unclear.

### Test 2: Framework Citation (Core Rule)
```
What security requirements from the CAF apply to cloud-hosted CRM systems?
```
**Expected**: Specific CAF clause references (e.g., "CAF B2.a", "CAF B4.c"), not vague statements.

### Test 3: Requirements Generation - Technical (Mode 2)
```
We are evaluating Salesforce for CRM. Generate draft security requirements from the CAF for assessing this SaaS product. Use technical format.
```
**Expected**: Table with ID, requirement statement, CAF clause mapping, rationale.

### Test 4: Requirements Generation - Commercial (Mode 2)
```
Convert those Salesforce requirements into commercial language suitable for an RFP.
```
**Expected**: Plain language, outcome-focused statements without technical jargon.

### Test 5: Compliance Assessment (Mode 3)
```
Review the attached supplier security response and assess whether it meets our baseline requirements. Flag any gaps that need clarification before we proceed.

[Attach a supplier security questionnaire response or design document]
```
**Expected**: Summary count, detailed table with evidence quotes, prioritised gap list, clarification questions.

### Test 6: Cross-Framework Mapping (Mode 4)
```
Show me how CAF Objective B (Protecting Against Cyber Attack) maps to ISO 27002:2022 and CIS Controls v8.
```
**Expected**: Mapping table with specific clause numbers, notes on scope differences.

### Test 7: Uncertainty Handling (Core Rule)
```
Does the supplier's proposal meet our requirements for incident response?

[Provide a document that doesn't mention incident response]
```
**Expected**: Should clearly state it cannot assess — should NOT fabricate an answer.

### Test 8: Document Update (if supported)
```
Add these three requirements to the attached spreadsheet in the "New Requirements" tab:
1. [Requirement text]
2. [Requirement text]
3. [Requirement text]
```
**Expected**: Updates spreadsheet or provides structured output for copy/paste.

### Test 9: Baseline Reference
```
What are our standing requirements for third-party access management?
```
**Expected**: Should pull from baseline requirements in Knowledge, citing specific IDs or sections.

### Test 10: Edge Case - Ambiguous Request
```
Compare these requirements.

[Provide only one set of requirements]
```
**Expected**: Should ask which set is baseline and request the comparison set.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Not citing specific clauses | Add to instructions: *"You must include the specific clause number for every framework reference."* |
| Too verbose | Add: *"Keep responses concise. Use tables for structured data."* |
| Speculating when data is missing | Add: *"Never speculate. If you cannot find evidence, state 'Unable to assess'."* |
| Wrong framework | Add: *"Use CAF as the primary framework unless I specify otherwise."* |
| Not asking clarifying questions | Add: *"If my request is ambiguous, ask one clarifying question before proceeding."* |
