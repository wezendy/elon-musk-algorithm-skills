# The Algorithm by Elon Musk

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![AGENTS.md compatible](https://img.shields.io/badge/AGENTS.md-compatible-brightgreen)](AGENTS.md)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-open%20standard-brightgreen)](skills/)

A decomposed set of agent skills (and Obsidian-compatible notes) implementing Elon Musk's 5-step engineering algorithm. The algorithm was articulated by Musk during the 2021 Starbase tour with Tim Dodd ("Everyday Astronaut") and curated in [*The Book of Elon*](https://www.simonandschuster.com/books/The-Book-of-Elon/Eric-Jorgenson/9781668158043) by Eric Jorgenson (Simon & Schuster, 2026).

Compatible with **Claude Code**, **Codex**, **Cursor**, and any other agent that reads the `AGENTS.md` or Agent Skills (`SKILL.md`) open standard.

Inspired by [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills). The Karpathy guidelines target LLM coding behavior; this framework targets engineering decision-making at the feature and system level. The two are complementary and can be installed together.

## The Algorithm

Five steps in strict order. The order is the framework: optimizing or automating a thing that should not exist is the most common engineering failure.

| Step | Skill | Role |
|------|-------|------|
| 1 | [`musk-step-1-question-requirements`](skills/musk-step-1-question-requirements/SKILL.md) | Every requirement needs a named human originator. |
| 2 | [`musk-step-2-delete-parts`](skills/musk-step-2-delete-parts/SKILL.md) | Cut aggressively. The 10% add-back rule calibrates the cut. |
| 3 | [`musk-step-3-simplify-optimize`](skills/musk-step-3-simplify-optimize/SKILL.md) | Only after steps 1 and 2. |
| 4 | [`musk-step-4-accelerate-cycle-time`](skills/musk-step-4-accelerate-cycle-time/SKILL.md) | Only after step 3. |
| 5 | [`musk-step-5-automate`](skills/musk-step-5-automate/SKILL.md) | Last. The Fremont rule. |

Plus an entry-point overview skill: [`musk-algorithm`](skills/musk-algorithm/SKILL.md).

## Why decomposed

Each step is its own skill with its own trigger conditions, its own Gate (which enforces prior steps are complete), and its own protocol. Three benefits:

1. **Specific triggers.** A phrase like "let's optimize this query" triggers the step 3 skill, which then refuses to proceed until steps 1 and 2 have visibly run.
2. **Order enforcement via Gates.** Each step skill includes a Gate section with backward wiki links. If the Gate cannot be satisfied, the skill stops and redirects.
3. **Obsidian-native.** The wiki links between skills work in any Obsidian vault, so the same files serve as a knowledge graph.

## Scope and Use Cases

This framework is primarily designed for **review and cleanup of existing systems** (brownfield work). The algorithm's natural verbs (delete, simplify, accelerate, automate) assume something already exists. Three of the five steps require existing state: step 2 needs an inventory, step 4 needs measured cycle time, step 5 needs a manual execution history.

### Best fit

* Auditing accumulated complexity in existing systems
* Pruning feature backlogs, dashboards, services, dependencies
* Reviewing technical debt with structured cuts
* Evaluating whether to automate a process that has been running manually

### Awkward fit (use with translation)

Building greenfield applications from scratch. Only steps 1, 2, and 3 are actively useful at the start; steps 4 and 5 wait until you have an operational system.

| Step | Greenfield meaning |
|------|---------------------|
| 1. Question requirements | Where most of the value lives. Refuse every feature without a named owner. |
| 2. Delete parts | "Default to no". Reject speculative abstractions and unrequested features. |
| 3. Simplify | Build the simplest thing that solves the named requirement. |
| 4. Accelerate | Not applicable until v1 exists. |
| 5. Automate | Not applicable until the system has been operated manually. |

### Wrong fit

Function-level coding decisions (variable naming, error handling style, refactoring patterns). The algorithm operates at the feature and system level, not the function level. For function-level guidance, pair with [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).

## Repository Layout

```
elon-musk-algorithm-skills/
├── AGENTS.md                                  # Canonical source of truth
├── CLAUDE.md                                  # Claude Code: imports AGENTS.md via @AGENTS.md
├── GEMINI.md                                  # Gemini CLI: mirror of AGENTS.md
├── CHANGELOG.md
├── README.md
├── EXAMPLES.md
├── LICENSE
├── skills/
│   ├── musk-algorithm/SKILL.md
│   ├── musk-step-1-question-requirements/SKILL.md
│   ├── musk-step-2-delete-parts/SKILL.md
│   ├── musk-step-3-simplify-optimize/SKILL.md
│   ├── musk-step-4-accelerate-cycle-time/SKILL.md
│   └── musk-step-5-automate/SKILL.md
├── .cursor/rules/musk-algorithm.mdc
└── .claude-plugin/
    ├── marketplace.json
    └── plugin.json
```

## Tool support

| File | Read by |
|------|---------|
| `AGENTS.md` | Codex, Cursor, and any other agent that reads the AGENTS.md open standard |
| `CLAUDE.md` (imports AGENTS.md) | Claude Code |
| `GEMINI.md` | Gemini CLI |
| `.cursor/rules/musk-algorithm.mdc` | Cursor (additionally, for project rule scoping) |
| `skills/*/SKILL.md` | Any agent supporting the Agent Skills (SKILL.md) open standard |
| `.claude-plugin/*.json` | Claude Code plugin marketplace |

For tools not listed above, point them at `AGENTS.md` directly (for example, `aider --read AGENTS.md`).

## Install

**Claude Code:**

```
/plugin marketplace add wezendy/elon-musk-algorithm-skills
/plugin install elon-musk-algorithm-skills@musk-algorithm-skills
```

**Any AGENTS.md-compatible tool (per-project):**

```
curl -o AGENTS.md https://raw.githubusercontent.com/wezendy/elon-musk-algorithm-skills/main/AGENTS.md
```

**Full clone (includes skills directory):**

```
git clone https://github.com/wezendy/elon-musk-algorithm-skills.git
```

## Using with Obsidian

The skills use Obsidian-style wiki links between them. To use as an Obsidian knowledge graph, flatten the `SKILL.md` files into folder-named `.md` files:

```bash
cd elon-musk-algorithm-skills
for d in skills/*/; do
  skill_name=$(basename "$d")
  cp "${d}SKILL.md" "your-vault/skills/${skill_name}.md"
done
```

After flattening, wiki links like `[[musk-step-1-question-requirements]]` resolve natively in Obsidian.

## Using with Cursor

Cursor reads `AGENTS.md` at the repo root natively. For explicit project rule scoping, `.cursor/rules/musk-algorithm.mdc` provides the same content in Cursor's native rule format.

## The 10% rule

Musk's calibration heuristic for step 2: if you do not end up adding back at least 10% of what you deleted, you did not delete enough. Adding back zero means the cuts were never load-bearing in the first place. The 10% rule is what distinguishes real engineering judgment from theatrical pruning.

## License

MIT
