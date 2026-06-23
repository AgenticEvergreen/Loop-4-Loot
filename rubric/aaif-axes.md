# AAIF Scoring Axes — Part B (The 4 Fixed Axes)

> **Scope:** This rubric applies to **AAIF-class candidates** — Agentic-AI Infrastructure & Frameworks: agent runtimes, agent loops, MCP servers, context/instruction conventions, gateways, and the protocols that wire them together.
>
> **Where this fits:** Part A (the 8-section deep-ride spine) produces a base verdict — **GO / CONDITIONAL / WATCH / NO-GO**. Part B (this file) scores the candidate on four fixed axes. The axes refine the verdict and, in the case of **A3-Sovereignty**, can override it entirely.
>
> **Core thesis:** *Agent = Model + Harness.* A decent model in a great harness beats a great model in a bad one — the harness is the moat. You **assess the LOOP** and **extract the LOOT** (the patterns/interfaces worth riding) **without taking on the framework's risk**. Ride the flow, own the wall.

---

## How to read this file

There are **exactly four axes**, each scored **GREEN / AMBER / RED**:

| Axis | One-line question |
|---|---|
| **A1 — Harness-Fit** | Does riding this *strengthen your loop*, or just add surface? |
| **A2 — Seam** | *Which* seam of the agent stack does it sit on? |
| **A3 — Sovereignty** | Does adopting it *preserve your wall*? **(HARD GATE)** |
| **A4 — Spread** | Does it enable *cheap-model + owned-harness = frontier output at low cost*? |

> **CRITICAL — do not confuse axes with seam values.**
> The four axes are **A1 / A2 / A3 / A4**.
> The four well-known AAIF project categories — **MCP-class**, **AGENTS.md-class**, **runtime/agent-loop class**, **gateway-class** — are **NOT the axes**. They are the **four category VALUES of the A2-Seam axis only**. A2 records *which one* the candidate occupies. Everything else (A1, A3, A4) is scored independently of which seam the candidate sits on.

---

## A1 — HARNESS-FIT

### The question

Does riding this candidate **strengthen the loop/harness you already own** — or does it merely add surface area (more config, more moving parts, more to maintain) without making the loop tighter?

A strong harness is built from a small set of load-bearing properties. The test for A1 is whether the candidate **supports, supplies, or composes cleanly with** those properties:

- **Iteration caps** — bounded retries / step budgets so the loop cannot run away.
- **Verify-terminate** — the loop checks its own work and stops on a real success condition (not "ran out of turns").
- **The ratchet** — *every mistake becomes a rule.* Failures feed back into durable guidance so the same error is not repeated.
- **File-based memory** — durable state on disk (instructions, learnings, scratch artifacts) rather than memory that dies with the context window.

A1 asks: **net effect on the harness.** Riding should leave your loop *more* bounded, *more* self-verifying, *more* durable — not less.

### GREEN

The candidate **reinforces** one or more load-bearing harness properties and composes cleanly with the rest.

Concrete examples:
- A runtime that exposes explicit **step/iteration budgets** and a **termination predicate** hook, so your verify-terminate logic plugs straight in.
- A convention that standardizes **file-based instructions/memory** the loop reads each turn (the ratchet has a natural home).
- A component that **subtracts** harness code you would otherwise hand-write — it replaces glue, not adds it.
- Clear extension points where *your* verify/cap/ratchet logic is first-class, not bolted on.

### AMBER

The candidate is **neutral-to-mixed**: it adds capability but also surface, and the net effect on the loop is unclear or requires non-trivial wrapping.

Concrete examples:
- A useful runtime that has **no native iteration cap or termination hook** — you must wrap it to enforce your own bounds.
- A component whose memory model is **in-process only**; durable file-based memory is possible but you must build the bridge.
- Strong features, but adopting them means importing a wider surface than the one property you actually wanted.

### RED

The candidate **works against** the harness: it adds surface without strengthening any load-bearing property, or it actively fights your loop discipline.

Concrete examples:
- An **unbounded** agent loop with no exposed caps and no clean way to inject a termination condition — riding it imports run-away risk.
- A framework that **hides** the loop entirely (no observable step boundary), so verify-terminate and the ratchet cannot attach.
- Memory that is **opaque and ephemeral** with no file-based escape hatch — every mistake is forgotten; the ratchet cannot turn.
- So much mandatory surface that maintaining the integration costs more than the loop it claims to improve.

### How A1 feeds the verdict

A1 is a **refining** axis, not a gate.
- **GREEN** strengthens the case for **GO**.
- **AMBER** pushes toward **CONDITIONAL** (ride it, but only with named wrapping/mitigations) or **WATCH**.
- **RED** is a strong signal toward **NO-GO** — riding something that weakens your loop usually means *don't ride; extract the idea instead.* (A1-RED does **not** auto-fail by itself — only A3 does that — but a RED here rarely survives the spine.)

---

## A2 — SEAM

### The question

**Which seam of the agent stack does this candidate sit on?** Record exactly **one**. A clean single-seam fit is healthy; a candidate that sprawls across seams (or whose seam is unclear) is a smell.

This axis is about **placement and clarity**, not quality of the idea. A great idea on an unclear seam is still hard to ride safely, because you cannot reason about its blast radius.

### The five seam VALUES

> These are the **values A2 can take** — they are **not** the four scoring axes. Pick the single value that best describes where the candidate lives.

| Seam VALUE | Class | What lives here | Example shape |
|---|---|---|---|
| **`protocol`** | **MCP-class** | Wire protocols / standard interfaces between agents and tools/data. | A Model-Context-Protocol-style server or client; a standardized tool/resource interface. |
| **`context`** | **AGENTS.md-class** | Instruction & context conventions the agent reads — the durable, file-based guidance layer. | An `AGENTS.md`-style convention; a standardized instructions/memory file the loop loads each turn. |
| **`runtime`** | **agent-loop / Goose-class** | The agent loop itself — the runtime that drives plan→act→observe→repeat. | A Goose-style agent runtime; an orchestration loop; a step executor. |
| **`gateway`** | **agentgateway-class** | The control-plane / proxy seam — routing, policy, auth, observability between agents and the outside world. | An agentgateway-style proxy; a model/tool gateway enforcing policy and routing. |
| **`tool-adjunct`** *(v1.1)* | a wrapped tool | Not a seam-native component — a data source / CLI / desktop app you *wrap into* a seam (usually `protocol`) via a thin adapter you author. Score by how cleanly it wraps. | A no-code scraper, a CLI utility, a desktop automation app — useful capability exposed *through* a wrapper, not occupying a seam itself. |

A candidate's seam value is **descriptive**: `A2 = runtime`, `A2 = protocol`, etc. The GREEN/AMBER/RED grade then measures *how cleanly it occupies that single seam*.

### GREEN

The candidate occupies **exactly one** seam value, cleanly. Its responsibility is legible and its boundary is sharp.

Concrete examples:
- A library that is *purely* `protocol` (MCP-class): it defines/serves an interface and does nothing else — no embedded runtime, no hidden gateway.
- A convention that is *purely* `context` (AGENTS.md-class): it standardizes what the agent reads, full stop.
- A runtime that is *purely* `runtime`: it drives the loop and leaves protocol, context, and gateway to other layers.

### AMBER

The candidate **mostly** sits on one seam but **bleeds** into a second, or its seam is defensible but only after analysis.

Concrete examples:
- A `runtime` that also ships an opinionated, hard-to-disable `gateway` — usable, but you are importing two seams when you wanted one.
- A `protocol` library that bundles a reference `runtime` you cannot easily separate — you can still ride the protocol, but the boundary is muddy.
- A candidate where the team must debate "is this context or runtime?" before agreeing — placement isn't obvious.

### RED

The candidate **sprawls** across seams, or its seam is **genuinely unclear**. It tries to be the protocol *and* the runtime *and* the gateway at once.

Concrete examples:
- An all-in-one framework that owns the loop, defines its own non-standard protocol, *and* inserts itself as the only gateway — riding any one piece drags in all three.
- A project whose docs cannot tell you which seam it is, because it changes per feature.
- A "platform" with no separable boundary — there is no single seam to extract.

### How A2 feeds the verdict

A2 is a **shape** axis — it gauges *rideability*, not desirability.
- **GREEN** (clean single seam) supports **GO** — you can extract exactly the LOOT you want.
- **AMBER** pushes toward **CONDITIONAL** — ride a sub-part, or vendor only the seam you need.
- **RED** (sprawl/unclear) pushes toward **WATCH** or **NO-GO** — sprawl is the enemy of "ride the flow, own the wall," because you cannot isolate what you are riding.

---

## A3 — SOVEREIGNTY  *(THE HARD GATE)*

### The question

Does adopting this candidate **preserve your wall** — your secrets, your control plane, your trust boundary, and your exit path — or does it create **lock-in / cede the gate**?

> **Never ride what takes your wall.**

The wall is everything that makes your system *yours*: where the secrets live, who holds the control plane, where the trust boundary sits, and whether you can **leave**. Riding the flow is fine. Surrendering the wall is not.

### A3 is a GATE, not a refiner

**A3 = RED is an AUTOMATIC NO-RIDE, regardless of every other score.**

It does not matter if A1 is GREEN, A2 is a perfectly clean seam, and A4 promises huge cost savings. If adopting the candidate takes your wall, the verdict is **NO-RIDE**. There is no averaging A3 against the other axes; a RED here **overrides the spine verdict to NO-RIDE**.

### What the wall covers (all four must survive)

1. **Secrets** — does adopting it require handing API keys, tokens, or credentials to a third party / opaque service you don't control?
2. **Control plane** — who decides routing, policy, model selection, and shutdown? Does it stay with you?
3. **Trust boundary** — does external/untrusted input get to cross into privileged execution without a gate *you* own?
4. **Exit path** — can you stop using it without rewriting your world? Is your data/config portable? Is there a clean off-ramp?

### GREEN

Adopting the candidate **fully preserves the wall**. You keep secrets, control plane, trust boundary, and a clean exit.

Concrete examples:
- Runs **in your environment**, **BYOK** (bring-your-own-key) — secrets never leave your control plane.
- Open, self-hostable, and **forkable**; the exit path is "stop calling it" with no data hostage.
- Standard, documented interfaces (e.g. a clean `protocol` seam) so you can **swap it out** behind your own boundary.
- The trust boundary stays where *you* put it — external input is gated by *your* wall, not theirs.

### AMBER

The candidate is **rideable but with conditions** — one wall property needs an explicit mitigation, and the mitigation is real and within your power.

Concrete examples:
- A `gateway` that *could* sit in your control plane but ships defaulting to a hosted mode — fine **only if** you self-host it (named mitigation).
- A runtime that wants broad credential scope — acceptable **only with** scoped tokens / a secrets broker you own.
- A useful component with a **non-trivial but real** exit path (migration is possible but costs work).
- Trust-boundary handling is adequate **only if** you wrap untrusted input through your own gate first.

> AMBER means: **CONDITIONAL** — you may ride it, but the conditions are mandatory and must be written into the adoption decision, not assumed.

### RED

Adopting the candidate **takes your wall**. Any one of the following is sufficient for RED:

- **Secrets ceded:** mandatory hand-off of keys/credentials to an opaque, third-party-controlled service with no self-host/BYOK option.
- **Control plane ceded:** routing, policy, model choice, or kill-switch lives with the vendor and cannot be repatriated.
- **Trust boundary dissolved:** untrusted/external input reaches privileged execution and there is **no gate you can own** in front of it.
- **No exit path:** proprietary lock-in — data/config not portable, no fork, leaving means rebuilding from scratch.

Concrete examples:
- A closed runtime that **requires** your production secrets be uploaded to its cloud, with no on-prem mode.
- A gateway that becomes the **sole** control plane and cannot be self-hosted or replaced.
- A framework whose state is trapped in a proprietary store with no export.

### How A3 feeds the verdict

A3 is the **hard gate**:
- **GREEN** — wall preserved; proceed on the spine verdict.
- **AMBER** — forces the verdict to **CONDITIONAL** at best; the named wall-mitigations become hard conditions of riding.
- **RED** — **AUTOMATIC NO-RIDE.** Overrides GO / CONDITIONAL / WATCH from the spine and every other axis. *Never ride what takes your wall.* (You may still **extract the idea** in a clean-room way — but you do not adopt the thing.)

---

## A4 — SPREAD

### The question

Does this candidate enable the **spread play**: **cheap model + owned harness = frontier-quality output at low cost**?

The thesis says the harness is the moat — which means a great harness should let a *cheaper, smaller model* punch far above its weight. A4 measures whether riding this candidate **widens that gap in your favor**: does it let you move work onto cheaper models, keep more value in *your* harness, and lower unit cost without lowering output quality?

### GREEN

The candidate **directly enables** cheap-model + owned-harness economics.

Concrete examples:
- **Model-agnostic**: lets you point the loop at any model, including cheap/local ones, and swap freely — value accrues to *your* harness, not a premium model lock.
- Pushes capability **into the harness** (good verification, good context, good tool interfaces) so a weaker model still produces frontier-grade output.
- **Lowers unit cost** at equal-or-better quality (e.g. enables local inference, batching, caching, or cheaper routing under your control).
- Demonstrable "small model + this + your wall ≈ frontier result."

### AMBER

The candidate is **cost-neutral or conditionally helpful** — it doesn't block the spread play but doesn't clearly advance it either.

Concrete examples:
- Works with cheap models in principle, but is **tuned/tested only** against premium models — spread is plausible, unproven.
- Adds capability that helps quality but **also adds cost** (extra calls, extra infra), leaving net economics flat.
- Model-swappable but with friction that discourages moving off the expensive default.

### RED

The candidate **works against** the spread play — it ties output quality to an expensive model or its own metered service.

Concrete examples:
- **Hard-locked** to a single premium model; cheap/local models are unsupported or crippled.
- A metered service where the value lives in *their* layer, not your harness — you pay per call forever and the moat is theirs.
- Architecture that demands frontier-model behavior to function at all, so there is no cheap-model path.

### How A4 feeds the verdict

A4 is a **refining / economic** axis.
- **GREEN** strengthens **GO** — this is the play working as intended.
- **AMBER** is compatible with **CONDITIONAL / WATCH** — ride it but watch the economics, or revisit when cheap-model support matures.
- **RED** pushes toward **NO-GO** on economic grounds (value accrues to someone else's layer), though A4-RED is **not** an automatic gate — only A3 is.

---

## Verdict Rule (Part A + Part B)

1. **Part A (the 8-section spine)** produces the base verdict: **GO / CONDITIONAL / WATCH / NO-GO**.
2. **Part B axes refine it:**
   - **A1 (Harness-Fit)** — strengthen the loop, or just add surface? GREEN → toward GO; RED → toward NO-GO.
   - **A2 (Seam)** — record the single seam VALUE (`protocol` / `context` / `runtime` / `gateway`); clean single-seam = GREEN; sprawl/unclear = AMBER/RED.
   - **A4 (Spread)** — does it enable cheap-model + owned-harness economics? GREEN → toward GO; RED → toward NO-GO on cost grounds.
3. **A3 (Sovereignty) is the HARD GATE — it can override everything:**

   > **If A3 = RED → the verdict is NO-RIDE**, regardless of the spine verdict and regardless of A1/A2/A4. No averaging, no exceptions. *Never ride what takes your wall.*
   >
   > If A3 = AMBER → the verdict is capped at **CONDITIONAL**, and the named wall-mitigations become mandatory conditions of adoption.

### Quick reference

| Axis | GREEN | AMBER | RED |
|---|---|---|---|
| **A1 Harness-Fit** | Strengthens the loop | Neutral / needs wrapping | Adds surface / fights the loop |
| **A2 Seam** | Clean single seam | Bleeds into a 2nd seam | Sprawls / unclear seam |
| **A3 Sovereignty** *(GATE)* | Wall fully preserved | One wall property needs a real mitigation | **Wall taken → AUTOMATIC NO-RIDE** |
| **A4 Spread** | Enables cheap-model economics | Cost-neutral / unproven | Locked to expensive model/service |

**Remember:** the four axes are **A1 Harness-Fit / A2 Seam / A3 Sovereignty / A4 Spread**. The four AAIF project categories (`protocol`/MCP, `context`/AGENTS.md, `runtime`/Goose, `gateway`/agentgateway) are the **values of A2 only** — never the axes themselves. A3 is the only axis with veto power.
