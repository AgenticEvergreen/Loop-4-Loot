# GOLDEN-REF.md — The Agent-Loop Golden Reference

> **Purpose:** This is the *why* behind the Loop-4-Loot method. The 8-section deep-ride spine (Part A) tells you *how* to assess a framework; the 4 scoring axes (Part B) tell you *what* to score. This file explains the underlying model of agent behavior that makes those two parts work — the load-bearing claims, the empirical findings, and the design patterns that separate a robust agent loop from a fragile one.
>
> Read this once before your first assessment, and again whenever a candidate framework's marketing tempts you to skip the loop and trust the model.

---

## 1. The Core Equation: Agent = Model + Harness

An "agent" is not a model. An agent is a **model running inside a harness** — the loop, the tools, the context-assembly, the memory, the guardrails, and the verification logic that wrap the raw model and turn next-token prediction into useful, bounded, repeatable work.

```
Agent = Model + Harness
```

The model supplies raw reasoning capability. The harness supplies *everything that makes that capability safe, directed, and reliable*: what context the model sees on each turn, which tools it can call, how its output is checked, when the loop stops, and what happens when it goes wrong.

**The thesis of this whole method:**

> A decent model in a great harness beats a great model in a bad one. **The harness is the moat.**

Models are converging and commoditizing — capability gaps between frontier models shrink with every release, and you usually can't control which model a customer or platform hands you. The harness is the part you *own*. It is where durable advantage lives, where your accumulated operational knowledge gets encoded, and where the difference between a demo and a production system is decided.

**The operating principle:**

> **If you're not the model, you're the harness.** Ride the flow, own the wall.

You will rarely be the one training the frontier model. So your engineering leverage is entirely on the harness side. "Ride the flow" = use the best available model as a swappable component. "Own the wall" = build, control, and harden the harness — that is the asset that compounds and the thing a competitor can't clone by switching API keys.

This is also why the method extracts **LOOT** (the patterns and interfaces worth riding) from a candidate's **LOOP** *without* adopting the framework wholesale: the valuable, transferable part of any agent framework is its harness design, not its lock-in.

---

## 2. The Empirical Anchor: Same Model, Different Harness, Big Rank Jump

The equation isn't just rhetoric. The clearest public evidence comes from standardized agentic benchmarks where the **model is held fixed and only the harness changes**.

On terminal/command-line agentic benchmarks (the Terminal-Bench family and similar agentic-coding evaluations), teams have repeatedly observed:

- **Holding the underlying model constant**, swapping the surrounding harness — the scaffolding, the tool interface, the context-assembly, the verification loop — produces **large jumps in task success rate and leaderboard rank**.
- The *same* frontier model can land mid-pack under a naive harness and near the top under a well-engineered one. The delta is attributable to the harness, because that's the only variable that moved.

The takeaway for an assessor: when a framework reports impressive benchmark numbers, **the number is mostly a statement about its harness, not about the model underneath**. Two frameworks running the identical model can differ enormously. That is exactly why this method scores the harness directly instead of trusting headline accuracy figures.

> **Assessment rule of thumb:** If a candidate's results would collapse when you swap its model for a cheaper one, you're being sold a model. If the results hold, you're being sold a harness — and *that* is loot worth extracting.

---

## 3. Loop Guardrails — What Makes a Loop Safe to Run Unattended

A raw agent loop is an unbounded `while` over a probabilistic function with side effects. Left alone it will burn budget, spin in circles, declare false victory, or take irreversible actions on bad reasoning. A production-grade harness installs **guardrails** that make the loop safe to run with limited human supervision. When assessing a framework, check for each of these — their presence (or absence) is one of the strongest signals of harness maturity.

### 3.1 Iteration caps
A hard ceiling on loop iterations per task. Prevents infinite loops and runaway sessions. The cap should be configurable and should fail *loudly* (surface the partial state) rather than silently truncate.

### 3.2 Token / cost budgets
A per-task and per-session budget on tokens and dollars. The loop checks consumption each iteration and halts (or escalates to a human) when the budget is exhausted. Cost is a first-class termination condition, not an afterthought.

### 3.3 No-progress detection
Detect when the agent is *not advancing* — repeating the same tool calls, producing near-identical outputs, oscillating between two states, or editing and re-editing the same region without the verification signal improving. A good harness measures progress against an external signal (tests passing, error count dropping) and breaks the loop when that signal flatlines.

### 3.4 Circuit-breakers
Trip-wires for pathological states: repeated identical errors, a tool failing N times in a row, a sudden spike in cost-per-step, or an action that touches a protected resource. When tripped, the loop pauses and hands control back rather than pushing through. Circuit-breakers convert silent failure modes into explicit, observable stops.

### 3.5 Terminate by verification, not self-assessment
**The single most important guardrail.** The loop must end because an *external, objective check* confirms the goal is met — tests pass, the build is green, the output matches a schema, a linter is clean — **not** because the model said "I'm done" or "this looks correct." Models are systematically over-confident about their own output. Self-assessed completion is the root cause of the most common agent failure: the confident wrong answer. Tie termination to ground truth the model cannot fake.

### 3.6 Human checkpoints for irreversible acts
Any action that is hard or impossible to undo — deleting data, sending external communication, deploying, spending real money, mutating production — must pass a human checkpoint or an explicit, narrowly-scoped allowlist. Reversible actions can run autonomously; irreversible ones get a gate. The asymmetry is deliberate: the cost of an unnecessary pause is seconds; the cost of an unguarded irreversible action can be unbounded.

> **Assessment lens:** Score a candidate on how many of 3.1–3.6 it provides *natively* vs. how many you'd have to bolt on yourself. The ones you'd bolt on are the harness work the framework offloaded onto you.

---

## 4. Memory: Curated File-Based Semantic Memory > Auto Vector Stores

A recurring finding across mature agent harnesses: **curated, file-based, semantically-organized memory outperforms automatic vector-store retrieval** for agent working memory.

- **Auto vector stores** chunk everything, embed it, and retrieve by nearest-neighbor similarity. They are easy to set up and they *feel* scalable, but they retrieve by surface similarity rather than relevance-to-the-current-decision, they surface stale or contradictory fragments, they're hard to inspect or correct, and they quietly accumulate noise that degrades retrieval over time.
- **Curated file-based memory** keeps knowledge as human-readable files (notes, rules, decision records, structured docs) organized by meaning and purpose. Retrieval is explicit and legible. A human (or the agent under a ratchet, see §6) edits the memory deliberately. What's in memory is *intended* to be there, you can read it, diff it, and correct it, and it doesn't rot the way an unsupervised embedding index does.

The deeper reason this works ties back to the core equation: **memory is part of the harness, and the harness should be something you own and can inspect.** A vector store is an opaque black box; a directory of curated files is an auditable asset. For agent reliability, *legible and correct* beats *automatic and large*.

This does **not** mean vector search has no place — it remains useful for genuine large-corpus retrieval (search over a big document set). The claim is narrower and load-bearing: for the agent's *own operating knowledge* — its rules, its conventions, its hard-won lessons — curated files win.

> **Assessment lens:** Does the framework's memory let you *read and edit* what the agent knows, or does it hide it behind an embedding index you can't inspect? Inspectable memory is loot; opaque memory is a liability.

---

## 5. Sub-Agents: Split Plan / Execute / Verify

A monolithic agent that plans, acts, and judges its own work in one context is structurally biased toward declaring success — the same context that produced the plan is asked to grade it, so it grades generously. Robust harnesses **split the work across sub-agents with separated responsibilities**:

- **Plan** — a sub-agent (often at high context / careful reasoning) decomposes the goal into steps and decides the approach. It does not execute.
- **Execute** — a sub-agent (often at fresh, cheaper context) carries out one step at a time against the plan. It does not get to redefine the goal.
- **Verify** — a *separate* sub-agent checks the result against objective criteria. Because it doesn't share the executor's context, it isn't invested in the executor's answer and is far more willing to fail it.

The separation is what gives §3.5 (terminate-by-verification) its teeth: the verifier is independent of the executor, so "done" is an outside judgment, not self-congratulation. It also localizes context — each sub-agent carries only what its job needs, which keeps each one sharp and cheap.

> **Assessment lens:** Does the framework support distinct plan/execute/verify roles, or is it one big agent marking its own homework? Role separation is a high-value harness pattern.

---

## 6. The Ratchet: Every Mistake Becomes a Rule

The mechanism that makes a harness *improve over time* instead of repeating its failures:

> **Every mistake becomes a rule.**

When the agent (or a human reviewing it) hits a failure — a wrong assumption, a broken convention, a footgun — the fix is not just to patch this one run. The fix is to **write a durable rule into the curated memory (§4)** so the failure mode is structurally prevented on every future run. The loop only turns one way: knowledge accumulates, mistakes don't recur. That's the ratchet.

This is why curated file-based memory and the ratchet are a matched pair: the ratchet needs a memory you can *deliberately edit*, and curated files are exactly that. An auto vector store can't ratchet — you can't cleanly write "never do X again" as an enforceable rule into an embedding blob.

The ratchet is also the compounding mechanism behind "own the wall": each operational lesson, once written, becomes part of the harness asset. A competitor who switches API keys doesn't get your accumulated rules. They get a blank ratchet.

> **Assessment lens:** Does the framework make it easy to encode "never do this again" as a persistent, enforced rule? A harness that learns is worth more than one that's merely capable today.

---

## 7. The Ralph-Loop: Reset Context Each Iteration, Keep State on Disk

A specific, powerful loop pattern worth knowing by name. The **Ralph-loop** runs an agent by:

1. **Resetting the context each iteration** — every loop pass starts from a clean, fresh context rather than dragging an ever-growing transcript forward.
2. **Keeping all durable state on disk** — the plan, the progress, the rules, the artifacts, the next action all live in files, not in the conversation.

Each iteration the agent reads the necessary state from disk, does one bounded unit of work, writes the result back to disk, and the context is discarded. The next iteration re-hydrates from disk and continues.

Why this is strong:

- **It defeats context rot.** Long-running agents degrade as their context fills with stale reasoning, dead ends, and noise. Resetting each pass keeps every iteration sharp and within the model's reliable context window.
- **It makes the loop crash-safe and resumable.** Because state is on disk, an iteration can die (timeout, crash, hit a cap) and the loop simply re-reads disk and continues. No in-memory state to lose.
- **It enforces the discipline that makes everything else work.** If your durable knowledge has to survive a context reset every single iteration, you are *forced* to write it down — which means it's curated (§4), it's ratchet-able (§6), and it's inspectable. The Ralph-loop operationalizes "the files are the truth."
- **It composes with sub-agents (§5) and guardrails (§3).** Fresh-context execution per iteration is the natural home for an executor sub-agent; the disk state is where the verifier checks ground truth; the iteration boundary is where caps and budgets are enforced.

The Ralph-loop is, in a sense, the purest expression of the whole golden reference: **plan at high context, execute at fresh context, persist all state to disk, verify against ground truth, and ratchet every lesson into a rule.**

> **Assessment lens:** Can the framework run its loop with periodic context resets and disk-backed state, or does it assume an ever-growing in-memory transcript? Disk-backed, resettable loops scale to long-running work; transcript-bound loops hit a wall.

---

## 8. How These Patterns Reinforce Each Other

The seven ideas above aren't a checklist of independent features — they form one coherent design:

- **Agent = Model + Harness** (§1) says the harness is where you invest.
- **Terminal-Bench** (§2) proves the harness is where the wins actually come from.
- **Guardrails** (§3) make the loop safe to run unattended — with *verification-based termination* (§3.5) as the keystone.
- **Curated file memory** (§4) makes the harness inspectable and correctable.
- **Plan/Execute/Verify sub-agents** (§5) give verification its independence and keep each context sharp.
- **The ratchet** (§6) makes the harness *improve*, and it needs the curated memory to write rules into.
- **The Ralph-loop** (§7) ties it together: reset context, persist state, verify against disk, ratchet every lesson.

A framework that has all seven is a mature harness — extract its loot freely. A framework missing several is offloading harness work onto you; budget for that work before you adopt it, or extract only the parts it does well and own the rest.

---

## Further Reading

Public sources behind the patterns in this reference. (Titles/authors are stable references; locate current URLs via search — links rot, ideas don't.)

- **Addy Osmani — "Agent Harness Engineering" / writing on agentic engineering.** The clearest popular articulation of *Agent = Model + Harness*, why the harness is the moat, and the engineering disciplines (context assembly, tool design, verification loops) that make agents reliable. Start here.
- **The ReAct → Loop-Engineering trend.** Begin with the original **ReAct** paper (*ReAct: Synergizing Reasoning and Acting in Language Models*, Yao et al., 2022) for the reason+act loop, then follow the line through tool-use / function-calling and into modern "loop engineering" / "agent scaffolding" writing that treats the surrounding loop — not the prompt — as the primary engineering surface.
- **Agent-harness / agentic-framework surveys.** Survey and comparison writeups of agent runtimes, scaffolds, and orchestration frameworks (the broad "LLM agents survey" literature plus practitioner comparisons of agent frameworks). Useful for seeing the design space of harnesses and where each candidate sits.
- **Terminal-Bench and agentic-coding benchmarks.** The benchmark family behind §2 — read the methodology to see *how* they hold the model fixed and vary the harness, which is exactly the controlled experiment this method relies on. Pair with agentic-coding leaderboards (e.g., terminal/CLI-task and software-engineering task benchmarks) to see the same model ranked differently under different harnesses.
- **The Ralph-loop pattern.** Community writeups of the "reset context each iteration, keep state on disk" agent-loop pattern (originating in agentic-coding practice). Read for the crash-safe, context-rot-resistant loop design in §7.
- **Context-window / context-rot research.** Empirical work on how model performance degrades as context grows and fills with noise — the evidence base for why fresh-context-per-iteration (§7) and curated-over-auto memory (§4) win in practice.

---

*This file is part of the Loop-4-Loot (L4L) method. It is the golden reference for the loop-assessment spine: the why beneath the how. Generic and publishable by design — no internal names, no private paths, no proprietary specifics. Ride the flow, own the wall.*
