# Claude Innovator

Structured creativity through isolated subagent dispatch. A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that generates genuine novelty by dispatching multiple AI agents with different perspectives, constraints, and domain expertise — then synthesizing what they find.

## Why

When a single AI generates "three options," they share the same framing, the same priors, the same blind spots. They're syntactically different but cognitively homogeneous.

Claude Innovator breaks this convergence by enforcing **isolation during generation** — each agent gets a different system prompt, a different domain vocabulary, a different set of constraints, and never sees another agent's output. Convergence happens separately, with structure, after the divergent thinking is done.

Innovation is inherently expensive. This plugin is deliberate inefficiency — exploring paths that mostly won't pay off, because the one that does is worth more than all the efficient convergent thinking would have produced.

## Techniques

### v0.1.0 (current)

| Skill | Technique | What It Does | Cost |
|-------|-----------|-------------|------|
| `/innovate:prompt` | Guided entry | Analyzes your problem and recommends which technique to use | Minimal |
| `/innovate:triz` | TRIZ Contradiction Analysis | Frames contradictions, applies 40 inventive principles from patent research | Moderate |
| `/innovate:morph` | Morphological Analysis | Decomposes into dimensions, mechanically enumerates combinations | Moderate |
| `/innovate:transfer` | Cross-Domain Transfer | Parallel agents solve your problem from distant domains (ecology, theater, etc.) | High |
| `/innovate:wargame` | War Game Simulation | Multi-turn role-committed simulation with hidden information | Very High |
| `/innovate:premortem` | Pre-Mortem Analysis | Parallel failure narratives from different stakeholder perspectives | Moderate |
| `/innovate:redblue` | Red/Blue Team | Adversarial stress-testing with specified attack dimensions | Moderate |
| `/innovate:conference` | Dialectical Debate | S1 (innovator) vs S2 (conservative) produce competing positions | Moderate |

### Planned (v0.2)

`/innovate:invert` (make it terrible, then flip), `/innovate:constrain` (systematically remove assumptions), `/innovate:synectics` (be the thing), `/innovate:brainwrite` (sequential build-on)

## Installation

```bash
claude plugin install innovate@eric-tools
```

Or clone and install locally:

```bash
git clone git@github.com:ewolters/claude-innovator.git
claude plugin install --local ./claude-innovator
```

## Usage

Start with `:prompt` to get a technique recommendation:

```
/innovate:prompt We need real-time SPC charts but the calculation is too expensive per data point
```

Then invoke the recommended technique directly:

```
/innovate:triz We need real-time SPC charts but the calculation is too expensive per data point
```

Every technique writes a structured markdown artifact to `docs/superpowers/innovate/` — a working document you can feed into brainstorming, planning, or any other workflow.

## How It Works

### Three Rules

1. **Isolation during generation.** Divergent agents never see each other's output. They get the problem statement and their own system prompt. An agent that has seen another agent's output will converge toward it.

2. **Persona specificity over generic roles.** Detailed, opinionated personas with constraints, values, and cognitive style — not "you are an engineer."

3. **Convergence is separate.** The synthesis agent sees all outputs but does not pick a winner. It extracts patterns, identifies tensions, surfaces surprises. You pick the winner.

### Divergence Tiers

Not all techniques produce equal cognitive divergence. We rated each honestly:

**Tier 1 — Genuinely different reasoning paths:** TRIZ (40 orthogonal operators), morphological analysis (mechanical enumeration), cross-domain transfer (domain-specific prompts change what's salient), war gaming (emergent multi-agent dynamics)

**Tier 2 — Meaningfully different with care:** Red/blue (depends on attack dimension specificity), pre-mortem (narrative frame differs from analytical risk), conference (structured disagreement with committed positions)

## Example

See [`examples/ci-plateau-cross-domain-transfer.md`](examples/ci-plateau-cross-domain-transfer.md) for a full run of `:transfer` — four isolated domain agents (ecology, theater, epidemiology, architecture) diagnose why a mature manufacturer's continuous improvement program has plateaued. Each domain produces a genuinely different ontology. The synthesis finds convergence without flattening the differences.

## Architecture

```
You: /innovate:transfer "my problem"
  |
  |-- Agent (Ecology)     --|
  |-- Agent (Theater)      --|-- isolation wall
  |-- Agent (Epidemiology) --|
  |-- Agent (Architecture) --|
  |
  v
Synthesis Agent (sees all outputs)
  |
  v
Artifact: docs/superpowers/innovate/YYYY-MM-DD-<topic>-transfer.md
  |
  v
Feed into brainstorming, planning, or direct use
```

## Prior Art

This plugin productizes patterns field-tested across two projects:

- **Object 271 Conference Pattern** — S1/S2 dialectical debate between innovator and conservative Claude sessions, with human arbiter. Produced genuinely different architectural conclusions for a knowledge graph system.
- **Novacula** — Early Anthropic API multi-agent simulation. Role-committed agents produced intelligence-product-grade artifacts with specific data, not debate transcripts.

## Relationship to Other Skills

```
Innovator (diverge — generate novel raw material)
    |
    v
Brainstorming (converge — shape into design)
    |
    v
Implementation Planning (structure — build it)
```

Innovator is a novelty multiplier, not a workflow. It produces raw ore — provocations, perspectives, surprising combinations, failure narratives — that focused thinking then refines.

## Sponsored by

[Svend](https://svend.ai) — hypothesis-driven decision science for manufacturing.

## License

Apache-2.0
