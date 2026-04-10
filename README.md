# Leapfrog Skill — LEAP Pattern for AI Agents

> **L**LM **E**ngineered **A**pplication **P**attern

A Claude Code skill (and universal agent manifest) for the LEAP philosophy: **code is a commodity, guardrails are the product.**

## What is LEAP?

Instead of writing code, you design applications as:
- **Prompts** — declarative intent (what to build)
- **Schemas** — contracts between parts (the architecture)
- **Tests** — behavioral specifications (the definition of "correct")

An AI agent generates all application code on demand. The code is gitignored. The prompts, schemas, and tests are the source of truth.

## Install (Claude Code)

```bash
git clone https://github.com/safitudo/leapfrog-skill.git ~/.claude/plugins/leapfrog-skill
claude --plugin-dir ~/.claude/plugins/leapfrog-skill
```

Then in any LEAP project, say **"build the app, start from master.md, run the app when done"** and Claude builds everything, runs tests, and serves it.

## Use with other agents

The LEAP manifest is just markdown. Copy the contents of `skills/leap/SKILL.md` into your agent's system prompt, `agents.md`, or equivalent configuration. The pattern is agent-agnostic.

### Cursor

Add to `.cursorrules`:
```
[paste contents of skills/leap/SKILL.md]
```

### Windsurf

Add to `.windsurfrules`:
```
[paste contents of skills/leap/SKILL.md]
```

### Any agent

Inject into the system prompt or agent configuration file. The core contract:
1. Read `master.md`
2. Read schemas and parts
3. Generate code into `src/`
4. Run tests
5. Fix until green

## Example LEAP project

See [todolist-leap-framework](https://github.com/safitudo/todolist-leap-framework) — a todo app with zero application code.

## The Guardrails Hierarchy

1. **Tests** — most valuable. Human-authored. The specification of correctness.
2. **Schemas** — the architecture. Define how parts connect.
3. **Prompts** — least valuable. Disposable. Any prompt that passes the tests is equally valid.

## License

MIT
