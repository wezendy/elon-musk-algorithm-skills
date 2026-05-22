---
name: musk-step-2-delete-parts
description: Step 2 of the Musk algorithm. Delete aggressively. PRIMARILY BROWNFIELD - use whenever the user is reviewing an existing system, codebase, process, set of features, dashboard, document collection, or any inventory of accumulated parts. Trigger on phrases like "clean this up", "we have too many", "should we keep", "is this still used", "let's prune", "what can we remove", or any review of accumulated complexity. The 10% add-back rule is the calibration signal. GREENFIELD ANALOG - "default to no" on every speculative abstraction, configurability hook, or feature that lacks a named requirement from step 1. Requires [[musk-step-1-question-requirements]] to be complete first.
---

# Step 2: Delete Any Part or Process You Can

**Cut first. If you do not add back ~10%, you did not cut hard enough.**

Part of the [[musk-algorithm]]. Second in the strict order.

## Gate

Before this step, verify in the conversation or diff:

* [[musk-step-1-question-requirements]] has produced a list of named, surviving requirements.

If absent, stop. Run step 1 first. No exceptions.

## Why this step exists

Smart engineers default to addition. Adding a part feels like progress. Removing one feels risky. The Musk algorithm inverts this default: every part is guilty until proven necessary by a surviving requirement from step 1.

Musk's calibration rule: if you do not end up adding back at least 10% of what you deleted, you did not delete enough. Adding back zero means the cuts were never load-bearing in the first place, which means the system has even more bloat than was just removed.

## Protocol

For every part in scope (feature, endpoint, table, column, service, dependency, config flag, document, meeting, role, dashboard, report):

1. **Inventory.** List every part. If the list is incomplete, stop and request the rest.
2. **Map each part to a surviving requirement.** If no surviving requirement from step 1 justifies a part, default to delete.
3. **Identify hidden invariants.** Code, especially, can assert things that are not in the requirements. Before deleting, ask: what does this code assert? If the assertion matters and lives nowhere else, the assertion must be relocated before the code is cut.
4. **Predict the add-back list.** Before any deletion, name the parts you expect to restore once pain shows up. Zero predicted add-back means the cut is theatrical. Recut.
5. **Execute the cut.**
6. **Audit the actual add-back over the relevant time horizon.** Less than 10% means the cut was too soft. Recut deeper. More than ~25% means the cut was sloppy. Redo with more care.

## Output format

```
Inventory: [list of all parts in scope]
Deletion list: [parts proposed for removal, with one-line justification each]
Justifications for kept parts: [each kept part tied to a named-owner requirement from step 1]
Predicted add-back: [parts you expect to restore, with reasoning]
Expected add-back percentage: [explicit number, target 10-25%]
Hidden invariants identified: [what kept code or processes assert]
Cut executed: [what was actually deleted]
Audit plan: [when and how the actual add-back will be measured]
```

## Software-specific translation

The original algorithm is about hardware. For software, data, and AI work:

| Hardware concept | Software equivalent |
|------------------|---------------------|
| Part | Feature, endpoint, table, column, service, dependency, config flag, document, meeting, role |
| Process step | Pipeline stage, manual review, approval gate, deployment step |
| Bracket / fastener | Adapter, wrapper, glue function, indirection layer |
| Test fixture | Mock, stub, fixture data |

**Critical rule for code:** deleted code can hide invariants the system depends on. Before deleting, identify what the code asserts (its preconditions, postconditions, ordering guarantees, error handling). Then either relocate the assertion or accept that it goes away. Test coverage and observability are the seatbelts. Cuts made without them are reckless, not aggressive.

## Anti-patterns to refuse

* "Cut everything, we can always restore from git." No. Identify invariants first.
* "Don't touch X, it might be used somewhere." Demand the evidence. "Might be used" is not "is used".
* "We need flexibility for future use cases." Future use cases are imagined. Delete now, rebuild later if the use case becomes real.
* Deletion list with zero predicted add-back. Recut. The cut is theatrical.
* "We deleted 50% but added back 60%." This is sloppy, not aggressive. Redo with care.

## Completion criteria

This step is done when:

* Every part in scope is either justified by a surviving requirement or deleted.
* Hidden invariants have been identified and either relocated or accepted.
* The deletion has been executed.
* The add-back audit has been planned (or completed, if enough time has passed).

Only then can [[musk-step-3-simplify-optimize]] begin.

## See also

* [[musk-algorithm]] (overview)
* [[musk-step-1-question-requirements]] (previous)
* [[musk-step-3-simplify-optimize]] (next)
