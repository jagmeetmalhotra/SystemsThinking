# Diagram Templates (Mermaid)

Conventions, following Meadows' stock-and-flow notation:

- **Stocks** = boxes (`[Name]`) — accumulations you can count.
- **Flows** = labeled arrows into/out of stocks (the "pipes").
- **Clouds** = sources/sinks outside the boundary (`(( ))` or a node named
  `Source`/`Sink`).
- **Reinforcing loop** = label the loop **R** (growth/runaway).
- **Balancing loop** = label the loop **B** (goal-seeking/stabilizing).
- Mark **delays** on the link that has the lag.

Mermaid can't draw true SDM pipes, so model stocks as nodes, flows as labeled
edges, and annotate loops with `R`/`B` nodes. Keep one diagram focused.

---

## 1. Stock-and-flow (single stock — the "bathtub")

```mermaid
flowchart LR
    src(( )) -- inflow --> S[Stock: Accounts Opened]
    S -- outflow --> snk(( ))
```

## 2. Linked stocks — a funnel / chain of sub-systems

```mermaid
flowchart LR
    src(( )) -- signups/wk --> A[Accounts Opened]
    A -- activation/wk --> B[Activated Users]
    B -- churn/wk --> snk(( ))
    B -- referrals/wk --> A
```

## 3. Reinforcing + balancing loops (growth meeting a limit)

```mermaid
flowchart TD
    U[Active Users] -- more users --> WOM[Word of Mouth]
    WOM -- signups --> U
    R(("R: growth")):::r --- WOM

    U -- approaches --> SAT[Market Saturation]
    SAT -- fewer new signups available --> U
    B(("B: limit")):::b --- SAT

    classDef r fill:#fde,stroke:#c39;
    classDef b fill:#def,stroke:#39c;
```

## 4. Balancing loop with a delay (causes oscillation)

```mermaid
flowchart TD
    BL[Support Backlog] -- "hire more agents (DELAY: ramp ~6wk)" --> CAP[Support Capacity]
    CAP -- clears tickets --> BL
    B(("B: capacity")):::b --- CAP
    classDef b fill:#def,stroke:#39c;
```

## 5. Sub-system map (hierarchy + interconnections)

Show sub-systems as subgraphs; edges across them are the **interconnections**
(usually a handed-off stock or an information flow).

```mermaid
flowchart LR
    subgraph ACQ[Acquisition]
        L[Leads]
    end
    subgraph ONB[Onboarding]
        AO[Accounts Opened] --> AU[Activated Users]
    end
    subgraph RET[Retention]
        RU[Retained Users]
    end
    subgraph SUP[Support]
        BK[Open Tickets]
    end

    L -- signups --> AO
    AU -- activation --> RU
    RU -- churn --> X(( ))
    BK -. "bad experience (info)" .-> RU
    RU -. "tickets created (info)" .-> BK
```

---

### Tips
- **Line breaks in node labels use `<br/>`, not `\n`** — Mermaid renders `\n`
  literally. Wrap multi-line labels in quotes: `(("B1: friction<br/>(dominant)"))`.
- Solid edge = physical/stock flow; dotted edge (`-.->`) = information flow.
- Put rates on flow labels when you have data (`signups/wk: 1,200`).
- Mark the **dominant loop** (e.g. bold or a note) — it's the headline finding.
- One diagram per question. Don't cram the whole company into one chart.
