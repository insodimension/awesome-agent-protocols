# agents.json — API Contracts for Agents

**Spec:** [agents.json](https://github.com/wild-card-ai/agents-json)
**Layer:** Tool and context
**Version:** v0.1.0

`agents.json` extends OpenAPI with the contracts an agent needs to reliably drive REST APIs: multi-step flows, cross-endpoint links, and auth requirements — all in a machine-readable JSON file served alongside the API.

## agents.json file

```json
{
  "info": {
    "title": "Acme Store API",
    "version": "1.0.0",
    "description": "E-commerce API with agent-ready contracts"
  },
  "openapi_spec_url": "https://api.acme.com/openapi.json",
  "flows": [
    {
      "name": "purchase_product",
      "description": "Search for a product, add to cart, and checkout",
      "steps": [
        {
          "endpoint": "/products/search",
          "method": "GET",
          "parameters": [
            {
              "name": "query",
              "in": "query",
              "description": "Product search term",
              "required": true
            }
          ],
          "output": {
            "name": "search_results",
            "description": "Array of matching products"
          }
        },
        {
          "endpoint": "/cart/add",
          "method": "POST",
          "parameters": [
            {
              "name": "product_id",
              "in": "body",
              "description": "ID from search results",
              "required": true
            }
          ],
          "output": {
            "name": "cart",
            "description": "Updated cart with item"
          }
        },
        {
          "endpoint": "/checkout",
          "method": "POST",
          "parameters": [
            {
              "name": "cart_id",
              "in": "body",
              "description": "Cart ID to purchase",
              "required": true
            }
          ]
        }
      ]
    }
  ],
  "links": [
    {
      "source": "/products/search",
      "target": "/products/{id}",
      "description": "Get full details for a search result",
      "parameters": {
        "id": "$response.body#/results/0/id"
      }
    }
  ],
  "auth": {
    "type": "bearer",
    "token_url": "https://api.acme.com/oauth/token"
  }
}
```

## Key points

- Layered on OpenAPI — references an existing `openapi.json`, adds agent-specific contracts on top
- **Flows** define multi-step sequences (search → add to cart → checkout) with parameter threading
- **Links** express cross-endpoint navigation (like HATEOAS, but for agents)
- Served at the API root alongside `openapi.json`
- Agents parse flows to execute multi-step API tasks without hardcoded orchestration
