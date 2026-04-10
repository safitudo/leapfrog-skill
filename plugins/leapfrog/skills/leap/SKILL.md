---
name: leap
description: "LEAP: LLM Engineered Application Pattern. Use when the user says 'build the app', 'Read master.md', 'rewrite this project', or when you find a master.md in the project root. Generates all code from prompts, runs tests, serves the app. Can also convert existing codebases into LEAP format."
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

## Rewrite mode — converting existing projects to LEAP

When the user asks to "rewrite" or "convert" an existing codebase into LEAP format:

### Step 1: Analyze the existing project

- Read the source code and understand the architecture
- Identify the public API — every exported function, class, type, constant
- Map the natural component boundaries (modules, classes, packages)
- Read the existing test suite — understand what's already tested

### Step 2: Extract schemas

- Create `schemas/` with type definitions covering the full public API
- Every function signature, every data type, every interface — extract it
- These become the contracts. Be exhaustive. Missing a schema means missing a guardrail.

### Step 3: Port tests

- Copy or rewrite existing tests into `tests/`
- Tests must be behavioral — test what the code does, not how it's structured
- Remove any tests that depend on implementation details (private functions, internal state)
- Add edge case tests you notice are missing in the original
- **Tests are the most valuable artifact.** Spend the most time here.

### Step 4: Decompose into parts

- Create `parts/` with one folder per natural component
- Each part gets a `master.md` prompt describing what that component does
- Each part gets a `schema.ts` with its specific contract
- Parts should be independent — no part's prompt should reference another part's implementation
- Keep parts small enough that an AI can generate each one correctly in a single shot

### Step 5: Write master.md

- Create root `master.md` describing the project, its purpose, and how parts connect
- Include tech constraints, generation rules, and verification steps
- Reference the original project for context

### Step 6: Verify the rewrite

- Add `src/` to `.gitignore`
- Delete or move aside the original source code
- Generate fresh code from the prompts: read `master.md` and build
- Run all ported tests
- If tests fail — improve prompts, add missing schema details, iterate
- Compare behavior against the original project

### Rewrite rules

- **Port tests first, write prompts second.** Tests anchor correctness; prompts are iterable.
- **Don't simplify the API.** The rewrite must be a drop-in replacement. Same public interface.
- **Don't skip edge cases.** If the original handles it, your tests must cover it.
- **Schemas must be complete.** Every public type, every exported symbol.
- **Keep the original's test cases.** They encode years of bug discoveries. Each test exists because something broke once.

## For other AI agents

This skill is portable. The LEAP pattern works with any AI coding agent.
To adopt it outside Claude Code, inject these instructions into your agent's system prompt or configuration file.

The core contract is simple:
1. Read `master.md`
2. Read schemas and parts
3. Generate code
4. Run tests
5. Fix until green
