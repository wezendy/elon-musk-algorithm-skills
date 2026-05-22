---
name: musk-step-4-accelerate-cycle-time
description: Step 4 of the Musk algorithm. Accelerate cycle time on what survived simplification. BROWNFIELD ONLY. Requires an existing, operational process with measurable cycle times. Not applicable to greenfield work where no cycle exists yet. Use whenever the user wants to reduce latency, shorten build/deploy time, speed up an existing pipeline, reduce time-to-insight on an established workflow, or compress any measured feedback loop. Trigger on phrases like "make this faster", "reduce cycle time", "speed up", "shorten the loop", "reduce latency", "less waiting", or "compress this process". Requires steps 1, 2, and 3 to be complete first.
---

# Step 4: Accelerate Cycle Time

**Only after step 3. Never speed up a process that should be deleted.**

Part of the [[musk-algorithm]]. Fourth in the strict order.

## Gate

Before this step, verify in the conversation or diff:

* [[musk-step-1-question-requirements]] has produced a list of named, surviving requirements.
* [[musk-step-2-delete-parts]] has produced an explicit deletion list and predicted add-back.
* [[musk-step-3-simplify-optimize]] has produced a complexity delta for the surviving parts.

If any is absent, stop. Run the missing step first.

## Why this step exists

Acceleration on top of accidental complexity hard-codes the complexity into a faster substrate. Once you have invested in making a slow, bloated process fast, the bloat is now even harder to remove because removing it would invalidate the acceleration work.

Musk's order is deliberate: cut, simplify, then speed up what remains. Speeding up before cutting and simplifying is wasted work, and creates an emotional sunk cost that prevents future cuts.

## Protocol

1. **Measure the current cycle.** End-to-end time. Per-step time. Wait time vs. work time. Queue depth at each step.
2. **Identify the longest wait, the longest queue, the longest manual step.** This is the bottleneck. Theory of constraints: any improvement that is not at the bottleneck is theater.
3. **Compress the bottleneck.** Not the easy parts. Not the parts that are already fast.
4. **State the correctness risk.** Acceleration almost always introduces shortcuts. Name them explicitly. Silent shortcuts become incidents.

## Output format

```
Current cycle time:
  - End-to-end: [duration]
  - Per step:
    - Step 1 ([name]): [duration], [work vs. wait]
    - Step 2 ([name]): [duration], [work vs. wait]
    - ...
Bottleneck: [the longest single contributor]
Why this is the bottleneck: [evidence, not assumption]
Acceleration proposal: [specific change at the bottleneck]
Correctness risk introduced: [explicit shortcuts, race conditions, weakened guarantees]
Verification plan: [how you will confirm the acceleration did not break correctness]
Projected new cycle time: [estimate]
```

## Software and data work translation

| Hardware cycle | Software equivalent |
|----------------|---------------------|
| Manufacturing line throughput | CI/CD pipeline duration |
| Setup time between batches | Cold start, deployment time |
| Tool change | Environment switch, context switch |
| Inspection cycle | Test suite, code review |
| Material wait | Queue depth, blocked tickets |

Common acceleration targets:

* **Build time.** Cache aggressively, parallelize, eliminate redundant work.
* **Test time.** Run only the tests affected by the change. Parallelize.
* **Deploy time.** Blue/green, canary, prebuilt images.
* **Time-to-insight.** Materialized views, precomputed aggregates (but verify step 1 first: is the insight actually needed?).
* **Approval cycle.** Eliminate handoffs. Reduce reviewer count to the minimum that catches the real failure modes.

## Anti-patterns to refuse

* "Let's parallelize this step." Is this step the bottleneck? If not, the parallelization is theater.
* "Let's add caching." For what specific call pattern, with what invalidation strategy? Caching without an invalidation plan is a future incident.
* "Let's add more workers." Workers do not help if the bottleneck is a serial database write.
* "We accelerated step 3 from 30s to 5s." Is step 3 the bottleneck? If step 5 takes 10 minutes, the gain is invisible.
* "We sped up the pipeline by skipping the test stage." This is not acceleration. This is removing a step. If the test stage should be removed, that is a step 2 decision, not a step 4 decision.

## Completion criteria

This step is done when:

* The bottleneck has been measured, not guessed.
* The acceleration has been applied at the bottleneck.
* The new cycle time has been measured.
* The correctness risk has been named and verified.

Only then can [[musk-step-5-automate]] begin.

## See also

* [[musk-algorithm]] (overview)
* [[musk-step-3-simplify-optimize]] (previous)
* [[musk-step-5-automate]] (next)
