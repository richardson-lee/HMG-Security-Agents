# Cloud Security Expert

A prompt for Microsoft Copilot covering UK public sector cloud security across AWS, Azure, GCP, and M365: architecture review, threat detection, secure migration, DevSecOps, posture assessment, and incident response.

---

## How to use

1. Open a new Microsoft Copilot chat.
2. Paste the contents of [`instructions.md`](instructions.md) as your first message.
3. Attach any architecture diagrams, security policies, CIS benchmark reports, or alert details you want assessed.
4. Ask a question — see Starter Prompts below.

You'll need to paste the prompt again at the start of each new chat.

---

## Starter Prompts

| Prompt | Topic |
|--------|-------|
| *"Review our AWS architecture against the Cloud Security Principles."* | Architecture review |
| *"What detection rules should we have for our Azure environment?"* | Detection design |
| *"We're migrating our case management system to Azure. What security controls do we need?"* | Secure migration |
| *"Review our GitHub Actions → Terraform → AWS pipeline for security gaps."* | DevSecOps |
| *"Assess our AWS account against CIS Benchmarks."* | Posture assessment |
| *"We've detected unusual API calls from an IAM user. Walk me through the response."* | Incident response |

---

## Validation Tests

### Test 1: Cloud Principle Citation
```
What Cloud Security Principles apply to data protection in AWS S3?
```
**Expected**: Specific citations ("Cloud Principle 1", "Cloud Principle 2", "Cloud Principle 3") with explanations. Not "cloud security principles around data protection".

### Test 2: Shared Responsibility
```
Who is responsible for patching EC2 instances in AWS?
```
**Expected**: OS patching is customer responsibility in IaaS. References shared responsibility model.

### Test 3: Architecture Review
```
We're deploying a citizen-facing web application to Azure. It handles OFFICIAL-SENSITIVE data. Review our architecture approach.
```
**Expected**: Asks about identity federation, network boundaries, data residency, encryption. Maps to specific Cloud Principles. Flags OFFICIAL-SENSITIVE handling.

### Test 4: Threat Detection
```
What AWS CloudTrail events should we alert on?
```
**Expected**: Specific event types (root usage, IAM policy changes, security group changes). References MITRE ATT&CK (T1078 etc.).

### Test 5: Migration Security
```
We're migrating our HR system to a SaaS provider. What security considerations apply?
```
**Expected**: Data classification, provider assurance, contract terms, exit strategy. References G-Cloud if appropriate.

### Test 6: DevSecOps
```
We use GitHub Actions to deploy Terraform to AWS. How do we secure this pipeline?
```
**Expected**: Secrets management, IaC scanning (Checkov/tfsec), OIDC vs static credentials, policy as code. Specific tool names.

### Test 7: Posture Prioritisation
```
We ran Prowler and got 47 high-severity findings. How should we prioritise?
```
**Expected**: Distinguishes public exposure from configuration drift. Asks about data classification and internet exposure. Quick wins vs architectural changes.

### Test 8: Incident Response
```
We've detected unusual API calls from an IAM user. What should we do?
```
**Expected**: Immediate containment (revoke keys, disable user), evidence preservation (CloudTrail export), investigation steps. Does NOT recommend "wait and see".

### Test 9: HMG UK Context
```
Can we store OFFICIAL data in AWS us-east-1?
```
**Expected**: UK data residency required for OFFICIAL unless documented exemption. Mentions sovereignty considerations.

### Test 10: Uncertainty Handling
```
Is our cloud environment secure?
```
**Expected**: Asks clarifying questions about provider, services, controls, data classification. Does NOT give a yes/no answer without information.

---

## Troubleshooting

| Problem | Add to the prompt |
|---------|-------------------|
| Not citing specific principles | *"Always cite the specific Cloud Security Principle number, e.g., Cloud Principle 5."* |
| Unclear on shared responsibility | *"Always explicitly state whether a control is customer responsibility, provider responsibility, or shared."* |
| Missing UK context | *"Always consider UK data residency and government classification when making recommendations."* |
| Generic tooling advice | *"Recommend specific tools by name, not generic categories."* |
| Speculating on configurations | *"Never assume cloud configurations. Ask for evidence or recommend specific checks."* |
| Over-engineering | *"Be proportionate. Recommend controls appropriate to classification and risk, not gold-plated solutions."* |
