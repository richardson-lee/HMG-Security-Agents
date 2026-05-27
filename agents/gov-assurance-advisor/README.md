# Government Assurance Advisor

A prompt for Microsoft Copilot covering UK public sector cyber security work: GovAssure scoping, CAF assessments, and Secure by Design reviews.

For cross-framework mapping (CAF ↔ ISO ↔ CIS ↔ NIST CSF), use the [Security Requirements Analyst](../security-requirements-analyst/) — that's the canonical owner.

---

## How to use

1. Open a new Microsoft Copilot chat.
2. Paste the contents of [`instructions.md`](instructions.md) as your first message.
3. Attach any policy documents, prior CAF assessments, or Secure by Design self-assessments you have.
4. Ask a question — see Starter Prompts below.

You'll need to paste the prompt again at the start of each new chat.

---

## Starter Prompts

| Prompt | What it does |
|--------|--------------|
| *"Help me scope our essential services using the Five Lenses approach."* | GovAssure scoping |
| *"Assess our current state against CAF Objective B."* | CAF gap analysis |
| *"Review this project against Secure by Design principles."* | SbD review |
| *"What's new in CAF 4.0 and how does it relate to GovAssure 2025-26?"* | Version awareness |

---

## Validation Tests

### Test 1: Five Lenses Scoping
```
We need to identify which systems are in scope for GovAssure. Start by helping me identify our essential services.
```
**Expected**: Starts with Lens 1 questions (citizen impact, legal mandates, CNI). Does NOT jump to listing systems.

### Test 2: Framework Citation
```
What CAF requirements apply to identity and access management?
```
**Expected**: Specific clauses (e.g., "CAF B2.a"), not vague references.

### Test 3: CAF Assessment
```
Assess our current state against CAF principle A3. We have a CMDB but it's not complete, and we don't have a data inventory.
```
**Expected**: "Partially Achieved" with specific gaps. References A3.a, A3.b, A3.c outcomes.

### Test 4: Secure by Design Review
```
We're in Alpha phase of a new citizen-facing digital service. Review our approach against Secure by Design.
```
**Expected**: Asks about threat modelling, architecture options, control selection. Assesses confidence profile.

### Test 5: Achievement Level Justification
```
We have security policies but they were last reviewed 3 years ago and staff aren't aware of them. What's the CAF B1 achievement level?
```
**Expected**: "Partially Achieved" or "Not Achieved" with justification. Does NOT inflate to "Achieved".

### Test 6: Proportionality
```
What's the minimum we need to achieve "Achieved" for CAF C1?
```
**Expected**: Proportionate guidance based on risk, not gold-plated. Distinguishes Baseline from Enhanced profile.

### Test 7: Uncertainty Handling
```
Would our current setup meet GovAssure requirements?
```
**Expected**: Asks clarifying questions about what "current setup" includes. Does NOT make assumptions.

### Test 8: Framework Mapping — refer out
```
Map our CAF-based controls to ISO 27001.
```
**Expected**: Points to the Security Requirements Analyst as the canonical mapping owner. A brief headline mapping for specific clauses is acceptable.

### Test 9: Scoping Pitfall
```
Here are the 50 systems we've identified for GovAssure scope: [list]
```
**Expected**: Asks about essential services first. Warns about bottom-up scoping.

### Test 10: Version Awareness
```
What's in CAF 4.0?
```
**Expected**: Mentions threat intelligence, threat hunting, AI risk. Notes GovAssure 2025-26 uses CAF 3.2.

---

## Troubleshooting

| Problem | Add to the prompt |
|---------|-------------------|
| Not citing specific clauses | *"Always cite the specific contributing outcome, e.g., CAF B2.a, not just the principle."* |
| Gold-plating | *"Be proportionate. Recommend what's needed for the risk level, not best practice in all cases."* |
| Skipping to systems in scoping | *"Always start Five Lenses with essential services. Never start with systems."* |
| Not distinguishing profiles | *"Always clarify whether Baseline or Enhanced profile applies before assessing."* |
| Speculating on compliance | *"Never speculate. If evidence is insufficient, state what's needed to assess."* |
