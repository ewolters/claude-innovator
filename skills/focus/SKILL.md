---
name: focus
description: >-
  Research-grounded focus group. Researches real customer segments first,
  then dispatches isolated persona agents to react to a prompt from their
  perspective. Produces diverse market perspectives grounded in actual
  customer data, not generic stakeholder archetypes.
argument-hint: "describe what you want customer perspectives on"
---

# Focus Group

Research real customer segments, build grounded personas, then run an
isolated focus group where each persona reacts independently to your prompt.

## Step 1: Read Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for architectural rules.

## Step 2: Dispatch Research Agent

Use the Agent tool. This agent researches the market to identify real
customer segments and build grounded personas. It uses web search to find
actual customer voices — forums, reviews, industry reports, social media,
trade publications.

**Prompt pattern:**
```
You are a market researcher preparing a focus group. Your job is to
identify 4 distinct customer segments for this product/concept and build
a grounded persona for each.

PRODUCT/CONCEPT:
{user's prompt}

Research process:
1. Search for real customer discussions, reviews, forum posts, and
   industry reports related to this product space
2. Identify 4 segments with GENUINELY DIFFERENT needs, constraints,
   and decision-making criteria
3. For each segment, build a persona grounded in what you found:

   - **Name and role:** A specific person (e.g., "Maria, quality manager
     at a 200-person auto parts manufacturer")
   - **Context:** Their daily reality — what they deal with, what
     frustrates them, what they've tried before
   - **Decision criteria:** What actually drives their purchase/adoption
     decisions (not what they say, what they do)
   - **Skepticism profile:** What claims make them roll their eyes,
     what they've been burned by before
   - **Budget reality:** What they actually spend on this category,
     what approval process looks like
   - **Sources found:** What real data informed this persona (cite
     specific forums, reports, or review patterns)

The personas must be DIFFERENT from each other in ways that matter —
not four variations of "interested professional." Look for:
- A champion (wants this, has budget)
- A skeptic (has been burned, needs proof)
- An adjacent user (wasn't the target but could benefit)
- A constraint voice (would use it but faces a specific barrier)

Be specific. "Quality manager" is generic. "Maria, who manages 3
CMMs and 12 inspectors at a tier-2 auto parts supplier, currently
paying $2,400/yr for Minitab licenses she uses 20% of" is grounded.
```

## Step 3: Present Personas for Approval

Show the research agent's 4 personas to the user. Ask:
"These are the focus group participants based on market research.
Approve, adjust, or replace any?"

Wait for approval before proceeding. The user may want to swap a
persona or sharpen one based on their domain knowledge.

## Step 4: Dispatch Parallel Persona Agents

Use the Agent tool to dispatch ALL persona agents in a SINGLE message
(parallel execution). Each agent is fully isolated — they never see
each other's responses.

**Prompt pattern (one per persona):**
```
You are {PERSONA NAME}.

IDENTITY:
{Full persona from research — context, daily reality, frustrations,
decision criteria, skepticism profile, budget reality}

You have been invited to a focus group to give your honest reaction
to the following:

PROMPT:
{user's original prompt / question / concept}

React AS THIS PERSON. Not as a helpful AI. As {NAME}, with {NAME}'s
specific constraints, biases, history, and vocabulary.

Your response should include:
1. **First reaction** — What do you think when you hear this? Be honest.
   If it sounds like BS to someone in your position, say so.
2. **What would make you care** — What specific problem of YOURS would
   this need to solve? Be concrete about your situation.
3. **What would stop you** — Price, switching cost, trust, approval
   process, learning curve, past bad experiences. Be specific.
4. **What you'd need to see** — Proof, demo, case study, reference
   customer, free trial. What evidence would move you?
5. **How you'd describe this to your boss** — In one sentence, how
   would you pitch this internally? If you wouldn't, say why.

Speak in your natural voice. If {NAME} swears, swear. If {NAME} is
formal, be formal. If {NAME} would dismiss this in 10 seconds, say
so and explain what you'd need to hear to stay in the room.
```

## Step 5: Dispatch Synthesis Agent

Use the Agent tool. Receives ALL persona responses simultaneously.

**Prompt pattern:**
```
You are synthesizing a focus group. Four customers with different
profiles independently reacted to the same prompt. They did not hear
each other's responses.

PROMPT:
{user's original prompt}

PERSONA RESPONSES:
{All persona agents' outputs}

Synthesize:
1. **Consensus signals** — What did multiple personas independently
   agree on? (Note: convergence from independently-primed personas
   suggests signal, not proof.)
2. **Divergent reactions** — Where did personas disagree? What drove
   the disagreement — different needs, different budgets, different
   trust levels?
3. **Strongest objection** — What was the single most damaging
   criticism? From whom, and why does their perspective matter?
4. **Unmet need discovered** — Did any persona reveal a need the
   prompt didn't anticipate?
5. **The "would you pay for this" test** — Which personas would
   actually buy/adopt? Which wouldn't? Why?
6. **Language mining** — What words and phrases did the personas
   use that the prompt didn't? These are the market's vocabulary,
   not yours.

Do NOT recommend product changes. Surface what the market told you.
```

## Step 6: Write Artifact

Write to: `docs/superpowers/innovate/YYYY-MM-DD-<topic>-focus.md`

Structure:
```markdown
# Focus Group: <topic>

**Date:** YYYY-MM-DD
**Technique:** Research-Grounded Focus Group
**Personas:** [list with one-line descriptions]

## Research Summary
{Key findings from the research step — segments identified, sources}

## Persona Profiles
{Full persona for each participant}

## Persona Responses

### [Persona 1 Name]
{Full response}

### [Persona 2 Name]
{Full response}

### [Persona 3 Name]
{Full response}

### [Persona 4 Name]
{Full response}

## Synthesis
**Consensus:** {what multiple personas agreed on}
**Divergence:** {where they split and why}
**Strongest objection:** {the most damaging criticism}
**Unmet need:** {what the prompt missed}
**Would pay:** {who buys, who doesn't, why}
**Language mining:** {the market's words, not yours}
```

## Quick Mode (--quick)

If the user passes `--quick`, skip the research step. Instead:
1. Ask the user to describe 3-4 customer types
2. Build personas from their descriptions + model knowledge
3. Proceed with Steps 4-6 as normal

This is faster and cheaper but less grounded. Use when you know
your market and just need diverse reactions, not market discovery.

## Important

- Research agent uses WebSearch to find REAL customer data. Not
  model knowledge alone. The grounding is what makes this different
  from "consider multiple perspectives."
- Persona agents run in PARALLEL. They never see each other.
- The user APPROVES personas before the focus group runs. The
  research output is a checkpoint, not a pass-through.
- Synthesis does NOT recommend. It surfaces what the market said.
- Persona agents should feel REAL. If a persona would be dismissive,
  bored, or hostile — let them. Polished focus group responses are
  useless focus group responses.
