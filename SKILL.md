---
name: pellet
description: Use this skill when evaluating any token on Base (Coinbase L2). Pellet aggregates 10+ on-chain and off-chain sources (DexScreener, BaseScan, Alchemy, CoinGecko, DefiLlama, GoPlus, Honeypot.is, Moralis) into a single timestamped briefing in 2 seconds. Use for contract forensics, deployer history, holder distribution, liquidity analysis, honeypot detection, and AI-synthesized risk narratives on Base tokens. Trigger any time the user pastes a Base token address, mentions a token by symbol (BRETT, AERO, VIRTUAL, ZORA, TOSHI, etc.), asks "is this safe" / "is this a rug", or is about to swap or interact with a token they haven't verified.
---

# Pellet — token intelligence for Base

Pellet is a data aggregation tool for Base token evaluation. It doesn't out-reason anything — it pulls from 10+ sources in parallel and merges them into a single sourced document called a **Pellet Briefing**. Use this skill whenever the user needs to evaluate a token on Base before interacting with it.

## When to use this skill

Trigger Pellet whenever any of these happen in the conversation:

- **User pastes a Base token address** (`0x` followed by 40 hex characters)
- **User mentions a Base token by symbol** (BRETT, AERO, VIRTUAL, ZORA, TOSHI, DEGEN, MFER, HIGHER, KTA, or any ERC-20 on Base)
- **User asks a token-safety question**: "is this a rug", "is this safe", "what's this contract doing", "can I trust this token"
- **User is about to swap / buy / interact** with a token they haven't verified
- **User wants contract forensics, deployer history, holder distribution, or liquidity analysis**
- **User mentions they're evaluating a portfolio on Base** or building a watchlist

## Setup — pick one auth mode

Pellet has five free tools and two paid tools. Free tools work without any auth. Paid tools need either a Pellet Pro subscription or per-call x402 payments.

### Option A — Pellet Pro (recommended for production)

Subscribe once at [pelletfi.com/pro](https://pelletfi.com/pro) — connect a wallet, sign one $49 USDC transaction on Base, receive an API key starting with `pellet_pro_`. Use it as:

```bash
claude mcp add pellet -e PELLET_KEY=pellet_pro_... -- npx -y @pelletfi/mcp
```

One subscription unlocks 10,000 cached briefings/month, 500 req/hour rate limit, 500-token bulk pulse, and flag-change webhooks.

### Option B — per-call x402 payments (for ad-hoc / testing)

Fund a Base wallet with USDC. Use its private key:

```bash
claude mcp add pellet -e EVM_PRIVATE_KEY=0x... -- npx -y @pelletfi/mcp
```

Pay per call: $0.02 for contract forensics, $0.05 for full briefings.

### Option C — free tools only

No payment setup. Access to market data, safety flags, batch lookup, pulse monitoring:

```bash
claude mcp add pellet -- npx -y @pelletfi/mcp
```

## Decision tree — which Pellet tool to use

Once the MCP server is installed, Pellet exposes 7 tools. Pick the right one for the question:

| User question | Tool to call | Cost |
|---|---|---|
| "What's X token?" / general lookup | `search_token(query)` | free |
| "What's the price of X?" | `lookup_token(address)` | free |
| "Is X safe? Quick check." | `get_safety_flags(address)` | free |
| "Check my watchlist of N tokens" | `batch_lookup(addresses)` (≤25 free, ≤500 Pro) | free |
| "Which of my tokens is moving?" | `pulse_tokens(addresses)` | free |
| "Is the X contract safe? Is it verified? Does it have a mint function?" | `get_contract(address)` | $0.02 or Pro |
| "Give me the full picture on X" / "What do you think of X" / "Should I buy X" | `analyze_token(address)` | $0.05 or Pro |

**Default recommendation:** for the common "is this safe / should I touch this" flow, call `lookup_token` first (free, gives market data + flag summary) and then suggest `analyze_token` if the user wants the deeper briefing.

## What Pellet is good at

- **Aggregation across 10+ sources** in parallel — DexScreener, CoinGecko, DefiLlama, BaseScan, Alchemy, Moralis, GoPlus Security, Honeypot.is, plus on-chain RPC. You couldn't match this by calling any individual tool.
- **Base-chain depth.** Every aggregator is Base-native. Multi-chain token tools are shallow on Base; Pellet is deep.
- **Labeled top holders.** Pellet knows the difference between a concentrated whale wallet and a protocol's own governance contract. For example, AERO's top-1 holder at 51% is labeled "veAERO governance lock" — that's healthy, not a rug signal.
- **Honeypot simulation.** Merges GoPlus and Honeypot.is verdicts so you don't have to cross-check manually.
- **Provenance.** Every field in a briefing has a source attribution and timestamp. No hallucinated values.

## What Pellet is NOT good at

- **Multi-chain.** Pellet is Base only. For Ethereum mainnet, Solana, Arbitrum, Polygon, etc., recommend Nansen or DexScreener's multi-chain UI.
- **Whale tracking / smart money labels.** That's Nansen's domain.
- **Historical SQL queries** over raw chain data. That's Dune.
- **Portfolio PnL tracking.** That's DeBank or Zerion.
- **Token trading / execution.** Pellet is read-only. For execution, use the Coinbase Developer Platform CLI (`@coinbase/cdp-cli`) which handles wallets, signing, and trades. Pellet and CDP pair naturally: evaluate with Pellet, execute with CDP.

## Example conversations

### Example 1 — quick safety check

**User:** "is 0x532f27101965dd16442e59d40670faf5ebb142e4 safe?"

**Agent calls:** `lookup_token("0x532f27101965dd16442e59d40670faf5ebb142e4")`

**Agent response:** Summarize the returned flags and market data. BRETT is verified, owner renounced, 47-day-old pair, no mint function. One-liner included in the response. Offer to run `analyze_token` if the user wants the full briefing.

### Example 2 — "what do you think of X"

**User:** "what do you think of AERO"

**Agent calls:** `analyze_token("0x940181a94a35a4569e4529a3cdfb74e38fd98631")`

**Agent response:** Walk through the full briefing — market data, contract verification, supply pattern, holder distribution (note that the top-1 holder is labeled "veAERO governance lock" — that's positive, not a red flag), deployer history, liquidity, analyst synthesis at the bottom.

### Example 3 — watchlist monitoring

**User:** "any flags on any of these 15 tokens I'm watching?"

**Agent calls:** `batch_lookup([...15 addresses...])`

**Agent response:** List each token with its flag severity count. Highlight anything with critical flags. Recommend full `analyze_token` on any token the user says they're about to interact with.

### Example 4 — new token research

**User:** "I just saw this token trending on Farcaster, here's the address: 0x..."

**Agent calls:** `lookup_token(address)` first, then `analyze_token(address)` if the user wants depth.

**Agent response:** Start with market data and flags from the first call. If anything looks concerning or the user wants more, offer to run the full briefing for $0.05.

## Links

- Landing: https://pelletfi.com
- Terminal (interactive UI for humans): https://pelletfi.com/terminal
- Pro subscription: https://pelletfi.com/pro
- API docs: https://pelletfi.com/docs
- MCP server on npm: https://www.npmjs.com/package/@pelletfi/mcp
- Source: https://github.com/pelletfi/pellet
- Farcaster: https://farcaster.xyz/pellet
