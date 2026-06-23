# Loop-4-Loot (L4L)

> A method for assessing agentic-AI frameworks **before** you adopt them.
> Assess the **loop**, extract the **loot** — without taking on the framework's risk.

[![License: Apache-2.0 (TBD)](https://img.shields.io/badge/license-Apache--2.0%20(TBD)-blue.svg)](#license)
[![Status: Draft](https://img.shields.io/badge/status-draft-orange.svg)](#)
[![Method: 8-section spine + 4 axes](https://img.shields.io/badge/method-8--section%20%2B%204%20axes-green.svg)](#the-method)
[![PRs: Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

---

## The thesis

**Agent = Model + Harness.**

A decent model in a great harness beats a great model in a bad one. The model is rented; the harness is yours. **The harness is the moat.**

Most teams evaluate frameworks by demoing the model. That's the wrong layer. The model gets better every quarter on its own — what you actually own, version, and compound is the *harness*: the loop, the memory, the verification, the control plane, the wall around your secrets.

So L4L assesses the **loop** and extracts the **loot**:

- **Loop** — the agent's actual control flow: how it iterates, when it stops, how it verifies, how it remembers, where it sits in your stack.
- **Loot** — the patterns and interfaces worth riding: the seam it sits on, the contract it exposes, the ratchet it gives your harness.

The governing rule:

> **Ride the flow, own the wall.**
> Take the patterns that strengthen your loop. Never cede the boundary that protects your secrets, control plane, and exit path.

You can ride a framework's *flow* (its useful loop and interfaces) while keeping your *wall* (sovereignty over secrets, trust boundary, and the ability to walk away). L4L exists to keep those two decisions separate — and to stop you from adopting a whole framework when all you needed was its loot.

---

## What's in this repo

| Artifact | What it is |
|---|---|
| **STANDARD** | The L4L method spec. Defines the 8-section deep-ride spine (Part A) and the 4 fixed scoring axes (Part B). The normative document — everything else conforms to it. |
| **TEMPLATE** | A blank assessment you copy per candidate. Pre-structured with the 8 spine sections and the 4-axis scorecard. Fill it in; don't reinvent the shape. |
| **GOLDEN-REF** | A fully worked reference assessment. Shows the expected depth, tone, and verdict reasoning. Your north star for "what good looks like." |
| **rubric** | The scoring rubric: GREEN / AMBER / RED definitions for each axis, plus the verdict mapping (spine GO/CONDITIONAL/WATCH/NO-GO + the A3 override). |
| **examples/** | Completed assessments of real candidates. Read these to calibrate before scoring your own. |

---

## The method

L4L has two parts. **Part A** is qualitative understanding; **Part B** is the scored decision.

### Part A — the 8-section deep-ride spine

A structured read of how the candidate actually works. You ride its loop section by section before you score anything — no scoring without understanding. (Full section list in **STANDARD**; the **TEMPLATE** has the headings ready to fill.)

The spine produces a directional read: **GO / CONDITIONAL / WATCH / NO-GO**.

### Part B — the 4 FIXED scoring axes

Every AAIF-class candidate (agentic frameworks, runtimes, MCP servers, gateways) is scored on **exactly these four axes**, each **GREEN / AMBER / RED**:

#### A1 — HARNESS-FIT
Does riding this **strengthen your loop/harness**, or just add surface?
Look for the things a good harness needs: iteration caps, verify-then-terminate, the ratchet (*every mistake becomes a rule*), and file-based memory. Surface that doesn't reinforce your loop is not loot.
→ **GREEN** = strengthens the harness · **AMBER** = neutral / partial · **RED** = pure surface or weakens the loop.

#### A2 — SEAM
**Which seam of the agent stack does it sit on?** The seam axis has **five values** — four agent-stack seams, plus one for tools that get *wrapped into* a seam — record the **one** the candidate occupies:

| Seam value | Class |
|---|---|
| `protocol` | MCP-class |
| `context` | AGENTS.md-class |
| `runtime` | agent-loop / Goose-class |
| `gateway` | agentgateway-class |
| `tool-adjunct` | a data source / CLI / desktop app that is *wrapped into* a seam (usually `protocol`) rather than occupying one natively |

→ **GREEN** = clean single-seam fit · **AMBER/RED** = sprawling across seams or unclear which seam it owns. A `tool-adjunct` scores on how cleanly it can be *wrapped* (a sharp CLI/API = GREEN; a sprawling do-everything app = AMBER/RED).

> **Note:** these values — MCP, AGENTS.md, runtime, gateway, plus tool-adjunct — are the **values of the A2-Seam axis only**. They are **not** the four scoring axes. (The four well-known AAIF projects map to the first four seams; `tool-adjunct` was added in **v1.1** for the common case of a high-value OSS *tool* you wrap rather than a seam-native component.)

#### A3 — SOVEREIGNTY  *(the hard gate)*
Does adopting it **preserve your wall** — secrets, control plane, trust boundary, exit path — or create lock-in and cede the gate?
→ **GREEN** = wall intact, clean exit · **AMBER** = some entanglement, exit costs effort · **RED** = lock-in / cedes the gate.

> **A3 = RED is an AUTOMATIC NO-RIDE, regardless of every other score.**
> **Never ride what takes your wall.**

#### A4 — SPREAD
Does it enable **cheap-model + owned-harness = frontier-quality output at low cost**?
The payoff of the thesis: a strong harness lets a cheaper model punch above its weight.
→ **GREEN** = clear cost-down-for-quality leverage · **AMBER** = marginal · **RED** = forces you onto expensive models for acceptable output.

> **CRITICAL:** the four axes are **A1 Harness-fit / A2 Seam / A3 Sovereignty / A4 Spread**. Use exactly these names.

---

## Quickstart (5 steps)

1. **Copy the TEMPLATE** — one assessment file per candidate.
2. **Fill the 8-section spine (Part A)** — ride the loop section by section; produce the directional read **GO / CONDITIONAL / WATCH / NO-GO**.
3. **Score the 4 axes (Part B)** — rate each GREEN / AMBER / RED:
   - **A1 Harness-fit** — strengthens your loop, or just surface?
   - **A2 Seam** — which one seam: `protocol` / `context` / `runtime` / `gateway`?
   - **A3 Sovereignty** — wall preserved, or lock-in?
   - **A4 Spread** — cheap model + owned harness = frontier output cheaply?
4. **Verdict** — map spine + axes to an outcome:

   | Outcome | Meaning |
   |---|---|
   | **ADOPT** | Ride the whole thing — loop and interfaces. |
   | **LOOT** | Take the patterns/interfaces; don't adopt the framework. |
   | **WRAP** | Use it, but behind your own boundary/adapter so you keep the wall. |
   | **WALK** | Don't ride. Walk away. |

   **The override:** if **A3 Sovereignty = RED**, the verdict is **WALK** — forced, no exceptions, no matter how green everything else is. The spine's NO-GO and an A3=RED both land on WALK; A3=RED is the non-negotiable gate.

5. **Write the ride plan** — for ADOPT / LOOT / WRAP, document exactly *what* you ride, *where* the seam attaches, *how* the wall stays intact, and *what* your exit path is. The ride plan is the deliverable.

---

## Verdict logic (at a glance)

```
spine read ──► GO ──┐
            CONDITIONAL ──┤
                 WATCH ──┤──► combine with A1/A2/A4 ──► ADOPT / LOOT / WRAP
                 NO-GO ──┴──────────────────────────► WALK
                                                        ▲
                          A3 Sovereignty = RED ─────────┘  (HARD OVERRIDE → WALK)
```

- The **spine** gives the directional read.
- The **axes** refine it into ADOPT / LOOT / WRAP.
- **A3 = RED overrides everything to WALK.** Never ride what takes your wall.

---

## Who it's for

- **Platform / infra teams** choosing an agent runtime, MCP server, context standard, or gateway and needing a defensible adopt/skip decision.
- **Engineering leads** who keep getting handed "let's adopt framework X" and want a repeatable rubric instead of a vibe.
- **Architects** designing an agent stack who want to be explicit about which seam each dependency owns — and where the wall is.
- **Anyone evaluating AAIF-class candidates** (agentic frameworks, runtimes, MCP servers, gateways) who wants the *loot* without the lock-in.

---

## Why this exists

**Teams keep forking frameworks they should have looted.**

The common failure: a framework demos well, so a team adopts the whole thing — runtime, conventions, control plane, and all — then spends months forking it, patching it, and fighting its assumptions. What they actually needed was one or two patterns (a clean seam, a good loop, a memory model) that they could have *looted* in an afternoon and ridden behind their own wall.

L4L is the discipline that catches this *before* adoption:

- It forces you to name the **seam** so you don't take on a runtime when you wanted a protocol.
- It forces you to check **sovereignty** so you don't trade your control plane for convenience.
- It separates **ride the flow** (the loop and interfaces worth taking) from **own the wall** (the boundary you never give up).

Most "should we adopt X" questions resolve to **LOOT** or **WRAP**, not **ADOPT**. This method makes that visible on purpose.

---

## Contributing

Contributions welcome — new example assessments, rubric refinements, and method clarifications especially.

**Hard rule for every file in this repo:** keep it **generic and publishable**. No internal/company/client names, no secrets, no private paths, no proprietary lore. A clean, professional method any team can pick up and use.

---

## License

**Apache-2.0 (TBD).** Intended license is Apache-2.0; the `LICENSE` file will be finalized before first tagged release.

---

*Loop-4-Loot — assess the loop, extract the loot. Ride the flow, own the wall.*
