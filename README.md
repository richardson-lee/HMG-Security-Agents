# HMG Security Agents

A collection of Microsoft Copilot Studio agents for UK public sector security work — GovAssure, CAF assessments, Secure by Design reviews, requirements analysis, and more.

---

## Available Agents

| Agent | Purpose | Link |
|-------|---------|------|
| **Security Requirements Analyst** | Compare requirements, generate framework-mapped requirements, assess supplier compliance | [View](agents/security-requirements-analyst/) |
| **Government Assurance Advisor** | GovAssure scoping, CAF assessments, Secure by Design reviews, framework mapping | [View](agents/gov-assurance-advisor/) |

---

## Quick Start

### Option A: Copilot Studio Agent (Recommended)

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create a new agent
3. Choose an agent from the table above and copy its `instructions.md` into the **Instructions** field
4. (Optional) Upload relevant documents to the agent's **Knowledge**
5. Publish and share with your team

### Option B: Paste into Regular Copilot Chat

If Copilot Studio agents are disabled in your environment:

1. Open a new chat in Microsoft Copilot
2. Copy the contents of the relevant `instructions.md` file
3. Paste it at the start of your conversation
4. Continue chatting — Copilot will follow these instructions for the session

> **Note**: You'll need to paste the instructions again each time you start a new chat.

---

## Frameworks Covered

These agents reference the following UK government and international security frameworks:

| Framework | Citation Format | Example |
|-----------|-----------------|---------|
| NCSC Cyber Assessment Framework (CAF) | CAF [Principle].[Outcome] | CAF B2.a |
| ISO 27001/27002:2022 | ISO 27002:2022 [Clause] | ISO 27002:2022 5.15 |
| CIS Controls v8 | CIS Control [Number].[Safeguard] | CIS Control 6.1 |
| NCSC Cloud Security Principles | Cloud Principle [Number] | Cloud Principle 2 |
| Cyber Essentials / CE+ | — | — |
| GovS 007 (Security) | — | — |

See [Framework Quick Reference](docs/framework-quick-reference.md) for detailed mappings and citation guidance.

---

## Repository Structure

```
/
├── README.md                              # This file
├── agents/
│   ├── security-requirements-analyst/
│   │   ├── README.md                      # Setup, validation tests, troubleshooting
│   │   └── instructions.md                # Copilot Studio instructions
│   └── gov-assurance-advisor/
│       ├── README.md                      # Setup, validation tests, troubleshooting
│       └── instructions.md                # Copilot Studio instructions
└── docs/
    └── framework-quick-reference.md       # CAF, Secure by Design, Five Lenses reference
```

---

## Contributing

To add a new agent:

1. Create a new folder under `agents/` with a descriptive name
2. Add `instructions.md` with Copilot Studio instructions
3. Add `README.md` with setup guide, starter prompts, and validation tests
4. Update this README's agent table
5. Submit a pull request

### Agent Design Guidelines

- **Clear modes**: Define distinct operational modes with specific goals and outputs
- **Specific citations**: Always require framework clause numbers, not vague references
- **Uncertainty handling**: Agents should ask clarifying questions, not speculate
- **British English**: All content should use British English spellings
- **Validation tests**: Include at least 5 tests to verify agent behaviour

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| v2.0 | January 2026 | Restructured as multi-agent repository; added Government Assurance Advisor |
| v1.1 | January 2026 | Split into README + agent-instructions.md |
| v1.0 | January 2026 | Initial Security Requirements Analyst release |

---

## Resources

- [GovAssure Overview](https://www.security.gov.uk/policy-and-guidance/govassure/)
- [NCSC Cyber Assessment Framework](https://www.ncsc.gov.uk/collection/cyber-assessment-framework)
- [Secure by Design](https://www.security.gov.uk/policy-and-guidance/secure-by-design/)
- [Microsoft Copilot Studio](https://copilotstudio.microsoft.com/)

---

*Created with assistance from Claude. Optimised for Microsoft Copilot Studio.*
