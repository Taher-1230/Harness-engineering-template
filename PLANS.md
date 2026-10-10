# Engineering Plans and Task Ledger

This file is the lightweight planning and verification ledger for AI-assisted engineering work. Keep active plans actionable and completed entries brief. Do not treat a planned step as complete until evidence supports it.

## How to use this file

For small changes, a concise checklist is enough. For non-trivial work, record the goal, scope, constraints, validation strategy, and risks before implementation. Update the plan as evidence changes the approach. Keep completed entries as a short audit trail; avoid accumulating speculative implementation detail.

## Active task template

### Task: <short descriptive title>

**Status:** Planned | In progress | Blocked | Complete  
**Goal:** <observable outcome>  
**Scope:** <files/modules included; explicit non-goals>  
**Invariants at risk:** <items from INVARIANTS.md>

**Plan**

- [ ] Inspect the existing implementation and relevant call sites.
- [ ] Define expected behavior and focused acceptance checks.
- [ ] Implement the smallest safe change.
- [ ] Add or update deterministic tests where practical.
- [ ] Run checks and inspect their outputs.
- [ ] Review the diff, verify invariants, and update documentation.

**Validation**

- Commands to run: <commands>
- Expected result: <what success means>
- Actual result: <fill only after running>

**Risks / Open questions**

- <known unknowns; add a resolution condition where useful>

**Completion notes**

- <files changed, evidence, and remaining limitations>

---

## Suggested harness-engineering loop

1. **Specify:** turn the request into observable acceptance criteria.
2. **Inspect:** read relevant code and record current behavior before modifying it.
3. **Plan:** define a small sequence of edits and verification strategy.
4. **Implement:** make the narrowest change that satisfies the criteria.
5. **Test:** run deterministic unit/static checks first; use live integrations only when necessary and authorized.
6. **Check invariants:** compare behavior and diff against `INVARIANTS.md`.
7. **Review:** inspect the complete patch for unrelated changes, secrets, error-path bugs, and documentation drift.
8. **Record:** update this plan with evidence and unresolved limitations.
9. **Report:** state what changed, what was tested, and what remains unverified.

## Current repository baseline

This ledger starts as a template, not a claim that a full test harness already exists. Verify the current repository before selecting commands. The project has a Python CLI and external integrations for LLM providers, Tavily search, HTTP retrieval, text extraction, and terminal rendering. Prefer mocked tests for normal development.

### Baseline checks to establish

- [ ] Inspect `requirements.txt` and determine the intended package installation command.
- [ ] Confirm the CLI entry point and provider-selection paths are runnable.
- [ ] Check whether an automated test suite exists; add a small suite for pure logic where useful.
- [ ] Establish a no-network syntax/compile check for Python modules.
- [ ] Add focused tests for citation matching, source numbering, tool dispatch, iteration limits, and fetch/extraction failure handling.
- [ ] Record pre-existing failures separately before making unrelated changes.

### Validation cautions

- Live model calls require provider credentials and can consume quota.
- Live search/page-fetch tests are unstable and should not be required for routine unit tests.
- Terminal rendering should be manually smoke-tested in supported terminal environments when modified.
- A syntax check alone does not establish end-to-end correctness.
