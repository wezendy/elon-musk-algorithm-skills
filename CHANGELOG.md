# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-22

### Added

* Initial release of the Musk Algorithm decomposed into six Agent Skills:
  - `musk-algorithm` (overview, entry point)
  - `musk-step-1-question-requirements`
  - `musk-step-2-delete-parts`
  - `musk-step-3-simplify-optimize`
  - `musk-step-4-accelerate-cycle-time`
  - `musk-step-5-automate`
* `AGENTS.md` as the canonical, vendor-neutral source of truth (Linux Foundation Agentic AI Foundation standard).
* `CLAUDE.md` using Anthropic's `@AGENTS.md` import directive (no duplication).
* `GEMINI.md` mirror of `AGENTS.md` for Gemini CLI users (Gemini CLI does not read `AGENTS.md` natively as of 2026).
* `.cursor/rules/musk-algorithm.mdc` Cursor project rule for inline IDE use.
* `.claude-plugin/marketplace.json` and `.claude-plugin/plugin.json` for Claude Code plugin marketplace distribution.
* Order enforcement via per-step `Gate` sections with backward Obsidian-style wiki links.
* `EXAMPLES.md` with before/after illustrations across software, data, and AI work.
* Greenfield translation table for application building contexts.
* Scope tags in every skill `description` field (BROWNFIELD ONLY, BOTH, GREENFIELD ANALOG) so skills trigger correctly for their intended use case.

### Acknowledgments

Inspired by [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills). The Karpathy guidelines target function-level LLM coding behavior; this framework targets feature and system-level engineering decisions. The two are complementary.
