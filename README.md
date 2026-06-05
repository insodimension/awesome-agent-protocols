# Awesome Agent Protocols [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of protocols and standards for AI agents.

🔌 wire protocol · 📐 spec / standard · 📄 format / convention · ⚠️ deprecated or merged

## Contents

- [Foundations](#foundations)
- [Tool and context](#tool-and-context)
- [Agent to agent](#agent-to-agent)
- [Agent to UI](#agent-to-ui)
- [Agent to editor](#agent-to-editor)
- [Serving and runtime](#serving-and-runtime)
- [Identity and discovery](#identity-and-discovery)
- [Agentic web](#agentic-web)
- [Payments and commerce](#payments-and-commerce)
- [Formats and conventions](#formats-and-conventions)
- [Governance](#governance)
- [Surveys](#surveys)
- [Examples](#examples)

## Foundations

_The editor-tooling protocols that predate agents. Same idea — decouple client from provider over JSON-RPC, collapse M×N to M+N._

- [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/) 🔌 - JSON-RPC standard for editor↔language-intelligence communication. The original M+N protocol; everything below copies this pattern. `protocol`
- [Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol/) 🔌 - JSON-RPC for editor↔debugger, through an intermediary adapter. `protocol`
- [Build Server Protocol (BSP)](https://build-server-protocol.github.io/) 🔌 - Scala Center / JetBrains JSON-RPC for IDE↔build-tool (targets, compile, test, run). `protocol`

## Tool and context

_How an agent calls tools, reads data, and gets context._

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io) 🔌 - Anthropic. JSON-RPC 2.0 over stdio or Streamable HTTP. Connects LLM hosts to tools, resources, and prompts. Now under the Linux Foundation. `protocol`
  - [Tools](https://modelcontextprotocol.io/specification/2025-11-25/server/tools) 🔌 - `tools/list` + `tools/call`. JSON-Schema-typed callable functions. `protocol`
  - [Resources](https://modelcontextprotocol.io/specification/2025-06-18/server/resources) 🔌 - URI-addressed readable context (files, rows, blobs) with subscriptions. `protocol`
  - [Prompts](https://modelcontextprotocol.io/specification/2025-06-18/server/prompts) 🔌 - Server-defined parameterized prompt templates, surfaced as slash commands. `protocol`
  - [Sampling](https://modelcontextprotocol.io/specification/2025-11-25/client/sampling) 🔌 - Server asks the client for an LLM completion. Supports tool use + `toolChoice`. `protocol`
  - [Elicitation](https://modelcontextprotocol.io/specification/draft/client/elicitation) 🔌 - Server pauses to ask the user for input (forms) or hand off to the browser (OAuth, payments). `protocol`
  - [Roots](https://modelcontextprotocol.io/specification/2025-11-25/client/roots) 🔌 - Client declares which `file://` paths servers may touch. `protocol`
  - [Apps (SEP-1865)](https://github.com/modelcontextprotocol/ext-apps) 🔌 - Servers ship interactive HTML UI as `ui://` resources, rendered in a sandboxed iframe. `protocol`
  - [Registry](https://github.com/modelcontextprotocol/registry) 📐 - Public server catalog + REST API. Standardized `server.json` publish format. `spec/standard`
- [OpenAI Apps SDK](https://developers.openai.com/apps-sdk) 🔌 - ChatGPT app = server logic + embedded widget. Converging with the MCP Apps extension. `protocol`
- [agents.json](https://github.com/wild-card-ai/agents-json) 📐 - Wildcard AI. Layered on OpenAPI — adds flows, links, and auth so agents can drive REST APIs. `spec/standard`
- [NLWeb](https://github.com/microsoft/NLWeb) 🔌 - Microsoft. Turns a website into a conversational, agent-queryable endpoint. `protocol`
- [Eclipse LMOS](https://eclipse.dev/lmos/docs/lmos_protocol/introduction/) 🔌 - Deutsche Telekom / Eclipse Foundation. W3C Web of Things-based agent + tool discovery. `protocol`
- [SLOP](https://github.com/agnt-gg/slop) 🔌 - Six REST endpoints (chat, tools, memory, resources, pay, info). Deliberately minimal. `protocol`
- [Arazzo](https://spec.openapis.org/arazzo/latest.html) 📐 - OpenAPI Initiative. YAML/JSON format for deterministic multi-step API workflows. `spec/standard`

## Agent to agent

_Cross-vendor, cross-framework agent collaboration._

- [Agent2Agent (A2A)](https://a2a-protocol.org) 🔌 - Google / Linux Foundation. JSON-RPC 2.0 + SSE + Agent Card discovery. Stateful async task delegation between peer agents. `protocol`
- [Agent Communication Protocol (IBM ACP)](https://agentcommunicationprotocol.dev) 🔌 ⚠️ - IBM / BeeAI. REST-native agent messaging. Merged into A2A (Aug 2025). `protocol`
- [Agent Network Protocol (ANP)](https://agent-network-protocol.com) 🔌 - Decentralized P2P. W3C `did:wba` authentication, JSON-LD descriptions. `protocol`
- [Agora](https://agoraprotocol.org) 🔌 - Oxford. Agents speak natural language for rare messages, then negotiate self-generated protocol docs for frequent ones. No central registry. `protocol`
- [AITP](https://aitp.dev) 🔌 - NEAR AI. Cross-trust-boundary Threads with pluggable capabilities for payments, decisions, on-chain identity. `protocol`
- [Coral](https://docs.coralos.ai/) 🔌 - MCP-native coordination. @mention-addressed threads, on-chain micropayments. `protocol`
- [HCS-10 OpenConvAI](https://hol.org/docs/standards/hcs-10/) 🔌 - Agent messaging over Hedera Consensus Service topics. Tamper-proof, broker-less. `protocol`
- [ACNBP](https://arxiv.org/abs/2506.13590) 🔌 - Draft academic. Discovery → negotiation → binding handshake with PKI signatures. `protocol`
- [W3C AI Agent Protocol CG](https://www.w3.org/community/agentprotocol/) 📐 - W3C community group for decentralized agent identity and discovery. Ships ANP. `spec/standard`

## Agent to UI

_Backend → frontend streaming of agent state, tool calls, and thinking._

- [AG-UI](https://docs.ag-ui.com) 🔌 - CopilotKit. Typed JSON events over HTTP/SSE from any agentic backend to any frontend. 9k+ GitHub stars. `protocol`
- [A2UI](https://a2ui.org) 🔌 - Google. Declarative generative-UI: JSON-Lines surface + data-model updates against a client widget catalog. `protocol`
- [AI SDK UI Stream Protocol](https://ai-sdk.dev/docs/ai-sdk-ui/stream-protocol) 🔌 - Vercel. SSE of typed message parts (text, reasoning, tool, data) consumed by `useChat`. `protocol`

## Agent to editor

_The "LSP for agents" — editor/IDE ↔ coding agent._

- [Agent Client Protocol (ACP)](https://agentclientprotocol.com) 🔌 - Zed. JSON-RPC for editor↔agent. Adopted by Zed, JetBrains, VS Code, Kiro, Cline. Spoken by Gemini CLI, Claude, Codex. `protocol`
- [LSAP](https://github.com/lsp-client/LSAP) 🔌 - Turns LSP capabilities into high-level agent-native "cognitive" tools. `protocol`
- [Cody Agent Protocol](https://github.com/sourcegraph/cody-public-snapshot) 🔌 ⚠️ - Sourcegraph. Bidirectional JSON-RPC over stdio. Effectively frozen. `protocol`

## Serving and runtime

_How an agent is invoked as a service — runs, threads, state._

- [LangChain Agent Protocol](https://github.com/langchain-ai/agent-protocol) 📐 - Framework-agnostic REST/OpenAPI. Agents, Runs, Threads, Store endpoints. `spec/standard`
- [Agent Connect Protocol (ACP)](https://spec.acp.agntcy.org/) 📐 - AGNTCY / Cisco. OpenAPI spec for remote agent invocation, any framework. `spec/standard`

## Identity and discovery

_How agents are named, found, and trusted._

- [Agent Name Service (ANS)](https://genai.owasp.org/resource/agent-name-service-ans-for-secure-al-agent-discovery-v1-0/) 📐 - OWASP. DNS-inspired discovery with PKI identity. Protocol-agnostic adapter over A2A, MCP, ACP. `spec/standard`
- [Project NANDA](https://github.com/projnanda) 📐 - MIT Media Lab. Federated "DNS for agents" — human-friendly handles → signed pointers → verifiable AgentFacts. `spec/standard`
- [AGNTCY Directory](https://docs.agntcy.org/dir/overview/) 🔌 - Cisco. Distributed, content-addressed registry. Signed OASF records as OCI artifacts over libp2p DHT. `protocol`
- [OASF](https://docs.agntcy.org/oasf/overview/) 📐 - AGNTCY. OCI-based schema describing agent attributes, skills, capabilities. `spec/standard`
- [ERC-8004](https://eips.ethereum.org/EIPS/eip-8004) 📐 - On-chain agent identity, reputation, and validation via three Ethereum registry contracts. `spec/standard`
- [SPIFFE / SPIRE](https://spiffe.io) 📐 - CNCF. Short-lived cryptographic workload identities, no secrets at rest. Increasingly used for agent runtime identity. `spec/standard`
- [AgentDNS](https://www.ietf.org/archive/id/draft-liang-agentdns-00.html) 📐 ⚠️ - Expired IETF draft. DNS-inspired naming + semantic discovery. Small reference impl. `spec/standard`
- [DNS-AID](https://datatracker.ietf.org/doc/draft-mozleywilliams-dnsop-dnsaid/) 📐 - IETF DNSOP draft. Publish/discover/verify agents through standard DNS records. Linux Foundation ref stack. `spec/standard`

## Agentic web

_Agents on the web — page tools, crawl permissions._

- [WebMCP](https://webmcp.link) 🔌 - W3C (Google, Microsoft). `navigator.modelContext` browser API exposing in-page tools to agents. `protocol`
- [Web-Agent Protocol (WAP)](https://www.otatech.ai/wap) 📄 - Record browser interactions via extension, replay them for autonomous web agents. `format/convention`
- [TDMRep](https://www.w3.org/community/tdmrep/) 📐 - W3C. Machine-readable mining permissions in HTTP headers, `.well-known`, or HTML. `spec/standard`

## Payments and commerce

_How agents pay — authorization, checkout, stablecoins._

- [AP2](https://ap2-protocol.org) 🔌 - Google + 60 partners. A2A extension. Cryptographically signed Mandates for agent-led payments, cards + stablecoins. `protocol`
- [x402](https://github.com/x402-foundation/x402) 🔌 - Coinbase / Linux Foundation. HTTP 402 with stablecoin payment in a header. Pay-per-call APIs. `protocol`
- [Agentic Commerce Protocol (ACP)](https://www.agenticcommerce.dev) 🔌 - OpenAI + Stripe. Agent-led checkout with Shared Payment Tokens. Powers Instant Checkout in ChatGPT. `protocol`
- [Universal Commerce Protocol (UCP)](https://ucp.dev) 📐 - Google + Shopify (Etsy, Target, Walmart, Wayfair). Product discovery through checkout. `spec/standard`
- [Cloudflare Pay Per Crawl](https://developers.cloudflare.com/ai-crawl-control/features/pay-per-crawl/what-is-pay-per-crawl/) 🔌 - HTTP 402 micropayments for AI crawlers, per request. `protocol`

## Formats and conventions

_Files, not messages. Often called "protocols" — they're not._

- [AGENTS.md](https://agents.md) 📄 - Project-specific instructions for coding agents. 60k+ repos. Agentic AI Foundation (Linux Foundation). `format/convention`
- [Agent Skills (SKILL.md)](https://agentskills.io) 📄 - Anthropic. Reusable agent capability as YAML frontmatter + instructions + optional scripts. `format/convention`
- [llms.txt](https://llmstxt.org) 📄 - Markdown at site root listing LLM-readable content. Fetched by coding agents, not yet honored by major crawlers. `format/convention`
- [ai.txt](https://site.spawning.ai/spawning-ai-txt) 📄 - Usage permissions for AI (no-training, no-inference, allow-RAG). Sits next to `robots.txt`. `format/convention`
- [JSONL transcripts](https://jsonlines.org) 📄 - One JSON object per line. How Claude Code, Codex, and others persist sessions. A log format, not a wire protocol. `format/convention`

## Governance

_Who stewards the open agent stack._

- [Agentic AI Foundation](https://aaif.io) 📐 - Linux Foundation. Co-founded by Anthropic, Block, OpenAI. Homes MCP, AGENTS.md, Goose. `spec/standard`
- [Linux Foundation — A2A](https://www.linuxfoundation.org/projects) 📐 - Hosts A2A (+ absorbed IBM ACP), AP2, and related infra. `spec/standard`
- [OpenAPI Initiative](https://www.openapis.org) 📐 - OpenAPI + Arazzo. The schema layer agent protocols build on. `spec/standard`

## Surveys

- [A Survey of AI Agent Protocols](https://arxiv.org/abs/2504.16736) - Two-dimensional taxonomy (context-oriented vs inter-agent, general vs domain-specific). Covers ANP, Agora, AITP, Coral.
- [A Survey of Agent Interoperability Protocols](https://arxiv.org/abs/2505.02279) - Side-by-side of MCP, ACP, A2A, ANP across nine dimensions, with a phased adoption roadmap.

## Examples

Wire-message examples in [`examples/`](examples/): MCP tool calls, A2A Agent Cards, agents.json flows, LangChain Runs/Threads, x402 payments, AP2 mandates.

## Related

- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers#readme)
- [Awesome A2A](https://github.com/ai-boost/awesome-a2a#readme)
- [Awesome Agentic Payments](https://github.com/bitrefill/awesome-agentic-payments#readme)

## Skills

This repo ships a [Claude Code skill](skills/agent-protocols/) that uses the catalog during development — recommends protocols, pulls wire examples, checks compliance, surfaces contribution opportunities.

## Contributing

Read [contributing.md](contributing.md) first. By participating you agree to the [Code of Conduct](code-of-conduct.md).
