# Cloud Security Expert Agent

A Copilot Studio agent for UK public sector cloud security work: architecture review, threat detection, secure migration, DevSecOps, posture assessment, and incident response. Combines SANS Cloud Security curriculum knowledge with UK government cloud requirements.

---

## Setup

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create a new agent
3. Copy the contents of [`instructions.md`](instructions.md) into the **Instructions** field
4. (Optional) Upload cloud architecture diagrams, security policies, or CIS benchmark reports to **Knowledge**
5. Publish and share with your team

### Alternative: Paste into Regular Copilot Chat

If agents are disabled in your environment:

1. Open a new chat in Microsoft Copilot
2. Paste the contents of [`instructions.md`](instructions.md) at the start
3. Continue chatting — Copilot will follow these instructions for the rest of the session

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Review our AWS architecture against the Cloud Security Principles"* | Architecture assessment (Mode 1) |
| *"What detection rules should we have for our Azure environment?"* | Threat detection design (Mode 2) |
| *"We're migrating our case management system to Azure. What security controls do we need?"* | Migration security planning (Mode 3) |
| *"Review our deployment pipeline for security gaps"* | DevSecOps assessment (Mode 4) |
| *"Assess our AWS account against CIS Benchmarks"* | Posture assessment (Mode 5) |
| *"Help me investigate suspicious activity in our cloud environment"* | Incident response (Mode 6) |

---

## Modes

| Mode | Purpose | SANS Courses Applied |
|------|---------|---------------------|
| **Cloud Security Architecture Review** | Assess architecture against Cloud Principles | SEC549, SEC510 |
| **Cloud Threat Detection & Response** | Design detection capabilities and rules | SEC541, SEC502 |
| **Secure Cloud Migration** | Plan security for cloud migration | SEC510, SEC540 |
| **DevSecOps and Pipeline Security** | Embed security in CI/CD pipelines | SEC540 |
| **Cloud Security Posture Assessment** | Assess estate against CIS Benchmarks | SEC510, SEC502 |
| **Cloud Incident Response** | Investigate or prepare for incidents | SEC541, SEC510 |

---

## Validation Tests

### Test 1: Cloud Principle Citation (Core Rule)
```
What Cloud Security Principles apply to data protection in AWS S3?
```
**Expected**: Specific citations like "Cloud Principle 1", "Cloud Principle 2", "Cloud Principle 3" with explanations. Should NOT say "cloud security principles around data protection".

### Test 2: Shared Responsibility Clarity (Core Rule)
```
Who is responsible for patching EC2 instances in AWS?
```
**Expected**: Clear statement that OS patching is customer responsibility in IaaS. Should reference the shared responsibility model.

### Test 3: Architecture Review (Mode 1)
```
We're deploying a citizen-facing web application to Azure. It handles OFFICIAL-SENSITIVE data. Review our architecture approach.
```
**Expected**: Should ask about identity federation, network boundaries, data residency, encryption. Should map to specific Cloud Principles. Should flag OFFICIAL-SENSITIVE handling requirements.

### Test 4: Threat Detection Design (Mode 2)
```
What AWS CloudTrail events should we alert on?
```
**Expected**: Specific event types like root account usage, IAM policy changes, security group modifications. Should reference MITRE ATT&CK techniques (e.g., T1078).

### Test 5: Migration Security (Mode 3)
```
We're migrating our HR system to a SaaS provider. What security considerations apply?
```
**Expected**: Should ask about data classification, provider assurance (Cloud Principles satisfaction), contract terms, exit strategy. Should reference G-Cloud if appropriate.

### Test 6: DevSecOps Assessment (Mode 4)
```
We use GitHub Actions to deploy Terraform to AWS. How do we secure this pipeline?
```
**Expected**: Should cover secrets management, IaC scanning (Checkov/tfsec), OIDC vs. static credentials, policy as code. Should give specific tool recommendations.

### Test 7: Posture Assessment (Mode 5)
```
We ran Prowler and got 47 high-severity findings. How should we prioritise?
```
**Expected**: Should distinguish public exposure risks from configuration drift. Should ask about data classification and internet exposure. Should recommend quick wins vs. architectural changes.

### Test 8: Incident Response (Mode 6)
```
We've detected unusual API calls from an IAM user. What should we do?
```
**Expected**: Should recommend immediate containment (revoke keys, disable user), evidence preservation (CloudTrail export), investigation steps. Should NOT recommend "wait and see".

### Test 9: HMG Context (UK Requirements)
```
Can we store OFFICIAL data in AWS us-east-1?
```
**Expected**: Should clarify UK data residency requirements for OFFICIAL data — UK regions required unless documented exemption. Should mention sovereignty considerations.

### Test 10: Uncertainty Handling (Core Rule)
```
Is our cloud environment secure?
```
**Expected**: Should ask clarifying questions about which provider, what services, current controls, data classification. Should NOT give a yes/no answer without information.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Not citing specific principles | Add: *"Always cite the specific Cloud Security Principle number, e.g., Cloud Principle 5, not just 'cloud security best practice'."* |
| Unclear on shared responsibility | Add: *"Always explicitly state whether a control is customer responsibility, provider responsibility, or shared."* |
| Missing UK context | Add: *"Always consider UK data residency requirements and government classification when making recommendations."* |
| Generic tooling advice | Add: *"Recommend specific tools by name, not generic categories. For example, 'use Checkov' not 'use an IaC scanner'."* |
| Speculating on configurations | Add: *"Never assume cloud configurations. Ask for evidence or recommend specific checks to verify."* |
| Over-engineering recommendations | Add: *"Be proportionate. Recommend controls appropriate to the data classification and risk level, not gold-plated solutions for every scenario."* |

---

## SANS Cloud Security Curriculum Coverage

This agent incorporates knowledge from:

| Course | Topic | GIAC Cert |
|--------|-------|-----------|
| SEC480 | AWS Security Essentials | AWS Secure Builder |
| SEC502 | Cloud Security Tactical Defense | GCLD |
| SEC510 | Cloud Security Engineering and Controls | GPCS |
| SEC540 | Cloud Native Security and DevSecOps | GCSA |
| SEC541 | Cloud Security Threat Detection | GCTD |
| SEC549 | Cloud Security Architecture | GCAD |

---

## Resources

- [NCSC Cloud Security Principles](https://www.ncsc.gov.uk/collection/cloud/the-cloud-security-principles)
- [NCSC Cloud Security Guidance](https://www.ncsc.gov.uk/collection/cloud)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks)
- [MITRE ATT&CK Cloud Matrix](https://attack.mitre.org/matrices/enterprise/cloud/)
- [AWS Well-Architected Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/)
- [Azure Security Benchmark](https://docs.microsoft.com/en-us/security/benchmark/azure/)
- [G-Cloud Framework](https://www.gov.uk/guidance/g-cloud-buyers-guide)
