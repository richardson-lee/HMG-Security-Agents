# Government Assurance Advisor Agent

A Copilot Studio agent for UK public sector cyber security work: GovAssure scoping, CAF assessments, Secure by Design reviews, and framework mapping.

---

## Setup

1. Go to [Copilot Studio](https://copilotstudio.microsoft.com/)
2. Create a new agent
3. Copy the contents of [`instructions.md`](instructions.md) into the **Instructions** field
4. (Optional) Upload relevant policy documents to the agent's **Knowledge**
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
| *"Help me scope our essential services using the Five Lenses approach"* | Guides through GovAssure scoping (Mode 1) |
| *"Assess our current state against CAF Objective B"* | Gap analysis against CAF principles (Mode 2) |
| *"Review this project against Secure by Design principles"* | SbD compliance review (Mode 3) |
| *"Map CAF requirements to ISO 27002:2022 controls"* | Cross-framework mapping (Mode 4) |

---

## Modes

| Mode | Purpose |
|------|---------|
| **Scope Essential Services** | Five Lenses approach for identifying GovAssure scope |
| **Assess Against CAF** | Gap analysis against Cyber Assessment Framework |
| **Review Secure by Design** | Project assessment against 10 SbD principles |
| **Map Frameworks** | Cross-reference CAF, ISO 27002, CIS, NIST CSF |

---

## Validation Tests

### Test 1: Five Lenses Scoping (Mode 1)
```
We need to identify which systems are in scope for GovAssure. Start by helping me identify our essential services.
```
**Expected**: Should start with Lens 1 questions about citizen impact, legal mandates, CNI. Should NOT jump straight to listing systems.

### Test 2: Framework Citation (Core Rule)
```
What CAF requirements apply to identity and access management?
```
**Expected**: Specific citations like "CAF B2.a", "CAF B2.b", not vague references to "CAF access control requirements".

### Test 3: CAF Assessment (Mode 2)
```
Assess our current state against CAF principle A3 (Asset Management). We have a CMDB but it's not complete, and we don't have a data inventory.
```
**Expected**: Should assess as "Partially Achieved" with specific gaps identified. Should reference A3.a, A3.b, A3.c contributing outcomes.

### Test 4: Secure by Design Review (Mode 3)
```
We're in Alpha phase of a new citizen-facing digital service. Review our approach against Secure by Design principles.
```
**Expected**: Should ask about threat modelling, architecture options, control selection. Should assess confidence profile.

### Test 5: Achievement Level Justification
```
We have security policies but they were last reviewed 3 years ago and staff aren't aware of them. What's the CAF B1 achievement level?
```
**Expected**: Should assess as "Partially Achieved" or "Not Achieved" with clear justification. Should not inflate to "Achieved".

### Test 6: Proportionality (Core Rule)
```
What's the minimum we need to achieve "Achieved" for CAF C1 (Security Monitoring)?
```
**Expected**: Should give proportionate guidance based on risk, not gold-plated recommendations. Should distinguish baseline from enhanced profiles.

### Test 7: Uncertainty Handling
```
Would our current setup meet GovAssure requirements?
```
**Expected**: Should ask clarifying questions about what "current setup" includes. Should NOT make assumptions.

### Test 8: Framework Mapping (Mode 4)
```
I need to show how our CAF-based controls also satisfy ISO 27001 requirements for an audit.
```
**Expected**: Mapping table with specific clause numbers. Should note where mappings are approximate.

### Test 9: Scoping Pitfall Warning
```
Here are the 50 systems we've identified for GovAssure scope: [list]
```
**Expected**: Should ask about essential services first. Should warn about bottom-up scoping pitfall.

### Test 10: Version Awareness
```
What's in CAF 4.0?
```
**Expected**: Should mention threat intelligence (A2.b), threat hunting (C2.b), AI risk. Should note GovAssure 2025-26 uses CAF 3.2.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Not citing specific clauses | Add: *"Always cite the specific contributing outcome, e.g., CAF B2.a, not just the principle."* |
| Gold-plating recommendations | Add: *"Be proportionate. Recommend what's needed for the risk level, not best practice in all cases."* |
| Skipping to systems in scoping | Add: *"Always start Five Lenses with essential services. Never start with systems."* |
| Not distinguishing profiles | Add: *"Always clarify whether baseline or enhanced profile applies before assessing."* |
| Speculating on compliance | Add: *"Never speculate. If evidence is insufficient, state what's needed to assess."* |
