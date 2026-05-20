<div align="center">
  <br/>
  <img src="https://img.shields.io/badge/status-production%20ready-22d3ee?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/solidity-0.8.26-10b981?style=flat-square" alt="Solidity"/>
  <img src="https://img.shields.io/badge/next.js-14-000000?style=flat-square" alt="Next.js"/>
  <img src="https://img.shields.io/badge/license-MIT-8b5cf6?style=flat-square" alt="License"/>
  <br/>
  <br/>
  <h1>GhostRoute Terminal</h1>
  <p>
    <a href="https://groute-rho.vercel.app/"><strong> Live Frontend</strong></a> &nbsp;|&nbsp;
    <a href="https://groute-2.onrender.com/api/system/health"><strong>⚡ API</strong></a>
  </p>
  <p><strong>Private Cross-Chain Liquidity Execution Terminal</strong></p>
  <p>Institutional-grade execution infrastructure for MEV-protected cross-chain routing,<br/>
  order fragmentation, route optimization, and on-chain settlement verification.</p>
  <br/>
  <pre align="center">Built for hedge funds - DAOs - market makers - treasury desks - liquidity providers - protocol operators</pre>
  <br/>
</div>

---

## Overview

GhostRoute Terminal is a unified institutional execution console for cross-chain token transfers. It replaces 10+ separate tools with one edge-to-edge terminal interface featuring private routing via Flashbots/private RPC, AI-powered route optimization via 0G Labs, order fragmentation across parallel routes, real-time settlement verification, and MEV protection.

### What It Solves

| Problem | GhostRoute Solution |
|---------|-------------------|
| MEV exposure (frontrunning, sandwich, backrunning) | Flashbots + privacy RPC integration, MEV guard |
| Slippage on large orders | Order fragmentation across 3-5 parallel routes |
| No execution privacy | Opaque order flow, stealth execution |
| Fragmented tooling | Single unified terminal (8 modules) |
| No settlement guarantees | On-chain proof verification with dispute resolution |
| No route intelligence | AI-powered path discovery via 0G Labs compute |

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | Next.js 14 (App Router), TypeScript, Tailwind CSS, AG Grid Enterprise, xterm.js, Recharts, Zustand, Lucide |
| **Backend** | Fastify 4, PostgreSQL 16 (Prisma ORM), Redis 7 (ioredis), BullMQ, WebSockets, Zod validation, ethers.js v6, Pino |
| **Contracts** | Solidity 0.8.26, Hardhat + Foundry, OpenZeppelin v5 |
| **AI/Infra** | 0G Labs Compute (solver), Storage (route data), DA (settlement proofs) |
| **Infrastructure** | Docker + Compose, Kubernetes, Coolify, Vercel, GitHub Actions |
| **Design** | Matte dark institutional palette, Bloomberg Terminal + TradingView Pro aesthetic |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    GhostRoute Terminal                       │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────────┐ │
│  │  Market   │  │  Route   │  │    AI    │  │   Command   │ │
│  │  Matrix   │  │Visualizer│  │  Solver  │  │  Terminal   │ │
│  ├──────────┤  ├──────────┤  ├──────────┤  ├─────────────┤ │
│  │Execution │  │Liquidity │  │Settlement│  │Alerts/Watch │ │
│  │ Blotter  │  │ Heatmap  │  │Inspector │  │   list      │ │
│  └──────────┘  └──────────┘  └──────────┘  └─────────────┘ │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │        Next.js SPA + API Proxy (rewrites)             │   │
│  │   Zustand stores + useWebSocket hook                  │   │
│  └──────────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
                            │
              ┌─────────────┴─────────────┐
              ▼                           ▼
┌──────────────────────┐   ┌──────────────────────────────┐
│  Fastify API Server  │   │  WebSocket ws://host/ws      │
│  Port 3001           │   │  Channels: market,execution, │
│  Rate Limit + CORS   │   │  settlement,alerts           │
│  Redis caching       │   └──────────────────────────────┘
│  BullMQ job queue    │
│  Prisma ORM (PG)     │
│  Services (RPC,      │
│   price-feed, etc.)  │
└──────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Smart Contracts (9)                        │
│                                                              │
│  IntentRouter · FragmentVault · RouteRegistry                │
│  SettlementVerifier · PrivacyScoreOracle                     │
│  TreasuryFeeCollector · Governance · RelayerRegistry · SolverAuction │
│                                                              │
│  All: AccessControl · Pausable · ReentrancyGuard · NatSpec  │
└─────────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Blockchain Networks                        │
│  Ethereum (1) · Arbitrum (42161) · Base (8453)              │
│  Solana (101) · Avalanche (43114) · BNB Chain (56)          │
└─────────────────────────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────────────────────────┐
│                  0G Infrastructure                           │
│  Compute (AI route solver) · Storage (route data cache)     │
│  DA (settlement proof availability)                         │
└─────────────────────────────────────────────────────────────┘
```

See [docs/architecture.md](docs/architecture.md) for detailed system design, data flow, and component interaction.

---

## User Flow (Step-by-Step)

Here's what happens from the moment you open the app to completing a cross-chain transfer:

```
STEP 1: OPEN APP → Market Matrix loads 6 chains from backend API
         ├─ Header shows KPI bar (TVL, Volume, Routes, MEV)
         ├─ Sidebar shows system health from /api/system/health
         ├─ AI Solver fetches recommendation from /api/routes/recommend
         └─ StatusStrip scrolls ticker events from /api/market/ticker

STEP 2: EXPLORE → Switch tabs to view each module
         ├─ LIQUIDITY: Pool depth bar charts fetched from /api/market/liquidity
         ├─ ROUTES: Fragment pipeline fetched from /api/routes/simulate
         └─ WATCHLIST: 7 tracked assets in right sidebar

STEP 3: EXECUTE → Click EXECUTION tab, fill the form
         ├─ Click SIMULATE → POST /api/execution/simulate with real params
         ├─ Click OPTIMIZE → POST /api/execution/optimize
         └─ Click EXECUTE → POST /api/execution/execute → polls until completed

STEP 4: MONITOR → Alerts fetched from /api/alerts on mount
         ├─ Alerts Feed shows route_success, mev_event, gas_spike
         └─ Status Strip updates with ticker from backend

STEP 5: VERIFY → Click SETTLEMENT tab, paste tx hash
         ├─ Inspect: POST /api/settlement/inspect for full details
         └─ Verify: GET /api/settlement/verify/:txHash for on-chain confirmation

STEP 6: POWER-USER → Click TERMINAL tab
         ├─ Type: route 50000 usdc arb eth private
         ├─ Type: status → system health
         └─ Type: help → all available commands
```

See [docs/walkthrough.md](docs/walkthrough.md) for the complete end-to-end walkthrough with every screen, API call, and data flow explained.

---

## Modules (10 Frontend Components)

### 1. Market Matrix
Real-time chain intelligence grid via AG Grid. 6 chains x 9 metrics (liquidity, spread, gas, bridge fee, slippage, latency, privacy, MEV, ETA). Live color-coded cells, column visibility toggling, auto-refresh every 30s, WebSocket push updates. Data from backend `/api/market/chains`.

**File:** `frontend/src/components/market-matrix/MarketMatrix.tsx`

### 2. Route Visualizer
Animated route graph showing every fragment's journey: wallet -> fragmentation -> bridge -> DEX -> liquidity pool -> settlement. Each step shows cost, latency, privacy score, confidence, and live status. Data from backend `/api/routes/simulate`.

**File:** `frontend/src/components/route-visualizer/RouteVisualizer.tsx`

### 3. AI Solver
Recommends optimal paths with confidence scoring, alternatives, bridge health metrics, and MEV forecasts. Explains why each route was chosen. Data from backend `/api/routes/recommend`.

**File:** `frontend/src/components/ai-solver/AiSolver.tsx`

### 4. Execution Blotter
Full order management form: source/destination asset & chain, amount, privacy toggle, fragmentation mode, slippage tolerance, bridge preference, MEV guard. Three-button workflow: Simulate -> Optimize -> Execute. All actions call live backend APIs.

**File:** `frontend/src/components/execution-blotter/ExecutionBlotter.tsx`

### 5. Liquidity Heatmap
Cross-chain depth visualization via Recharts bar charts + pool grid view. Depth chart with colored bars per chain. Pool grid shows token-level depth, APY, and utilization. Data from backend `/api/market/liquidity`.

**File:** `frontend/src/components/liquidity-heatmap/LiquidityHeatmap.tsx`

### 6. Settlement Inspector
On-chain proof inspection. Enter a tx hash or route ID to view: transaction hash, route ID, proof hash, settlement state, fees, relayer, confirmations. Data from backend `/api/settlement/proofs` and `/api/settlement/verify/:txHash`.

**File:** `frontend/src/components/settlement-inspector/SettlementInspector.tsx`

### 7. Command Terminal
xterm.js-inspired power-user terminal with route/simulate/inspect/watch/compare/status commands and history support.

**File:** `frontend/src/components/command-terminal/CommandTerminal.tsx`

### 8. Alerts Feed
Live operational feed. Filter by type: bridge outage, route success, MEV event, liquidity spike, relayer failure, gas spike. Severity indicators (info/warning/critical). Data from backend `/api/alerts`.

**File:** `frontend/src/components/alerts-feed/AlertsFeed.tsx`

### 9. Recent Executions
Execution tape for the operations desk. Shows route ID, asset, amount, route pair, timestamp, and status with success/failure indicators. Polls live backend execution orders.

**File:** `frontend/src/components/recent-executions/RecentExecutions.tsx`

### 10. Watchlist
Compact portfolio tracker. 7 assets (USDC, ETH, BTC, SOL, AVAX, ARB, BNB) with price, 24h change %, PnL. Color-coded gain/loss indicators. Chain attribution per asset.

**File:** `frontend/src/components/watchlist/Watchlist.tsx`

---

## Quick Start

```bash
# Clone
git clone https://github.com/elonmasai7/groute.git
cd groute

# Install dependencies
cd contracts && npm install --legacy-peer-deps && cd ..
cd backend && npm install --legacy-peer-deps && cd ..
cd frontend && npm install --legacy-peer-deps && cd ..

# Start infrastructure (PostgreSQL + Redis)
docker compose -f docker/docker-compose.yml up -d

# Initialize database
cd backend && npx prisma db push && npx prisma db seed && cd ..

# Start backend (terminal 1)
cd backend && npm run dev

# Start frontend (terminal 2)
cd frontend && npm run dev
```

Open **[http://localhost:3000](http://localhost:3000)**

All frontend `/api/*` requests are proxied to the backend server at `http://localhost:3001` via Next.js rewrites (configured in `next.config.js`).

Backend now runs in strict live-data mode:
- PostgreSQL and Redis are required at startup
- WebSocket emits only real published events (no synthetic generator)
- Market, routes, execution, alerts, and settlement routes return live DB/service data only

---

## Documentation

| Document | Description |
|----------|-------------|
| [Walkthrough](docs/walkthrough.md) | Step-by-step user journey: every screen, action, and data flow |
| [Architecture](docs/architecture.md) | System design, data flow, component interaction, state management |
| [Live Data Plan](docs/architecture-live.md) | Migration plan: replace mock data with real blockchain/API data |
| [API Reference](docs/api.md) | All API endpoints with request/response schemas |
| [Smart Contracts](docs/contracts.md) | 9 Solidity contracts -- spec, functions, events, deployment |
| [Deployment Guide](docs/deployment.md) | Local, Docker, K8s, Vercel, Coolify, Production |
| [Data Flow](docs/data-flow.md) | End-to-end transaction flow, state management, WebSocket protocol |
| [State Management](docs/state-management.md) | Zustand stores, store hierarchy, cross-store interactions |
| [Database Schema](docs/database.md) | Prisma models, relationships, migrations |
| [CTO Review](docs/cto-review.md) | Strategic assessment: grades, risks, recommendations |
| [ML Architecture](docs/ml-architecture.md) | AI/ML model design for route optimization and MEV prediction |
| [Docs Audit](docs/AUDIT-IMPROVEMENT-PLAN.md) | Documentation audit and improvement plan |
| [Security](SECURITY.md) | Contract security, MEV protection, key management |
| [Contributing](CONTRIBUTING.md) | Development setup, code standards, PR process |

---

## Project Structure

```
groute/
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── page.tsx                    # 3-row institutional workspace layout
│   │   │   ├── layout.tsx
│   │   │   ├── market-matrix/
│   │   │   ├── execution-desk/
│   │   │   ├── solver-marketplace/
│   │   │   ├── route-analysis/
│   │   │   ├── liquidity-intelligence/
│   │   │   ├── settlement/
│   │   │   ├── command-terminal/
│   │   │   ├── alerts/
│   │   │   └── watchlist/
│   │   ├── components/
│   │   │   ├── market-matrix/
│   │   │   ├── route-visualizer/
│   │   │   ├── ai-solver/
│   │   │   ├── execution-blotter/
│   │   │   ├── liquidity-heatmap/
│   │   │   ├── settlement-inspector/
│   │   │   ├── command-terminal/
│   │   │   ├── alerts-feed/
│   │   │   ├── recent-executions/
│   │   │   ├── watchlist/
│   │   │   └── layout/
│   │   ├── stores/
│   │   ├── hooks/
│   │   ├── lib/
│   │   └── types/
│   └── next.config.js
├── backend/
│   ├── src/
│   │   ├── index.ts                    # strict live-data boot (DB + Redis required)
│   │   ├── routes/
│   │   │   ├── market.ts
│   │   │   ├── execution.ts
│   │   │   ├── settlement.ts
│   │   │   ├── routes.ts
│   │   │   ├── alerts.ts
│   │   │   └── solver.ts
│   │   ├── services/
│   │   │   ├── rpc-provider.ts
│   │   │   ├── price-feed.ts
│   │   │   └── defillama.ts
│   │   ├── websocket/
│   │   │   └── handler.ts              # real event stream only
│   │   └── middleware/
│   └── prisma/schema.prisma
├── contracts/
│   ├── contracts/
│   │   ├── IntentRouter.sol
│   │   ├── FragmentVault.sol
│   │   ├── RouteRegistry.sol
│   │   ├── SettlementVerifier.sol
│   │   ├── PrivacyScoreOracle.sol
│   │   ├── TreasuryFeeCollector.sol
│   │   ├── Governance.sol
│   │   ├── RelayerRegistry.sol
│   │   └── SolverAuction.sol
│   └── hardhat.config.ts
├── docker/
│   ├── docker-compose.yml
│   ├── Dockerfile.backend
│   └── Dockerfile.frontend
├── k8s/
│   └── deployment.yaml
├── docs/
├── coolify.json
├── vercel.json
├── .env.example
├── SECURITY.md
├── CONTRIBUTING.md
└── README.md
```

---

## Deployment Options

| Method | Command |
|--------|---------|
| **Vercel** (frontend only) | `cd frontend && npx vercel --prod` |
| **Coolify** (full stack) | Auto-detected via `coolify.json` |
| **Docker Compose** | `docker compose -f docker/docker-compose.yml up -d --build` |
| **Kubernetes** | `kubectl apply -f k8s/deployment.yaml` |
| **Contracts** | `cd contracts && npx hardhat run scripts/deploy.ts --network <network>` |

See [docs/deployment.md](docs/deployment.md) for detailed guides.

---

## License

MIT — See [LICENSE](LICENSE)
