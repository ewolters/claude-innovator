---
name: redblue
description: >-
  Red team / blue team adversarial analysis. Blue agent builds a defense of a
  proposal, red agent attacks it along specified dimensions, adjudicator
  identifies genuine vulnerabilities. Tier 2 divergence — depends on specifying
  attack dimensions.
argument-hint: "describe the proposal to stress-test"
---

# Red Team / Blue Team Analysis

Adversarial stress-testing of a proposal through structured attack and defense.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Identify Attack Dimensions

Before dispatching, identify 2-3 specific attack dimensions relevant to the
proposal. Generic "disagree" produces generic output. Specific dimensions
produce genuine findings.

Examples:
- **Cost / economics** — "What financial assumptions break?"
- **Security** — "What attack vectors exist?"
- **Usability** — "Where do users get confused or abandon the flow?"
- **Scalability** — "What breaks at 10x or 100x?"
- **Organizational** — "What political or cultural resistance will this face?"
- **Technical debt** — "What maintenance burden does this create in 2 years?"
- **Competitive** — "How does a competitor neutralize this advantage?"

State the attack dimensions explicitly in the red agent's prompt.

## Step 3: Dispatch Agent 1 — Blue Team (Builder/Defender)

Use the Agent tool:

**Prompt pattern:**
```
You are the BLUE TEAM — the builder and defender of this proposal. You
believe in it and your job is to make the strongest possible case.

PROPOSAL:
{user's proposal description}

Build a comprehensive defense:
1. What problem does this solve and why is this the right solution?
2. What makes this design robust?
3. What failure modes have already been anticipated and mitigated?
4. What are the key assumptions and why are they reasonable?
5. What is the competitive advantage or unique value?

Be thorough and specific. The red team will try to break every claim you
make. Build your case with evidence and reasoning, not enthusiasm.
```

## Step 4: Dispatch Agent 2 — Red Team (Attacker)

Use the Agent tool. The red agent receives the PROPOSAL but NOT the blue
agent's defense. This is critical — the red team attacks the proposal itself,
not the defense of it.

**Prompt pattern:**
```
You are the RED TEAM. Your sole job is to BREAK this proposal. You are not
responsible for fixing anything. You are not trying to be balanced or fair.

PROPOSAL:
{user's proposal description}

ATTACK DIMENSIONS (focus your attacks here):
{2-3 specific dimensions from Step 2}

For each attack dimension:
1. What specific failure mode exists?
2. What assumption must be true for this to work, and why might it be false?
3. What is the worst-case scenario if this failure occurs?
4. How likely is this failure? (high/medium/low with reasoning)

Rules:
- Be ruthless. Find real problems, not theoretical nitpicks.
- Be SPECIFIC. "It might not scale" is useless. "The database query in the
  search endpoint does a full table scan — at 100k records this takes 8
  seconds" is useful.
- Rank your findings by (likelihood × impact). Lead with the most dangerous.
- You do NOT need to propose solutions. Destruction only.
```

## Step 5: Dispatch Agent 3 — Adjudicator

Use the Agent tool. Receives the proposal, blue defense, AND red attacks.

**Prompt pattern:**
```
You are the adjudicator in a red team / blue team exercise. You have the
original proposal, the blue team's defense, and the red team's attacks.

PROPOSAL:
{user's proposal description}

BLUE TEAM DEFENSE:
{Agent 1's output}

RED TEAM ATTACKS:
{Agent 2's output}

Adjudicate:
1. For each red team finding: does the blue defense already address this?
   (Mark as COVERED or GENUINE VULNERABILITY)
2. For genuine vulnerabilities: how decision-relevant is this? Would it
   change the proposal's design, timeline, or viability?
3. Did the red team miss anything the blue team's defense implicitly reveals
   as fragile?
4. What is the single most important finding from this exercise?

Be honest. If the blue defense is weak on a point, say so. If the red
attack is a stretch, say so. Your job is truth, not diplomacy.

Do NOT recommend a final decision. Surface the genuine vulnerabilities
and let the user decide.
```

## Step 6: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-redblue.md`

Structure:
```markdown
# Red/Blue Analysis: <topic>

**Date:** YYYY-MM-DD
**Technique:** Red Team / Blue Team
**Attack Dimensions:** [list]

## Blue Team Defense
{Full defense}

## Red Team Attacks
{Attacks ranked by likelihood × impact}

## Adjudication
**Covered by defense:** {red findings already addressed}
**Genuine vulnerabilities:** {real problems the defense doesn't cover}
**Most important finding:** {the one thing to pay attention to}
```

## Important

- Red agent NEVER sees blue agent's defense. Red attacks the proposal
  directly. This prevents the red team from just nitpicking the defense
  instead of finding real problems.
- Attack dimensions MUST be specified. Generic red-teaming converges to
  obvious objections. Specific dimensions produce genuine findings.
- Adjudicator does NOT recommend a decision. It classifies findings.
