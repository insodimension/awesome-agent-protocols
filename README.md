# Awesome Agent Protocols [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated, classified map of the protocols and standards that connect AI agents — to tools, to each other, to users, to editors, and to the open web.

These are not competitors. They sit at different layers of the agent stack: one defines how an agent talks to a *tool*, another how it talks to another *agent*, another how it talks to a *UI* or an *editor*, another how it *pays*. Most real systems speak several at once. This list groups them by that layer and rules, per entry, on what each thing actually *is*.

**Legend** — 🔌 wire protocol · 📐 spec / standard (data model or API contract) · 📄 format / convention (a file, not a protocol) · ⚠️ merged, deprecated, or legacy.

**The four "ACP"s** — the most overloaded acronym in the field. They are unrelated: **Agent Client Protocol** (Zed — editor↔agent), **Agent Communication Protocol** (IBM — agent↔agent, now merged into A2A), **Agent Connect Protocol** (AGNTCY — remote agent invocation), and **Agentic Commerce Protocol** (OpenAI + Stripe — checkout). Each is listed under its own layer below.

## Contents

- [Foundations and lineage](#foundations-and-lineage)
- [Tool and context](#tool-and-context)
- [Agent to agent](#agent-to-agent)
- [Agent to UI](#agent-to-ui)
- [Agent to editor and client](#agent-to-editor-and-client)
- [Agent serving and runtime](#agent-serving-and-runtime)
- [Identity, discovery and registries](#identity-discovery-and-registries)
- [Agentic web and browser](#agentic-web-and-browser)
- [Payments and commerce](#payments-and-commerce)
- [Formats and conventions](#formats-and-conventions)
- [Governance and standards bodies](#governance-and-standards-bodies)
- [Surveys and further reading](#surveys-and-further-reading)
- [Examples](#examples)

## Foundations and lineage

The editor-tooling protocols that predate the agent era and are the explicit template the agent protocols imitate — decouple a client from a capability provider over JSON-RPC, and `M×N` integrations collapse to `M+N`.

- [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/) 🔌 - Microsoft's 2016 JSON-RPC standard decoupling editors from language intelligence (completion, hover, go-to-definition, diagnostics); the architectural blueprint the agent ecosystem copies. `protocol`
- [Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol/) 🔌 - Microsoft's JSON wire protocol abstracting editor↔debugger communication through an intermediary adapter; the debugging-side sibling of the editor-protocol family. `protocol`
- [Build Server Protocol (BSP)](https://build-server-protocol.github.io/) 🔌 - Scala Center and JetBrains JSON-RPC standard for IDE↔build-tool communication (targets, sources, compile, test, run), modeled on its language-server cousin. `protocol`

## Tool and context

How an agent discovers and calls tools, reads data, and pulls in context from external systems.

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io) 🔌 - Anthropic's open client-server wire protocol (JSON-RPC 2.0 over stdio or Streamable HTTP) for connecting LLM hosts to external tools, resources, and prompts; the de-facto tool-integration standard, now stewarded by the Linux Foundation's Agentic AI Foundation. `protocol`
  - [Tools primitive](https://modelcontextprotocol.io/specification/2025-11-25/server/tools) 🔌 - Servers advertise JSON-Schema-typed callable functions (`tools/list`) that the model invokes (`tools/call`) — the tool-calling core. `protocol`
  - [Resources primitive](https://modelcontextprotocol.io/specification/2025-06-18/server/resources) 🔌 - Exposes readable, URI-addressed context (files, rows, blobs) with discovery, retrieval, templating, and change subscriptions. `protocol`
  - [Prompts primitive](https://modelcontextprotocol.io/specification/2025-06-18/server/prompts) 🔌 - Server-defined parameterized prompt templates that a host surfaces as reusable, user-invocable actions such as slash commands. `protocol`
  - [Sampling](https://modelcontextprotocol.io/specification/2025-11-25/client/sampling) 🔌 - A server-initiated LLM-completion request routed back through the client, now with tools and `toolChoice` so servers run their own supervised agentic loops. `protocol`
  - [Elicitation](https://modelcontextprotocol.io/specification/draft/client/elicitation) 🔌 - A server pauses mid-request to ask the user for structured input (form mode) or to hand a sensitive OAuth/payment flow to the browser (URL mode, SEP-1036). `protocol`
  - [Roots](https://modelcontextprotocol.io/specification/2025-11-25/client/roots) 🔌 - A client capability declaring which `file://` roots servers may operate within, kept fresh via a change notification. `protocol`
  - [Apps Extension (SEP-1865)](https://github.com/modelcontextprotocol/ext-apps) 🔌 - First official extension: servers predeclare interactive HTML UI as `ui://` resources that hosts render in a sandboxed iframe and that talk back over the same JSON-RPC wire. `protocol`
  - [Registry](https://github.com/modelcontextprotocol/registry) 📐 - Official open catalog plus REST API for discovering public servers, enforcing a standardized `server.json` publish format with namespace verification. `spec/standard`
- [OpenAI Apps SDK](https://developers.openai.com/apps-sdk) 🔌 - OpenAI's open SDK extending the tool protocol so a ChatGPT app defines both its server-side logic and an embedded interactive widget, now converging onto the shared Apps extension. `protocol`
- [agents.json](https://github.com/wild-card-ai/agents-json) 📐 - Wildcard AI specification layered on OpenAPI that adds the contracts (flows, links, auth) an agent needs to reliably drive existing REST APIs. `spec/standard`
- [NLWeb](https://github.com/microsoft/NLWeb) 🔌 - Microsoft project turning a website into a conversational, agent-queryable endpoint; every instance is also a server speaking the tool protocol. `protocol`
- [Eclipse LMOS Protocol](https://eclipse.dev/lmos/docs/lmos_protocol/introduction/) 🔌 - Deutsche Telekom and Eclipse Foundation vendor-neutral "Internet of Agents" protocol built on W3C Web of Things for describing, discovering, and connecting agents and tools. `protocol`
- [SLOP](https://github.com/agnt-gg/slop) 🔌 - A deliberately minimal "just REST" HTTP+JSON pattern giving agents six endpoints (chat, tools, memory, resources, pay, info) as a radically simpler counterpoint. `protocol`
- [Arazzo Specification](https://spec.openapis.org/arazzo/latest.html) 📐 - OpenAPI Initiative standard for describing deterministic multi-step API/tool workflows as a YAML/JSON document, increasingly aimed at safe agent execution. `spec/standard`

## Agent to agent

How independent agents — on different frameworks, from different vendors — discover each other, delegate tasks, and collaborate.

- [Agent2Agent (A2A)](https://a2a-protocol.org) 🔌 - Google-originated, Linux Foundation-governed protocol (JSON-RPC 2.0 over HTTP plus SSE, with Agent Card discovery) for peer agents to delegate stateful async tasks; the de-facto cross-vendor interop standard. `protocol`
- [Agent Communication Protocol (IBM ACP)](https://agentcommunicationprotocol.dev) 🔌 ⚠️ - IBM/BeeAI's REST-native agent messaging protocol; merged into A2A under the Linux Foundation in August 2025 and now maintained only as a migration path. `protocol`
- [Agent Network Protocol (ANP)](https://agent-network-protocol.com) 🔌 - Open, decentralized peer-to-peer protocol for agents to discover, authenticate (W3C `did:wba`), negotiate, and collaborate across the web via JSON-LD descriptions — "the HTTP of the agentic web." `protocol`
- [Agora](https://agoraprotocol.org) 🔌 - Oxford academic meta-protocol where agents speak natural language for rare messages and switch to self-generated, hash-addressed protocol documents for frequent ones, negotiating structure with no central registry. `protocol`
- [Agent Interaction and Transaction Protocol (AITP)](https://aitp.dev) 🔌 - NEAR AI protocol for agents to communicate and transact across trust boundaries over Threads (modeled on the Assistants/Threads API), with pluggable capabilities for payments, decisions, and on-chain identity. `protocol`
- [Coral Protocol](https://docs.coralos.ai/) 🔌 - MCP-native decentralized coordination layer where any-framework agents collaborate via @mention-addressed thread messages and can discover, hire, and pay each other with on-chain micropayments. `protocol`
- [HCS-10 OpenConvAI](https://hol.org/docs/standards/hcs-10/) 🔌 - On-chain wire standard for agents to discover and message each other through Hedera Consensus Service topics — tamper-proof, broker-less channels with a guarded registry. `protocol`
- [Agent Capability Negotiation and Binding Protocol (ACNBP)](https://arxiv.org/abs/2506.13590) 🔌 - Draft academic protocol for a secure discovery→pre-screening→negotiation→binding handshake between heterogeneous agents, anchored to a name service with PKI signatures. `protocol`
- [W3C AI Agent Protocol CG](https://www.w3.org/community/agentprotocol/) 📐 - W3C Community Group building open standards for decentralized agent identity, description, and discovery on the agentic web; its flagship deliverable is the Agent Network Protocol. `spec/standard`

## Agent to UI

How an agentic backend streams its thinking, tool calls, and state to a user-facing frontend.

- [AG-UI](https://docs.ag-ui.com) 🔌 - CopilotKit's open, event-based protocol streaming a single typed JSON event sequence over HTTP/SSE from an agentic backend to any frontend; the agent↔UI lane, with broad framework adoption. `protocol`
- [A2UI](https://a2ui.org) 🔌 - Google's open, framework-agnostic declarative generative-UI protocol where agents stream JSON-Lines surface and data-model updates against a client-side widget catalog — data, not executable code. `protocol`
- [AI SDK UI Message Stream Protocol](https://ai-sdk.dev/docs/ai-sdk-ui/stream-protocol) 🔌 - Vercel's SSE wire protocol of typed JSON message parts (text, reasoning, tool, data) that `useChat` consumes directly when a backend sets the stream header. `protocol`

## Agent to editor and client

The "LSP for agents" lane — how a code editor or IDE drives a coding agent over a wire protocol.

- [Agent Client Protocol (ACP)](https://agentclientprotocol.com) 🔌 - Zed Industries' open JSON-RPC protocol standardizing editor↔coding-agent communication; adopted by Zed, JetBrains, VS Code, Kiro, OpenCode, and Cline, and spoken by Gemini CLI, Claude, and Codex. `protocol`
- [Language Server Agent Protocol (LSAP)](https://github.com/lsp-client/LSAP) 🔌 - Open protocol that turns low-level language-server capabilities into high-level, agent-native "cognitive" tools, giving coding agents repository-scale intelligence. `protocol`
- [Sourcegraph Cody Agent Protocol](https://github.com/sourcegraph/cody-public-snapshot) 🔌 ⚠️ - Vendor-specific bidirectional JSON-RPC over stdio letting non-JS editors drive a spawned Cody process; an early editor-to-agent design now effectively frozen. `protocol`

## Agent serving and runtime

How an agent is invoked, configured, and run as a service — runs, threads, and state over an API.

- [LangChain Agent Protocol](https://github.com/langchain-ai/agent-protocol) 📐 - Framework-agnostic REST/OpenAPI for serving agents in production, standardizing Agents, Runs, Threads, and Store endpoints so any client or orchestrator can drive any compliant runtime. `spec/standard`
- [Agent Connect Protocol (ACP)](https://spec.acp.agntcy.org/) 📐 - AGNTCY's OpenAPI/REST specification defining a standard interface to remotely invoke and configure agents regardless of the framework they were built with. `spec/standard`

## Identity, discovery and registries

How agents are named, found, and trusted — the directory and identity layers above the wire protocols.

- [Agent Name Service (ANS)](https://genai.owasp.org/resource/agent-name-service-ans-for-secure-al-agent-discovery-v1-0/) 📐 - OWASP GenAI Security Project DNS-inspired framework for secure agent discovery using PKI-backed identity and a protocol-agnostic adapter layer over A2A, MCP, and ACP. `spec/standard`
- [Project NANDA](https://github.com/projnanda) 📐 - MIT Media Lab federated discovery index ("DNS for AI agents") resolving a human-friendly handle to a signed pointer that dereferences to cryptographically verifiable AgentFacts metadata. `spec/standard`
- [AGNTCY Agent Directory Service](https://docs.agntcy.org/dir/overview/) 🔌 - Cisco/AGNTCY distributed, content-addressed registry that discovers agents by capability, publishing signed OASF records as OCI artifacts indexed over a libp2p DHT. `protocol`
- [Open Agentic Schema Framework (OASF)](https://docs.agntcy.org/oasf/overview/) 📐 - AGNTCY's OCI-based extensible schema describing an agent's attributes, skills, and capabilities — the data model underpinning the agent directory. `spec/standard`
- [ERC-8004](https://eips.ethereum.org/EIPS/eip-8004) 📐 - Ethereum standard giving agents portable, censorship-resistant on-chain identity, reputation, and validation via three registry contracts; a trustless trust layer extending A2A. `spec/standard`
- [SPIFFE / SPIRE](https://spiffe.io) 📐 - CNCF workload-identity standard issuing short-lived cryptographic identities (SVIDs) with no secrets at rest, increasingly the way autonomous agents get verifiable runtime identity. `spec/standard`
- [AgentDNS](https://www.ietf.org/archive/id/draft-liang-agentdns-00.html) 📐 ⚠️ - DNS-inspired root naming plus semantic discovery and unified auth/billing for agent and tool services; an expired individual IETF draft with a small reference implementation. `spec/standard`
- [DNS-AID](https://datatracker.ietf.org/doc/draft-mozleywilliams-dnsop-dnsaid/) 📐 - IETF DNSOP draft (formerly BANDAID) plus a Linux Foundation reference stack that publishes, discovers, and verifies agents and servers through standard DNS records. `spec/standard`

## Agentic web and browser

How agents act on the web — exposing page tools to agents, and telling crawlers what they may do.

- [WebMCP](https://webmcp.link) 🔌 - W3C Web ML Community Group standard (Google and Microsoft) exposing in-page tools to agents through a `navigator.modelContext` browser API, shifting automation from DOM-scraping to semantic tool calls. `protocol`
- [Web-Agent Protocol (WAP)](https://www.otatech.ai/wap) 📄 - Open framework that records real browser interactions via an extension and replays them — exactly or goal-directed — for autonomous web agents, optionally exposing each task as a server. `format/convention`
- [TDMRep](https://www.w3.org/community/tdmrep/) 📐 - W3C Text and Data Mining Reservation Protocol embedding machine-readable mining permissions in HTTP headers, `.well-known`, or HTML — a high-integrity signal vendors are increasingly bound to respect. `spec/standard`

## Payments and commerce

How agents transact — authorizing payments and completing purchases on a user's behalf.

- [Agent Payments Protocol (AP2)](https://ap2-protocol.org) 🔌 - Google-led open protocol (an A2A extension, with 60+ partners) using cryptographically signed Mandates to prove user intent and authorize agent-led payments across cards and stablecoins. `protocol`
- [x402](https://github.com/x402-foundation/x402) 🔌 - Coinbase-originated, Linux Foundation-governed protocol reviving HTTP 402: a server answers with payment instructions and the client signs a stablecoin payment into a header, enabling pay-per-call APIs. `protocol`
- [Agentic Commerce Protocol (ACP)](https://www.agenticcommerce.dev) 🔌 - OpenAI and Stripe open standard for agent-led checkout (Instant Checkout in ChatGPT), introducing a Shared Payment Token so an app initiates payment without seeing card credentials. `protocol`
- [Universal Commerce Protocol (UCP)](https://ucp.dev) 📐 - Google and Shopify open standard (with Etsy, Wayfair, Target, Walmart) providing the building blocks of agentic commerce from product discovery to checkout across merchant backends. `spec/standard`
- [Cloudflare Pay Per Crawl](https://developers.cloudflare.com/ai-crawl-control/features/pay-per-crawl/what-is-pay-per-crawl/) 🔌 - HTTP 402 micropayment scheme letting content owners charge verified AI crawlers per request, layering price and charge headers on signed bot-auth requests. `protocol`

## Formats and conventions

Worth knowing, and frequently called "protocols" — but they are not. Each is a file format or documentation convention: data at rest, not messages on a wire. (This is the section that answers "is JSONL a protocol?" — no.)

- [AGENTS.md](https://agents.md) 📄 - OpenAI-originated, Agentic AI Foundation-governed markdown convention giving coding agents project-specific instructions; adopted by 60k+ repositories and most coding agents. `format/convention`
- [Agent Skills (SKILL.md)](https://agentskills.io) 📄 - Anthropic's open format packaging a reusable agent capability as a `SKILL.md` (YAML frontmatter plus instructions) and optional bundled scripts and assets, loaded on demand. `format/convention`
- [llms.txt](https://llmstxt.org) 📄 - A markdown file at a site root listing LLM-readable content with one-line descriptions; routinely fetched by coding agents, though not yet formally honored by the major crawlers. `format/convention`
- [ai.txt](https://site.spawning.ai/spawning-ai-txt) 📄 - A root-level permissions file declaring purpose-based usage rules for AI (no-training, no-inference, allow-RAG), sitting alongside `robots.txt` as an access-intent signal. `format/convention`
- [JSONL session transcripts](https://jsonlines.org) 📄 - The de-facto convention (Codex rollouts, Claude Code, and others) of persisting an agent session as JSON Lines — one metadata header plus one event per line; a durable, resumable log format, not a wire protocol. `format/convention`

## Governance and standards bodies

Who stewards the open agent stack.

- [Agentic AI Foundation (AAIF)](https://aaif.io) 📐 - Linux Foundation directed fund (co-founded by Anthropic, Block, OpenAI) and neutral home for the open agentic stack, anchoring the tool protocol, the goose agent, and the project-instructions convention. `spec/standard`
- [Linux Foundation — A2A Project](https://www.linuxfoundation.org/projects) 📐 - Neutral governance host for the agent-to-agent protocol (and the absorbed IBM communication protocol), the payment-rail protocol, and related agent infrastructure. `spec/standard`
- [OpenAPI Initiative](https://www.openapis.org) 📐 - Linux Foundation body behind OpenAPI and the workflow specification that agent tool-use and serving standards increasingly build upon. `spec/standard`

## Surveys and further reading

- [A Survey of AI Agent Protocols](https://arxiv.org/abs/2504.16736) 📐 - Academic survey proposing a two-dimensional taxonomy (context-oriented vs inter-agent, general vs domain-specific) covering ANP, Agora, AITP, Coral, and more. `spec/standard`
- [A Survey of Agent Interoperability Protocols](https://arxiv.org/abs/2505.02279) 📐 - Side-by-side analysis of the four load-bearing protocols (the tool, communication, agent-to-agent, and network protocols) across nine dimensions, with a phased adoption roadmap. `spec/standard`

## Examples

See the [examples/](examples/) directory for verbatim wire-message examples: MCP tool definitions, A2A Agent Cards, agents.json flows, LangChain Runs/Threads, x402 HTTP 402 payments, and AP2 mandates.

## Related

- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers#readme) - The canonical large directory of Model Context Protocol servers.
- [Awesome A2A](https://github.com/ai-boost/awesome-a2a#readme) - Agents, tools, servers, and clients for the agent-to-agent protocol.
- [Awesome Agentic Payments](https://github.com/bitrefill/awesome-agentic-payments#readme) - Protocols, specs, and SDKs across the agentic-commerce stack.

## Contributing

Contributions are welcome. Read the [contributing guidelines](contributing.md) first — note especially the four-way classification (🔌 / 📐 / 📄 / framework), which is the discipline this list exists to keep. By participating you agree to the [Code of Conduct](code-of-conduct.md).
