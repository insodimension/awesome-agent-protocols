# A2A — Agent Card

**Protocol:** [Agent2Agent (A2A)](https://a2a-protocol.org)
**Layer:** Agent to agent
**Spec version:** v1.0 (June 2025)

An Agent Card is a JSON document (served at `/.well-known/agent.json`) that describes an agent's identity, capabilities, and endpoint. Other agents discover it to decide whether and how to delegate tasks.

## Agent Card

```json
{
  "name": "TravelAgent",
  "description": "Plans travel itineraries, books flights and hotels",
  "url": "https://travel.example.com/a2a",
  "version": "1.0.0",
  "capabilities": {
    "streaming": true,
    "pushNotifications": true,
    "stateTransitionHistory": true
  },
  "authentication": {
    "schemes": ["Bearer"]
  },
  "defaultInputModes": ["text/plain", "application/json"],
  "defaultOutputModes": ["text/plain", "application/json"],
  "skills": [
    {
      "id": "plan-trip",
      "name": "Trip Planning",
      "description": "Creates a multi-day travel itinerary given destinations and dates",
      "tags": ["travel", "planning", "itinerary"],
      "examples": [
        "Plan a 5-day trip to Tokyo in March",
        "Find the cheapest flights from NYC to London next week"
      ]
    },
    {
      "id": "book-hotel",
      "name": "Hotel Booking",
      "description": "Searches and books hotels matching budget and preferences",
      "tags": ["travel", "hotels", "booking"]
    }
  ]
}
```

## Sending a task

```jsonc
// → Caller sends a task via JSON-RPC
{
  "jsonrpc": "2.0",
  "id": "task-001",
  "method": "tasks/send",
  "params": {
    "id": "trip-abc123",
    "message": {
      "role": "user",
      "parts": [
        {
          "type": "text",
          "text": "Plan a 3-day trip to Paris in September, budget $2000"
        }
      ]
    }
  }
}

// ← Agent responds with task status
{
  "jsonrpc": "2.0",
  "id": "task-001",
  "result": {
    "id": "trip-abc123",
    "status": {
      "state": "completed"
    },
    "artifacts": [
      {
        "name": "itinerary",
        "parts": [
          {
            "type": "text",
            "text": "Day 1: Arrive CDG, check in Hotel Le Marais ($150/night)..."
          }
        ]
      }
    ]
  }
}
```

## Key points

- Discovery: Agent Cards at `/.well-known/agent.json` (or via registry)
- Wire: JSON-RPC 2.0 over HTTPS, with SSE for streaming (`tasks/sendSubscribe`)
- Task states: `submitted` → `working` → `input-required` → `completed` / `failed` / `canceled`
- v1.0 removed the `kind` field from message parts (breaking change from draft)
- Governed by the Linux Foundation
