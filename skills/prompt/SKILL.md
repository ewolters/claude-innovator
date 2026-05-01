---
name: prompt
description: >-
  Guided entry to innovation techniques. Describe what you need fresh thinking on
  and get a technique recommendation with rationale. Does not dispatch subagents —
  you invoke the recommended technique yourself.
argument-hint: "describe what you need novel thinking on"
---

# Innovation Prompt

Recommend the right innovation technique for a problem.

## Step 1: Read Technique Vocabulary

Read `${CLAUDE_PLUGIN_ROOT}/techniques/vocabulary.md` for the full technique catalog
with problem shape indicators.

## Step 2: Analyze the Problem

Look at the user's description and match against problem shapes:

| Signal | Recommended Technique |
|--------|----------------------|
| "Improving X makes Y worse" / contradiction / tradeoff | `:triz` |
| Multiple independent dimensions / design space exploration | `:morph` |
| "Stuck" / same solutions keep appearing / domain-locked | `:transfer` |
| Strategy needs stress-testing / competitive dynamics | `:wargame` |
| About to commit to a plan / need failure modes | `:premortem` |
| Have a specific proposal to attack | `:redblue` |
| Design decision with real tradeoffs / need structured debate | `:conference` |

## Step 3: Recommend

Present ONE primary recommendation with a one-line rationale explaining WHY this
technique fits the problem shape. Then list 1-2 alternatives with brief notes on
when they'd be better instead.

Format:

> For this problem I'd recommend **`:technique`** — [one-line rationale tied to
> the specific problem].
>
> Alternatives: `:other` if [condition], `:other2` if [condition].

## Important

- Do NOT dispatch subagents. This skill only recommends.
- Do NOT ask clarifying questions. Match on what the user gave you.
- Keep it to 3-5 sentences total. The user wants a pointer, not a lecture.
- If the problem genuinely doesn't fit any technique, say so — don't force a match.
