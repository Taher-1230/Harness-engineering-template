# Claude Code Project Guide

> This file is the Claude-specific entry point. The shared engineering contract lives in `AGENTS.md`, `INVARIANTS.md`, and `PLANS.md`. Follow all of them; this file does not override them.

## First steps

1. Read `AGENTS.md` completely.
2. Read `INVARIANTS.md` before implementation edits.
3. Read `README.md` and inspect the source files relevant to the task.
4. Review `git status --short` and preserve all pre-existing changes.
5. For work spanning multiple files or requiring investigation, record a plan in `PLANS.md` before coding.

## Execution contract

- Work from the repository's actual state; do not infer APIs or file behavior from names alone.
- Prefer a narrow patch addressing the root cause over a rewrite.
- Keep provider adapters separate where Groq and Gemini API protocols differ.
- Preserve tool-call ordering, source provenance, citation numbering, iteration bounds, and Arabic/RTL rendering.
- Avoid live model or web calls in routine tests. Mock external clients and use deterministic fixtures.
- Run appropriate checks, inspect their output, and disclose checks that could not run.
- Review the final diff for accidental edits, secrets, and unrelated changes.
- Update documentation and the plan when behavior, configuration, or user-facing steps change.

## When stuck

Investigate first. If progress is blocked by missing requirements, environment access, or credentials, make safe progress on parts that do not depend on the blocker and document the exact remaining requirement. Do not silently weaken safeguards or fabricate successful verification.

## Final response format

Summarize:

1. Change made and rationale
2. Files affected
3. Verification performed (commands and outcomes)
4. Remaining risks, known limitations, or user action needed
