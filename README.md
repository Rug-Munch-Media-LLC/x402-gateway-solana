# Rug Munch Intelligence — x402 Gateway (Solana)

## Overview

This Cloudflare Worker handles x402 USDC micropayments on Solana using the Coinbase x402 facilitator protocol. It verifies on-chain payments via Solana RPC and routes paid requests to the Rug Munch Intelligence backend.

**97 tools** are available via this gateway.

## Supported Chain

| Chain  | Symbol |
|--------|--------|
| Solana | SOL    |

## Payment Flow

```
Client                                Worker                              Backend
  |                                      |                                    |
  |  request + X-Payment-Authorization   |                                    |
  |------------------------------------->|                                    |
  |                                      |  verify via Solana RPC/payments    |
  |                                      |  (Coinbase x402 facilitator)        |
  |                                      |                                    |
  |                                      |  forward verified request          |
  |                                      |----------------------------------->|
  |                                      |                                    |
  |                                      |         result                     |
  |                                      |<-----------------------------------|
  |            result                     |                                    |
  |<-------------------------------------|                                    |
```

1. Client sends a request with the `X-Payment-Authorization` header.
2. The Worker verifies the payment via Solana RPC/payments using the Coinbase x402 facilitator protocol.
3. If verified, the Worker forwards the request to the backend.
4. The backend processes the request and returns the result through the Worker to the client.

## Trial Access

| Verification Level  | Free Requests |
|---------------------|---------------|
| Fingerprint only    | 1             |
| Wallet verified     | 3             |

After trial requests are consumed, a valid x402 USDC micropayment is required per request.

## Worker Endpoint

```
https://x402-sol.rugmuncher.workers.dev
```

## Payment Address (Solana)

```
Gix4P9AmwcZRGzr2hCEME5m2QAvY86dBfm8c7e7MpFzv
```

## Frontend

```
https://rugmunch.io
```

---

## RugCharts — Professional Charting & Analysis

Rug Munch Intelligence includes **RugCharts**, a next-generation charting and analytics platform:

- **Live trades streaming** — watch buys and sells hit the tape in real time
- **TA bot analysis** — automated technical analysis signals overlaid directly on charts
- **Professional charting** — candlesticks, order flow, volume profile, and more
- **Multi-chain coverage** — trade visualization across all supported chains
- **Scam detection built in** — every token is scored for rug-pull risk before it even renders

RugCharts delivers professional-grade charting with fraud detection baked in from the ground up. No more trading into a black hole — know what you're buying before you buy it.