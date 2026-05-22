---
name: musk-algorithm
description: Apply Elon Musk's 5-step engineering algorithm in strict order. PRIMARY USE CASE is reviewing, auditing, cleaning up, or restructuring existing systems (brownfield work). Trigger on phrases like "clean this up", "optimize this", "we have too many", "should we keep X", "let's automate Y", "this is bloated", "review this architecture", or any request to evaluate or compress an existing system. SECONDARY USE CASE is greenfield planning ("should we build X", "do we need this feature") with translation. Steps 2, 4, 5 reinterpret as "default to no", "design for iteration", "design for manual first". DO NOT use for function-level coding decisions like variable naming, error handling style, or refactoring patterns; for those, pair this framework with andrej-karpathy-skills which operates at the function level.
---

# Musk Algorithm

Engineering decision framework derived from Elon Musk's 5-step algorithm, as articulated during the 2021 Starbase tour with Tim Dodd and curated in *The Book of Elon* by Eric Jorgenson (Simon & Schuster, 2026).

**Scope:** Primarily for reviewing and cleaning up existing systems. The algorithm's natural verbs (delete, simplify, accelerate, automate) assume something already exists. For greenfield application building, apply with translation (see below). For function-level coding behavior, pair with [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

**Tradeoff:** This framework biases toward deletion over preservation, and toward order over speed. For trivial tasks, use judgment.

**Order is mandatory.** Do not advance to a later step until the earlier steps are visibly complete in the conversation or diff. Optimizing or automating something that should not exist is the most common engineering failure. Each step skill has a Gate section that enforces this.

## The five steps

Execute in strict order. Each step is its own skill. Follow the wiki links.

1. [[musk-step-1-question-requirements]] - Every requirement needs a named human. Then challenge it.
2. [[musk-step-2-delete-parts]] - Cut first. The 10% add-back rule calibrates the cut.
3. [[musk-step-3-simplify-optimize]] - Only after 1 and 2. The most-quoted Musk failure: optimizing a thing that should not exist.
4. [[musk-step-4-accelerate-cycle-time]] - Only after 3. Never speed up a process that should be deleted.
5. [[musk-step-5-automate]] - Last. The Fremont rule.

## How to use this skill

When invoked, do the following in order:

1. Confirm scope with the user. What system, process, feature, or decision is in play. What is out of scope.
2. Identify which step is appropriate to start at. **Default is always step 1.** The user may believe they are at step 3 or step 5. Push back. Verify steps 1 and 2 have happened.
3. Load the corresponding step skill via its wiki link.
4. Execute that step's protocol.
5. Confirm completion criteria for that step before moving to the next.

## Anti-patterns to refuse

* "Skip ahead to optimization." Refuse. Verify steps 1 and 2 first.
* "We already know what to delete." Demand the deletion list and the predicted add-back. If absent, treat step 2 as not done.
* "Compliance / the team / best practice requires this." Demand the named human. If unavailable, treat the requirement as deletable.
* "It's just a small automation." Verify the manual process has been run long enough to expose edge cases.

## Working signal

The framework is working if:

* Deletion lists arrive before optimization plans.
* Requirements carry the name of a real person.
* "Let's automate this" gets pushed back to step 1.
* Net parts, lines, dependencies, or processes go down across the work.

## Domain translation

This algorithm originated in hardware (rockets, cars, factories). For software, data, and AI work, "part" maps to feature, endpoint, table, column, service, dependency, config flag, document, meeting, or role. Deleted code can hide invariants. The translation requires care, which is why steps 1 and 2 demand explicit attribution and invariant identification.

## Greenfield translation

For new application building (no existing system to review), the algorithm shifts emphasis:

| Step | Greenfield meaning |
|------|---------------------|
| 1. Question requirements | Where 80% of the value lives. Refuse every feature without a named owner. |
| 2. Delete parts | "Default to no". Reject speculative abstractions and unrequested features before they exist. |
| 3. Simplify | Build the simplest thing that solves the named requirement. |
| 4. Accelerate | Not applicable until v1 exists and there is a measurable cycle. |
| 5. Automate | Not applicable until the system has been operated manually long enough to surface edge cases. |

For greenfield, only steps 1, 2, and 3 are actively useful. Steps 4 and 5 wait until you have an operational system.

## Pairing with Karpathy guidelines

This framework operates at the **feature and system level** (what to build, what to cut, what to automate). For **function-level coding behavior** (when Claude is actually writing code), pair with [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills). They do not conflict; they operate at different granularities.

## See also

* [[musk-step-1-question-requirements]]
* [[musk-step-2-delete-parts]]
* [[musk-step-3-simplify-optimize]]
* [[musk-step-4-accelerate-cycle-time]]
* [[musk-step-5-automate]]
