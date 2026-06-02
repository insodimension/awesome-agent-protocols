# Protocol Decision Tree

How to pick the right AI agent protocol for what you're building. Start from the task, not the protocol.

## Primary Decision: What layer is your task on?

```
What are you building?
│
├─ Agent needs to call tools / read data / get context
│  └─ → MCP (Model Context Protocol)
│     ├─ Tool calling? → MCP tools primitive
│     ├─ Reading files/data? → MCP resources primitive
│     ├─ Reusable prompt templates? → MCP prompts primitive
│     ├─ Server needs LLM completion? → MCP sampling
│     ├─ Server needs user input mid-request? → MCP elicitation
│     ├─ Interactive UI in the chat? → MCP Apps Extension (SEP-1865)
│     └─ Making a REST API agent-ready? → agents.json (layered on OpenAPI)
│
├─ Agent talks to another agent (cross-vendor/cross-framework)
│  └─ → A2A (Agent2Agent Protocol)
│     ├─ Task delegation? → tasks/send + tasks/sendSubscribe
│     ├─ Agent discovery? → Agent Card at /.well-known/agent.json
│     ├─ Decentralized / DID-based? → ANP (Agent Network Protocol)
│     └─ On-chain channels? → HCS-10 OpenConvAI or Coral Protocol
│
├─ Agent backend streams to a frontend UI
│  └─ → AG-UI (CopilotKit) or A2UI (Google)
│     ├─ Event-based SSE stream? → AG-UI
│     ├─ Declarative generative UI? → A2UI (JSON-Lines surface updates)
│     └─ Vercel AI SDK stream? → AI SDK UI Message Stream Protocol
│
├─ Editor/IDE drives a coding agent
│  └─ → ACP (Zed — Agent Client Protocol)
│     ├─ Zed, JetBrains, VS Code, Kiro? → ACP (native support)
│     └─ LSP-style agent tools? → LSAP (Language Server Agent Protocol)
│
├─ Serving an agent as an API (runs, threads, state)
│  └─ → LangChain Agent Protocol (REST/OpenAPI)
│     ├─ Remote agent invocation? → Agent Connect Protocol (AGNTCY ACP)
│     └─ Framework-agnostic? → LangChain Agent Protocol v0.1.6+
│
├─ Agent identity, discovery, or registry
│  └─ Choose by trust model:
│     ├─ DNS-inspired + PKI? → ANS (OWASP)
│     ├─ Federated + DID? → Project NANDA
│     ├─ Content-addressed + DHT? → AGNTCY Directory Service
│     ├─ On-chain identity? → ERC-8004
│     └─ Workload identity (CNCF)? → SPIFFE/SPIRE
│
├─ Agent acts on the web / browser
│  └─ → WebMCP (W3C, navigator.modelContext API)
│     ├─ Record & replay browser actions? → Web-Agent Protocol (WAP)
│     └─ Mining/crawl permissions? → TDMRep (W3C)
│
├─ Agent makes payments
│  └─ Choose by payment rail:
│     ├─ Card + mandate-based? → AP2 (Google, A2A extension)
│     ├─ Stablecoin per-request? → x402 (HTTP 402, Coinbase/LF)
│     ├─ Checkout flow (Stripe)? → Agentic Commerce Protocol (OpenAI+Stripe)
│     └─ Full commerce (discovery→checkout)? → UCP (Google+Shopify)
│
└─ None of the above / file format / convention
   ├─ Project instructions for agents? → AGENTS.md
   ├─ Reusable agent capability? → SKILL.md (Agent Skills)
   ├─ LLM-readable site content? → llms.txt
   ├─ AI usage permissions? → ai.txt
   └─ Session persistence? → JSONL (format, not a protocol)
```

## The Four ACPs — Disambiguation

The acronym "ACP" is used by four unrelated protocols. Always disambiguate:

| Full Name | Short ID | Who | Layer | Status |
|---|---|---|---|---|
| Agent Client Protocol | ACP/Zed | Zed Industries | Editor ↔ agent | Active, growing |
| Agent Communication Protocol | ACP/IBM | IBM / BeeAI | Agent ↔ agent | Merged into A2A (Aug 2025) |
| Agent Connect Protocol | ACP/AGNTCY | Cisco / AGNTCY | Remote agent invocation | Active |
| Agentic Commerce Protocol | ACP/Commerce | OpenAI + Stripe | Checkout / payments | Active |

**Rule:** Never write just "ACP" — always qualify with the org or layer.

## Multi-Protocol Composition

Real systems speak several protocols at once. Common pairings:

| Scenario | Protocols | How they compose |
|---|---|---|
| Agent with tools that delegates to other agents | MCP + A2A | MCP for tool access, A2A for delegation |
| Agent backend streaming to React UI | MCP + AG-UI | MCP server-side, AG-UI for the frontend stream |
| Agent with payments | A2A + AP2 | A2A for task flow, AP2 extends it with mandates |
| Coding agent in an IDE | ACP + MCP | ACP for editor↔agent wire, MCP for the agent's tool access |
| Discoverable agent with identity | A2A + ANS | A2A Agent Card for capabilities, ANS for PKI identity |
| Agent-ready REST API | agents.json + MCP | agents.json for API contracts, optionally expose as MCP server |

## Edge Cases

| Situation | Don't use | Use instead | Why |
|---|---|---|---|
| Two agents in the same process | A2A | Direct function calls | A2A is for cross-vendor/cross-network |
| Agent reading a database | Custom REST adapter | MCP resources | MCP already handles URI-addressed data |
| Streaming raw SSE to a frontend | Custom event format | AG-UI typed events | AG-UI defines the exact event shapes |
| Agent needs to pay for an API call | Custom billing | x402 (if stablecoin) or AP2 (if card) | Don't reinvent payment authorization |
| Agent discovering other agents on LAN | mDNS/Bonjour hacks | A2A Agent Card + local discovery | Agent Card is the standard format |
