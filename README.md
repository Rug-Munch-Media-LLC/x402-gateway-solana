# Rug Munch Intelligence — x402 Gateway (Solana)

## What This Is

This Cloudflare Worker is the **x402 payment gateway** for Rug Munch Intelligence on Solana. It sits between clients and the backend, enforcing per-call USDC micropayments via the Coinbase x402 facilitator protocol.

This is infrastructure — not a standalone product. All 97 tools live in the [RMI backend](https://github.com/Rug-Munch-Media-LLC/rug-munch-intelligence-mcp). This worker just handles the money.

## Architecture

```
Client (MCP / HTTP / App)
  │
  ▼
x402 Gateway (this worker)  ◄── checks USDC payment or trial balance
  │
  ▼
RMI Backend (97 tools)      ◄── actual intelligence processing
```

- **MCP clients** (Claude Desktop, Cursor) connect via the [rug-munch-intelligence-mcp](https://github.com/Rug-Munch-Media-LLC/rug-munch-intelligence-mcp) package, which calls this gateway
- **HTTP clients** (curl, bots, apps) call this gateway directly at `POST /api/v1/x402-tools/{tool}`
- **Web app** at [rugmunch.io](https://rugmunch.io) calls through this gateway

Same backend, same tools, same payment — regardless of how you access it.

## Supported Chain

| Chain  | Symbol | Verification Method |
|--------|--------|-------------------|
| Solana | SOL    | Coinbase x402 facilitator protocol |

## Payment Flow

```
Client                              Gateway                             Backend
  │                                    │                                   │
  │  request + X-Payment-Authorization │                                   │
  │───────────────────────────────────►│                                   │
  │                                    │  verify via Solana RPC/payments    │
  │                                    │  (Coinbase x402 facilitator)       │
  │                                    │                                   │
  │                                    │  forward verified request          │
  │                                    │──────────────────────────────────►│
  │                                    │                                   │
  │                                    │           result                   │
  │                                    │◄──────────────────────────────────│
  │           result                    │                                   │
  │◄───────────────────────────────────│                                   │
```

1. Client sends a request with the `X-Payment-Authorization` header
2. Gateway verifies the payment via Solana RPC using the Coinbase x402 facilitator
3. If valid, forwards the request to the backend
4. Backend returns result through the gateway

## Trial Access

| Verification Level  | Free Requests per Tool |
|---------------------|----------------------|
| Fingerprint only    | 1                    |
| Wallet verified     | 3                    |

After trial requests are consumed, a valid x402 USDC micropayment is required per request.

## Endpoints

- **Gateway**: `https://x402-sol.rugmuncher.workers.dev`
- **Tools**: `POST /api/v1/x402-tools/{tool_name}`
- **Catalog**: `GET /api/v1/x402/tools-catalog`
- **Dashboard**: `GET /api/v1/x402/dashboard`
- **Discovery**: `GET /.well-known/x402`

## Payment Address (Solana)

```
Gix4P9AmwcZRGzr2hCEME5m2QAvY86dBfm8c7e7MpFzv
```

## Related

- [rug-munch-intelligence-mcp](https://github.com/Rug-Munch-Media-LLC/rug-munch-intelligence-mcp) — MCP client for AI agents (97 tools)
- [x402-gateway-base](https://github.com/Rug-Munch-Media-LLC/x402-gateway-base) — Base + EVM payment gateway
- [rugcharts](https://github.com/Rug-Munch-Media-LLC/rugcharts) — Professional charting & TA analysis
- [rugmunch.io](https://rugmunch.io) — Web app

## License

Proprietary — Copyright 2026 Rug Munch Media LLC. All rights reserved.