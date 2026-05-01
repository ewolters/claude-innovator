# Innovation Technique Vocabulary

Reference data for all techniques. Skills read this file for dispatch patterns,
persona templates, and problem shape matching.

Innovation is inherently expensive. These techniques are deliberate inefficiency —
exploring paths that mostly won't pay off, because the one that does is worth more
than all the efficient convergent thinking would have produced.

## Architectural Rules

1. **Isolation during generation.** Divergent agents NEVER see each other's output.
   They get the problem statement and their own system prompt. An agent that has seen
   another agent's output WILL converge toward it.

2. **Persona specificity over generic roles.** Detailed, opinionated personas with
   constraints, values, and cognitive style — not "you are an engineer."

3. **Convergence is separate.** The synthesis agent sees all outputs but does NOT
   pick a winner. It extracts patterns, identifies tensions, surfaces surprises.
   The user or brainstorming picks the winner.

## Output Convention

All techniques write artifacts to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-<technique>.md`

Artifacts are working documents, not chat transcripts. They must stand alone as
readable, reusable reference material that brainstorming or other workflows can
consume as context.

---

## Techniques

### triz
- **Full name:** TRIZ Contradiction Analysis + Inventive Principles
- **Origin:** Genrich Altshuller, Soviet patent analysis (1946-1985)
- **Divergence tier:** 1 (genuinely different reasoning paths)
- **Problem shape:** Contains a contradiction — improving parameter A degrades parameter B. Two requirements seem mutually exclusive.
- **Dispatch:** 3 sequential agents (contradiction framer → principle applicator → solution evaluator)
- **Agent count:** 3
- **Cost:** Moderate (3 sequential calls)

### morph
- **Full name:** Morphological Analysis (Zwicky Box)
- **Origin:** Fritz Zwicky, astrophysics/engineering (1940s)
- **Divergence tier:** 1 (genuinely different — combinatorial enumeration is mechanical, not model-directed)
- **Problem shape:** Can be decomposed into independent dimensions. Want to explore the full combinatorial space including non-obvious pairings.
- **Dispatch:** 2 agents + code enumeration + 1 evaluator
- **Agent count:** 3 + code step
- **Cost:** Moderate (3 calls + code)

### transfer
- **Full name:** Cross-Domain Analogical Transfer
- **Origin:** Cognitive science, Gick & Holyoak (1980)
- **Divergence tier:** 1 (domain-specific prompts change what patterns are salient)
- **Problem shape:** Feels domain-locked. Solutions keep looking the same. Need fresh framing from distant fields.
- **Dispatch:** N parallel domain agents → 1 synthesis agent
- **Agent count:** 4-6 (default 4 domains + 1 synthesis)
- **Cost:** High (5 calls, 4 parallel)

### wargame
- **Full name:** Multi-Turn Role-Committed Simulation
- **Origin:** Military planning, cybersecurity
- **Divergence tier:** 1 (emergent multi-agent dynamics with hidden information)
- **Problem shape:** Need to stress-test a strategy, explore competitive dynamics, or surface emergent problems that static analysis misses.
- **Dispatch:** N agents with persistent roles, turn-based with hidden info, narrator
- **Agent count:** 3-5 roles × 3-5 turns + narrator
- **Cost:** Very high (12-26 calls)

### premortem
- **Full name:** Prospective Hindsight Failure Narrative
- **Origin:** Gary Klein (1998), naturalistic decision making
- **Divergence tier:** 2 (narrative failure frame differs from analytical risk)
- **Problem shape:** Before committing to a plan. Need failure modes that analytical risk listing misses.
- **Dispatch:** N parallel stakeholder agents → 1 pattern extractor
- **Agent count:** 3-4 (default 3 perspectives + 1 extractor)
- **Cost:** Moderate (4 calls, 3 parallel)

### redblue
- **Full name:** Red Team / Blue Team Adversarial Analysis
- **Origin:** Military intelligence, cybersecurity
- **Divergence tier:** 2 (depends on specifying attack dimensions)
- **Problem shape:** Have a proposal and need it stress-tested. Need to find vulnerabilities, not just risks.
- **Dispatch:** 2 sequential (blue builds → red attacks) → 1 adjudicator
- **Agent count:** 3
- **Cost:** Moderate (3 sequential calls)

### conference
- **Full name:** S1/S2 Dialectical Debate
- **Origin:** Hegelian dialectic, field-tested in Object 271 (Kjerne project)
- **Divergence tier:** 2 (structured disagreement with committed positions)
- **Problem shape:** Design decision with real tradeoffs. Need structured disagreement to surface tensions before deciding.
- **Dispatch:** 2 parallel (S1 innovator + S2 conservative) → conference document
- **Agent count:** 3 (2 parallel + 1 synthesizer)
- **Cost:** Moderate (3 calls, 2 parallel)

### invert (v0.2)
- **Full name:** Inversion — "Make It Terrible" Then Flip
- **Divergence tier:** 2
- **Problem shape:** Stuck in positive framing. Need failure modes and unstated requirements.
- **Dispatch:** 2 sequential (failure designer → inverter)

### constrain (v0.2)
- **Full name:** Constraint Removal
- **Divergence tier:** 1
- **Problem shape:** Feel boxed in by assumptions. Need to test which constraints are real vs inherited.
- **Dispatch:** 2 agents (assumption lister → per-constraint solver → insight extractor)

### synectics (v0.2)
- **Full name:** Synectics — Personal/Symbolic Analogy
- **Divergence tier:** 2
- **Problem shape:** Need radical reframing. Want to "be the thing" and see from inside.
- **Dispatch:** 4 parallel (direct/personal/symbolic/fantasy) → 1 mapper

### brainwrite (v0.2)
- **Full name:** 6-3-5 Brainwriting
- **Divergence tier:** 2
- **Problem shape:** Need volume of ideas that build on each other without groupthink.
- **Dispatch:** 3-4 agents in sequence, each building on prior output
