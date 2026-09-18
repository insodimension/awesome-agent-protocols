# Agent Plugins — Minimal Plugin

**Protocol:** [Agent Plugins](https://github.com/agentplugins/agent-plugins-spec)
**Layer:** Formats and conventions
**Spec version:** 1.0.0

A plugin is a directory with a required `plugin.json` manifest, an optional `skills/` folder of Agent Skills, and an optional `mcp.json` declaring MCP servers. Verbatim quick start from the spec README:

## Smallest useful plugin

```text
hello-plugin/
├── plugin.json
└── skills/
    └── greet/
        └── SKILL.md
```

In `plugin.json`:

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "hello-plugin"
}
```

In `skills/greet/SKILL.md`:

```markdown
---
name: greet
description: Greet the user and offer help.
---

Greet the user and offer help.
```

A client that supports skills can load the plugin by reading `plugin.json` and discovering `skills/greet/SKILL.md`. How the client exposes the skill to users or models is outside the Agent Plugins specification.

## Key points

- Packaging only: marketplaces, installation, permissions, sandboxing and trust stay with each client.
- Interoperability floor, not identical behavior — UX and execution policy are client-defined.
- Supported at 1.0 launch: ChatGPT and Codex apps, Cursor, GitHub Copilot (CLI, SDK, app), VS Code, Kiro; Google has announced it is joining as Core Maintainer (not yet reflected in MAINTAINERS.md).
- Spec stewardship is vendor-neutral (Amazon, Cursor, Microsoft, OpenAI, Vercel on the TSC); 1.1.0 is a working draft.
