---
name: morph
description: >-
  Morphological analysis (Zwicky box). Decomposes a problem into independent
  dimensions, generates options per dimension, then mechanically enumerates
  combinations — including non-obvious pairings no one would reach by holistic
  thinking. Tier 1 divergence.
argument-hint: "describe the design problem to decompose"
---

# Morphological Analysis

Systematically explore the combinatorial design space by decomposing the problem
into orthogonal dimensions and enumerating cross-product combinations.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Dispatch Agent 1 — Dimension Decomposer

Use the Agent tool:

**Prompt pattern:**
```
You are decomposing a design problem into independent dimensions for
morphological analysis.

PROBLEM:
{user's problem description}

Identify 4-6 ORTHOGONAL dimensions of this problem. Each dimension must be:
- Genuinely independent (changing one doesn't force a change in another)
- Meaningful (not trivial — each dimension represents a real design choice)
- At the same level of abstraction

For each dimension, write:
- Dimension name
- What design choice it represents
- Why it's independent of the other dimensions

Bad example: "color" and "visual style" (not independent)
Good example: "data storage mechanism" and "user interaction model" (independent)
```

## Step 3: Dispatch Agent 2 — Option Generator

Use the Agent tool. Receives the problem AND Agent 1's dimensions. **Critical:
this agent must NOT think about compatibility between dimensions.**

**Prompt pattern:**
```
You are generating options for each dimension of a morphological box.

PROBLEM:
{user's problem description}

DIMENSIONS:
{Agent 1's output}

For EACH dimension, generate 3-5 options. Rules:
- Do NOT think about compatibility between dimensions. Each dimension is
  independent. An option for dimension A should NOT be chosen because it
  "works well with" an option in dimension B.
- Include at least ONE option per dimension that seems impractical or unusual.
  The whole point of morphological analysis is finding non-obvious pairings.
- Label clearly: Dimension → Option 1, Option 2, etc.
- Keep each option to one sentence. No evaluation.
```

## Step 4: Combinatorial Enumeration (Code Step)

Use Bash to run a Python script that generates random combinations from the
morphological box. This step is CODE, not an agent — mechanical enumeration
is the point.

```python
import random
import json

# Parse the dimensions and options from Agent 2's output
# (Claude will need to extract these into a structured format)
dimensions = {
    "Dimension 1": ["Option A", "Option B", "Option C"],
    "Dimension 2": ["Option X", "Option Y", "Option Z"],
    # ... etc
}

# Generate all possible combinations count
total = 1
for opts in dimensions.values():
    total *= len(opts)

# Sample 8 random combinations (or all if fewer than 8 total)
n_samples = min(8, total)
combinations = []
seen = set()
while len(combinations) < n_samples:
    combo = {dim: random.choice(opts) for dim, opts in dimensions.items()}
    key = tuple(sorted(combo.items()))
    if key not in seen:
        seen.add(key)
        combinations.append(combo)

for i, combo in enumerate(combinations, 1):
    print(f"\n--- Combination {i} ---")
    for dim, opt in combo.items():
        print(f"  {dim}: {opt}")
```

## Step 5: Dispatch Agent 3 — Combination Evaluator

Use the Agent tool. Receives the problem, the morphological box, AND the
random combinations.

**Prompt pattern:**
```
You are evaluating randomly-generated combinations from a morphological box.
These combinations were NOT selected for quality — they are mechanical
cross-products. Your job is to find what's interesting.

PROBLEM:
{user's problem description}

MORPHOLOGICAL BOX:
{The full dimension × option table}

RANDOM COMBINATIONS:
{The 8 combinations from the code step}

For each combination:
1. Is it feasible as-is? (yes/no/with modification)
2. If not feasible, what ONE modification would make it viable?
3. What is INTERESTING about this pairing? What does it suggest that
   holistic thinking would miss?

After evaluating all combinations:
- Identify the 3 most promising (feasible or near-feasible with the most
  interesting properties)
- Note any DIMENSION INTERACTIONS you didn't expect — pairings that create
  emergent value or emergent problems

Do NOT pick a single winner. Surface what's interesting.
```

## Step 6: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-morph.md`

Structure:
```markdown
# Morphological Analysis: <topic>

**Date:** YYYY-MM-DD
**Technique:** Morphological Analysis (Zwicky Box)

## Morphological Box

| Dimension | Option 1 | Option 2 | Option 3 | Option 4 |
|-----------|----------|----------|----------|----------|
| ...       | ...      | ...      | ...      | ...      |

**Total design space:** N combinations

## Random Combinations Evaluated
{8 combinations with evaluator's notes}

## Most Promising
{Top 3 with rationale}

## Unexpected Interactions
{Dimension pairings that created emergent value or problems}
```

## Important

- The code enumeration step is MECHANICAL. Do not let an agent select
  "interesting" combinations — that defeats the purpose.
- Agent 2 must NOT consider cross-dimension compatibility. If they do,
  the morphological box collapses to conventional thinking.
- Agent 3 does NOT pick a winner.

---

After writing, report the file path and confirm it was written. Do NOT run git commands.
