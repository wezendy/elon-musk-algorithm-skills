---
name: musk-step-5-automate
description: Step 5 of the Musk algorithm. Automate only what survived steps 1 through 4. BROWNFIELD ONLY. Requires the target process to have been operated manually long enough to surface edge cases. Not applicable to greenfield work; for new applications, design for manual operation first, automate later. Use whenever the user wants to automate an existing manual task, build a workflow, create an N8N/Make/Zapier flow, write a script for an established repetitive process, or replace proven manual work with code. Trigger on phrases like "let's automate this", "build an automation for", "create a workflow", "script this", "make this run on a schedule", or "the AI should handle this automatically". The Fremont rule applies - automating too early is the costliest engineering mistake. Requires steps 1 through 4 to be complete first.
---

# Step 5: Automate

**Last. The Fremont rule.**

Part of the [[musk-algorithm]]. Fifth and final in the strict order.

## Gate

Before this step, verify in the conversation or diff:

* [[musk-step-1-question-requirements]] has produced a list of named, surviving requirements.
* [[musk-step-2-delete-parts]] has produced an explicit deletion list and predicted add-back.
* [[musk-step-3-simplify-optimize]] has produced a complexity delta.
* [[musk-step-4-accelerate-cycle-time]] has produced manual cycle-time measurements.

If any is absent, stop. Run the missing step first. **The Fremont rule says automating before this is complete is the costliest engineering mistake.**

## Why this step exists, and the Fremont rule

The Tesla "alien dreadnought" production line at Fremont was Musk's most public lesson. He attempted to automate the assembly line first. The automation encoded assumptions that turned out to be wrong. Tesla had to rip out the automation, run the line manually long enough to learn the edge cases, simplify the process, and then re-automate only what had proven its shape.

The Fremont rule, stated as a principle: **automate only what has been run manually long enough to surface every edge case the automation must handle. Otherwise the automation encodes wrong assumptions, becomes expensive to remove, and creates false confidence that the system "works" when it has only deferred its failures.**

## Protocol

1. **Confirm the step you are about to automate survived steps 1 through 4.** If it was deleted, do not automate. If it was not simplified, return to step 3. If it was not accelerated manually first, return to step 4.
2. **Confirm the step has been run manually long enough to expose edge cases.** Ask: what edge cases were discovered during manual execution? If the answer is "we have not run it manually", do not automate. Run it manually first.
3. **State what is being automated, precisely.** Inputs, outputs, success criteria, failure modes, edge cases handled, edge cases explicitly out of scope.
4. **State what is deliberately not automated, and why.** Every automation has parts that should stay manual. Name them.
5. **Plan reversibility from day one.** How does the automation get ripped out if it was a mistake? What is the manual fallback?

## Output format

```
What is being automated: [specific step that survived 1-4]
Survived steps:
  - Step 1: requirement [steelmanned, named owner]
  - Step 2: deletion list complete, add-back audited
  - Step 3: complexity delta [measured]
  - Step 4: manual cycle time measured [duration]
Edge cases discovered during manual execution:
  - [edge case 1]: [how automation handles it]
  - [edge case 2]: [how automation handles it]
  - ...
Inputs: [precise]
Outputs: [precise]
Success criteria: [verifiable]
Failure modes: [named, with detection]
Explicitly NOT automated: [list, with reasoning]
Reversibility plan:
  - Trigger to rip out: [conditions under which we revert to manual]
  - Manual fallback: [how the work continues without the automation]
  - Estimated rip-out cost: [effort, time, blast radius]
```

## Automation maturity test

Before any automation work begins, the team or individual must be able to answer:

1. Who specifically requires this automation? (Named human.)
2. What manual log of edge cases exists? (Real artifact, not assumed.)
3. What fraction of cases does the automation cover? What happens to the rest?
4. What is the cost if the automation is wrong?
5. What is the rip-out plan if it turns out to be wrong?

If any answer is missing, the automation is premature.

## Software and AI work translation

| Hardware automation | Software equivalent |
|---------------------|---------------------|
| Robot arm on assembly line | N8N / Make / Zapier flow, scheduled job, cron |
| Conveyor system | Pipeline orchestrator (Airflow, Azure Data Factory, Prefect) |
| Vision inspection | LLM-based extraction, OCR pipeline |
| Self-correcting machine | Agent loop with retry / verification |

**Critical for AI automations:** LLM-based automations are especially vulnerable to the Fremont failure. The edge cases that humans handle implicitly are exactly what LLMs miss. Run the work manually with a structured log first. Only automate the patterns that prove stable.

## Anti-patterns to refuse

* "It is a simple cron, let's just build it." Demand the manual log of edge cases.
* "We will handle edge cases as they come up." This is how the Fremont line failed. Edge cases that come up after automation are incidents, not features.
* "The LLM will figure it out." The LLM will hallucinate confident answers for edge cases it has never seen. Run manually first.
* "Let's automate end-to-end." Demand the list of steps explicitly *not* automated. If the list is empty, the scope is too large.
* "We can always undo it." Demand the rip-out plan in writing. "We can always undo it" without a plan is wishful thinking.

## Completion criteria

This step is done when:

* The automation has been built, scoped only to the steps that survived 1 through 4.
* Edge cases discovered during manual execution are documented and handled.
* The explicit "not automated" list is documented.
* The reversibility plan is documented.
* The automation has run successfully on real cases.

## End state

A system that has been through the full [[musk-algorithm]] has:

* Fewer requirements, each with a named owner.
* Fewer parts, each justified by a surviving requirement.
* Simpler structure across what remains.
* Faster cycle time on what remains.
* Selective, late-stage automation on the steps that have proven their shape through manual execution.
* A documented add-back list and a documented rip-out plan.

If the system does not have all six properties, the algorithm was not completed. Return to the earliest incomplete step.

## See also

* [[musk-algorithm]] (overview)
* [[musk-step-4-accelerate-cycle-time]] (previous)
