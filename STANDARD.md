# The L4L Standard — Loop-4-Loot

> **Assess an agent LOOP, extract the LOOT — ride the flow, own the wall.**

This is the specification for evaluating agentic-AI frameworks, runtimes, MCP servers, and gateways **before** you adopt them. It is the contract every L4L assessment file conforms to.

---

## 0. First Principles

**Agent = Model + Harness.** A decent model in a great harness beats a great model in a bad one. The harness — your loop, your memory, your guardrails, your control plane — is the moat. Models are rented and swappable; the harness is what you own and what compounds.

The job of an L4L assessment is therefore **not** "should we install this?" It is two sharper questions:

1. **What is worth riding** — which patterns, interfaces, and ideas in this candidate would strengthen *our* harness?
2. **What must never cross the wall** — which parts would cede our secrets, control plane, trust boundary, or exit path?

We extract the LOOT (the riding-worthy patterns) **without** taking on the framework's risk. We ride ideas and interfaces first, code last, and we treat all external code as untrusted until it has passed a security gate.

**The motto is operational, not poetic:**
- *Ride the flow* — adopt the loop's good patterns into your own harness.
- *Own the wall* — never let the adopted thing become the thing that holds your keys, your routing, or your kill switch.
- *Never ride what takes your wall* — sovereignty is a hard gate, not a tradeoff.

---

## 1. Scope & Vocabulary

**In scope (AAIF-class candidates):** agentic frameworks, agent runtimes / loops, MCP servers, agent gateways, context/instruction-file conventions, and adjacent tooling that sits inside or alongside an agent stack.

**Definitions used throughout:**

- **Loop** — the iterate→act→observe→verify cycle an agent runs.
- **Harness** — everything around the model that shapes, bounds, and remembers the loop: iteration caps, verification/termination, memory, tool wiring, permissions.
- **Wall** — your sovereignty boundary: where secrets live, where the control plane runs, where the trust boundary is drawn, and the path by which you could exit the dependency.
- **Loot** — the riding-worthy extract: patterns, interfaces, schemas, and ideas you can re-implement or adopt into your own harness.
- **Seam** — the layer of the agent stack a candidate occupies (see A2).
- **Ride** — adopt a candidate's value, in increasing order of trust: *ideas → interfaces → code*.

---

## 2. Structure of an Assessment

Every L4L assessment of a single candidate is **one file** with two parts:

- **Part A — The Deep-Ride Spine** (8 sections). The narrative + evidence. Produces a spine verdict: **GO / CONDITIONAL / WATCH / NO-GO**.
- **Part B — The Four Fixed Axes** (A1/A2/A3/A4). The scorecard. Each axis is **GREEN / AMBER / RED**.

The **Verdict Rule** (Section 5 below) combines them. The single override: **A3-Sovereignty = RED ⇒ NO-RIDE**, regardless of every other score.

---

# Part A — The Deep-Ride Spine (8 Sections)

Write all eight, in order. Each section below states **what goes in it** and the **quality bar** it must clear. Sections are short and evidence-bearing — assertions without sources do not clear the bar.

---

### A.1 — Provenance

**What goes in it:**
- **Date** of assessment (and the candidate's version / commit / release tag examined).
- **Sources** — the exact docs, repos, READMEs, specs, and pages reviewed, each citable.
- **Collection method** — explicitly: **static analysis only; no code executed before the security gate**. State what was read, never run.
- **Confidence** — HIGH / MEDIUM / LOW, with a one-line reason (e.g. "docs current but sparse on auth model").

**Quality bar:** A reader can reproduce the assessment from the sources listed. The no-execution rule is stated as a fact, not an aspiration. Confidence is justified, not asserted. Every later claim in the file traces back to a source named here.

---

### A.2 — Snapshot

**What goes in it:**
- **What it is** — one or two plain sentences. No marketing language.
- **License** — the actual SPDX identifier and any usage restrictions, copyleft reach, or CLA requirements.
- **Maturity** — release cadence, version stability, issue/PR health, governance (single-vendor vs. foundation vs. community), and whether it is production-grade or experimental.
- **Mission** — what problem the project exists to solve, in its own framing.

**Quality bar:** A decision-maker who reads only this section understands what the thing is, whether the license is compatible, and how much to trust its stability. License is precise (an SPDX string, not "open source"). Maturity is evidenced (dates, counts), not vibes.

---

### A.3 — Inventory

**What goes in it:**
- The **repos / modules / packages** that make up the candidate.
- The **capabilities** each exposes — the surface area, listed concretely.
- Dependencies that matter for trust or footprint (notable transitive deps, native binaries, network-calling components).

**Quality bar:** The inventory is complete enough that the Architecture Map (A.4) and Security Caveats (A.7) can be written from it without re-discovery. Nothing load-bearing is omitted. "Capabilities" means things it can *do*, not features it *markets*.

---

### A.4 — Architecture Map

**What goes in it:**
- The candidate's **layers**, drawn as a small diagram or layered list (e.g. transport ↔ protocol ↔ runtime ↔ tool/plugin ↔ policy/control plane).
- Where the **trust boundaries** fall between layers.
- Where **secrets, credentials, and network egress** enter and exit.
- Which layer is the **seam** this candidate actually occupies (feeds A2).

**Quality bar:** The map makes the seam obvious and the data-flow legible. Anyone reading it can point to "where does a secret go" and "where does code execute." If the architecture is unclear from available sources, say so — an honest "opaque" is worth more than an invented diagram.

---

### A.5 — Verdict (stated upfront)

**What goes in it:**
- The spine verdict — **GO / CONDITIONAL / WATCH / NO-GO** — stated **first**, before the ride plan, with a one-paragraph justification.
  - **GO** — strong harness-fit, clean seam, sovereignty preserved; ride it.
  - **CONDITIONAL** — worth riding **only if** named conditions are met (e.g. "self-host the gateway," "ride the schema, not the server").
  - **WATCH** — not now; promising but immature, or risk not yet resolvable; re-assess on a named trigger.
  - **NO-GO** — do not ride. (Also the forced result when A3 = RED — see Verdict Rule.)

**Quality bar:** The verdict is unambiguous and appears before the plan, so a reader gets the answer before the reasoning. CONDITIONAL verdicts name their conditions explicitly. WATCH verdicts name the re-assessment trigger. The verdict is consistent with the Part B axes (Section 5 reconciles them).

---

### A.6 — Ride Plan

**What goes in it:**
- **What to ride** — the specific patterns, interfaces, and ideas worth extracting (the loot).
- **In what order** — sequenced by trust, lowest-trust-first:
  1. **Ride ideas** — adopt the concept / pattern into your own harness; write zero of their code.
  2. **Ride interfaces** — adopt the schema, protocol, or API shape; re-implement your own side.
  3. **Ride code** — adopt actual code **only after** it clears the security gate (A.7) and only the minimum needed.
- The explicit rule that **all external code is untrusted until a security gate clears it**, named as a step, not an afterthought.

**Quality bar:** The plan is concrete and ordered. It never jumps to "install it" before ideas/interfaces have been considered. Every code-adoption step is gated behind A.7. Each ride item ties back to a capability in A.3.

---

### A.7 — Security Caveats

**What goes in it (the four standing surfaces):**
- **Code-execution surface** — what runs, where, with what privileges; arbitrary-tool / plugin / shell reach; sandboxing (or its absence).
- **Data / training exposure** — does using it send your prompts, code, or data anywhere; any telemetry, training-data harvesting, or third-party egress.
- **Supply-chain / injection** — dependency trust, unsigned artifacts, auto-update channels, and prompt-injection vectors (tool descriptions, fetched content, MCP responses).
- **Secret reach** — what credentials it can see or demand; where they are stored; whether it could exfiltrate them.

**Quality bar:** Each of the four surfaces is addressed explicitly (or marked "not applicable" with reason). This is the gate that A.6's code-riding steps depend on — so it must be specific enough to act on. Findings are tied to the inventory (A.3) and architecture (A.4), not generic security boilerplate.

---

### A.8 — Adoption Sequence

**What goes in it:**
- An **ordered list of concrete steps** to actually adopt the riding-worthy parts, from first action to steady state.
- Each step is independently verifiable (you can tell when it's done) and reversible where possible (preserves the exit path).
- Where the security gate (A.7) sits in the sequence — code adoption never precedes it.

**Quality bar:** A competent engineer could execute the sequence without re-deriving the plan. Steps are concrete (not "integrate it"), ordered, and respect the trust-ascending order from A.6. The sequence ends in a state where the wall is still owned.

---

# Part B — The Four Fixed Axes

These four axes are **fixed** — every AAIF-class assessment scores exactly these, no more, no fewer. Each is **GREEN / AMBER / RED**.

> **Read this once and never confuse it:** the four axes are **A1 Harness-fit · A2 Seam · A3 Sovereignty · A4 Spread.** The well-known AAIF projects — MCP, AGENTS.md-class conventions, Goose-class runtimes, agentgateway-class gateways — are **NOT** axes. They are the four **category values of the A2-Seam axis only.** Do not turn the projects into the axes.

---

### A1 — Harness-Fit

**Question:** Does riding this **strengthen your loop/harness**, or does it just add surface?

A strong harness has a recognizable spine: **iteration caps** (the loop can't run away), **verify-then-terminate** (it stops when the goal is checked, not when it's tired), **the ratchet** (every mistake becomes a rule — failures are written back into the harness so they can't recur), and **file-based memory** (state lives in durable files, not ephemeral context).

Score by whether the candidate *reinforces* that spine.

- **GREEN** — Riding it directly strengthens the loop: it adds or improves iteration control, verification/termination, the mistake-into-rule ratchet, or durable file-based memory. It makes the harness tighter, not bigger.
- **AMBER** — Mixed: it offers real value but also adds surface that must be managed; net-positive only with discipline, or it strengthens one part while bloating another.
- **RED** — It adds surface without strengthening the loop: more moving parts, more to maintain, no improvement to caps/verify/ratchet/memory. Pure footprint.

---

### A2 — Seam

**Question:** **Which seam** of the agent stack does this sit on?

The seam axis has **five values** — four agent-stack seams plus one for *wrapped* tools — record the **one** the candidate occupies:

| Value | Class | What sits here |
|---|---|---|
| **protocol** | MCP-class | Tool/data interchange protocols and the servers that speak them |
| **context** | AGENTS.md-class | Instruction-file / context conventions that steer the agent |
| **runtime** | Goose-class | The agent loop / execution runtime itself |
| **gateway** | agentgateway-class | The traffic / policy / routing layer in front of models or tools |
| **tool-adjunct** | *(v1.1)* | A data source / CLI / desktop app that you *wrap into* a seam (usually `protocol`, via a thin adapter) rather than a seam-native component. The integration work lives in *your* wrapper, not the tool. |

Score by how cleanly the candidate fits a single seam.

- **GREEN** — Clean single-seam fit. It occupies exactly one of {protocol, context, runtime, gateway} and does it well. Easy to reason about, easy to swap, easy to wall off.
- **AMBER** — Spans two seams or its seam is partly unclear; usable, but the boundary blur makes integration and exit harder.
- **RED** — Sprawls across three or more seams or the seam is genuinely unclear. It wants to be your whole stack — which makes everything below (especially A3) harder.

> Record the chosen value explicitly, e.g. `A2: GREEN — seam = protocol`.

---

### A3 — Sovereignty  ⛔ HARD GATE

**Question:** Does adopting it **preserve your wall** — secrets, control plane, trust boundary, and exit path — or does it create lock-in and cede the gate?

This is the gate. **Never ride what takes your wall.**

- **GREEN** — The wall stays yours. Secrets remain under your control, the control plane stays on your side, the trust boundary is clean, and there is a clear, cheap exit path. You can stop using it tomorrow.
- **AMBER** — Some sovereignty pressure: soft lock-in, an awkward but real exit, or a control-plane component that must be self-hosted/replaced to stay safe. Ridable **only** as CONDITIONAL with the condition named.
- **RED** — It takes your wall: it demands your secrets, becomes your control plane, owns the trust boundary, or has no realistic exit (hard lock-in). **A3 = RED is an automatic NO-RIDE regardless of every other score.**

---

### A4 — Spread

**Question:** Does it enable **cheap-model + owned-harness = frontier-quality output at low cost**?

Spread measures whether the candidate widens the gap between cost and quality — the whole reason the harness is the moat. A good harness lets a cheaper model punch above its weight.

- **GREEN** — Riding it materially improves the cost/quality frontier: it lets a cheaper or smaller model produce frontier-grade output by leaning on your harness. Real spread.
- **AMBER** — Some leverage, but it's marginal, narrow, or only pays off at scale you don't have yet.
- **RED** — No spread, or negative: it ties you to expensive models, adds cost without quality, or only works with a single premium model.

---

# Section 5 — The Verdict Rule

The spine and the axes combine as follows.

### 5.1 The spine produces the headline verdict

Part A (Section A.5) yields one of: **GO / CONDITIONAL / WATCH / NO-GO**, justified by the eight sections.

### 5.2 The hard override

> **If A3-Sovereignty = RED, the verdict is forced to NO-RIDE — full stop, regardless of the spine verdict and regardless of A1, A2, and A4 all being GREEN.**

There is no trade that buys back a ceded wall. A candidate that takes your secrets, your control plane, your trust boundary, or your exit path is a NO-RIDE even if it is otherwise perfect. *Never ride what takes your wall.*

### 5.3 Reconciliation guidance (no A3-RED present)

When A3 is GREEN or AMBER, reconcile the axes with the spine verdict:

| Axis pattern | Typical spine verdict |
|---|---|
| A1/A2/A3/A4 mostly **GREEN** | **GO** |
| A3 **AMBER** (named, fixable lock-in) or one axis AMBER | **CONDITIONAL** — name the condition (e.g. "self-host the control plane," "ride the protocol schema, not the server binary") |
| Promising but immature / unresolved risk, axes not yet scorable | **WATCH** — name the re-assessment trigger |
| Multiple **RED** axes, or RED A1 (pure surface, no harness gain) | **NO-GO** |
| **A3 = RED** (any other scores) | **NO-RIDE** (override — see 5.2) |

The spine verdict and the axis scorecard must be **consistent**. If they disagree, the file is wrong — fix the reasoning, don't paper over it. The one place they cannot disagree is A3-RED: that always wins.

---

# Section 6 — Hard Rules for Every L4L File

These are non-negotiable for any file written to this standard:

1. **Generic and publishable.** No internal, company, client, or project names. No secrets. No private paths. No proprietary lore. A clean, professional OSS method file any team could pick up and use.
2. **Static-only before the gate.** No external code is executed before it clears the A.7 security gate. The Provenance section states this as fact.
3. **Ride order is trust-ascending.** Ideas → interfaces → code. Never adopt code before ideas and interfaces have been weighed and the gate has cleared.
4. **The four axes are fixed.** A1 Harness-fit / A2 Seam / A3 Sovereignty / A4 Spread — always exactly these. The AAIF projects are values of A2 only.
5. **A3-RED is absolute.** A ceded wall is a NO-RIDE, no exceptions, no buy-backs.
6. **Every claim is sourced.** If it isn't in Provenance, it isn't in the assessment.

---

*The L4L Standard. Assess the loop, extract the loot, ride the flow, own the wall.*
