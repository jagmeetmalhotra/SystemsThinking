# Design: `systems-thinking` Skill

**Date:** 2026-07-01
**Status:** Approved (pending spec review)

## Purpose

A reusable Claude Code skill that applies Donella Meadows' systems thinking
(*Thinking in Systems: A Primer*) to analyze and optimize any real-world system
or sub-system — e.g. a user onboarding flow, a growth engine, a support queue.

Given a system, the skill runs a **guided diagnostic**: it interviews the user
(and reads any available data files) to build a stock/flow/feedback model,
detects system traps, and produces a prioritized list of interventions ranked
by leverage. It steers the user toward high-leverage structural change rather
than low-leverage number-tweaking.

## Decisions (locked)

| Decision | Choice |
|---|---|
| Interaction style | **Guided diagnostic** — work phase by phase, confirm the model before recommending |
| Input | Prose description **and** data files (CSVs, dashboard exports) that Claude reads to quantify stocks/flows |
| Deliverable | **Report + Mermaid diagram + leverage-ranked action list** |
| Structure | `SKILL.md` workflow + `references/` for detailed frameworks (Approach B) |
| Install location | Personal skill: `~/.claude/skills/systems-thinking/` (usable in any project) |
| Data-ingestion script | Out of scope (YAGNI) — Claude reads data files directly |

## File layout

```
~/.claude/skills/systems-thinking/
  SKILL.md                  # guided-diagnostic workflow + phase definitions
  references/
    leverage-points.md      # the 12 leverage points, ranked, with how to spot/apply each
    system-traps.md         # the 8 archetypes: symptoms + how to escape
    diagram-templates.md    # Mermaid stock-flow & feedback-loop templates
```

## Frontmatter

```yaml
name: systems-thinking
description: >
  Use when analyzing or optimizing any real-world system or sub-system —
  onboarding, growth, support, supply, hiring, etc. Applies Donella Meadows'
  systems thinking (stocks, flows, feedback loops, system traps, 12 leverage
  points) to diagnose structure and recommend prioritized interventions.
```

## Workflow (the body of `SKILL.md`)

Claude proceeds through phases **one at a time**, confirming the model with the
user before moving on. When data files are present, Claude reads them to
quantify each step rather than guessing.

| Phase | What Claude does |
|---|---|
| 0. Frame | Pin down the system boundary, its *purpose/function*, and the symptom or goal the user cares about. |
| 1. Stocks | Identify key accumulations (accounts opened, active users, support backlog). Quantify from data if available. |
| 2. Flows | Map inflows/outflows for each stock (signups in, churn out) and their rates. |
| 3. Feedback loops | Identify reinforcing loops (growth engines) and balancing loops (constraints/saturation), plus delays. |
| 4. Sub-systems | Decompose into nested sub-systems and their interconnections (hierarchy). |
| 5. Traps | Check the system against the 8 archetypes (policy resistance, drift to low performance, success-to-the-successful, escalation, etc.). |
| 6. Leverage | Map candidate interventions onto the 12 leverage points and rank by effectiveness (Meadows' order). Flag low-leverage number-tweaks vs. high-leverage goal/paradigm shifts. |

## Deliverable

At the end Claude writes `./systems-analysis-<system>.md` in the current
project (and shows it inline):

1. **Structured report** — boundary & purpose, stocks, flows, loops,
   sub-systems, traps detected.
2. **Mermaid stock-flow + loop diagram** — boxes = stocks, pipes = flows,
   labeled R (reinforcing) / B (balancing) loops.
3. **Prioritized action list** — each recommendation tagged with its leverage
   point (#1–12) and a leverage/effort note, sorted high-leverage first.

## Guardrails (baked into the skill)

- Prefer high-leverage interventions over fiddling with numbers/parameters.
- Surface the system's *real* goal — a system optimizing the wrong goal is itself a trap.
- Expose hidden mental models / paradigms behind the system.
- Explicitly flag when a tempting fix is low-leverage.

## Reference content (sourced from the book)

- **`leverage-points.md`** — the 12 leverage points in increasing order of
  effectiveness: 12 numbers/parameters, 11 buffers, 10 stock-and-flow
  structures, 9 delays, 8 balancing feedback loops, 7 reinforcing feedback
  loops, 6 information flows, 5 rules, 4 self-organization, 3 goals,
  2 paradigms, 1 transcending paradigms. Each with how to spot it and how to
  apply it to a real system.
- **`system-traps.md`** — policy resistance, tragedy of the commons, drift to
  low performance, escalation, success to the successful, shifting the burden
  to the intervenor (addiction/dependence), rule beating, seeking the wrong
  goal. Each with symptoms and the way out.
- **`diagram-templates.md`** — Mermaid templates for stock-and-flow diagrams
  and reinforcing/balancing feedback loops.

## Out of scope

- Automated data-parsing scripts (Claude reads files directly).
- Quantitative system-dynamics simulation (this is a qualitative + light-quant
  diagnostic, matching the book's "primer" level).
