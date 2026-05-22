---
name: musk-step-3-simplify-optimize
description: Step 3 of the Musk algorithm. Simplify and optimize what survived deletion. Applies to BOTH brownfield (simplify existing survivors) and greenfield (build the simplest thing that works). Use whenever the user wants to optimize performance, refactor code, reduce complexity, improve a DAX measure, tune a query, restructure a pipeline, or choose the simplest viable implementation for a new feature. Trigger on phrases like "optimize this", "make this faster", "refactor", "simplify", "clean up", "reduce complexity", "improve performance", or "build the simplest version". Requires [[musk-step-1-question-requirements]] and [[musk-step-2-delete-parts]] to be complete first. This is the most common entry point where engineers want to skip ahead; refuse.
---

# Step 3: Simplify and Optimize

**Only after steps 1 and 2. Optimizing a thing that should not exist is the most common smart-engineer failure.**

Part of the [[musk-algorithm]]. Third in the strict order.

## Gate

Before this step, verify in the conversation or diff:

* [[musk-step-1-question-requirements]] has produced a list of named, surviving requirements.
* [[musk-step-2-delete-parts]] has produced an explicit deletion list and predicted add-back.

If either is absent, stop. Run the missing step first. **This is the most common skip point. Refuse aggressively.**

## Why this step exists

Schools train convergent logic: answer the question. Engineers instinctively optimize before questioning whether the thing should exist. Musk identifies this as the single most common failure mode in smart engineering teams.

A simplification or optimization of a part that survived steps 1 and 2 is high-leverage. The same work on a part that should have been deleted is wasted, and worse, it hard-codes the unjustified part deeper into the system.

## Protocol

For each surviving part:

1. **Write the justification sentence.** Before any optimization, state in one sentence: *"This exists because [named person from step 1] requires [steelmanned requirement] and removing it causes [specific failure]."* If you cannot write this sentence, return to [[musk-step-1-question-requirements]]. This is the hard gate.
2. **Measure the pre-optimization state.** Parts, lines, parameters, branches, dependencies, special cases. Without this baseline, any "optimization" is unverifiable.
3. **Reduce.** In priority order: parameters, branches, special cases, moving parts, cognitive load, lines.
4. **Match existing style.** Do not refactor adjacent code that is not in scope. Surgical changes only.
5. **Measure the post-optimization state.** Report the complexity delta.

## Output format

```
Part: [what is being simplified]
Justification sentence: "This exists because [named person] requires [requirement] and removing it causes [failure]."
Pre-optimization state:
  - Parts: N
  - Lines: N
  - Parameters: N
  - Branches: N
  - Dependencies: N
  - Special cases: N
Optimization moves:
  1. [move + justification]
  2. [move + justification]
Post-optimization state:
  - Parts: N (delta)
  - Lines: N (delta)
  - Parameters: N (delta)
  - Branches: N (delta)
  - Dependencies: N (delta)
  - Special cases: N (delta)
Out-of-scope code touched: [should be empty, or explicitly justified]
```

## Software-specific guidance

* **Reduce parameters.** A function with 8 parameters is signalling that it does multiple things. Split, or bundle into a structured input.
* **Reduce branches.** Cyclomatic complexity is a proxy for how many states a future reader must hold in their head.
* **Reduce special cases.** Three near-identical code paths with one tiny difference is one code path with one parameter.
* **Reduce indirection.** Adapter calling wrapper calling helper is three layers where one might do.
* **Do not add abstractions for single-use code.** Karpathy's principle applies here.

## Anti-patterns to refuse

* "Let's optimize this query." Verify steps 1 and 2 first. Is the query still needed?
* "Let's refactor this module to use a strategy pattern." For how many strategies? If one, the pattern is bloat.
* "Let's add a config flag for flexibility." For which named user's requirement? If none, the flag is bloat.
* Optimizing adjacent code that was not in scope. Out of scope. Mention it, do not touch it.
* Optimizing a part whose justification sentence cannot be written. Return to step 1.

## Completion criteria

This step is done when:

* Every surviving part has a justification sentence on the record.
* Every optimized part has a measured complexity delta.
* No out-of-scope code has been touched without explicit justification.

Only then can [[musk-step-4-accelerate-cycle-time]] begin.

## See also

* [[musk-algorithm]] (overview)
* [[musk-step-2-delete-parts]] (previous)
* [[musk-step-4-accelerate-cycle-time]] (next)
