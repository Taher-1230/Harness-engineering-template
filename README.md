# AI Researcher Agent — Harness Engineering Template

A documentation-first harness engineering setup for a Python research agent. It combines the project's research workflow with a repeatable process for AI-assisted code changes: **inspect → plan → implement → verify → review → report**.

This template is based on the public structure and README of [AI-Researcher-Agent](https://github.com/ahmedmick/AI-Researcher-Agent). The repository is a CLI research agent with provider-specific tool-calling loops, Tavily search, page extraction, source tracking, citation matching, and Arabic/RTL terminal rendering.

> These Markdown files guide an AI coding agent; they do not enforce correctness on their own. Automated tests, static checks, CI, and repository permissions are the enforcement layer to add as the project matures.

## The five files

| File | Purpose |
|---|---|
| `README.md` | Project overview, setup context, and workflow |
| `AGENTS.md` | Shared instructions for repository-aware coding agents |
| `CLAUDE.md` | Claude Code-specific entry point that defers to shared rules |
| `INVARIANTS.md` | Behaviors and safety properties changes must preserve |
| `PLANS.md` | Task planning, acceptance criteria, validation evidence, and risks |

## Harness workflow

1. **Specify** the requested outcome as observable acceptance criteria.
2. **Inspect** relevant implementation files, configuration, call sites, and the current working tree.
3. **Check invariants** before changing code.
4. **Plan** non-trivial or cross-file work in `PLANS.md`.
5. **Implement** the smallest change that satisfies the request.
6. **Verify** with deterministic checks first; use live APIs only when needed.
7. **Review** the full diff, look for secrets and unrelated edits, and re-check invariants.
8. **Record and report** what changed, which commands ran, their outcomes, and anything left unverified.

## Current project context

The inspected project structure includes:

- `main.py` and `intro.py` — application entry and startup flow.
- `Gemini_web_search.py` — Gemini Interactions API agent loop.
- `Groq_web_search.py` — Groq chat-completions agent loop (the source README describes this path as in progress).
- `tools.py` — Tavily web search and HTTP page extraction.
- `arabic_parser.py` — terminal rendering helpers for Arabic/RTL output.
- `requirements.txt` — Python dependencies.

Treat source code as the authority for current behavior. Files can change, and the existence of a provider module does not imply that every path is fully integrated or production-ready.

## Configuration and running

The implementation reads configuration from environment variables / `.env`. Names observed in the code and existing README include:

- `TAVILY_API_KEY`
- `Gemini_API_KEY`
- `GROQ_API_KEY`
- `MODEL`
- `QUERY` (used by the Groq path)

Example placeholders only:

```dotenv
TAVILY_API_KEY=replace_me
Gemini_API_KEY=replace_me
GROQ_API_KEY=replace_me
MODEL=replace_with_supported_model_name
QUERY=replace_with_a_research_question
```

Never commit real credentials. Before executing the application, re-check the current entry point, provider selection flow, expected model identifiers, and required environment variables. Live runs may use API quota and require network access.

## Development and verification

Start with safe, local checks such as:

```bash
python -m compileall -q .
```

This checks syntax only; it does **not** prove correct runtime behavior. Inspect the test layout and dependency metadata before choosing additional commands. Prefer deterministic unit tests with mocked provider and network clients so routine checks do not depend on API keys, current search rankings, or external websites.

For changes to core behavior, add focused tests for relevant areas such as citation matching, source numbering, bounded tool loops, retry handling, URL validation, page extraction failures, and Arabic/RTL rendering.

## Design principles

- **Evidence over assertion:** test results and captured source metadata are stronger evidence than model confidence.
- **Small diffs:** avoid unrelated cleanup and broad rewrites.
- **Bounded execution:** agent loops and retries must have explicit limits.
- **Clear boundaries:** keep provider-specific API protocols separate when they differ.
- **Safe defaults:** avoid unnecessary network calls, destructive operations, and dependency additions.
- **Explicit uncertainty:** record missing credentials, unrun checks, and known limitations.
- **Human ownership:** don't silently make irreversible decisions.

## Important limitations

This is a **documentation-first harness template**, not a completed automated test harness. The files help agents follow a common process, but reliable enforcement requires automated tests, static analysis, CI gates, and appropriate repository access controls. Never report a check as passing unless it was actually executed and its output inspected.
