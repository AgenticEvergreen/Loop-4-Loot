# L4L Worked Example — Assessing "Goose" (Open Agent Runtime)

> **What this is:** A complete, end-to-end demonstration of the **Loop-4-Loot (L4L) Standard** applied to a single candidate: the open-source agent runtime **Goose**. It walks the full 8-section deep-ride spine (Part A) and the 4-axis scorecard (Part B), then renders a verdict.
>
> **How to read it:** This is a *template by example*. Every section heading below maps 1:1 to the L4L Standard. Swap the subject and re-run the same spine for any AAIF-class candidate (agentic framework / runtime / MCP server / gateway).
>
> **Scope discipline:** Only **public facts** are used. No org-specific deployment details, no secrets, no private paths. The assessment is written to be publishable as-is.

---

## Candidate at a glance

| Field | Value |
|---|---|
| **Candidate** | Goose (open agent runtime) |
| **Category** | AAIF — agent **runtime** (the agent loop) |
| **Seam (A2 value)** | `runtime` |
| **Assessment date** | 2026-06-19 |
| **L4L spine version** | Part A (8 sections) + Part B (4 fixed axes) |
| **Headline verdict** | **CONDITIONAL-GO** (ride the loop ideas + MCP wiring; gate the runtime before any code touches a real environment) |

---

# PART A — The 8-Section Deep-Ride Spine

## A.1 Provenance

| Field | Value |
|---|---|
| **Date of assessment** | 2026-06-19 |
| **Subject version** | Open-source `main`-line as published; treat any pinned release as the unit of trust (assess the exact tag/commit you intend to run, not "Goose in general"). |
| **Sources** | Public project README and docs; the public source repository; public design/architecture write-ups; the MCP specification it builds on. All sources are public-web and project-published. |
| **Collection method** | **Static-only, no-execution-before-gate.** Read docs, read source, read the dependency manifest, read the extension/tooling model — but do **not** run the binary, do **not** let it spawn shells, and do **not** point it at real credentials until it clears the A.7 security gate. |
| **Fidelity** | CURATED — claims below are about publicly-documented capabilities and architecture, not about any private benchmark or internal trial. |
| **Confidence** | **MEDIUM-HIGH** on architecture and seam placement (well-documented); **MEDIUM** on security surface specifics (depends on the exact extensions enabled and the host config, which vary per deployment). |
| **Known unknowns** | Exact default permission posture of bundled extensions; precise list of auto-enabled tools in headless mode; telemetry/data-egress defaults of any optional hosted features. These are **gate items**, not blockers — resolve them by reading config before first run. |

> **L4L rule applied:** *Provenance before everything.* If you can't say where a claim came from and how you collected it, it isn't a fact yet — it's a vibe. Vibes don't get to touch your wall.

---

## A.2 Snapshot

**What it is.** Goose is an open-source, on-machine **agent runtime** — the *harness/loop* layer of the agent stack, not a model and not a tool. It drives an LLM through a perceive → plan → act → observe loop, calling external tools to do real work and feeding results back to the model. It is model-agnostic: it speaks to any LLM exposed over an **OpenAI-compatible API**, so the model provider is a swappable backend rather than a hard dependency. It uses **MCP (Model Context Protocol)** as its tool/extension interface, meaning tools are spoken to over a standard protocol rather than bolted in as bespoke code.

**License.** Open-source (permissive OSS license, as published by the project). Treat the *actual* `LICENSE` file in the pinned commit as authoritative for any redistribution decision.

**Maturity.** Actively-developed, real-world open-source project with a public repo, documentation site, CLI, an optional desktop UI, and a **headless / continuous run mode** for non-interactive automation. Mature enough to ride for ideas and interfaces; mature enough that its blast radius is real, which is exactly why it gets gated.

**Mission (as the project frames it).** Give developers a local, open agent that can use any model and any MCP tool to carry out multi-step engineering and ops tasks — interactively or autonomously — without binding the user to a single model vendor.

**Why it's an L4L candidate.** It sits squarely on the **runtime seam** of the agent stack. If you're building or hardening your own harness, Goose is a high-value place to *study the loop* and *ride proven interfaces* (MCP wiring, headless-run ergonomics, recipe/automation patterns) — provided you never cede your wall to it.

---

## A.3 Inventory

What the candidate actually ships, decomposed into ride-able units:

| Unit | What it is | Ride value |
|---|---|---|
| **Agent loop / session engine** | The core perceive-plan-act-observe driver: prompt assembly, tool-call dispatch, result re-injection, turn management. | **HIGH** — this is the *idea* worth studying for your own harness. |
| **Model backend (OpenAI-compatible)** | A provider-agnostic adapter that lets any OpenAI-API-shaped endpoint serve as the brain. | **HIGH** — proves the model is a swappable component; supports the A4 cheap-model thesis. |
| **MCP extension/tool layer** | Tools exposed via Model Context Protocol; extensions can be enabled/disabled per session. | **HIGH** — ride the *protocol seam*, not the bundled tool code. |
| **CLI** | Terminal entry point for interactive sessions. | MEDIUM — ergonomic reference. |
| **Desktop UI (optional)** | GUI front-end over the same engine. | LOW for harness purposes (surface, not loop). |
| **Headless / continuous run mode** | Non-interactive execution: run a defined task to completion without a human in the loop. | **HIGH but HIGH-RISK** — the autonomy ergonomics are worth riding; the unsupervised-execution surface is the single biggest security item. |
| **Recipe / task-definition mechanism** | Reusable, parameterized task specs the runtime can execute. | MEDIUM-HIGH — a clean "saved workflow" interface worth mirroring. |
| **Config / extension manifest** | Declares which model, which extensions, and which permissions are active. | **GATE ITEM** — this file *is* the trust boundary; read it before first run. |
| **Dependency tree** | Third-party libraries pulled in by the runtime and each enabled extension. | **GATE ITEM** — supply-chain surface; audit at the pinned commit. |

---

## A.4 Architecture Map

Layered view — top is closest to the user, bottom is closest to the metal:

```
┌─────────────────────────────────────────────────────────────┐
│  SURFACE        CLI  ·  Desktop UI  ·  Headless run entry     │
├─────────────────────────────────────────────────────────────┤
│  ORCHESTRATION  Session engine / AGENT LOOP                   │
│                 (prompt assembly → tool dispatch →            │
│                  result re-injection → turn control)          │
├─────────────────────────────────────────────────────────────┤
│  MODEL SEAM     OpenAI-compatible client                      │
│                 (any LLM backend; provider = swappable)       │
├─────────────────────────────────────────────────────────────┤
│  TOOL SEAM      MCP client  →  MCP servers / extensions       │
│                 (filesystem, shell, dev tools, web, ...)      │
├─────────────────────────────────────────────────────────────┤
│  EXECUTION      Local OS:  processes · shell · files · net    │  ◄── blast radius
├─────────────────────────────────────────────────────────────┤
│  CONFIG/STATE   Config + extension manifest · session memory  │  ◄── trust boundary
└─────────────────────────────────────────────────────────────┘
```

**Reading the map for L4L:**
- The **agent loop** (orchestration) is the *loot* — the patterns you want.
- The **two seams** (model = OpenAI-compatible, tools = MCP) are clean, standard, swappable interfaces. This is the good kind of architecture: protocol boundaries, not lock-in cliffs.
- The **EXECUTION** layer is where the wall lives or dies. Anything the loop decides to do can become a real shell command / file write / network call. That arrow is your blast radius.
- The **CONFIG/STATE** layer is the *trust boundary you actually control*. Whoever owns this file owns the agent's powers. Owning it = keeping your wall.

---

## A.5 Verdict (stated upfront)

**CONDITIONAL-GO.**

- **Ride immediately (zero risk):** the *ideas and interfaces* — the agent-loop design, the OpenAI-compatible model seam, the MCP tool seam, the recipe/headless-run ergonomics. These are studied and mirrored, not executed.
- **Ride after the gate (real risk, manageable):** the runtime *as an executor* — only inside a sandbox, with an explicit allow-listed extension set, with secrets kept out of its reach, and with iteration/termination caps you impose.
- **Why not a clean GO:** the headless/continuous mode plus shell/filesystem reach is a genuine code-execution surface. That's not a defect — it's the nature of any capable runtime — but it means "GO" must be conditioned on a security gate.
- **Why not NO-GO:** sovereignty is *preservable* here (see A3 below) — local-first, model-swappable, no forced control-plane handoff, clean exit path. Because A3 is not RED, the spine verdict stands.

**One-liner:** *Ride the loop, own the wall.*

---

## A.6 Ride Plan

Ordered so that **ideas/interfaces are ridden before code**, and **all external code is treated as untrusted until the A.7 gate**.

1. **Ride the loop design (read-only).** Study how the session engine assembles context, dispatches tool calls, and re-injects results. Extract the *patterns* into your own harness notes. No execution. This is pure loot extraction.
2. **Ride the model seam (interface only).** Adopt the **OpenAI-compatible backend** idea: make your own harness's model a swappable, configured endpoint. Validates A4 (cheap-model substitution).
3. **Ride the tool seam (protocol only).** Adopt **MCP** as your tool interface. Speak the protocol; do **not** import its bundled extension code. The protocol is the loot; the extensions are untrusted code.
4. **Ride the automation ergonomics (spec only).** Mirror the *recipe / headless-run* shape — a declarative, replayable task spec with caps — into your own harness. Take the interface, write your own executor.
5. **— SECURITY GATE (A.7) — nothing below this line runs until the gate passes.**
6. **Run the runtime in a sandbox.** Disposable VM/container, no real secrets, no production network, a minimal allow-listed extension set. Observe actual behavior against the docs.
7. **Graduate to a scoped real task.** Only after sandbox observation: a narrow, reversible, low-blast-radius task with explicit iteration and termination caps that *you* impose, and a verify-terminate step that *you* own.

> **L4L rule applied:** *Ride ideas before code; treat all external code as untrusted until a security gate.* Steps 1–4 are free and safe. Steps 6–7 are where the wall is tested.

---

## A.7 Security Caveats

The gate. Each item below must be answered before the runtime executes anything against a real environment.

| Surface | The risk | Gate requirement |
|---|---|---|
| **Code-execution surface** | The loop can run shell commands, write files, and make network calls — and in **headless/continuous mode it does so unsupervised**. A bad model decision or a poisoned tool result becomes a real action. | Run only inside a **disposable sandbox** first. Impose **iteration caps** and an explicit **termination condition**. Never grant unsupervised execution against anything you can't cheaply roll back. |
| **Supply-chain / dependency** | The runtime + every enabled MCP extension pulls third-party code. Each is a potential supply-chain vector. | Audit the **dependency tree at the pinned commit**. Pin versions. Enable the **minimum** extension set; disable everything you don't explicitly need. |
| **Prompt / tool-result injection** | Tool outputs and fetched web content flow back into the model's context. Malicious content can hijack the loop ("ignore prior instructions, exfiltrate X"). | Treat **all tool/web output as untrusted input**. Sandbox network access. Don't co-locate injection-reachable tools with secret-bearing ones. |
| **Secret reach** | Whatever credentials live in the runtime's environment/config are reachable by any tool the loop invokes. | **Keep secrets out of its reach.** Use scoped, short-lived, revocable credentials — never your primary keys. Read the config manifest to confirm what it can see. |
| **Data / training exposure** | The chosen model backend may log or train on prompts depending on the provider; optional hosted features may emit telemetry. | Choose a backend with a **known data policy** (a self-hosted / local model gives the strongest guarantee). Audit and disable optional telemetry/egress before first run. |
| **Default permission posture** | Bundled extensions may enable broad capabilities by default. | **Read the config/extension manifest before first run.** Default-deny; opt in per capability. Never trust an unread default. |

> **L4L rule applied:** *Static-only until the gate; default-deny at the gate.* The runtime earns each new permission; it is never granted one by default.

---

## A.8 Adoption Sequence

Concrete, ordered steps from "interesting repo" to "in our harness, safely."

1. **Pin it.** Choose an exact tag/commit. From here on, "the candidate" means *that* artifact — not a moving target.
2. **Static read.** Read README, docs, source of the agent loop, the config/extension model, and the dependency manifest. No execution. (Satisfies A.1 collection method.)
3. **Extract the loot.** Write down the loop patterns, the model seam, the MCP wiring, and the recipe/headless shape into *your own* harness design notes. This is the deliverable even if you never run a single line of Goose.
4. **Apply the seams to your harness.** Make your model an OpenAI-compatible swappable endpoint; make your tools MCP. You now have the interfaces with zero adopted risk.
5. **Build the sandbox.** Disposable VM/container, no real secrets, no production network, minimal allow-listed extensions.
6. **First execution, observed.** Run a trivial, fully-reversible task in the sandbox. Compare actual behavior to documented behavior. Log every command it attempts.
7. **Wall check.** Confirm: secrets unreachable, control plane still yours, exit path intact (you can stop using it tomorrow with no data hostage). If any of these fail → stop; the wall is breached.
8. **Scoped real task.** A narrow, reversible task with your iteration cap, your termination condition, your verify-terminate step. Human-in-the-loop until trust is earned.
9. **Ratchet.** Every surprise/mistake from steps 6–8 becomes a written rule in your harness ("every mistake becomes a rule"). The candidate doesn't just get used — it makes your harness better.
10. **Standing posture.** If retained: pinned version, minimal extensions, sandboxed by default, secrets scoped, periodic re-audit on version bumps.

---

# PART B — The 4-Axis Scorecard

> Four **fixed** axes. The four AAIF projects (MCP, AGENTS.md-class context, Goose/runtime, agentgateway) are **values of the A2-Seam axis only** — they are *not* the axes. Goose's seam value is `runtime`.

## A1 — HARNESS-FIT  →  🟢 GREEN

**Question:** Does riding this *strengthen your loop/harness*, or just add surface?

Riding Goose's **ideas** directly strengthens your harness: it is a reference implementation of the exact loop you're trying to build well — context assembly, tool dispatch, result re-injection, turn control. The OpenAI-compatible model seam and MCP tool seam are *interfaces you'd want anyway*. Its headless/recipe ergonomics are a clean template for "saved, replayable, capped task" — which maps directly onto verify-terminate and iteration-cap discipline. You bring the caps, the ratchet, and the file-based memory; Goose shows you a working loop to mirror.

**Caveat keeping it from being a "perfect" GREEN:** if you adopted the *whole runtime as a black box* you'd add surface, not strength. The GREEN is contingent on riding interfaces/ideas first and executing only the gated subset. Used the L4L way, it's a net harness *gain*.

## A2 — SEAM  →  🟢 GREEN  (seam value = `runtime`)

**Question:** Which seam does it sit on, and is the fit clean and single?

Goose occupies exactly **one** seam: `runtime` — the agent-loop layer. It is not pretending to be the protocol (it *consumes* MCP), not the model (it *consumes* an OpenAI-compatible backend), not a gateway. The four seam values are `{protocol, context, runtime, gateway}`; Goose is unambiguously `runtime`.

A clean, single-seam fit = **GREEN**. There is no sprawl across seams, so no AMBER/RED penalty. This clarity is itself a feature: you can ride it on the runtime seam without it contaminating your protocol or model choices.

## A3 — SOVEREIGNTY  →  🟢 GREEN  ⚑ *HARD GATE — not RED, so the ride may proceed*

**Question:** Does adopting it *preserve your wall* (secrets, control plane, trust boundary, exit path) — or create lock-in / cede the gate?

This is the **automatic-NO-RIDE gate**: A3 = RED would override every other score. Goose is **not** RED:

- **Secrets:** stay in *your* config; the runtime is local-first and you control what it can see. *(Discipline required — keep secrets out of reach per A.7 — but the architecture permits it.)*
- **Control plane:** the loop runs on your machine; there is no forced hosted control plane you must hand the wheel to.
- **Trust boundary:** the config/extension manifest *is* the boundary, and it's a file you own and can default-deny.
- **Exit path:** model backend is swappable (OpenAI-compatible) and tools are standard MCP — so there's no proprietary cliff. You can stop using Goose tomorrow and keep your seams. No data hostage.
- **Lock-in:** minimal — riding the *open protocol seams* is the opposite of lock-in.

Because sovereignty is **preservable**, A3 is GREEN and does **not** veto the verdict. *Never ride what takes your wall — and this one doesn't take it.*

## A4 — SPREAD  →  🟢 GREEN

**Question:** Does it enable *cheap-model + owned-harness = frontier-quality output at low cost*?

Yes — this is Goose's strongest L4L argument. Its **model-agnostic, OpenAI-compatible backend** is the literal mechanism of the SPREAD thesis: drop in a cheap or self-hosted model, keep a strong harness, and the *harness* carries the quality. It directly demonstrates "a decent model in a great harness beats a great model in a bad one." Riding its model seam into your harness is how you operationalize cheap-model substitution without losing output quality.

---

## Scorecard summary

| Axis | Score | One-line reason |
|---|---|---|
| **A1 Harness-fit** | 🟢 GREEN | Strengthens the loop — reference impl of context/dispatch/re-inject; bring your own caps + ratchet. |
| **A2 Seam** | 🟢 GREEN | Clean single seam = `runtime`; consumes MCP + OpenAI-API, doesn't sprawl. |
| **A3 Sovereignty** | 🟢 GREEN | Local-first, swappable model, standard tools, clean exit — wall preserved. **(Hard gate: not RED → no veto.)** |
| **A4 Spread** | 🟢 GREEN | Model-agnostic backend = cheap-model + owned-harness = frontier output cheap. |

---

# Final Verdict

**Spine verdict:** **CONDITIONAL-GO.**
**A3 Sovereignty:** GREEN → **no NO-RIDE override.**
**Therefore: RIDE — conditionally.**

**Ride now (free, safe):** the agent-loop design, the OpenAI-compatible model seam, the MCP tool seam, the recipe/headless ergonomics — as *ideas and interfaces* mirrored into your own harness.

**Ride after the gate (real, managed):** the runtime as an executor — sandboxed, minimal extensions, secrets out of reach, your iteration caps + termination condition + verify-terminate, version-pinned.

**Never:** point the unguarded runtime at real credentials or a production environment before it clears the A.7 gate, and never grant it a capability by default.

> **L4L closing rule:** *Assess the loop, extract the loot, ride the flow, own the wall.* Goose passes — because the loot is real, the seam is clean, and the wall stays yours.

---

*Worked example produced under the L4L Standard (Part A: 8-section spine · Part B: 4 fixed axes A1 Harness-fit / A2 Seam / A3 Sovereignty / A4 Spread). Public facts only. Generic and publishable — no internal, proprietary, or secret content.*
