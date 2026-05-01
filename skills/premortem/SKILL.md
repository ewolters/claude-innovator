---
name: premortem
description: >-
  Prospective hindsight failure narrative. Parallel agents write post-mortems
  from different stakeholder perspectives, set one year in the future after the
  project has failed spectacularly. Generates 30% more failure modes than
  analytical risk listing. Tier 2 divergence.
argument-hint: "describe the plan or project to pre-mortem"
---

# Pre-Mortem Analysis

Generate failure narratives from multiple stakeholder perspectives to surface
risks that analytical listing misses.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Select Perspectives

Choose 3 stakeholder perspectives relevant to the problem. Each must have
DIFFERENT incentives and visibility. Examples:

- **The customer** — cares about whether it works, not how
- **The engineer** — cares about technical debt, maintenance burden, complexity
- **The investor/business owner** — cares about ROI, timeline, opportunity cost
- **The competitor** — sees the market response, not the internal struggle
- **The regulator** — sees compliance gaps, liability exposure
- **The new hire** — joins 6 months in, sees the mess with fresh eyes

Pick perspectives that will produce DIFFERENT failure stories, not three
versions of the same concern.

## Step 3: Dispatch Parallel Stakeholder Agents

Use the Agent tool to dispatch ALL stakeholder agents in a SINGLE message
(parallel execution). Each agent is fully isolated.

**Prompt pattern (one per perspective):**
```
You are {STAKEHOLDER ROLE}. It is {current date + 1 year}.

The project described below has FAILED. Not a quiet wind-down — a notable,
embarrassing failure that people in the industry talk about as a cautionary
tale.

PROJECT:
{user's plan/project description}

Write the post-mortem from YOUR perspective. Be specific:
1. What happened? Tell the story chronologically.
2. When did the first warning signs appear? What were they?
3. Who ignored them and why?
4. What was the root cause? (Not "poor planning" — the specific decision
   or assumption that broke.)
5. What was the human cost? (Careers damaged, relationships strained,
   trust lost — not just financial.)
6. What would you have done differently if you could go back?

Write as a NARRATIVE, not a bullet list. The story format is the point —
it activates different reasoning than analytical risk assessment.

Commit fully to the failure. Do not hedge with "but it could have succeeded
if..." The project failed. Tell us why.
```

## Step 4: Dispatch Pattern Extractor Agent

Use the Agent tool. Receives ALL failure narratives simultaneously.

**Prompt pattern:**
```
You are analyzing three independent failure narratives about the same project.
Each was written from a different stakeholder perspective. They did not
communicate with each other.

PROJECT:
{user's plan/project description}

FAILURE NARRATIVES:
{All stakeholder agents' outputs}

Extract:
1. **Common failure themes:** Causes that appear in 2+ narratives (these are
   the most likely risks — independent convergence suggests robustness).
2. **Unique risks:** Failure modes that only ONE perspective surfaced (these
   are the blind spots — the things the other stakeholders didn't see).
3. **Earliest warning signs:** Across all narratives, what is the FIRST thing
   that goes wrong? This is the canary in the coal mine.
4. **Root cause consensus:** Do the narratives agree on root cause, or do they
   blame different things? Disagreement about root cause is itself a finding.
5. **Assumptions challenged:** What assumptions in the original plan do these
   narratives call into question?

Do NOT recommend fixes. Surface the failure patterns. The user decides what
to do about them.
```

## Step 5: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-premortem.md`

Structure:
```markdown
# Pre-Mortem: <topic>

**Date:** YYYY-MM-DD
**Technique:** Prospective Hindsight
**Perspectives:** [list of stakeholder roles]

## Failure Narratives

### [Stakeholder 1] Perspective
{Full narrative}

### [Stakeholder 2] Perspective
{Full narrative}

### [Stakeholder 3] Perspective
{Full narrative}

## Pattern Analysis
**Common themes:** {risks that appeared in multiple narratives}
**Unique risks:** {blind spots only one perspective caught}
**Earliest warning signs:** {the canary}
**Root cause consensus/divergence:** {do they agree?}
**Assumptions challenged:** {what the original plan took for granted}
```

## Important

- Stakeholder agents run in PARALLEL. They never see each other's narratives.
- The NARRATIVE format is essential. "List risks" produces different (worse)
  output than "tell the story of failure." Do not convert to bullet lists.
- Pattern extractor does NOT recommend fixes. It surfaces patterns.
