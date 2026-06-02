---
name: agent-protocols
description: "Guide agent harness engineering toward the right AI agent protocol standards (MCP, A2A, ACP, AG-UI, AP2, x402, etc.) instead of reinventing. Analyzes what you're building, recommends protocols, pulls spec details and wire-message examples, checks compliance, and surfaces contribution opportunities. Triggers: '/agent-protocols', 'which protocol', 'should I use MCP or', 'what protocol for', 'agent protocol', 'protocol selection', 'MCP vs A2A', 'wire protocol for agents', 'agent interop', 'agent standard', 'extend MCP', 'contribute to A2A', 'protocol compliance', 'agent transport', 'agent discovery'. Self-improving: every run ends with a gotcha review."
---

# Agent Protocols

Guides agent/harness engineering toward using, extending, and contributing to established AI agent protocol standards rather than inventing ad-hoc solutions.

**Knowledge base:** This skill lives inside the [awesome-agent-protocols](../../README.md) repo, which catalogs 53+ protocols across 11 stack layers. The catalog is the source of truth; this skill makes it actionable during development.

This skill is **self-improving**. Every run must end with a gotcha review (see [Self-Improvement Protocol](#self-improvement-protocol)).

> **This skill is a checklist, not a suggestion.** Every mode below is a numbered checklist. You MUST tick each `[ ]` by writing its status + exit-criterion outcome in your response BEFORE moving to the next step. Skipping a step without documenting why is a skill violation.

---

## Pre-flight — ALWAYS (before any mode)

- [ ] **PF-1. Read [references/gotchas.md](references/gotchas.md).** Exit: you can state the 3 most recent dated entries in one sentence each (or confirm the log is empty).
- [ ] **PF-2. Read the protocol catalog.** Read the repo's `README.md` (two levels up from this skill) to load the current protocol taxonomy. Exit: you can name the 11 stack layers.
- [ ] **PF-3. Read the decision tree.** Read [references/decision-tree.md](references/decision-tree.md). Exit: you can map the 10 primary "you're building..." rows to their protocols.
- [ ] **PF-4. Understand the user's context.** What are they building? What layer of the stack? What's the integration target? Exit: one sentence stating the task and its stack layer.
- [ ] **PF-5. Pick the mode.** Exit: one of Recommend / Lookup / Audit / Contribute written down.

## Modes

| Mode | When to use | Output |
|---|---|---|
| **Recommend** | User is building something and needs to know which protocol(s) apply | Protocol recommendation with rationale + wire examples |
| **Lookup** | User wants spec details, wire messages, or examples for a specific protocol | Spec summary + verbatim wire examples from `examples/` |
| **Audit** | User has existing code and wants to check protocol compliance or find protocol opportunities | Compliance report + specific recommendations |
| **Contribute** | User built something protocol-adjacent and wants to contribute back | Contribution path: which spec, which repo, what format |

---

## Mode: Recommend

Analyze what the user is building and recommend the right protocol(s).

- [ ] **R-1. Classify the task by stack layer.** Map the user's work to one or more layers from the [decision tree](references/decision-tree.md). Exit: layer(s) named with confidence.
- [ ] **R-2. Identify candidate protocols.** For each layer, list the 1-3 most relevant protocols from the catalog. Exit: candidates listed with one-line rationale each.
- [ ] **R-3. Check for the ACP collision.** If any candidate is called "ACP", disambiguate which of the four ACPs applies (Zed editor, IBM/merged, AGNTCY remote, OpenAI+Stripe commerce). Exit: correct ACP identified or "no ACP involved".
- [ ] **R-4. Pull wire examples.** For the top recommendation, read the relevant example from the repo's `examples/` directory. Exit: verbatim wire message shown to user.
- [ ] **R-5. State the recommendation.** Format:
  ```
  **Protocol:** <name> (<layer>)
  **Why:** <1-2 sentences>
  **Spec:** <canonical URL>
  **Wire example:** <shown above>
  **Extend, don't reinvent:** <what the user should import/extend vs build from scratch>
  ```
  Exit: recommendation delivered in this format.
- [ ] **R-6. Flag multi-protocol scenarios.** If the task spans layers (e.g. tool access + agent discovery), state which protocol handles which part and how they compose. Exit: composition stated or "single-protocol task".
- [ ] **R-7. Pass the [Pre-Recommend Gate](#pre-recommend-gate).** Exit: every G-rule returns PASS.
- [ ] **R-8. Update gotchas.** Exit: any new learning appended to [references/gotchas.md](references/gotchas.md), or "nothing new" noted.

## Mode: Lookup

Pull spec details and wire-message examples for a specific protocol.

- [ ] **L-1. Identify the protocol.** Resolve the user's reference to an exact protocol name from the catalog. Handle ACP disambiguation if needed. Exit: exact protocol name + catalog layer.
- [ ] **L-2. Read the catalog entry.** Pull the protocol's entry from the repo's README. Exit: entry text shown.
- [ ] **L-3. Check for a wire example.** Read the repo's `examples/` directory for a matching example file. Exit: example shown, or "no example yet — candidate for contribution".
- [ ] **L-4. Pull authoritative spec details.** For the key facts the user needs, fetch from the protocol's canonical spec URL (listed in the catalog entry). Exit: spec details summarized with source link.
- [ ] **L-5. State version-specific nuances.** Flag any known version changes, breaking changes, or gotchas (e.g. x402 v1→v2 header rename, A2A v1.0 `kind` removal). Exit: nuances stated or "none known".
- [ ] **L-6. Update gotchas.** Exit: any new learning appended, or "nothing new" noted.

## Mode: Audit

Review existing code for protocol compliance or missed protocol opportunities.

- [ ] **A-1. Scope the audit.** What files/packages is the user asking about? Exit: file paths listed.
- [ ] **A-2. Read the code.** Examine the transport, driver, adapter, or client code. Exit: code read, purpose understood.
- [ ] **A-3. Map to protocols.** For each piece of functionality, identify which protocol it implements (or should implement). Exit: mapping table produced.
- [ ] **A-4. Check compliance.** For each mapped protocol, verify the implementation matches the spec:
  - Correct wire format (JSON-RPC 2.0, REST, SSE, etc.)
  - Correct message shapes (method names, parameter schemas)
  - Required capabilities advertised
  - Discovery mechanisms present (Agent Card, `.well-known`, etc.)
  Exit: compliance issues listed, or "compliant".
- [ ] **A-5. Identify protocol opportunities.** Are there ad-hoc solutions that could be replaced by a standard protocol? Exit: opportunities listed with specific protocol recommendations.
- [ ] **A-6. Pass the [Pre-Recommend Gate](#pre-recommend-gate).** Exit: every G-rule returns PASS.
- [ ] **A-7. Update gotchas.** Exit: any new learning appended, or "nothing new" noted.

## Mode: Contribute

Surface how the user's work could contribute back to protocol specs.

- [ ] **C-1. Identify the contribution type.** One of: bug report, spec clarification, new feature proposal (SEP/RFC), reference implementation, example/docs. Exit: type named.
- [ ] **C-2. Find the right repo.** Look up the protocol's canonical GitHub repo from the catalog. Exit: repo URL stated.
- [ ] **C-3. Check contribution guidelines.** Read the repo's CONTRIBUTING.md or equivalent. Exit: process summarized (issue first? RFC? SEP?).
- [ ] **C-4. Draft the contribution.** Help the user write the issue, PR description, or SEP draft. Exit: draft produced.
- [ ] **C-5. Cross-reference our catalog.** If this changes the protocol landscape, note what should be updated in the repo's README. Exit: catalog update noted or "no catalog change".
- [ ] **C-6. Update gotchas.** Exit: any new learning appended, or "nothing new" noted.

---

## Pre-Recommend Gate (MUST pass before delivering a protocol recommendation)

Every recommendation must pass every row below. Mark each `[ ]` with PASS / FAIL. A FAIL blocks — fix it first.

- [ ] **G0. No reinvention.** The recommendation does NOT suggest building something a protocol already provides. If the user's task maps to an existing protocol primitive, the answer is "use it", not "build your own".
- [ ] **G1. Correct protocol.** The recommended protocol actually operates at the stack layer the user's task lives on. Tool-calling tasks get MCP, not A2A. Agent-to-agent tasks get A2A, not MCP. UI streaming gets AG-UI, not raw SSE.
- [ ] **G2. ACP disambiguated.** If any "ACP" is mentioned, it's clear which of the four it is.
- [ ] **G3. Version-accurate.** Spec details reference the current version (not a draft or deprecated version) unless explicitly discussing history.
- [ ] **G4. Extend, don't duplicate.** If the user's codebase already has types/interfaces for a protocol (especially OMP/MCP types), the recommendation extends them rather than creating parallel types.
- [ ] **G5. Contribution path stated.** If the user's work reveals a gap in a protocol spec, the recommendation includes "this could be contributed as [SEP/RFC/issue] to [repo]".

---

## Protocol Quick Reference

The full catalog lives in the repo's [README.md](../../README.md). Here's the decision shortcut:

| You're building... | Primary protocol | Layer |
|---|---|---|
| Tool/function calling for an LLM | **MCP** (tools primitive) | Tool & context |
| Agent-to-agent task delegation | **A2A** (tasks/send) | Agent to agent |
| Agent backend → frontend streaming | **AG-UI** (SSE events) | Agent to UI |
| Editor/IDE ↔ coding agent | **ACP** (Zed — Agent Client Protocol) | Editor/client |
| Agent serving API (runs, threads) | **LangChain Agent Protocol** | Serving/runtime |
| Agent discovery / identity | **ANS** or **ANP** | Identity/discovery |
| Agent payments | **AP2** (mandates) or **x402** (HTTP 402) | Payments |
| Describing agent capabilities | **Agent Card** (A2A) or **OASF** (AGNTCY) | Discovery |
| Making REST APIs agent-ready | **agents.json** | Tool & context |
| Web page → agent tool exposure | **WebMCP** | Agentic web |

See [references/decision-tree.md](references/decision-tree.md) for the full decision tree with edge cases.

---

## Shared Infrastructure

- **Protocol decision tree:** [references/decision-tree.md](references/decision-tree.md)
- **Contribution paths:** [references/contribution-paths.md](references/contribution-paths.md)
- **Gotchas log:** [references/gotchas.md](references/gotchas.md)
- **Protocol catalog (source of truth):** [../../README.md](../../README.md)
- **Wire examples:** [../../examples/](../../examples/)

---

## Self-Improvement Protocol

Every run must end with a gotcha review. New learnings die silently if you don't log them.

**During the run:**
- Undocumented protocol behavior or spec discrepancy → capture symptom + resolution verbatim.
- Protocol version change not reflected in the catalog → note the actual current state.
- User pushback corrected a protocol recommendation → ALWAYS log this. Pushback is the highest-value gotcha source.
- New protocol discovered that's not in the catalog → note it for catalog addition.
- Composition pattern between protocols that wasn't obvious → capture the pattern.

**At end of run:**
1. **Promote the pattern** to its permanent home BEFORE appending the log entry:
   - Protocol selection pattern → [references/decision-tree.md](references/decision-tree.md)
   - Contribution process detail → [references/contribution-paths.md](references/contribution-paths.md)
   - New protocol for the catalog → repo's README.md
   - New hard rule → Pre-Recommend Gate above
2. **Then** append a 1-3 line entry to [references/gotchas.md](references/gotchas.md). Each entry must point to the promoted reference where the full fix lives.
3. **Never** silently let a new learning die. If you discovered it, log it.

**Gotcha entry format (1-3 lines):**
```markdown
### YYYY-MM-DD — <one-line title>
Symptom → cause → fix. See [<promoted-ref>#<anchor>](<promoted-ref>#<anchor>) for the full pattern.
Context: <what you were doing + protocol / layer / trigger>.
```

Use today's absolute date — never relative.

---

## Failure / Recovery

| Symptom | Cause / Fix |
|---|---|
| Recommended MCP for agent-to-agent task | Wrong layer — MCP is tool/context, A2A is agent-to-agent. Re-check [decision tree](references/decision-tree.md) |
| Said "ACP" without disambiguating | Four protocols share the acronym. Always specify which: Zed, IBM(merged), AGNTCY, or OpenAI+Stripe |
| Recommended a deprecated protocol | Check the catalog for `deprecated` tags. IBM ACP → merged into A2A. Cody Agent Protocol → frozen |
| Wire example doesn't match current spec | Spec may have updated. Fetch from the canonical spec URL, not just the examples dir |
| User says "we already have types for this" | Check the project codebase first. Extend existing types (G4), don't create parallels |
| Protocol not in the catalog | New protocol discovered — add to catalog via Contribute mode, then recommend |
| User building something that spans two layers | Multi-protocol composition — state which protocol handles which part (R-6) |

---

## Related

- **awesome-agent-protocols repo** — the catalog this skill is built on (two levels up)
- **deep-research skill** — use for investigating new/emerging protocols not yet in the catalog
- **skill-creator** — the guide that produced this skill
