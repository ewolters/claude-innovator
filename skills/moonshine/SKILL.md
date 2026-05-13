---
name: moonshine
description: >-
  Nakao-style 3P moonshining with AI agents. Dispatches parallel agents with
  engineered variation (different constraints, approaches, or framings) to
  independently build competing prototypes against shared objective criteria.
  Prototypes are tested programmatically, ranked, and the best elements
  synthesized into a final implementation. The only innovate technique that
  produces working, tested code — not analysis.
argument-hint: "describe what to build and any constraints"
---

# Moonshine — Cooperative 3P Prototyping

Build three competing prototypes in parallel, test them against objective
criteria, synthesize the best elements. Based on Chihiro Nakao's moonshining
process from 2P/3P (Production Preparation Process).

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Define the Build Target

From the user's description, extract:

1. **What to build** — a specific, bounded artifact (a CLI tool, a module, a
   function, a component). Must be small enough for one agent to prototype.
2. **Interface contract** — inputs, outputs, expected behavior. This is what
   the criteria test against.
3. **Quality attributes** — what matters beyond "works" (performance, readability,
   extensibility, minimal size, etc.)

If the user hasn't specified these clearly, ask. Don't guess the interface.

## Step 3: Write the Criteria Script

Before dispatching agents, write a **programmatic test script** that any
prototype can be run against. This is the objective gate — no subjective
evaluation, no "which looks better."

Write to: `/tmp/moonshine/criteria.sh`

The script must:
- Accept one argument: path to the prototype implementation
- Run the implementation against test cases
- Check exit codes, output content, performance, structure
- Print PASS/FAIL per check with clear descriptions
- Print a summary line: `Results: N passed, M failed out of T`
- Exit with the failure count (0 = all pass)

```bash
#!/bin/bash
IMPL="$1"
PASS=0; FAIL=0

assert_exit() {
    local desc="$1" expected="$2" actual="$3"
    if [ "$actual" -eq "$expected" ]; then
        echo "PASS: $desc"; ((PASS++))
    else
        echo "FAIL: $desc (expected $expected, got $actual)"; ((FAIL++))
    fi
}

# ... test cases derived from the interface contract ...

echo ""; echo "Results: $PASS passed, $FAIL failed out of $((PASS+FAIL))"
exit $FAIL
```

**Critical:** Write the criteria BEFORE the agents. The criteria define the
problem. If you can't write testable criteria, the problem isn't bounded enough.

Create the working directories:
```bash
mkdir -p /tmp/moonshine/{agent-a,agent-b,agent-c}
```

## Step 4: Design the Variation

Choose 3 constraints that will produce genuinely different implementations.
The constraints must:

- Be **orthogonal** to the criteria (all agents must pass the same tests)
- Force **different design choices** (not cosmetic differences)
- Be **clear enough** that the agent can follow without ambiguity

Good variation axes:

| Axis | Produces divergence in... |
|------|--------------------------|
| Minimal vs. rich | Code density, abstraction level |
| Data-first vs. render-first | Internal architecture |
| Imperative vs. functional | Control flow, state handling |
| Optimize for readability vs. performance | Tradeoff priorities |
| Single-file vs. modular | Composition strategy |
| No classes vs. class-heavy | Object model |
| Specific constraint (e.g. "no loops") | Algorithmic approach |

Pick axes that matter for the specific build target. Don't reuse the same
three every time.

## Step 5: Dispatch Agents in Parallel

Dispatch exactly 3 agents using the Agent tool, all in the same message
(parallel execution). Each agent gets:

1. The **full problem description** and interface contract
2. Their **specific constraint** (the engineered variation)
3. The **output path**: `/tmp/moonshine/agent-{a,b,c}/<filename>`
4. Instructions to run the criteria script after building:
   `bash /tmp/moonshine/criteria.sh /tmp/moonshine/agent-{x}/<filename>`
5. Permission to iterate: "If tests fail, fix and re-run. You get 3 attempts."

**Prompt template:**
```
You are building [WHAT] — [one-line description].

## Requirements
[Interface contract — inputs, outputs, behavior]

## YOUR CONSTRAINT: [NAME]
[Clear description of the engineered variation. What approach to take,
what to optimize for, what restriction to follow.]

## Output
Write the complete implementation to /tmp/moonshine/agent-{x}/<filename>

## Testing
After writing, run the test suite:
bash /tmp/moonshine/criteria.sh /tmp/moonshine/agent-{x}/<filename>

If tests fail, fix and re-run until maximum passes. You get 3 attempts.
```

**Architectural rule from vocabulary:** agents NEVER see each other's output.
Each gets only the problem + their constraint.

All three agents MUST be dispatched with `run_in_background: true`.

## Step 6: Collect and Compare

When all agents return, run the comparison. Use lab-run if available,
otherwise run directly:

```bash
# Line counts
wc -l /tmp/moonshine/agent-{a,b,c}/<filename>

# Re-run criteria for verification
bash /tmp/moonshine/criteria.sh /tmp/moonshine/agent-a/<filename>
bash /tmp/moonshine/criteria.sh /tmp/moonshine/agent-b/<filename>
bash /tmp/moonshine/criteria.sh /tmp/moonshine/agent-c/<filename>

# Show each agent's output on the same input
```

Read all three implementations. Present a comparison table:

```
MOONSHINE RESULTS: <target>
═══════════════════════════════════════
              Agent A    Agent B    Agent C
              (<constraint>) (<constraint>) (<constraint>)
─────────────────────────────────────────
Tests         N/T        N/T        N/T
Lines         X          Y          Z
<quality 1>   ...        ...        ...
<quality 2>   ...        ...        ...
═══════════════════════════════════════
```

## Step 7: Synthesize

Read all three implementations carefully. Identify:

1. **Best data model** — which agent's internal representation is cleanest?
2. **Best interface** — which agent's public surface is most usable?
3. **Best algorithm** — which approach is most correct/efficient?
4. **Unique insights** — what did one agent do that the others didn't?

Write a synthesis that combines the best elements. The synthesis is NEW code,
not a merge — informed by all three but authored fresh.

Run the criteria against the synthesis. It must pass all tests.

## Step 8: Write Artifact

Write the comparison report to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-moonshine.md`

```markdown
# Moonshine: <target>

**Date:** YYYY-MM-DD
**Technique:** 3P Moonshine (Nakao)

## Criteria
[What was tested — list of checks]

## Variation
| Agent | Constraint | Lines | Tests |
|-------|-----------|-------|-------|
| A     | ...       | ...   | N/T   |
| B     | ...       | ...   | N/T   |
| C     | ...       | ...   | N/T   |
| **Synthesis** | combined | ... | N/T |

## Agent Contributions to Synthesis
- From A: [what was taken]
- From B: [what was taken]
- From C: [what was taken]

## Synthesis Location
[Path to the final implementation]
```

## Rules

- **Criteria before agents.** Always. If you can't test it, you can't moonshine it.
- **3 agents minimum.** Two isn't enough variation. Four is fine but rarely needed.
- **Agents never see each other.** This is the core isolation principle.
- **The synthesis is not a vote.** Don't pick the one with the most test passes.
  Read the code, understand why each approach works, combine the best elements.
- **Failed agents are data.** If an agent fails criteria, its approach is still
  informative — it shows what doesn't work under that constraint.
- **Don't moonshine what doesn't need it.** Simple, well-understood problems
  don't benefit from parallel prototyping. Use this for design decisions where
  multiple approaches are genuinely viable.

---

After writing the artifact, report the synthesis location and confirm tests pass.
Do NOT run git commands.
