# Security Requirements Analyst Agent

A ready-to-use persona for Microsoft Copilot that helps with GRC work: comparing security requirements, generating new requirements mapped to frameworks, and assessing supplier compliance.

---

## Quick Start

There are two ways to use this:

### Option A: Create a Copilot Studio Agent (Recommended)

If your organisation has Copilot Studio enabled:

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create a new agent
3. Copy the contents of [`agent-instructions.md`](agent-instructions.md) into the **Instructions** field
4. (Optional) Upload your baseline security requirements to the agent's **Knowledge**
5. Publish and share with your team

### Option B: Paste into a Regular Copilot Chat

**If agents are disabled in your environment** (common in corporate/public sector settings), you can still use this:

1. Open a new chat in Microsoft Copilot
2. Copy the contents of [`agent-instructions.md`](agent-instructions.md)
3. Paste it at the start of your conversation
4. Continue chatting — Copilot will follow these instructions for the rest of the session

> **Note**: You'll need to paste the instructions again each time you start a new chat. The behaviour won't persist across sessions, but it works identically within a single conversation.

---

## Starter Prompts

Once the agent is set up (or instructions pasted), try these:

| Prompt | What it does |
|--------|--------------|
| *"Compare our baseline security requirements against a supplier's security documentation and highlight any gaps."* | Runs a gap analysis (Mode 1) |
| *"Generate security requirements for a new cloud-hosted system, mapped to the CAF framework."* | Creates requirements with framework mapping (Mode 2) |
| *"Show me how our CAF-based requirements map to ISO 27002:2022 controls."* | Cross-framework mapping (Mode 4) |

---

## What It Does

The agent operates in four modes:

| Mode | Purpose |
|------|---------|
| **Compare Requirements** | Gap analysis between two requirement sets |
| **Generate Requirements** | Create new requirements mapped to frameworks (technical or commercial format) |
| **Assess Compliance** | Check if supplier docs meet your baseline |
| **Map Frameworks** | Cross-reference between CAF, ISO 27002, CIS Controls, etc. |

Supported frameworks: NCSC CAF, ISO 27001/27002:2022, CIS Controls v8, NCSC Cloud Security Principles, Cyber Essentials, GovS 007.

---

## Setup Checklist (Copilot Studio)

- [ ] Created the agent in Copilot Studio
- [ ] Copied instructions from `agent-instructions.md` into the Instructions field
- [ ] Uploaded baseline security requirements to Knowledge (optional but recommended)
- [ ] Configured connectors (SharePoint, etc.) if needed
- [ ] Tested with validation prompts below

---

## Validation Tests

Use these to check the agent is working correctly:

| Test | Prompt | Expected Behaviour |
|------|--------|-------------------|
| Framework citation | *"What CAF requirements apply to cloud CRM?"* | Should cite specific clauses like "CAF B2.a", not vague references |
| Uncertainty handling | *"Does this doc meet our incident response requirements?"* (provide doc that doesn't mention IR) | Should say it cannot assess, not fabricate an answer |
| Ambiguous request | *"Compare these requirements."* (provide only one set) | Should ask which is baseline and request the comparison set |

See the full list of 10 validation tests in the [Validation Tests](#full-validation-tests) section below.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Not citing specific clauses | Add to instructions: *"You must include the specific clause number for every framework reference."* |
| Too verbose | Add: *"Keep responses concise. Use tables for structured data."* |
| Speculating when data is missing | Add: *"Never speculate. If you cannot find evidence, state 'Unable to assess'."* |
| Wrong framework | Add: *"Use CAF as the primary framework unless I specify otherwise."* |
| Not asking clarifying questions | Add: *"If my request is ambiguous, ask one clarifying question before proceeding."* |

---

## Full Validation Tests

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

## Version History

| Version | Date | Notes |
|---------|------|-------|
| v1.1 | January 2026 | Split into README + agent-instructions.md, added starter prompts |
| v1.0 | January 2026 | Initial version |

---

*Created with assistance from Claude. Optimised for Microsoft Copilot Studio.*
