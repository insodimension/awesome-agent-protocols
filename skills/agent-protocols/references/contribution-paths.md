# Contribution Paths

How to contribute back to each protocol's spec or ecosystem. Use this when the user's work reveals a gap, pattern, or improvement that benefits the broader protocol community.

## By Protocol

### MCP (Model Context Protocol)

- **Spec repo:** [github.com/modelcontextprotocol/specification](https://github.com/modelcontextprotocol/specification)
- **Process:** Spec Enhancement Proposals (SEPs) — file an issue first, then a PR with the SEP document
- **SDK repos:** [python-sdk](https://github.com/modelcontextprotocol/python-sdk), [typescript-sdk](https://github.com/modelcontextprotocol/typescript-sdk)
- **Registry:** [github.com/modelcontextprotocol/registry](https://github.com/modelcontextprotocol/registry) — publish a server here
- **Apps Extension:** [github.com/modelcontextprotocol/ext-apps](https://github.com/modelcontextprotocol/ext-apps)
- **Governance:** Agentic AI Foundation (Linux Foundation)
- **Good first contributions:** New MCP server implementations, SDK bug fixes, SEP proposals for missing capabilities

### A2A (Agent2Agent)

- **Spec repo:** [github.com/google/A2A](https://github.com/google/A2A)
- **Process:** Issues for discussion, PRs for spec changes
- **Governance:** Linux Foundation AI & Data
- **Good first contributions:** Agent Card examples, reference implementations in new languages, interop test cases

### ACP/Zed (Agent Client Protocol)

- **Spec:** [zed.dev/acp](https://zed.dev/acp)
- **Registry:** [zed.dev/acp](https://zed.dev/acp) — register your agent
- **Process:** GitHub issues on the Zed repo for spec feedback
- **Good first contributions:** New agent implementations, editor client implementations

### AG-UI

- **Repo:** [github.com/ag-ui-protocol/ag-ui](https://github.com/ag-ui-protocol/ag-ui)
- **Process:** Issues and PRs
- **Good first contributions:** New event types, framework adapters, frontend component libraries

### AP2 (Agent Payments Protocol)

- **Spec:** [ap2-protocol.org](https://ap2-protocol.org)
- **Process:** Spec feedback via issues
- **Good first contributions:** Mandate validation libraries, payment processor adapters

### x402

- **Repo:** [github.com/x402-foundation/x402](https://github.com/x402-foundation/x402)
- **Governance:** Linux Foundation
- **Process:** Issues and PRs
- **Good first contributions:** New chain support, facilitator implementations

### agents.json

- **Repo:** [github.com/wild-card-ai/agents-json](https://github.com/wild-card-ai/agents-json)
- **Process:** Issues for spec discussion, PRs for changes
- **Good first contributions:** agents.json files for popular APIs, validation tooling

### ANP (Agent Network Protocol)

- **Repo:** [github.com/agent-network-protocol/AgentNetworkProtocol](https://github.com/agent-network-protocol/AgentNetworkProtocol)
- **Process:** Issues and PRs
- **Good first contributions:** DID method implementations, agent description examples

### AGENTS.md

- **Governance:** Agentic AI Foundation (Linux Foundation)
- **Process:** Community conventions — no formal spec repo (yet)
- **Good first contributions:** AGENTS.md files in your own projects, tooling for validation

### LangChain Agent Protocol

- **Repo:** [github.com/langchain-ai/agent-protocol](https://github.com/langchain-ai/agent-protocol)
- **Process:** Issues for discussion, PRs for spec/SDK
- **Good first contributions:** Client implementations in new languages, test suites

## Contribution Types

| Type | When | Format |
|---|---|---|
| **Bug report** | Spec says X but implementations do Y | GitHub issue with reproduction |
| **Spec clarification** | Ambiguity in the spec causes divergent implementations | Issue + proposed wording |
| **New capability** | Your use case isn't covered by any existing primitive | SEP/RFC (protocol-specific format) |
| **Reference implementation** | You built a compliant implementation worth sharing | PR to the SDK/examples repo |
| **Example/docs** | Better examples or docs would help adoption | PR to docs or our awesome-agent-protocols repo |
| **Interop test** | You verified two implementations work together | PR to test suite or blog post |

## Our Catalog

When any of the above changes the protocol landscape, also update the repo's `README.md`:
- New protocol → add entry in the correct section
- Protocol deprecated/merged → add `deprecated` tag
- Version bump with breaking changes → update description
- New example → add to `examples/` directory
