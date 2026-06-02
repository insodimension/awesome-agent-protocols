# AP2 — Agent Payments Protocol Mandate

**Protocol:** [Agent Payments Protocol (AP2)](https://ap2-protocol.org)
**Layer:** Payments and commerce
**Spec version:** v1.0 (2025)

An A2A extension for agent-led payments. A cryptographically signed **Mandate** proves user intent — the agent presents it to a payment processor who validates the signature before charging.

## Mandate creation

The user authorizes a payment scope (amount, merchant, duration) and the client signs it:

```json
{
  "mandate": {
    "id": "mandate-2026-abc123",
    "version": "1.0",
    "issuer": {
      "name": "User Wallet App",
      "id": "wallet:user-42"
    },
    "subject": {
      "name": "ShoppingAgent",
      "id": "agent:shopping-agent-xyz"
    },
    "scope": {
      "merchants": ["merchant:acme-store"],
      "maxTransactionAmount": {
        "amount": "500.00",
        "currency": "USD"
      },
      "maxTotalAmount": {
        "amount": "2000.00",
        "currency": "USD"
      },
      "allowedCategories": ["electronics", "books"],
      "validFrom": "2026-06-02T00:00:00Z",
      "validUntil": "2026-06-09T23:59:59Z"
    },
    "constraints": {
      "requireConfirmationAbove": {
        "amount": "100.00",
        "currency": "USD"
      },
      "maxTransactionsPerDay": 5
    }
  },
  "signature": {
    "algorithm": "ES256",
    "value": "eyJhbGciOiJFUzI1NiJ9...",
    "certificate": "https://wallet.example.com/.well-known/signing-cert.pem"
  }
}
```

## Agent uses the mandate to pay

```jsonc
// Agent → Payment Processor
{
  "jsonrpc": "2.0",
  "id": "pay-001",
  "method": "payments/execute",
  "params": {
    "mandateId": "mandate-2026-abc123",
    "transaction": {
      "merchant": "merchant:acme-store",
      "amount": {
        "amount": "49.99",
        "currency": "USD"
      },
      "description": "Wireless headphones — Sony WH-1000XM6",
      "category": "electronics"
    },
    "mandateSignature": "eyJhbGciOiJFUzI1NiJ9..."
  }
}

// ← Payment Processor validates mandate + executes
{
  "jsonrpc": "2.0",
  "id": "pay-001",
  "result": {
    "transactionId": "txn-789xyz",
    "status": "completed",
    "amount": {
      "amount": "49.99",
      "currency": "USD"
    },
    "remainingBudget": {
      "amount": "1950.01",
      "currency": "USD"
    }
  }
}
```

## Key points

- **Mandates** are the core primitive — user-signed authorization scopes, not per-transaction approvals
- Constraints: per-tx caps, daily limits, merchant allowlists, category restrictions, confirmation thresholds
- Built as an A2A extension — uses the same JSON-RPC transport and Agent Card discovery
- 60+ launch partners including Google, Visa, Mastercard, Stripe, PayPal
- Supports card rails and stablecoin rails (complements x402 for crypto-native paths)
- The signature is cryptographic proof of user intent — legally and technically binding
