# HMG Security Agents

A collection of Microsoft Copilot prompts for UK public sector security work — GovAssure, CAF assessments, Secure by Design reviews, requirements analysis, and more. Each "agent" is a focused system prompt you paste at the start of a Copilot chat.

---

## Available Agents

| Agent | Purpose | Link |
|-------|---------|------|
| **Security Requirements Analyst** | Compare requirements, generate framework-mapped requirements, assess supplier compliance, cross-framework mapping (canonical owner) | [View](agents/security-requirements-analyst/) |
| **Government Assurance Advisor** | GovAssure scoping, CAF assessments, Secure by Design reviews | [View](agents/gov-assurance-advisor/) |
| **Cloud Security Expert** | Cloud architecture, threat detection, secure migration, DevSecOps, posture assessment, and incident response across AWS, Azure, GCP, M365 | [View](agents/cloud-security-expert/) |
| **Evidence Tracker Populator** | First-pass fill of the HMG Security Requirements Evidence Tracker from supplier documentation | [View](agents/evidence-tracker-populator/) |
| **Security Case Populator** | First-pass draft of the DBS Security Case from threat model, design documentation, and requirements evidence | [View](agents/security-case-populator/) |

---

## Quick Start

1. Open a new chat in [Microsoft Copilot](https://copilot.microsoft.com/).
2. Pick an agent from the table above and copy the contents of its `instructions.md`.
3. Paste it as your first message in the chat.
4. Attach any relevant documents (supplier responses, design docs, threat models, the evidence tracker template, etc.).
5. Ask a question — see each agent's README for Starter Prompts.

You'll need to paste the prompt again at the start of each new chat.

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
| CIS Benchmarks (Cloud) | CIS [Provider] [Benchmark] [Control] | CIS AWS Foundations 1.1 |
| MITRE ATT&CK Cloud | T[Number] [Technique Name] | T1078.004 Cloud Accounts |
| CSA Cloud Controls Matrix | CCM [Control ID] | CCM IVS-03 |
| ISO 27017:2015 | ISO 27017 [Control] | ISO 27017 CLD.6.3 |

See [Framework Quick Reference](docs/framework-quick-reference.md) for detailed mappings and citation guidance.

---

## Repository Structure

```
/
├── README.md                              # This file
├── agents/
│   ├── security-requirements-analyst/
│   │   ├── README.md                      # How to use, starter prompts, validation tests
│   │   └── instructions.md                # The prompt to paste into Copilot
│   ├── gov-assurance-advisor/
│   ├── cloud-security-expert/
│   ├── evidence-tracker-populator/
│   └── security-case-populator/
│       (each follows the same README + instructions.md pattern)
└── docs/
    └── framework-quick-reference.md       # CAF, Secure by Design, Five Lenses reference
```

---

## Contributing

To add a new agent:

1. Create a new folder under `agents/` with a descriptive name
2. Add `instructions.md` — the chat-paste prompt
3. Add `README.md` with how-to-use, starter prompts, and validation tests
4. Update this README's agent table
5. Submit a pull request

### Prompt Design Guidelines

- **Short and focused** — target ≤700 words. The model already knows what frameworks are; don't teach them.
- **Specific citations** — always require framework clause numbers (e.g., CAF B2.a), not vague references.
- **Uncertainty handling** — prompts should make the agent ask clarifying questions, not speculate.
- **British English** — all content uses British English spellings.
- **Validation tests** — include at least 5 representative tests in the README.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| v2.4 | May 2026 | Refocused as chat-paste prompts (not Copilot Studio agents): aggressively slimmed every `instructions.md` to ≤800 words; removed framework reference tables, methodology tutorials, and tooling lists that the model already knows; reframed READMEs for chat-paste; restored single Cloud Security Expert |
| v2.3 | May 2026 | (Superseded by v2.4 — was a Copilot Studio-oriented expansion based on a misunderstood deployment target) |
| v2.2 | April 2026 | Added Security Case Populator agent |
| v2.1 | April 2026 | Added Evidence Tracker Populator agent |
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
