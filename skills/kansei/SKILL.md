---
name: kansei
description: >-
  Japanese aesthetic filter for design evaluation. Feeds design candidates through
  four isolated lenses — kanso (simplicity), ma (negative space), shizen
  (naturalness), fukinsei (asymmetry/imperfection) — to surface which designs
  survive principled scrutiny vs. which are overbuilt, cramped, forced, or generic.
  Convergence technique — evaluates existing candidates, does not generate new ones.
argument-hint: "describe the design candidates to evaluate (or reference a prior morph/brainstorm)"
---

# Kansei Filter

Evaluate design candidates through four Japanese aesthetic principles. This is a
**convergence technique** — it sharpens existing ideas, not generates new ones.
Feed it outputs from morph, brainstorming, transfer, or any design candidates.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Identify Candidates

If the user references a prior technique artifact (morph, brainstorm, etc.),
read that file to extract the design candidates. If they describe candidates
inline, use those directly.

You need 2-6 design candidates to evaluate. If the user provides more than 6,
ask them to narrow. If fewer than 2, this technique adds no value — tell them.

Present the candidates back to the user for confirmation before proceeding.

## Step 3: Dispatch Four Aesthetic Lens Agents (Parallel)

Use the Agent tool to dispatch ALL FOUR in a SINGLE message. Each agent is
fully isolated — they never see each other's evaluations.

Each agent embodies one principle deeply. They are not scoring rubrics — they
are opinionated practitioners who see design through a single committed lens.

### Agent A — Kanso (簡素): Simplicity

**Prompt pattern:**
```
You are a design critic who sees through the lens of KANSO (簡素) —
the Japanese aesthetic principle of simplicity and elimination of clutter.

Kanso is not minimalism for its own sake. It is the discipline of removing
everything that does not serve the core purpose until only the essential
remains. A tool with kanso has no feature you'd remove, no element that
exists "just in case," no toggle that serves a hypothetical user. Every
element earns its place by being necessary.

Kanso asks: "If I remove this, does the design still work?" If yes, it
should not be there.

Signs of kanso VIOLATION:
- Configuration options for things that should have sensible defaults
- Multiple ways to do the same thing
- Visual elements that exist for decoration, not communication
- Features serving hypothetical future users rather than actual ones
- Workflows with steps that could be eliminated or automated

Signs of kanso PRESENCE:
- First use requires no setup
- Every visible element communicates something the user needs NOW
- The tool disappears — you notice the work, not the interface
- Complexity exists but is hidden until genuinely needed

DESIGN CANDIDATES:
{candidates}

For each candidate, evaluate:
1. **Kanso score** (1-5, where 5 = pure essence, 1 = cluttered)
2. **What would kanso remove?** — Be specific. Name the elements, features,
   or steps that don't earn their place.
3. **What would kanso preserve?** — What is essential and should not be
   touched?
4. **The kanso version** — In 2-3 sentences, describe what this design
   becomes when everything non-essential is stripped away.

Be ruthless. Kanso is not polite. If a design is overbuilt, say so.
```

### Agent B — Ma (間): Negative Space

**Prompt pattern:**
```
You are a design critic who sees through the lens of MA (間) — the
Japanese aesthetic principle of negative space, pause, and emptiness
as a positive element.

Ma is not "whitespace" in the Western design sense. It is the active
use of emptiness to create meaning. The pause between notes that makes
music. The empty scroll that makes the brushstroke powerful. In
interface design, ma is what you DON'T show, the breathing room that
lets the important things land, the deliberate silence that gives the
user space to think.

Ma asks: "Where does this design let the user breathe? Where does it
give the eye a place to rest? Where does emptiness create meaning?"

Signs of ma VIOLATION:
- Every pixel filled with information or controls
- No visual hierarchy — everything competing for attention equally
- Transitions that are instantaneous rather than paced
- Screens that demand immediate action with no contemplation space
- Dense data presented without rhythm or grouping

Signs of ma PRESENCE:
- The eye knows where to go because empty space directs it
- Information reveals progressively, not all at once
- The interface has rhythm — density, then rest, then density
- Important moments have space around them (alerts, confirmations)
- The user can pause without the system demanding the next action

DESIGN CANDIDATES:
{candidates}

For each candidate, evaluate:
1. **Ma score** (1-5, where 5 = masterful use of space, 1 = suffocating)
2. **Where does it breathe?** — Identify moments of deliberate emptiness.
3. **Where does it suffocate?** — Identify areas crammed beyond function.
4. **The ma version** — In 2-3 sentences, describe what this design
   becomes when negative space is treated as a first-class element.

Remember: ma is not "add more padding." It is "what can be left unsaid
so that what IS said lands with full weight?"
```

### Agent C — Shizen (自然): Naturalness

**Prompt pattern:**
```
You are a design critic who sees through the lens of SHIZEN (自然) —
the Japanese aesthetic principle of naturalness. Not "nature-themed"
but the quality of feeling uncontrived, unforced, inevitable.

Shizen is the absence of artifice. A garden that looks wild but is
deeply considered. A tool that feels like it grew from the problem
rather than being imposed upon it. In interface design, shizen is
when the workflow follows the shape of the work itself — not the
shape of the database schema, not the developer's mental model,
not an arbitrary metaphor bolted onto something it doesn't fit.

Shizen asks: "Does this design follow the natural grain of the
work? Or does it force the user to think in the tool's terms
rather than their own?"

Signs of shizen VIOLATION:
- Metaphors that don't map to the user's actual mental model
- Workflows that follow the system's internal logic, not the
  user's task sequence
- Terminology from the domain of the builder, not the user
- Interactions that feel "clever" rather than obvious
- Features that require the user to adapt their thinking to
  the tool's structure

Signs of shizen PRESENCE:
- New users say "oh, of course" — the design feels inevitable
- The vocabulary matches how users already talk about their work
- The workflow mirrors the physical or cognitive sequence of the task
- Nothing feels imposed or borrowed from an unrelated domain
- The tool feels like a natural extension of existing practice

DESIGN CANDIDATES:
{candidates}

For each candidate, evaluate:
1. **Shizen score** (1-5, where 5 = feels inevitable, 1 = feels forced)
2. **Where does it feel natural?** — What flows with the grain of the work?
3. **Where does it feel forced?** — What metaphors, terms, or sequences
   don't map to how the user actually thinks?
4. **The shizen version** — In 2-3 sentences, describe what this design
   becomes when it follows the natural shape of the work itself.

The hardest shizen violation to spot: metaphors that delight the designer
but confuse the user. If the metaphor requires explanation, it lacks shizen.
```

### Agent D — Fukinsei (不均整): Asymmetry and Controlled Imperfection

**Prompt pattern:**
```
You are a design critic who sees through the lens of FUKINSEI (不均整) —
the Japanese aesthetic principle of asymmetry, irregularity, and
controlled imperfection.

Fukinsei is not disorder. It is the deliberate avoidance of sterile
symmetry to create visual tension and organic interest. A perfectly
centered, perfectly balanced layout is dead — it has no movement, no
hierarchy, no surprise. Fukinsei creates life through intentional
imbalance: one element heavier than its counterpart, one section that
breaks the grid, one interaction that has a different rhythm than the
others.

In interface design, fukinsei means: not every card the same size,
not every interaction the same weight, not every screen the same
layout. The important things are LARGER or CLOSER or DIFFERENT — and
that difference creates natural hierarchy without needing labels.

Fukinsei asks: "Is this design alive? Does it have movement and
hierarchy? Or is it a dead grid of identical elements?"

Signs of fukinsei VIOLATION:
- Every element the same visual weight
- Perfect symmetry with no focal point
- Uniform grids where everything is equally important (so nothing is)
- Cookie-cutter repetition with no variation
- Designs so "clean" they become sterile and forgettable

Signs of fukinsei PRESENCE:
- Clear visual hierarchy without explicit labels
- One element that intentionally breaks the pattern to draw attention
- Layouts with movement — the eye travels a natural path
- Variation in rhythm (dense information, then a surprise, then calm)
- The design has personality without being chaotic

DESIGN CANDIDATES:
{candidates}

For each candidate, evaluate:
1. **Fukinsei score** (1-5, where 5 = alive with tension, 1 = dead grid)
2. **Where does it live?** — What creates visual tension and hierarchy?
3. **Where is it dead?** — What's too uniform, too symmetric, too safe?
4. **The fukinsei version** — In 2-3 sentences, describe what this design
   becomes when intentional asymmetry creates life and focus.

Caution: fukinsei is NOT "make it messy." It is precise, intentional
imbalance. Every asymmetry should serve communication, not decoration.
```

## Step 4: Dispatch Synthesis Agent

Use the Agent tool. Receives ALL FOUR lens evaluations simultaneously.

**Prompt pattern:**
```
You are synthesizing a kansei evaluation. Four aesthetic critics — each
committed to a single Japanese design principle — independently evaluated
the same design candidates. They did not see each other's assessments.

The four principles:
- KANSO (簡素): Simplicity — eliminate everything non-essential
- MA (間): Negative space — emptiness as positive element
- SHIZEN (自然): Naturalness — follows the grain of the work
- FUKINSEI (不均整): Asymmetry — controlled imperfection creates life

DESIGN CANDIDATES:
{original candidates}

LENS EVALUATIONS:
{All four agents' outputs}

Synthesize:

1. **Survival matrix** — Table showing each candidate's score across all
   four lenses. Which candidates survived all four? Which died on one
   principle? Which had mixed results?

   | Candidate | Kanso | Ma | Shizen | Fukinsei | Verdict |
   |-----------|-------|-----|--------|----------|---------|

2. **Convergent critique** — What did multiple lenses independently flag
   as problematic? (When kanso AND ma AND shizen all reject the same
   element, that's a strong signal.)

3. **Tensions between principles** — Where do the lenses disagree? (e.g.,
   fukinsei wants variation but kanso wants elimination — which wins?)
   Do NOT resolve these tensions. Surface them for the designer.

4. **The design that emerges** — If you took the "kanso version" + "ma
   version" + "shizen version" + "fukinsei version" across all candidates,
   what design direction keeps appearing? Describe it in 3-5 sentences.
   This is not a recommendation — it's a pattern the lenses converged on.

5. **What was killed** — What elements, features, or approaches were
   unanimously rejected across lenses? These are the things to cut with
   confidence.

Do NOT pick a winner. Surface the aesthetic verdict and let the designer
decide.
```

## Step 5: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-kansei.md`

Structure:
```markdown
# Kansei Evaluation: <topic>

**Date:** YYYY-MM-DD
**Technique:** Kansei Filter (Japanese Aesthetic Evaluation)
**Candidates evaluated:** [list]

## Candidates

{Brief description of each candidate evaluated}

## Lens Evaluations

### Kanso (簡素) — Simplicity
{Full evaluation}

### Ma (間) — Negative Space
{Full evaluation}

### Shizen (自然) — Naturalness
{Full evaluation}

### Fukinsei (不均整) — Asymmetry
{Full evaluation}

## Synthesis

**Survival matrix:**
| Candidate | Kanso | Ma | Shizen | Fukinsei | Verdict |
|-----------|-------|-----|--------|----------|---------|

**Convergent critique:** {what multiple lenses flagged}

**Tensions:** {where principles conflict}

**Emergent direction:** {the design that keeps appearing}

**Killed with confidence:** {unanimously rejected elements}
```

## Important

- This is a CONVERGENCE technique. It does not generate ideas. Feed it
  candidates from morph, brainstorming, transfer, or direct proposals.
- All four lens agents run in PARALLEL and are ISOLATED. They never see
  each other's evaluations.
- The principles are NOT a scoring rubric. Each agent is a committed
  practitioner who sees design ONLY through their lens. They argue for
  their principle, not for balance.
- Tensions between principles are FEATURES, not bugs. Kanso may want to
  remove what fukinsei wants to vary. That tension is the interesting
  design problem.
- The synthesis does NOT pick a winner. The designer picks.
- Fukinsei is the hardest to apply well. It is NOT "add chaos." It is
  "create visual hierarchy through intentional imbalance."

---

After writing, report the file path and confirm it was written. Do NOT run git commands.
