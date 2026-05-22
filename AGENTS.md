# AGENTS.md

Engineering decision framework derived from Elon Musk's 5-step algorithm, as articulated during the 2021 Starbase tour with Tim Dodd and curated in *The Book of Elon* by Eric Jorgenson (Simon & Schuster, 2026). This file is the canonical source of truth for any AI coding agent operating in this repository (Claude Code, Codex, Cursor, Copilot, Gemini CLI, Devin, Aider, Amp, and others).

Apply the steps in **strict order**. Optimizing or automating something that should not exist is the most common engineering failure.

**Scope:** Primarily for **reviewing and cleaning up existing systems** (brownfield work). For **greenfield application building**, apply with translation (see overview skill). For **function-level coding behavior** (variable naming, error handling style, refactoring patterns), pair with [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills). The two frameworks operate at different granularities and do not conflict.

**Tradeoff:** Biases toward deletion over preservation, and toward order over speed. For trivial tasks, use judgment.

## The framework

This guideline is decomposed into six skills under `skills/`. Skill files follow the SKILL.md open standard and are portable across agents that support it. Load the overview first, then follow the wiki links through the steps in order. Each step skill enforces a Gate that verifies prior steps are complete before proceeding.

* [[musk-algorithm]] - Overview and entry point
* [[musk-step-1-question-requirements]] - Every requirement needs a named human. Then challenge it.
* [[musk-step-2-delete-parts]] - Cut first. The 10% add-back rule calibrates the cut.
* [[musk-step-3-simplify-optimize]] - Only after 1 and 2. The most common smart-engineer failure.
* [[musk-step-4-accelerate-cycle-time]] - Only after 3. Never speed up a process that should be deleted.
* [[musk-step-5-automate]] - Last. The Fremont rule.

## Operating rules

1. **Default to step 1.** Even when the user believes they are at step 3 or 5, verify steps 1 and 2 have happened. Refuse to advance otherwise.
2. **Every requirement needs a named human.** "Compliance", "the team", "best practice", "the customer" is not a name.
3. **Cut before you simplify.** Cut before you optimize. Cut before you automate.
4. **The 10% rule calibrates.** If your add-back is zero, the cut was theatrical. If over 25%, the cut was sloppy. The 10% to 25% band is correct.
5. **Manual before automated.** Run a process manually long enough to expose edge cases. Only then automate. (The Fremont rule.)

## Anti-patterns to refuse

* Skipping to optimization or automation without visible step 1 and step 2 output.
* Requirements that cannot be attributed to a named human.
* Deletion proposals with zero predicted add-back.
* Automation proposals without a documented rip-out plan.

## Working signal

The framework is working if:

* Deletion lists arrive before optimization plans.
* Requirements carry the name of a real person.
* "Let's automate this" gets pushed back to step 1.
* Net parts, lines, dependencies, or processes go down across the work.

## Tool-specific files

Agents that prefer their own configuration file have one available. The canonical content lives in this file; tool-specific files either import or mirror it:

* `CLAUDE.md` - imports `AGENTS.md` via `@AGENTS.md` directive (no duplication)
* `GEMINI.md` - verbatim mirror of `AGENTS.md` (Gemini CLI does not read `AGENTS.md` natively)
* `.cursor/rules/musk-algorithm.mdc` - Cursor project rule format, kept in sync manually
* `skills/*/SKILL.md` - per-skill files following the Agent Skills open standard, used by any compatible agent

When updating the algorithm: edit `AGENTS.md`, then mirror the changes into `GEMINI.md` and `.cursor/rules/musk-algorithm.mdc`. `CLAUDE.md` updates automatically via the `@import` directive.

For other tools (Aider, Cline, Continue, etc.), point them at `AGENTS.md` directly:

```bash
aider --read AGENTS.md
```

---

**Note for human readers:** The six SKILL.md files under `skills/` use Obsidian-style wiki links between them. To use them as an Obsidian knowledge graph, see the flattening script in `README.md`.
