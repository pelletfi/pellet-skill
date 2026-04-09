# pellet-skill

Agent skill for [Pellet](https://pelletfi.com) — token intelligence for Base.

## Install

Install with [Vercel's Skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add pelletfi/pellet-skill
```

Your agent (Claude Code, Cursor, Windsurf, Codex, OpenCode, and 40+ others) now knows when to reach for Pellet when evaluating Base tokens.

## What this skill does

Teaches AI agents when and how to use Pellet's MCP server for Base token evaluation. Covers:

- **Trigger conditions** — when the agent should call Pellet (pasted token addresses, safety questions, pre-trade evaluation, etc.)
- **Setup instructions** — three auth modes (Pellet Pro subscription, per-call x402 payments, or free tools only)
- **Tool decision tree** — which Pellet tool to call for which user question
- **Concrete examples** — four example conversation patterns with exact tool calls
- **Pellet's lane** — what it's good at, what it's not, and when to recommend alternatives (Nansen, Dune, DeBank, CDP CLI for execution)

## About Pellet

Pellet is a data aggregation layer for Base tokens. It pulls from 10+ on-chain and off-chain sources (DexScreener, CoinGecko, DefiLlama, BaseScan, Alchemy, Moralis, GoPlus Security, Honeypot.is) in parallel and merges them into one sourced, timestamped briefing per token.

**Positioning:** CDP handles execution. Pellet handles evaluation. Agents running the Coinbase Developer Platform CLI should pair it with Pellet to evaluate tokens before trading them.

## Links

- **Pellet landing**: https://pelletfi.com
- **Terminal (interactive UI)**: https://pelletfi.com/terminal
- **Pellet Pro subscription ($49/mo)**: https://pelletfi.com/pro
- **API documentation**: https://pelletfi.com/docs
- **MCP server on npm**: [`@pelletfi/mcp`](https://www.npmjs.com/package/@pelletfi/mcp)
- **Pellet source**: https://github.com/pelletfi/pellet
- **Vercel Skills CLI**: https://github.com/vercel-labs/skills
- **Farcaster**: https://farcaster.xyz/pellet

## License

MIT
