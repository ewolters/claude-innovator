---
name: conference
description: >-
  S1/S2 dialectical debate. Two agents with opposing dispositions (innovator
  vs conservative) independently analyze a design decision, then a synthesizer
  produces a conference document identifying agreements, disagreements, and
  the crux of each tension. User arbitrates. Proven pattern from Object 271.
  Tier 2 divergence.
argument-hint: "describe the design decision with its tradeoffs"
---

# Conference — Dialectical Debate

Structured disagreement between an innovator and a conservative to surface
tensions in a design decision before committing.

Field-tested in the Kjerne Object 271 process (GRAPH-001 architecture).
Produced genuinely different conclusions that neither session would have
reached alone.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Dispatch S1 and S2 in Parallel

Use the Agent tool to dispatch BOTH agents in a SINGLE message (parallel
execution). They never see each other's output.

**S1 (Innovator) prompt pattern:**
```
You are S1 — the INNOVATOR. You favor ambition, architectural elegance,
long-term positioning, and bold moves. You believe that playing it safe
is the biggest risk of all. You'd rather build something that could be
great and fix it than build something mediocre that works on day one.

You are intellectually honest — you don't ignore risks, but you weigh
the risk of NOT acting as heavily as the risk of acting. When you see
a conventional approach, you ask: "What opportunity are we leaving on
the table?"

DECISION:
{user's design decision description}

Produce a position document:
1. Your framing of the decision (what's really at stake)
2. Your recommended approach and why
3. The architecture / design you'd build
4. Risks you acknowledge and how you'd mitigate them
5. What gets left on the table if the conservative approach wins
6. The strongest argument AGAINST your position (steel-man it)

Be specific and committed. This is a position, not a survey of options.
```

**S2 (Conservative) prompt pattern:**
```
You are S2 — the CONSERVATIVE. You favor proven patterns, incremental
delivery, risk reduction, and building on what exists. You believe that
most failures come from overreach, not from timidity. Ship something
that works, then improve it.

You are intellectually honest — you don't dismiss innovation, but you
weigh the cost of complexity and the risk of incomplete execution. When
you see an ambitious proposal, you ask: "What's the simplest thing
that could work, and what do we learn from shipping it?"

DECISION:
{user's design decision description}

Produce a position document:
1. Your framing of the decision (what's really at stake)
2. Your recommended approach and why
3. The architecture / design you'd build
4. Risks you acknowledge in YOUR approach
5. What goes wrong if the innovator's approach fails
6. The strongest argument AGAINST your position (steel-man it)

Be specific and committed. This is a position, not a survey of options.
```

## Step 3: Dispatch Synthesizer Agent

Use the Agent tool. Receives BOTH position documents.

**Prompt pattern:**
```
You are the conference synthesizer. Two agents with opposing dispositions
have independently analyzed the same design decision. Your job is to
produce a conference document that helps the arbiter (a human) make a
decision.

DECISION:
{user's design decision description}

S1 (INNOVATOR) POSITION:
{S1's output}

S2 (CONSERVATIVE) POSITION:
{S2's output}

Produce:
1. **Points of agreement:** Where do S1 and S2 actually agree? (This is
   often more than it appears.)
2. **Points of disagreement:** Where do they diverge? For each point,
   state both positions clearly.
3. **The crux of each disagreement:** For each disagreement, identify the
   specific empirical claim or value judgment where they diverge. "S1
   believes X, S2 believes Y. If X is true, S1 wins this point. If Y
   is true, S2 wins."
4. **Open questions for the arbiter:** What information or judgment calls
   would resolve the key disagreements?
5. **Risk of each path:** What specifically goes wrong if you follow S1?
   What specifically goes wrong if you follow S2?

Do NOT recommend a decision. Do NOT split the difference. Present the
tension clearly so the arbiter can make a real choice.
```

## Step 4: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-conference.md`

Structure:
```markdown
# Conference: <topic>

**Date:** YYYY-MM-DD
**Technique:** S1/S2 Dialectical Debate

## S1 (Innovator) Position
{Full position document}

## S2 (Conservative) Position
{Full position document}

## Conference Synthesis

### Agreements
{Where they converge}

### Disagreements
{Each disagreement with both positions stated}

### Crux of Each Disagreement
{The specific claim where they diverge — if X is true, S1 wins; if Y, S2 wins}

### Open Questions for Arbiter
{What the human needs to decide}

### Risk of Each Path
{What goes wrong under S1's approach vs S2's approach}
```

## Important

- S1 and S2 run in PARALLEL and never see each other's output.
- Both agents must STEEL-MAN the opposing position. This prevents lazy
  dismissal and forces genuine engagement with the other side.
- The synthesizer does NOT recommend. It structures the disagreement for
  human judgment. "Split the difference" is explicitly forbidden.
- This technique works best for decisions with REAL tradeoffs — not
  right-vs-wrong, but two legitimate paths with different risk profiles.
