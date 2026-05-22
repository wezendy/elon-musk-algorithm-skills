# EXAMPLES.md

Before/after pairs showing the Musk Algorithm applied to real engineering decisions.

---

## Example 1: "Let's optimize this slow report"

### Before (no algorithm)

> **User:** The quarterly margin report takes 14 minutes to refresh. Can you optimize the DAX?
>
> **LLM:** Sure. I'll rewrite the measures to use SUMX instead of CALCULATE, denormalize the date dimension, and add columnstore indexes. Here are the changes...

**Result:** 6 hours of work. Report refresh drops to 8 minutes. Two weeks later the finance team mentions they have not looked at the report since Q2 because it was replaced by a Power BI app. The optimization optimized a thing that should not have existed.

### After (algorithm applied)

> **User:** The quarterly margin report takes 14 minutes to refresh. Can you optimize the DAX?
>
> **LLM:** Before optimizing, step 1: who requires this report and what decisions does it inform? Step 2: is anyone still using it? In the last 90 days, what was the open rate? If it is below a threshold, the optimization is wasted work. Can you check usage stats before I touch the DAX?

**Result:** Report turns out to have 2 opens in 90 days, both from the analyst who built it. Report gets deleted. Six hours saved.

---

## Example 2: "Build me an automation for invoice processing"

### Before

> **User:** Build an automation that pulls invoices from the shared inbox, extracts line items with OCR, and writes them to the ERP.
>
> **LLM:** Sure. I'll use Azure Document Intelligence, build a Logic App, set up a Service Bus queue for retry handling, and write a custom connector to the ERP. Here is the architecture diagram and the deployment scripts...

**Result:** Two weeks of build time. The automation handles 60% of invoices correctly. The other 40% have edge cases the team did not know existed (multi-page POs, non-Latin characters, embedded credit notes). The team now manually reprocesses 40% of invoices, which is more work than the original manual flow.

### After

> **User:** Build an automation that pulls invoices from the shared inbox, extracts line items with OCR, and writes them to the ERP.
>
> **LLM:** This is step 5 work. Before automating, I need to know what step 1 through 4 produced. Specifically:
>
> 1. Who in the team named the requirement "extract line items automatically"? What decision is blocked without it?
> 2. Of the invoices currently received, what fraction are structurally similar enough to share one extraction template?
> 3. Has anyone run the manual extraction long enough to catalog the edge cases? If not, the automation will encode incorrect assumptions.
> 4. What is the rip-out plan if the automation makes more work than it saves?
>
> If we have not done steps 1 through 4, I would propose: spend two weeks manually processing invoices with a structured log of every edge case. Then we automate the patterns that survive.

**Result:** Manual logging week reveals that 70% of invoices come from 8 vendors with stable formats. Automation is scoped to those 8 vendors only. The other 30% stay manual. Total build is 3 days, not 2 weeks, and handles 100% of its scope correctly.

---

## Example 3: "We need a new field on the customer table"

### Before

> **User:** Add a `customer_tier` field to the customers table. Marketing needs it for segmentation.
>
> **LLM:** Done. Added the column, wrote the migration, updated the API contract, added it to the admin UI, and updated the analytics dashboard to break out by tier.

**Result:** Six months later, the column is 70% NULL. No one is sure who at Marketing actually uses it. The dashboard breakdown is broken because the tier values are inconsistent.

### After

> **User:** Add a `customer_tier` field to the customers table. Marketing needs it for segmentation.
>
> **LLM:** Step 1 first. "Marketing" is not a name. Who specifically is asking, and what segmentation decision is blocked without this field today?
>
> Also: is there a reason the tier cannot be derived from existing columns (lifetime spend, account age, plan name) at query time? Storing a tier makes it stale by definition. A view or a measure does not.

**Result:** Conversation reveals the actual requester is one person on Marketing, and the tier they want is fully derivable from `plan_name` and `created_at`. No column added. A view is created. Six months later it still works.

---

## Example 4: Smart-person requirement

### Before

> **User:** Our CTO said every microservice must implement OpenTelemetry, structured logging, distributed tracing, and a `/health` endpoint with deep dependency checks. Add it to this new service.
>
> **LLM:** Understood. Here is the boilerplate. About 400 lines added.

**Result:** The "new service" is a cron job that runs once a day, reads a CSV, and writes another CSV. The 400 lines of observability boilerplate are 80% of the codebase. Six months later, the team is afraid to delete the service because of all the wiring around it.

### After

> **User:** Our CTO said every microservice must implement OpenTelemetry, structured logging, distributed tracing, and a `/health` endpoint with deep dependency checks. Add it to this new service.
>
> **LLM:** This is a smart-person requirement, which makes it the most dangerous kind. Before applying it, step 1: what specific failure does the CTO want this to prevent? Is the requirement intended for request-serving microservices, or for every workload regardless of shape?
>
> This particular workload is a once-a-day cron with no inbound traffic. A `/health` endpoint with no caller is observability theater. Structured logging plus a non-zero exit code might be the entire requirement here.
>
> I would propose: ask the CTO whether the policy covers batch jobs of this shape, and steelman the answer either way before we add 400 lines.

**Result:** CTO confirms the policy was written for request-serving services. The cron gets a logger and an exit code. 380 lines of boilerplate saved. The requirement is reformulated as: *"Request-serving services require OpenTelemetry, tracing, and `/health`. Batch jobs require structured logging and meaningful exit codes."*

---

## Example 5: The 10% add-back rule

### Before

> **User:** Clean up our internal documentation. We have too many docs.
>
> **LLM:** I'll archive everything older than 12 months that has not been viewed in 6 months.

**Result:** 800 docs archived. Two weeks later, 5 docs are restored. The cut was fine but the calibration is invisible: was 5 the right number? Was the cut too soft? Too hard? No one knows.

### After

> **User:** Clean up our internal documentation. We have too many docs.
>
> **LLM:** I'll apply the 10% rule. First, propose deletions and predict the add-back list.
>
> Proposed deletion: 800 docs (everything older than 12 months and not viewed in 6 months).
>
> Predicted add-back: 80 to 200 docs (10% to 25%). Most likely categories of restore: onboarding docs that are read once per new hire (low view count, high importance), runbooks for rare incidents (might be unviewed for years and then critical), and architecture decision records (low traffic but load-bearing).
>
> If the actual add-back is under 80, I will recut deeper. If it is over 200, the cut was sloppy and I will redo it with better filters (e.g., excluding the runbooks/ folder by default).

**Result:** Actual add-back is 47 docs (5.9%). Cut was too soft. Recut goes deeper, deleting another 200 docs. Second-round add-back is 28 docs (14%). Calibrated. Final state: ~570 docs remaining, all justified by recent use or named owner.
