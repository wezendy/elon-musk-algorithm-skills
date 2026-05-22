---
name: musk-step-1-question-requirements
description: Step 1 of the Musk algorithm. Question every requirement and demand a named human originator. Applies to BOTH brownfield (reviewing existing requirements) and greenfield (evaluating new requirements before building). Use whenever the user introduces a requirement, constraint, policy, or "we need to" statement. Trigger on phrases like "we need to", "the requirement is", "compliance says", "best practice is", "the team wants", "X is required because", "should we build X", "do we need this feature", or any rule or feature request that lacks a clear named owner. Also trigger as the first step when the [[musk-algorithm]] overview is invoked.
---

# Step 1: Question Every Requirement

**Every requirement needs a named human. Then challenge it.**

Part of the [[musk-algorithm]]. First in the strict order. No gate. This is the entry point.

## Why this step exists

Requirements accumulate anonymously. Once they have no owner, they become permanent. The most damaging requirements come from senior, smart, or admired sources because they get the least scrutiny. A requirement without a named owner is presumed invented.

Musk's framing: requirements must carry a name. Not "the legal department". Not "safety". Not "the customer". A real human, alive, who can defend it.

## Protocol

For each requirement in scope:

1. **Demand attribution.** Ask who specifically originated it. Push back on "the team", "compliance", "best practice", "the customer". These are not names.
2. **Steelman it.** Restate the requirement in the strongest possible form, in the originator's own framing if possible.
3. **Challenge it.** Ask what specifically would break if it were removed. Demand evidence the cost is real, not assumed. The smarter the originator, the harder the challenge.
4. **Make it less dumb.** Reformulate the survivor to be sharper, more specific, narrower in scope. A vague requirement that survives unrevised is a failure of this step.

## Output format

For each requirement, produce:

```
Requirement: [verbatim original]
Originator: [named human, or DELETE if none]
Steelman: [strongest version]
Challenge: [what might be wrong, vestigial, or overreaching]
Verdict: KEEP | REVISE | DELETE
If REVISE: [new wording]
If DELETE: [what breaks if removed, or "nothing identifiable"]
```

## Anti-patterns to refuse

* "Just trust me, it's required." No. Demand the named originator.
* "This came from [senior person], so it must be right." Wrong direction. Smart-person requirements get *more* scrutiny, not less.
* "Compliance / legal / safety requires it." Demand the specific regulation, article, and the human inside the organization accountable for that interpretation.
* "We've always done it this way." This is the strongest signal to challenge.
* "It might be useful later." Default to DELETE. "Later" requirements are imagined, not real.

## Completion criteria

This step is done when:

* Every requirement in scope has a named human owner, or has been deleted.
* Every surviving requirement has been steelmanned and challenged on the record.
* Every survivor is sharper than it was at the start.

Only then can [[musk-step-2-delete-parts]] begin.

## Examples

**Before:**
> "Every microservice must implement OpenTelemetry, structured logging, distributed tracing, and a /health endpoint with deep dependency checks."

**Step 1 challenge:**
> Originator: CTO (named). Steelman: production request-serving services need observability for incident response. Challenge: this workload is a once-a-day cron with no inbound traffic. A /health endpoint with no caller is observability theater. Verdict: REVISE. New wording: "Request-serving services require OpenTelemetry, tracing, and /health. Batch jobs require structured logging and meaningful exit codes."

## See also

* [[musk-algorithm]] (overview)
* [[musk-step-2-delete-parts]] (next)
