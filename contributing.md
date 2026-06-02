# Contributing

Contributions are welcome — this list only stays useful if it stays accurate and current. Please read these guidelines before opening a pull request.

By participating you agree to abide by our [Code of Conduct](code-of-conduct.md).

## What belongs here

This is a list of **protocols and standards for AI agents** — the wire contracts, data models, and conventions that let agents talk to tools, to each other, to users, to editors, and to the wider web.

Every entry is tagged with one of four **classifications**. Getting this right is the whole point of the list, so be rigorous:

| Tag | Means | Test |
|-----|-------|------|
| **`protocol`** | A true wire protocol: defines the actual messages exchanged between two or more parties. | Could two independent implementations interoperate purely from the spec? (e.g. MCP, A2A, ACP) |
| **`spec/standard`** | A data model, schema, or API contract — structures information but isn't itself a message-exchange protocol. | Is it a shape/contract rather than a conversation? (e.g. Agent Cards, OASF, agents.json) |
| **`format/convention`** | A file format or documentation convention. **Not a protocol.** | Is it a file you write/read, not messages on a wire? (e.g. `AGENTS.md`, `llms.txt`, JSONL transcripts) |
| **`framework`** | An SDK, library, or application. **Generally out of scope** — link the *standard* it implements instead. | Is it code you depend on rather than a spec you implement? (e.g. LangChain, CopilotKit) |

If something is a `framework`, it usually does **not** get its own entry — find the open standard underneath it and list that. Frameworks may be mentioned as *implementations* of a listed standard.

### Quality bar

- **Real and citable.** Link a primary source: an official spec site, a canonical GitHub repo, or a founding announcement. No blog-only "concepts."
- **Agent-relevant.** It must be about AI agents / LLM applications specifically (or be foundational lineage like LSP that the agent ecosystem directly builds on).
- **Maintained or historically significant.** Active specs, or ones that shaped the space (note merged/deprecated status honestly).
- **Accurately classified.** See the table above.

## Entry format

Add entries as a single list item, in this exact shape (enforced by `awesome-lint`):

```
- [Name](https://primary-source-url) - One-sentence description of what it is. `tag`
```

Rules (each is a lint check):

1. Single `-` marker, one space after it.
2. `[Name](URL)` — non-empty link text, valid URL.
3. ` - ` (space-hyphen-space) between the link and the description.
4. Description starts with a capital/Proper/`CONSTANT`/`camelCase` word and ends with `.`, `!`, `?`, or `…`.
5. Don't repeat the entry's own name inside its description, and don't use the word "Awesome".
6. Keep the entry on one line (no hard-wrapping), no trailing whitespace.
7. End with the classification as inline code: `` `protocol` ``, `` `spec/standard` ``, `` `format/convention` ``, or `` `framework` ``.

**Ordering:** within a section, keep entries **alphabetical by name**. Maintainers may promote especially load-bearing standards to the top of a section — match the surrounding order when in doubt.

Place the entry in the section that matches its **layer in the agent stack** (tool/context, agent-to-agent, agent-to-UI, agent-to-editor, serving/runtime, identity/discovery, agentic web, payments/commerce, or formats & conventions). If it spans layers, pick the primary one and cross-reference in the description.

## Process

1. **Search first** — make sure it isn't already listed (no duplicate links; the linter enforces this).
2. **One item per PR** is easiest to review.
3. **Run the linter locally** before submitting:
   ```
   npx awesome-lint
   ```
   The build must pass with zero errors.
4. Open the PR with a short rationale and your primary source link.

Thanks for helping keep the map of the agent ecosystem honest.
