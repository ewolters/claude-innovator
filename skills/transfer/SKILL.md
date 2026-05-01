---
name: transfer
description: >-
  Cross-domain analogical transfer. Dispatches parallel agents grounded in
  distant domains (logistics, game design, epidemiology, etc.) to solve the
  same problem from radically different frames. Tier 1 divergence — domain-specific
  prompts change what patterns are even salient.
argument-hint: "describe the problem (optionally: --domains 'domain1,domain2,domain3')"
---

# Cross-Domain Analogical Transfer

Solve the same problem from multiple distant domains simultaneously, then
extract transferable structural principles.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Select Domains

If the user provided `--domains`, use those. Otherwise, select 4 domains that
are DISTANT from the problem's native domain. The further the source domain,
the more abstract (and potentially general) the transferred principle.

Domain pool (pick domains distant from the problem):
- Logistics / supply chain
- Game design / player engagement
- Epidemiology / contagion dynamics
- Military strategy / force deployment
- Ecology / ecosystem dynamics
- Architecture / spatial design
- Finance / risk management
- Theater / performance / improvisation
- Materials science / metallurgy
- Cuisine / restaurant operations

Avoid domains CLOSE to the problem. If the problem is about software architecture,
don't pick "systems engineering" — pick "cuisine" or "epidemiology."

## Step 3: Dispatch N Parallel Domain Agents

Use the Agent tool to dispatch ALL domain agents in a SINGLE message (parallel
execution). Each agent is fully isolated — they never see each other's output.

**Prompt pattern (one per domain):**
```
You are a {DOMAIN} expert. You have spent your career in {DOMAIN} and think
in {DOMAIN} terms, frameworks, and patterns.

A colleague from another field has come to you with this problem:

PROBLEM:
{user's problem description}

Solve this problem AS A {DOMAIN} PROBLEM. Use {DOMAIN} vocabulary, patterns,
and frameworks. Do NOT reference the original problem's domain. Translate the
problem entirely into {DOMAIN} terms, then solve it within that frame.

Your solution should:
1. Reframe the problem in {DOMAIN} language
2. Identify the {DOMAIN} pattern or principle that applies
3. Describe the concrete solution in {DOMAIN} terms
4. Extract the abstract structural principle that makes this solution work
   (this is what transfers back to the original domain)

Be specific. Use real {DOMAIN} techniques, not hand-waving.
```

## Step 4: Dispatch Synthesis Agent

Use the Agent tool. This agent receives ALL domain solutions simultaneously.

**Prompt pattern:**
```
You are synthesizing solutions from multiple domains that were all applied to
the same underlying problem. Each expert solved the problem within their own
domain — they did not communicate with each other.

ORIGINAL PROBLEM:
{user's problem description}

DOMAIN SOLUTIONS:
{All domain agents' outputs}

Your job:
1. For each domain solution, extract the STRUCTURAL PRINCIPLE — the abstract
   pattern that is independent of the specific domain.
2. Identify which structural principles are NOVEL relative to the problem's
   native domain — things that practitioners in the original field would not
   typically consider.
3. Identify any structural principles that MULTIPLE independently-primed domains
   converged on — this suggests robustness, though convergence partly reflects
   shared model priors, not just deep structural truth. Flag it as signal worth
   noting, not proof.
4. Note any domain solutions that suggest a fundamentally different FRAMING
   of the problem itself (not just a different solution, but a different
   understanding of what the problem is).

Do NOT pick a winner. Do NOT rank by feasibility. Surface what's structurally
interesting and what transfers.
```

## Step 5: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-transfer.md`

Structure:
```markdown
# Cross-Domain Transfer: <topic>

**Date:** YYYY-MM-DD
**Technique:** Analogical Transfer
**Domains:** [list of domains used]

## Domain Solutions

### [Domain 1]
{Reframing + solution + structural principle}

### [Domain 2]
{Reframing + solution + structural principle}

... (one section per domain)

## Synthesis
**Novel principles:** {principles uncommon in the problem's native domain}
**Convergent principles:** {principles multiple domains arrived at independently}
**Reframing insights:** {any domain that reframed the problem itself}
```

## Important

- Domain agents run in PARALLEL in a single Agent tool message. They must
  never see each other's output.
- Domain selection matters more than domain count. Three truly distant domains
  beat six nearby ones.
- The synthesis agent extracts STRUCTURE, not recommendations. The user or
  brainstorming decides what to do with the principles.
