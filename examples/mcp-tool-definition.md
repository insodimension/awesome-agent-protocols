# MCP — Tool Definition

**Protocol:** [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
**Layer:** Tool and context
**Spec version:** 2025-06-18

A server advertises tools via `tools/list` and the client invokes them via `tools/call`. Both are JSON-RPC 2.0 messages over stdio or Streamable HTTP.

## Server advertises a tool

```jsonc
// → Client sends tools/list
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/list"
}

// ← Server responds with available tools
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "tools": [
      {
        "name": "get_weather",
        "description": "Get current weather for a location",
        "inputSchema": {
          "type": "object",
          "properties": {
            "location": {
              "type": "string",
              "description": "City name or coordinates"
            },
            "units": {
              "type": "string",
              "enum": ["celsius", "fahrenheit"],
              "default": "celsius"
            }
          },
          "required": ["location"]
        }
      }
    ]
  }
}
```

## Client calls the tool

```jsonc
// → Client invokes the tool
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "get_weather",
    "arguments": {
      "location": "San Francisco",
      "units": "fahrenheit"
    }
  }
}

// ← Server returns the result
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Current weather in San Francisco: 68°F, partly cloudy, humidity 72%"
      }
    ]
  }
}
```

## Key points

- Transport: stdio (newline-delimited JSON-RPC) or Streamable HTTP (`POST` with optional SSE upgrade)
- Tool schemas use standard JSON Schema for `inputSchema`
- Results return an array of typed `content` blocks (text, image, resource)
- Servers can declare `annotations.readOnlyHint`, `destructiveHint`, etc. for tool safety metadata
