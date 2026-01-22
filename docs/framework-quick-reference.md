# Framework Quick Reference

Quick reference for UK government security frameworks used across the agents.

---

## CAF Objectives and Principles

| Objective | Principle | Description |
|-----------|-----------|-------------|
| **A. Managing Risk** | A1 | Governance |
| | A2 | Risk management |
| | A3 | Asset management |
| | A4 | Supply chain |
| **B. Protecting** | B1 | Service protection policies and processes |
| | B2 | Identity and access control |
| | B3 | Data security |
| | B4 | System security |
| | B5 | Resilient networks and systems |
| | B6 | Staff awareness and training |
| **C. Detecting** | C1 | Security monitoring |
| | C2 | Proactive security event discovery |
| **D. Minimising Impact** | D1 | Response and recovery planning |
| | D2 | Lessons learned |

### CAF Achievement Levels

| Level | Description |
|-------|-------------|
| Not Achieved | Significant gaps; outcome not met |
| Partially Achieved | Some elements in place; gaps remain |
| Achieved | Outcome met; good practice demonstrated |
| Exceeded | Exemplary practice beyond requirements |

---

## Secure by Design Principles

| # | Principle | Summary |
|---|-----------|---------|
| 1 | Create responsibility for cyber security risk | Assign accountable risk owners with authority |
| 2 | Source secure technology products | Security due diligence on third-party products |
| 3 | Adopt a risk-driven approach | Establish risk appetite; maintain dynamic risk assessment |
| 4 | Design usable security controls | User research to ensure controls are fit for purpose |
| 5 | Build in detect and respond | Logging, monitoring, alerting, response capabilities |
| 6 | Design flexible architectures | Enable easier integration of new security controls |
| 7 | Minimise the attack surface | Only necessary capabilities, software, data, hardware |
| 8 | Defend in depth | Layered controls so single failures don't compromise |
| 9 | Embed continuous assurance | Ongoing confidence in security controls |
| 10 | Make changes securely | Security embedded in design, development, deployment |

### Self Assessment Tracker Confidence Profiles

| Profile | Spend Control Outcome |
|---------|----------------------|
| High | Approval to proceed |
| Medium | Discussion required; conditional approval possible |
| Low | No approval — significant work required |

---

## Five Lenses Summary

```
Essential Service
    └── Functions (what enables it)
            └── Infrastructure (what it runs on)
                    └── Systems (specific applications)
                            └── Locations (where they're hosted)
```

| Lens | Focus | Key Question |
|------|-------|--------------|
| 1 | Essential Services | If this failed, what's the citizen impact? |
| 2 | Organisational Functions | What business processes enable this service? |
| 3 | Core Infrastructure | What networks, cloud, identity services support it? |
| 4 | Systems and Applications | Which systems are critical? What's the priority? |
| 5 | Sites and Locations | Where are systems hosted? What are the dependencies? |

---

## Citation Format Guide

Always cite framework references with specific clause numbers:

| Framework | Format | Example |
|-----------|--------|---------|
| NCSC CAF | CAF [Principle].[Outcome] | CAF B2.a |
| ISO 27002:2022 | ISO 27002:2022 [Clause] | ISO 27002:2022 5.15 |
| CIS Controls v8 | CIS Control [Number].[Safeguard] | CIS Control 6.1 |
| NIST CSF 2.0 | [Function].[Category] | PR.AC |
| Cloud Security Principles | Cloud Principle [Number] | Cloud Principle 2 |

**Never use**:
- "CAF says..."
- "According to ISO..."
- "The framework requires..."

**Always use**:
- "CAF B2.a requires..."
- "ISO 27002:2022 5.15 states..."
- "CIS Control 6.1 specifies..."

---

## Cross-Framework Mapping

| Topic | CAF | NIST CSF | CIS v8 | ISO 27001 |
|-------|-----|----------|--------|-----------|
| Governance | A1 | GV | — | A.5.1-5.4 |
| Risk management | A2 | ID.RA, ID.RM | — | A.5.7-5.8 |
| Asset management | A3 | ID.AM | Control 1, 2 | A.5.9-5.14 |
| Supply chain | A4 | ID.SC | Control 15 | A.5.19-5.23 |
| Access control | B2 | PR.AC | Control 5, 6 | A.5.15-5.18 |
| Data security | B3 | PR.DS | Control 3 | A.5.33-5.38 |
| System security | B4 | PR.IP | Control 4, 7 | A.8.8-8.9 |
| Network security | B5 | PR.AC, PR.PT | Control 12, 13 | A.8.20-8.22 |
| Awareness | B6 | PR.AT | Control 14 | A.6.3 |
| Monitoring | C1 | DE.CM | Control 8 | A.8.15-8.16 |
| Detection | C2 | DE.AE | Control 17 | A.5.25 |
| Response | D1 | RS | Control 17 | A.5.24-5.28 |
| Lessons learned | D2 | RS.IM, RC.IM | — | A.5.27 |

---

## Key Resources

**GovAssure**:
- [GovAssure Overview](https://www.security.gov.uk/policy-and-guidance/govassure/)
- [GovAssure Guidance](https://www.security.gov.uk/policy-and-guidance/govassure/guidance/)
- [WebCAF](https://www.security.gov.uk/policy-and-guidance/govassure/webcaf/)

**CAF**:
- [NCSC Cyber Assessment Framework](https://www.ncsc.gov.uk/collection/cyber-assessment-framework)

**Secure by Design**:
- [Secure by Design Overview](https://www.security.gov.uk/policy-and-guidance/secure-by-design/)
- [Secure by Design Principles](https://www.security.gov.uk/policy-and-guidance/secure-by-design/principles/)
- [Self Assessment Tracker](https://www.security.gov.uk/policy-and-guidance/secure-by-design/activities/tracking-secure-by-design-progress/)
