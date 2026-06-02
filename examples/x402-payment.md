# x402 — HTTP 402 Payment Protocol

**Protocol:** [x402](https://github.com/x402-foundation/x402)
**Layer:** Payments and commerce
**Version:** v2 (June 2025, Linux Foundation)

Revives HTTP 402 (Payment Required): a server demands payment, the client signs a stablecoin transaction into a header, and the facilitator settles it on-chain. Pay-per-call APIs with no accounts.

## Request without payment

```http
GET /api/premium-data HTTP/1.1
Host: api.example.com
```

```http
HTTP/1.1 402 Payment Required
Content-Type: application/json

{
  "x402Version": 2,
  "accepts": [
    {
      "scheme": "exact",
      "network": "base",
      "maxAmountRequired": "100000",
      "resource": "https://api.example.com/api/premium-data",
      "description": "Access to premium data endpoint",
      "mimeType": "application/json",
      "payTo": "0x1234567890abcdef1234567890abcdef12345678",
      "maxTimeoutSeconds": 30,
      "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
      "extra": {
        "name": "USDC",
        "decimals": 6
      }
    }
  ]
}
```

## Request with payment

```http
GET /api/premium-data HTTP/1.1
Host: api.example.com
X-PAYMENT: eyJzY2hlbWUiOiJleGFjdCIsIm5ldHdvcmsiOiJiYXNlIiwicGF5bG9hZCI6eyJzaWduYXR1cmUiOiIweGFiYzEyMy4uLiIsImF1dGhvcml6YXRpb24iOnsiZnJvbSI6IjB4Y2xpZW50Li4uIiwidG8iOiIweDEyMzQuLi4iLCJ2YWx1ZSI6IjEwMDAwMCIsInZhbGlkQWZ0ZXIiOjE3MTk5MDAwMDAsInZhbGlkQmVmb3JlIjoxNzE5OTAwMDMwfX19
```

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-PAYMENT-RESPONSE: {"success":true,"transaction":"0xdeadbeef...","network":"base"}

{
  "data": "premium content here"
}
```

## Key points

- **v2 breaking change**: header renamed from `PAYMENT-SIGNATURE` to `X-PAYMENT`
- Payment is a Base64-encoded JSON payload containing a signed EIP-3009 authorization
- Settlement via a **facilitator** (intermediary that verifies + submits the on-chain tx)
- Currently supports USDC on Base, Arbitrum, and Ethereum mainnet
- No accounts, no API keys — just a wallet signature per request
- Governed by the Linux Foundation x402 Foundation
