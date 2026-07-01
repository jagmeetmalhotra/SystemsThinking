# The Diagnostic Method — Detailed Steps

Deep how-to for Phases 0–4 of the workflow: **Frame → Stocks → Flows → Feedback
loops → Sub-systems**. Grounded in *Thinking in Systems* (Ch. 1, 3, 4). A
running **onboarding** example threads through every phase.

Golden rule (Ch. 7, "get the beat"): **study the system's behavior before you
touch it.** Watch how the numbers move over time first; don't leap to causes.

---

## Phase 0 — Frame the system

**Goal:** decide what you're analyzing and what "good" means, before modeling.

**Steps**
1. **Draw the boundary.** List what's *inside* (you can change it) vs. an
   *external input* (you can't, this pass). Boundaries are chosen for the
   problem — there are no clean boundaries in reality. Too narrow hides the
   cause; too wide is unanalyzable. State it explicitly so it can be challenged.
2. **Plot behavior over time.** Get the historical graph of the metric you care
   about. Is it growing, declining, steady, oscillating, overshooting? The
   *shape over time* is the single biggest clue to the structure.
3. **Infer the purpose from behavior — not from what people say.** "The least
   obvious part of a system, its function or purpose, is often the most crucial
   determinant of the system's behavior." Ask: *if I only watched what this
   system does, what would I conclude it is optimizing for?* Surface the gap
   between the stated goal and the revealed one.
4. **State the symptom/goal** the user wants changed, in behavior-over-time
   terms ("activation rate flat at 40% for 6 months despite signups doubling").

**Questions to ask the user**
- "What's inside this system vs. a given input you can't change right now?"
- "Show me the last 6–12 months of the key numbers — what shape are they?"
- "What does this system *actually* reward today?" (vs. the official goal)

**Onboarding example**
- Boundary: in = signup → account-open → activation steps → first value;
  out (external) = ad spend driving signups, macro demand.
- Behavior: signups ↑ 2×, activated users flat. Classic sign of a *balancing
  limit* inside onboarding.
- Stated goal: "activate users." Revealed goal (from behavior): "open as many
  accounts as possible" — the team is rewarded on accounts opened, so the system
  optimizes the wrong thing. Flag for Phase 5 (seeking the wrong goal).

---

## Phase 1 — Find the stocks

**Goal:** identify the accumulations — the things that build up or drain down.
A **stock is the present memory of the history of changing flows.**

**Steps**
1. **List what you could count if everything froze.** Stocks are nouns/levels:
   accounts opened, activated users, trial users, open tickets, candidates in
   pipeline, cash, trust, reputation, skill, goodwill.
2. **Read the data.** If CSVs / dashboard exports / query results exist, open
   them. Quantify each stock's current level and its trend. Ask the user for
   the data before you estimate — don't invent numbers you can measure.
3. **Distinguish stocks from flows.** A stock is a level (1,200 active users);
   a flow is a rate (300 new users/week). If it has units "per [time]," it's a
   flow, not a stock.
4. **Note the key property:** stocks change *slowly* — only as fast as their
   flows allow. This is why systems have momentum and inertia, and why a quick
   intervention on a stock rarely sticks. It also *decouples* inflow from
   outflow, which is what lets systems get out of balance and oscillate.

**Onboarding example — stocks**
| Stock | Level (read from data) | Trend |
|---|---|---|
| Accounts Opened (not yet activated) | 8,400 | ↑ fast |
| Activated Users | 3,300 | flat |
| Retained Users (90-day) | 2,100 | slight ↓ |
| Open Support Tickets | 540 | ↑ |

---

## Phase 2 — Map the flows

**Goal:** for each stock, find what fills it and what drains it, with rates.

**Steps**
1. **For each stock, name inflows and outflows.** Draw them as pipes in/out.
   One stock's outflow is often the next stock's inflow (a chain).
2. **Quantify rates from data** (per week/month). Note measured vs. assumed.
3. **Apply the dynamics rule:** a stock rises only while *total inflow > total
   outflow*. So there are always **two ways to grow a stock**: raise inflow OR
   lower outflow. Teams obsess over inflow (more signups) and ignore the often
   cheaper, higher-leverage outflow (reduce churn / reduce activation drop-off).
4. **Find the bottleneck flow** — the one actually constraining the behavior you
   care about.

**Onboarding example — flows**
```
signups/wk: 2,000  →  [Accounts Opened]  → activation/wk: 600  →  [Activated]
                       [Accounts Opened]  → abandon/wk: 1,400 (OUT, ignored!)
[Activated] → churn/wk: 120 → (lost)      [Activated] → refer/wk: 90 → signups
```
Insight: inflow to *Accounts Opened* doubled, but the **activation flow is the
bottleneck** (only 30% convert; 1,400/wk abandon). Pumping more signups can't
move "Activated" — the constraint is the activation flow, not the signup flow.

---

## Phase 3 — Find the feedback loops (+ delays)

**Goal:** identify the loops that make the system generate its own behavior.
A feedback loop = a closed chain where a stock's level influences a flow that
changes that same stock.

**Steps**
1. **Find reinforcing loops (R).** A stock feeds its own growth (or decline):
   more users → more referrals → more users; more revenue → more spend → more
   revenue. These cause exponential growth or collapse. Mark direction:
   virtuous (growth) or vicious (spiral).
2. **Find balancing loops (B).** Goal-seeking / limiting / stabilizing: the
   loop pushes a stock toward a target or resists change. Market saturation,
   capacity ceilings, onboarding friction, budget caps, quality gates. **Every
   reinforcing loop eventually runs into a balancing one** — find the limit that
   will bite, and when.
3. **Locate delays.** Time lags between an action and its effect (hiring ramp,
   word-of-mouth lag, perception/reporting lag). Delays in balancing loops cause
   **overshoot and oscillation** — if a metric swings up and down, look for a
   delayed balancing loop, not "bad execution."
4. **Determine the dominant loop now, and watch for shifting dominance.** System
   behavior changes when one loop overtakes another. The *story* of the metric
   is usually "loop A dominated, then loop B took over." That shift is your
   headline.

**Onboarding example — loops**
- **R1 (virtuous, weak):** Activated → referrals → signups → … . Real but small
  (90/wk) because the activated stock is flat.
- **B1 (the limiter):** Accounts Opened → activation step friction → abandons →
  caps Activated. This balancing loop is what's holding the system down.
- **B2 (with delay):** rising Open Tickets → slower support → worse experience →
  more churn (delay ~3–4 wk between bad support and churn shows up). Explains
  why retention dips lag the ticket spike.
- **Dominant loop:** B1 (activation friction). Growth spending feeds a stock
  (Accounts Opened) whose outflow is dominated by abandonment.

---

## Phase 4 — Decompose into sub-systems (hierarchy & interconnections)

**Goal:** break the system into nested sub-systems and map how they connect —
because *"the purpose of a hierarchy is always to help its originating
sub-systems do their jobs better,"* and most dysfunction lives at the seams.

**Steps**
1. **Identify sub-systems.** Each is a mini-system with its own stocks, flows,
   and loops. Onboarding sits inside Growth = Acquisition → Onboarding →
   Activation → Retention, with Support alongside.
2. **Find the interconnections.** Trace, for each sub-system, what it *sends to*
   and *receives from* the others. Interconnections are usually:
   - a **handed-off stock** (the output of one is the input of the next), or
   - an **information flow** (a signal that changes another sub-system's flow).
   The output of Acquisition (leads) is the input of Onboarding; the *information*
   "bad support experience" flows from Support into Retention's churn flow.
3. **Inspect the seams.** Most malfunction is in the interconnections and the
   information crossing them — not inside one box. A wrong, missing, or delayed
   signal across an interface breaks the whole.
4. **Check for suboptimization.** When a sub-system's own goal dominates at the
   expense of the whole, the system malfunctions. Acquisition optimizing
   "signups" (its local goal) floods Onboarding with poorly-matched users and
   *lowers* whole-system activation. That's a hierarchy/goal misalignment, not
   an onboarding-execution problem.
5. **Remember bounded rationality (Ch. 4).** Each sub-system acts rationally on
   the limited, delayed information it has. Don't blame the actors — fix the
   information flows and goals they're responding to. "Bounded rationality means
   people make quite reasonable decisions based on the information they have"
   — change the information, change the behavior.

**Onboarding example — interconnections**
```
[Acquisition] --leads (stock)--> [Onboarding] --activated (stock)--> [Retention]
[Support] --"bad experience" (info, delayed)--> raises [Retention] churn
[Acquisition] goal "max signups" --suboptimization--> lowers whole-system activation
```
Finding: the failure is at the **Acquisition→Onboarding seam** (volume over
fit) plus the **Support→Retention information delay**, not "the onboarding UX."

---

## Hand-off to Phase 5 & 6
With the model built and **confirmed by the user**, carry forward:
- traps to test → `system-traps.md` (this example already flags *seeking the
  wrong goal* and likely *shifting the burden* — paid acquisition masking weak
  activation).
- interventions to rank → `leverage-points.md` (note the obvious "more signups"
  move is a #12 number-tweak pushed the wrong way; the activation friction (#10
  structure), the Acquisition goal (#3), and the Support→churn information flow
  (#6) are the real leverage).
