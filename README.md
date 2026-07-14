<div align="center">
  <a href="https://grid-lock.tech" style="display: inline-block; margin: 0; line-height: 0;">
    <img
      src="logo-white.png"
      alt="Gridlock"
      width="300"
      style="display: block; margin: 0; background:#0a0a0a; padding:18px 24px; border-radius:16px;"
    />
  </a>
  <h1 style="margin: 0; padding: 0; font-size: 5rem; font-weight: 700; letter-spacing: -0.04em; line-height: 1;">
    Gridlock
  </h1>
  <p>
    <strong>Decentralized AI inference on Robinhood Chain — with enforceable latency SLAs.</strong>
  </p>
  <p>
    OpenAI-compatible API · distributed GPU workers · automatic SLA penalties · prepaid ETH credits · EVM wallet auth
  </p>

  <p>
    <a href="https://grid-lock.tech"><img src="https://img.shields.io/badge/Web-grid--lock.tech-000?style=for-the-badge" alt="Website" /></a>
    <a href="https://api.grid-lock.tech"><img src="https://img.shields.io/badge/API-api.grid--lock.tech-111?style=for-the-badge" alt="API" /></a>
    <a href="https://www.npmjs.com/package/@gridlock-compute/native-worker"><img src="https://img.shields.io/badge/npm-@gridlock--compute%2Fnative--worker-CB3837?style=for-the-badge&logo=npm&logoColor=white" alt="npm" /></a>
    <a href="https://github.com/Gridlockcompute"><img src="https://img.shields.io/badge/GitHub-Gridlockcompute-24292F?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" /></a>
  </p>
</div>

---

## What Is Gridlock?

Gridlock is a decentralized inference network for latency-sensitive AI. Customers send **OpenAI-compatible** requests with a speed tier; desktop, CLI, and browser workers run inference locally; the router tracks **time-to-first-token**, throughput, and SLA outcomes.

If a worker is too slow, **penalty credits are added to the customer's billing balance automatically** — no tickets, no disputes. The same penalty is deducted from the worker's **pending earnings ledger** (no bond escrow required).

**Settlement runs on [Robinhood Chain](https://robinhood.com/chain/)** (EVM). Wallets connect via **SIWE**; customers pre-fund **native ETH** for off-chain credits; workers set an **earnings wallet** and withdraw earned ETH. Optional on-chain contracts (staking, fees, governance) live in [`contracts`](https://github.com/Gridlockcompute/contracts).

## Core Repositories

| Repository | Purpose | Stack |
|------------|---------|-------|
| [`router`](https://github.com/Gridlockcompute/router) | (PRIVATE) OpenAI-compatible API, worker registry, WebSocket job hub, ETH billing, SIWE auth, Redis cache, Supabase persistence | Node.js, Hono, TypeScript, viem |
| [`contracts`](https://github.com/Gridlockcompute/contracts) | EVM programs — fee collector, grid staking, governance, attestation (optional on-chain hooks) | Solidity, Foundry |
| [`worker-desktop`](https://github.com/Gridlockcompute/worker-desktop) | Cross-platform desktop worker for operators contributing local compute | Electron, TypeScript, Ollama |
| [`native-worker`](https://github.com/Gridlockcompute/native-worker) | Headless CLI worker for servers and GPU boxes — published on npm as `@gridlock-compute/native-worker` | Node.js, TypeScript, WebSocket, Ollama/vLLM |

## Architecture

```text
Customers / Agents
      |
      |  OpenAI-compatible HTTPS API  (wallet SIWE + API keys)
      v
Gridlock Router  <-------------------->  Worker Clients
 Hono API                               Desktop / native CLI / Browser (WebLLM)
 Redis KV cache                         Ollama / vLLM inference
 Supabase state                         TTFT + TPOT reporting
      |
      |  optional on-chain settlement (Robinhood Chain EVM)
      v
FeeCollector · GridStaking · Governance · Attestation
```

## Network Capabilities

- **OpenAI-compatible API** at [`https://api.grid-lock.tech`](https://api.grid-lock.tech)
- **Latency SLAs** — batch, standard, realtime, and confidential tiers
- **Automatic SLA penalties** — slow responses credit the customer balance; worker penalty deducted from pending earnings
- **Worker marketplace** — desktop app, npm CLI (`gridlock-native-worker`), and in-browser WebLLM worker
- **WebSocket dispatch** for live job assignment (one active connection per wallet)
- **Prefill / decode roles** for disaggregated inference routing
- **KV-cache warm path** via Redis-backed prefix routing
- **ETH billing** — deposit native ETH → off-chain credits; workers withdraw earned ETH to `earnings_wallet`
- **EVM wallet auth** — SIWE sign-in, wallet-owned API keys, session tokens
- **Grid staking** — optional on-chain stake for fee-share boosts
- **TEE-ready confidential tier** for privacy-sensitive inference
- **x402** payment rail support on Robinhood Chain when credits are insufficient

## Robinhood Chain

| Network | Chain ID | RPC |
|---------|----------|-----|
| Mainnet | `4663` | `https://rpc.mainnet.chain.robinhood.com` |
| Testnet | `46630` | `https://rpc.testnet.chain.robinhood.com` |

Explorer: [mainnet](https://robinhoodchain.blockscout.com) · [testnet](https://explorer.testnet.chain.robinhood.com)

## Mainnet Contracts

Deployed on Robinhood Chain mainnet (chain `4663`). All contracts verified on [Blockscout](https://robinhoodchain.blockscout.com).

| Contract | Address |
|----------|---------|
| GRID token | [`0x62537c09a874cfe886e052d5afcd28356a98e549`](https://robinhoodchain.blockscout.com/address/0x62537c09a874cfe886e052d5afcd28356a98e549) |
| GridlockRegistry | [`0xC3F9E16d21F88DC5a7d89317EEC0e1c62206E1Cb`](https://robinhoodchain.blockscout.com/address/0xC3F9E16d21F88DC5a7d89317EEC0e1c62206E1Cb) |
| Attestation | [`0xD82dC78E2B2B079820E54513bEf4F6649c15b2dA`](https://robinhoodchain.blockscout.com/address/0xD82dC78E2B2B079820E54513bEf4F6649c15b2dA) |
| JobRouter | [`0xfEEa8b7b2B90CE699238c23a03f0607972150446`](https://robinhoodchain.blockscout.com/address/0xfEEa8b7b2B90CE699238c23a03f0607972150446) |
| FeeCollector | [`0x2604413db30ef67f28dc50e88daBfC89d6F1f4e0`](https://robinhoodchain.blockscout.com/address/0x2604413db30ef67f28dc50e88daBfC89d6F1f4e0) |
| GridStaking | [`0x32C074317C86318f5a41137E64AEf611324CA9A9`](https://robinhoodchain.blockscout.com/address/0x32C074317C86318f5a41137E64AEf611324CA9A9) |
| GridlockGovernance | [`0x124cA42770B8DDcD8e6a9D2EF5201c0b0165eE4E`](https://robinhoodchain.blockscout.com/address/0x124cA42770B8DDcD8e6a9D2EF5201c0b0165eE4E) |

Full deployment details: [`contracts`](https://github.com/Gridlockcompute/contracts)

## Tech Stack

<p>
  <strong>API and Web</strong><br />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/Hono-E36002?style=for-the-badge&logo=hono&logoColor=white" alt="Hono" />
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React" />
  <img src="https://img.shields.io/badge/viem-626262?style=for-the-badge" alt="viem" />
  <img src="https://img.shields.io/badge/wagmi-626262?style=for-the-badge" alt="wagmi" />
</p>

<p>
  <strong>Workers</strong><br />
  <img src="https://img.shields.io/badge/Electron-47848F?style=for-the-badge&logo=electron&logoColor=white" alt="Electron" />
  <img src="https://img.shields.io/badge/Ollama-000?style=for-the-badge&logo=ollama&logoColor=white" alt="Ollama" />
  <img src="https://img.shields.io/badge/vLLM-76B900?style=for-the-badge&logo=nvidia&logoColor=white" alt="vLLM" />
  <img src="https://img.shields.io/badge/WebSocket-010101?style=for-the-badge" alt="WebSocket" />
  <img src="https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white" alt="npm" />
</p>

<p>
  <strong>Chain and Infra</strong><br />
  <img src="https://img.shields.io/badge/Robinhood_Chain-00C805?style=for-the-badge" alt="Robinhood Chain" />
  <img src="https://img.shields.io/badge/EVM-3C3C3D?style=for-the-badge&logo=ethereum&logoColor=white" alt="EVM" />
  <img src="https://img.shields.io/badge/Foundry-000?style=for-the-badge" alt="Foundry" />
  <img src="https://img.shields.io/badge/Solidity-363636?style=for-the-badge&logo=solidity&logoColor=white" alt="Solidity" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white" alt="Redis" />
  <img src="https://img.shields.io/badge/Supabase-3FCF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
</p>

## Quick Start

### Call the API

```bash
curl https://api.grid-lock.tech/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_GRIDLOCK_API_KEY" \
  -d '{
    "model": "llama-3.1-8b-instant",
    "messages": [{ "role": "user", "content": "Explain Gridlock in one sentence." }],
    "gridlock": { "sla": "standard" }
  }'
```

### Connect a wallet

Open [grid-lock.tech](https://grid-lock.tech), connect an **EVM wallet on Robinhood Chain**, and use the dashboard for billing, API keys, worker earnings, and withdrawals.

### Run a worker

**Desktop app** (Windows, macOS, Linux):

```text
https://github.com/Gridlockcompute/worker-desktop/releases
```

**Native CLI** (servers & headless GPU boxes):

```bash
npm install -g @gridlock-compute/native-worker
gridlock-native-worker --wallet 0xYourEvmAddress
```

Source: [`Gridlockcompute/native-worker`](https://github.com/Gridlockcompute/native-worker)

**Browser worker** — launch from the **Worker** tab on [grid-lock.tech/dashboard](https://grid-lock.tech/dashboard) (WebLLM; one wallet connection at a time).

## For Developers

| Goal | Start here |
|------|------------|
| API routes, billing, worker routing, persistence | [`router`](https://github.com/Gridlockcompute/router) |
| EVM contracts and deployments | [`contracts`](https://github.com/Gridlockcompute/contracts) |
| Website, dashboard, and in-browser worker | [`frontend`](https://github.com/Gridlockcompute/frontend) |
| Packaged operator app | [`worker-desktop`](https://github.com/Gridlockcompute/worker-desktop) |
| Headless / server worker (npm) | [`native-worker`](https://github.com/Gridlockcompute/native-worker) · [`@gridlock-compute/native-worker`](https://www.npmjs.com/package/@gridlock-compute/native-worker) |

## Links

- Website: [grid-lock.tech](https://grid-lock.tech)
- Docs: [grid-lock.tech/docs](https://grid-lock.tech/docs)
- Production API: [api.grid-lock.tech](https://api.grid-lock.tech)
- GitHub: [Gridlockcompute](https://github.com/Gridlockcompute)
- Worker Desktop: [releases](https://github.com/Gridlockcompute/worker-desktop/releases)
- Native worker (npm): [@gridlock-compute/native-worker](https://www.npmjs.com/package/@gridlock-compute/native-worker)
- Robinhood Chain: [chain.robinhood.com](https://robinhood.com/chain/)

---

<div align="center">
  <strong>Gridlock</strong><br />
  AI that pays you back if it's too slow.
</div>
