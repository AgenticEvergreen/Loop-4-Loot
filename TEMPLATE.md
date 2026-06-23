# Loop-4-Loot — Deep-Ride TEMPLATE

> Copy-paste this file per candidate. Fill every section. Keep it **generic and publishable** — no
> internal/company/client names, no secrets, no private paths, no proprietary lore.
>
> **Method in one line:** *Agent = Model + Harness.* Assess an agent **LOOP**, extract the **LOOT**
> (the patterns/interfaces worth riding) without taking on the framework's risk — **ride the flow,
> own the wall.**
>
> **How to read this template:** Part A (the 8-section spine) is the deep ride. Part B (the 4 FIXED
> axes) is the scorecard. The spine produces an upfront verdict (GO / CONDITIONAL / WATCH / NO-GO);
> the scorecard's **A3 Sovereignty = RED overrides everything to NO-RIDE**.

**Candidate:** `<name>`
**Reviewer:** `<handle>`
**Date:** `<YYYY-MM-DD>`

---

## Part A — The 8-Section Deep-Ride Spine

### 1. Provenance
> Where this assessment's facts came from, and how fresh they are.
- **Date assessed:** `<YYYY-MM-DD>`
- **Sources:** `<repo URLs, docs, release notes, papers — cite each>`
- **Collection method:** **static-only, no execution before the security gate** (read code/docs; do not run untrusted code until Section 7 clears it)
- **Confidence:** `<HIGH / MEDIUM / LOW>` — `<why>`

### 2. Snapshot
> The one-paragraph "what is this" before any analysis.
- **What it is:** `<one or two sentences>`
- **License:** `<SPDX id + any restrictions>`
- **Maturity:** `<stars / release cadence / version / backing org / production usage>`
- **Mission:** `<the problem it claims to solve>`

### 3. Inventory
> Enumerate the moving parts — repos, modules, capabilities.
- **Repos / packages:** `<list>`
- **Modules / components:** `<list>`
- **Capabilities / features:** `<list>`

### 4. Architecture Map
> Draw the layers — how the thing is built and where the boundaries sit.
- **Layers:** `<e.g. transport → protocol → loop → tools → storage>`
- **Boundaries / interfaces:** `<where one layer hands off to the next; pluggable vs. fused>`
- **Diagram (optional):** `<ASCII or link>`

### 5. Verdict (upfront)
> State the call before the ride plan, so the rest of the doc justifies it.
- **Spine verdict:** `<GO / CONDITIONAL / WATCH / NO-GO>` — `<one-line rationale>`

### 6. Ride Plan
> What to ride, in what order. **Ride ideas/interfaces before code.** Treat ALL external code as untrusted until the Section-7 security gate clears it.
- **Ride first (ideas / interfaces):** `<patterns, protocols, schemas worth adopting>`
- **Ride later (code), only post-gate:** `<modules to vendor/wrap, in order>`
- **Do NOT ride:** `<parts to leave behind>`

### 7. Security Caveats
> The risk surface. This is the gate Section 6 defers to.
- **Code-execution surface:** `<what runs, where, with what privileges>`
- **Data / training exposure:** `<does your data leave; is it used for training/telemetry>`
- **Supply-chain / injection:** `<dependency risk, prompt-injection vectors, untrusted content paths>`
- **Secret reach:** `<what credentials/keys/tokens it can see or require>`

### 8. Adoption Sequence
> Ordered, concrete steps to actually adopt (or to walk).
1. `<step>`
2. `<step>`
3. `<step>`
4. `<step>`

---

## Part B — AAIF Axes Scorecard

> For AAIF-class candidates (agentic frameworks / runtimes / MCP servers / gateways).
> Score each of the **four FIXED axes** GREEN / AMBER / RED.
>
> **A2 Seam** records WHICH ONE seam the candidate sits on — the seam axis has four category
> VALUES: `{ protocol = MCP-class, context = AGENTS.md-class, runtime = agent-loop/Goose-class,
> gateway = agentgateway-class }`. Clean single-seam fit = GREEN; sprawling/unclear = AMBER/RED.
> (These four projects are **values of A2 only** — they are NOT the four axes.)

| Axis | Score | Note |
|------|-------|------|
| **A1 Harness-fit** — does riding this STRENGTHEN your loop/harness (iteration caps, verify-terminate, the ratchet "every mistake becomes a rule", file-based memory), or just add surface? | `<GREEN / AMBER / RED>` | `<note>` |
| **A2 Seam** — WHICH seam does it occupy? One of `{protocol / context / runtime / gateway}`. Clean single-seam = GREEN; sprawling/unclear = AMBER/RED. | `<GREEN / AMBER / RED>` | Seam = `<protocol / context / runtime / gateway>` — `<note>` |
| **A3 Sovereignty** — does adopting it PRESERVE your wall (secrets, control plane, trust boundary, exit path), or create lock-in / cede the gate? | `<GREEN / AMBER / RED>` | `<note>` |
| **A4 Spread** — does it enable cheap-model + owned-harness = frontier-quality output at low cost? | `<GREEN / AMBER / RED>` | `<note>` |

> **🚧 HARD GATE — A3 Sovereignty = RED forces NO-RIDE.** An automatic NO-RIDE regardless of every
> other score. *Never ride what takes your wall.*

**Verdict:** `<ADOPT / LOOT / WRAP / WALK>`

> **Verdict guide:**
> - **ADOPT** — ride it whole; strengthens the harness, clean seam, wall preserved.
> - **LOOT** — extract the patterns/interfaces; leave the framework behind.
> - **WRAP** — adopt behind your own boundary/adapter; never let it hold the gate.
> - **WALK** — do not ride.
>
> **Rule:** the spine (Section 5) gives GO / CONDITIONAL / WATCH / NO-GO; **A3 Sovereignty = RED
> overrides the verdict to WALK (NO-RIDE)** no matter what the other axes say.
