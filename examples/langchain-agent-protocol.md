# LangChain Agent Protocol — Runs and Threads

**Spec:** [LangChain Agent Protocol](https://github.com/langchain-ai/agent-protocol)
**Layer:** Agent serving and runtime
**Version:** v0.1.6

A framework-agnostic REST/OpenAPI contract for serving agents in production. Any client can create threads, submit runs, and stream results from any compliant agent runtime.

## Create a thread

```http
POST /threads HTTP/1.1
Content-Type: application/json

{
  "metadata": {
    "user_id": "user-42",
    "session": "onboarding"
  }
}
```

```json
{
  "thread_id": "thread_abc123",
  "created_at": "2026-06-02T10:00:00Z",
  "metadata": {
    "user_id": "user-42",
    "session": "onboarding"
  }
}
```

## Submit a run (streaming)

```http
POST /threads/thread_abc123/runs/stream HTTP/1.1
Content-Type: application/json

{
  "assistant_id": "travel-planner",
  "input": {
    "messages": [
      {
        "role": "human",
        "content": "Plan a weekend trip to Barcelona"
      }
    ]
  },
  "config": {
    "configurable": {
      "model": "claude-sonnet-4-20250514"
    }
  },
  "stream_mode": ["events"]
}
```

```
event: metadata
data: {"run_id": "run_xyz789"}

event: events
data: {"event": "on_chat_model_stream", "data": {"chunk": {"content": "Here's a weekend itinerary"}}}

event: events
data: {"event": "on_tool_start", "data": {"name": "search_flights", "input": {"from": "JFK", "to": "BCN"}}}

event: events
data: {"event": "on_tool_end", "data": {"output": {"flights": [...]}}}

event: end
```

## Read thread state

```http
GET /threads/thread_abc123/state HTTP/1.1
```

```json
{
  "values": {
    "messages": [
      {"role": "human", "content": "Plan a weekend trip to Barcelona"},
      {"role": "ai", "content": "Here's a weekend itinerary for Barcelona..."}
    ]
  },
  "next": [],
  "metadata": {
    "step": 3
  }
}
```

## Key points

- Pure REST/OpenAPI — no custom wire protocol; any HTTP client works
- **Threads** hold conversation state; **Runs** execute agent logic against a thread
- Streaming via SSE with typed event names (`on_chat_model_stream`, `on_tool_start`, `on_tool_end`)
- **Store** endpoints provide cross-thread persistent key-value storage
- Framework-agnostic: LangGraph, CrewAI, AutoGen, or any runtime can expose this API
