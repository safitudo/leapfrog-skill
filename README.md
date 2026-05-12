# leap-skill

> Claude Code plugin for [LEAP](https://github.com/safitudo/leap) — LLM Engineered Application Pattern.

A philosophy where **code is a commodity, guardrails are the product**. Apps are built from prompts, schemas, and tests. An AI agent generates all the code.

## Install

> **Requires Claude Code CLI.** The VS Code extension does not yet support `/plugin` commands directly.
>
> **In VS Code:** open the integrated terminal (`Terminal → New Terminal`), run `claude`, then run the install commands in that session.

**Step 1:** Add the marketplace:
```
/plugin marketplace add safitudo/leap-skill
```

**Step 2:** Install the plugin:
```
/plugin install leapfrog-skill@leapfrog-skill
```

## Use

In any LEAP project, say:

> **"build the app, start from master.md, run the app when done"**

Claude reads the prompts, schemas, and tests, generates all code into `src/`, runs the test suite, and serves the app.

Or for converting an existing project to LEAP:

> **"convert this project to LEAP format"**

## Learn more

- **[github.com/safitudo/leap](https://github.com/safitudo/leap)** — philosophy, spec, manifesto
- **[semver-leap](https://github.com/safitudo/semver-leap)** — rewrite of npm's semver parser (5,632/5,632 tests passing)
- **[todolist-leap](https://github.com/safitudo/todolist-leap)** — the original proof of concept

## Use with other agents

The LEAP manifest is portable. Copy the contents of [`AGENTS.md`](https://github.com/safitudo/leap/blob/main/AGENTS.md) from the hub repo into any agent's config file (`.cursorrules`, `.windsurfrules`, system prompt, etc.).

## License

MIT
