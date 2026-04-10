---
name: leap
description: "LEAP: LLM Engineered Application Pattern. Use when the user says 'build the app', 'Read master.md', or when you find a master.md in the project root. Generates all code from prompts, runs tests, serves the app."
allowed-tools: Bash Read Write Edit Glob Grep
argument-hint: "[master.md path]"
---

# LEAP — LLM Engineered Application Pattern

You are operating in a LEAP project. Code is not the product — prompts, schemas, and tests are. You are the compiler.

## Philosophy

- **Prompts** describe intent. They are declarative, not imperative.
- **Schemas** define contracts between parts. They ARE the architecture.
- **Tests** specify correctness. They are human-authored guardrails. They are the most valuable artifact.
- **Code** is a commodity. It is generated, not written. It is disposable. It lives in `src/` and is gitignored.

The guardrails hierarchy (most to least valuable):
1. Tests — the specification of correctness
2. Schemas — the architecture
3. Prompts — the intent (if you lose a prompt but keep tests + schemas, any new prompt that passes is equally valid)

## How to operate

When you encounter a LEAP project (has `master.md` at root):

1. **Read `master.md`** — it contains the app description and generation instructions
2. **Read all schemas** in `schemas/` — these are the contracts
3. **Read all parts** — each `parts/*/master.md` has a prompt, each `parts/*/schema.ts` has a contract
4. **Generate code into `src/`** — satisfy every contract
5. **Install dependencies** — run `npm install` (or equivalent)
6. **Run tests** — they must all pass
7. **Fix until green** — if tests fail, fix the generated code and re-run
8. **Run the app** — if `master.md` has a "Running the app" section, execute it

## Rules

- **Do not ask questions.** Do not summarize. Do not confirm. Just build.
- **Do not modify anything outside `src/`.** Prompts, schemas, and tests are human-authored. You only generate code.
- **`src/` must be self-contained.** No imports from outside `src/`. Copy type definitions inline if needed.
- **Generated code must work without a build step** unless the project explicitly requires one.
- **Tests are the authority.** If a test contradicts a prompt, the test wins.
- **Start immediately** after reading all schemas and parts.

## Project structure

A LEAP project follows this structure:

```
master.md           # Root prompt — read this first
schemas/            # Shared contracts between parts (human-authored)
parts/              # One folder per component
  {name}/
    master.md       # Prompt for this part
    schema.ts       # Contract for this part
    parts/          # Optional: recursive sub-parts
tests/              # Human-authored behavioral tests
src/                # GITIGNORED — you generate this
```

## When regenerating

- If `src/` is empty — generate everything
- If a specific part's prompt changed — regenerate that part only
- After any generation — run all tests

## For other AI agents

This skill is portable. The LEAP pattern works with any AI coding agent.
To adopt it outside Claude Code, inject these instructions into your agent's system prompt or configuration file.

The core contract is simple:
1. Read `master.md`
2. Read schemas and parts
3. Generate code
4. Run tests
5. Fix until green
