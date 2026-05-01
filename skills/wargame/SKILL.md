---
name: wargame
description: >-
  Multi-turn role-committed simulation. Assigns persistent roles (attacker,
  defender, market, regulator) and plays through a scenario turn by turn,
  each role making decisions from their incentive structure. Surfaces emergent
  dynamics that static analysis misses. Tier 1 divergence. High token cost.
argument-hint: "describe the scenario/strategy to stress-test"
---

# War Game Simulation

Multi-turn, multi-agent simulation with persistent roles and hidden information.

**Cost warning:** This is the most expensive technique. 3 roles × 4 turns =
12+ agent calls plus a narrator. Use when the problem genuinely requires
dynamic simulation, not just risk listing.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Design the Game

Based on the user's scenario, define:

1. **Roles** (3-4): Each needs a name, identity, incentive structure, information
   access level, and private strategy. Common patterns:
   - Competitor / Your company / Market / Regulator
   - Attacker / Defender / Civilian population / Media
   - Incumbent / Disruptor / Customer / Investor

2. **Turn count** (3-5): Fewer turns = cheaper, more focused. More turns =
   richer emergent dynamics. Default to 4.

3. **Public vs. private information**: Each role sees public actions from all
   other roles but NOT their internal reasoning or private strategy.

Present the game design to the user and get approval before dispatching.

## Step 3: Execute Turns

For each turn, dispatch each role agent SEQUENTIALLY (each role responds to
the previous roles' public actions in that turn).

**Role agent prompt pattern:**
```
You are {ROLE NAME}.

IDENTITY: {detailed identity — who you are, what you want, how you think}
INCENTIVES: {what you're optimizing for, what you'd sacrifice}
PRIVATE STRATEGY: {your approach — other roles cannot see this}

SCENARIO:
{original scenario description}

TURN {N} — SITUATION:
{public actions from all previous turns}

Based on the current situation, decide your action this turn.

Output:
- PUBLIC ACTION: {what you do — visible to all roles}
- PRIVATE REASONING: {why you did it — visible only in the final artifact,
  not to other role agents during the game}
```

**Critical isolation rule:** When constructing the prompt for role B in turn N,
include ONLY the PUBLIC ACTIONs from all roles in previous turns and from
roles that have already acted in turn N. NEVER include PRIVATE REASONING
from other roles.

## Step 4: Dispatch Narrator Agent

After all turns complete, dispatch a narrator agent that sees EVERYTHING —
all public actions AND all private reasoning from all roles across all turns.

**Prompt pattern:**
```
You are the narrator of a war game simulation. You have complete visibility
into all roles' public actions AND private reasoning across all turns.

SCENARIO:
{original scenario}

COMPLETE GAME LOG:
{all turns — public actions and private reasoning for all roles}

Write:
1. **Narrative summary:** Tell the story of what happened across all turns.
   What dynamics emerged? Where did the game surprise you?
2. **Emergent findings:** What dynamics arose from the interaction between
   roles that no single role anticipated?
3. **Vulnerabilities surfaced:** Where did the strategy under test fail or
   show stress? What assumptions broke?
4. **Information asymmetry insights:** Where did private strategies collide
   in ways the roles couldn't see?
5. **Key decision points:** Which turn/action was the inflection point?

Do NOT recommend a strategy. Surface what happened and what it reveals.
```

## Step 5: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-wargame.md`

Structure:
```markdown
# War Game: <topic>

**Date:** YYYY-MM-DD
**Technique:** Multi-Turn Simulation
**Roles:** [list]
**Turns:** N

## Game Design
{Roles, incentives, scenario}

## Turn-by-Turn Log

### Turn 1
**[Role A]:** {public action}
**[Role B]:** {public action}
...

### Turn 2
...

## Private Reasoning (Revealed)
{Each role's private reasoning per turn — the hidden information}

## Narrator Analysis
{Narrative summary, emergent findings, vulnerabilities, key decision points}
```

## Important

- This technique requires user approval of game design before dispatching.
  The roles and scenario shape everything — get them right.
- NEVER leak private reasoning between role agents during the game. This is
  the core mechanic. The hidden information is what produces emergence.
- Role agents are SEQUENTIAL within a turn (each sees prior roles' public
  actions that turn). Turns are sequential. There is no parallelism.
- The narrator sees everything. Other agents do not.
