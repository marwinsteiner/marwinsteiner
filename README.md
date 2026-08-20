## Hi, I'm Marwin 👋

[![LinkedIn](https://img.shields.io/badge/linkedin-%230077B5.svg?&style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/marwin-alexander-steiner/)
[![Substack](https://img.shields.io/badge/Substack-FF6719?style=for-the-badge&logo=substack&logoColor=white)](https://substack.com/@marwin628124)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:marwin.steiner@gmail.com)
[![HKJC API](https://img.shields.io/badge/🐎%20HKJC%20Racing%20API-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://hkjc-web.vercel.app/)

I'm a finance graduate from [Bayes Business School](https://www.bayes.city.ac.uk/) (BSc Investment & Financial Risk Management, Top Decile) with experience in data engineering, systematic trading, and derivatives research. If it's convex and complex, I'm interested!

Right now, I'm building data and execution infrastructure for systematic trading in C++ and continuing my research in the relative-value volatility space.

Currently forward-testing three quirky volatility statistical arbitrage edges in index vol. All of them (combined) lead to market-neutral returns.

### Previous roles:
- Data Engineer at Swiss Re
- Summer Intern at Swiss Life Asset Managers
- Spring Insight at Barclays

### What I'm thinking about

A few threads I'm currently pulling on:

- **Building volatility surfaces.** In my bachelor's thesis, I focused on volatility statistical arbitrage. To define fair value in the $\mathbb{Q}$ domain, you need an IV parametrisation. The tooling I built for this is now [`pysvi`](https://github.com/marwinsteiner/pysvi) [![PyPI Downloads](https://static.pepy.tech/personalized-badge/svi-py?period=total&units=INTERNATIONAL_SYSTEM&left_color=BLACK&right_color=GREEN&left_text=downloads)](https://pepy.tech/projects/svi-py), an open-source Python library on PyPI. 
- **Event-driven & prediction markets.** I've been building automated trading infrastructure for [Polymarket](https://github.com/marwinsteiner/polymarket-bot). In my view, prediction markets are one of the most interesting laboratories for testing probabilistic reasoning under uncertainty.
- **Longhand.** I did not like existing $\LaTeX$ editors, so I created my own research workspace: [Longhand.dev](https://www.longhand.dev)
- **HKJC API.** As a transient interest, I wrote a scraper for and strung an API around the Hong Kong Jockey Club website, allowing retrieval of historic race and form data because I did not want to pay for existing services. The goal was to use machine learning to identify consistent race winners. This research is not publicly available.

### Paper Abstracts
_Title: Mean Reversion in the Intraday Implied Volatility Surface of S&P 500 Options_

We study intraday S&P 500 index option implied volatilities using an extended stochastic
volatility-inspired parametrisation, recalibrated jointly across all expiries at 60-second inter
vals over 63 trading days. Two experiments are conducted. The first decomposes the recon
structed implied volatility surface via functional principal component analysis, fits a vector
autoregression to the resulting factor scores, and tests whether the surface-level forecast can
outperform a random walk. The second measures the basis between each quoted implied
volatility and the fitted surface, tests for serial dependence, and assesses the economic scale
of the resulting deviations against the bid–ask spread.

_Title: Reverse-Engineering a Dominant Market Maker from Level 4 Order Book Data (ongoing)_

Avellaneda and Stoikov's definition of market making as a stochastic optimal control problem is one of the seminal works in the academic study of market making. However, testing whether a large liquidity provider actually follows this method requires detailed order book data, not commonly disseminated by traditional financial exchanges. Recently, a Level 4 order book dataset from the perpetual futures exchange Hyperliquid was published, for the first time allowing us to pose the question whether any large market makers actually use this model. We identify the largest market maker by traded- against resting notional, reconstructing its quotes, quoting ladder, and inventory every second. Arrival intensities are estimated from the wallet's own resting orders by censored Poisson maximum likelihood estimation. We find the wallet's quoting mostly inconsistent with the model tested. 

### My Systematic Trading Setup

Most of my time goes into a systematic trading system that runs end-to-end on a local Kubernetes cluster: research, calibration, execution, risk, reconciliation and post-trade accounting. It is split into three repositories so that a new strategy is a *new pod*, not a new system.

- **`trading-core`**: a shared C++ SDK, consumed as a git submodule. It owns the pluggable interfaces (market data, broker, volatility surface, IV solver) and the event envelope every pod speaks. Strategies depend on interfaces, never on a vendor.
- **`trading-infra`**: the control plane. Ingests every pod's events over a length-prefixed TCP envelope, enforces risk limits against *broker-truth* margin, aggregates the cross-strategy book, records everything, and runs post-trade reporting.
- **`trading-platform`**: research and observability. An immutable tick/bar archive, a materializer into QuestDB, a seekable replay engine for what-if calibration, and Grafana for dashboards, alerting and audited operator actions.

```mermaid
flowchart TB

subgraph SRC["Market data sources"]
  direction LR
  DBTO["Databento<br/>TBBO / MBP-1 ticks"]
  DXL["DXLink<br/>live option chains"]
  VEND["Yahoo, Nasdaq Trader,<br/>SEC, OpenFIGI, FINRA"]
end

subgraph PLAT["trading-platform: research, data lake, observability"]
  direction TB
  DBN[("DBN archive<br/>immutable, re-downloadable")]
  EQA[("Equities archive<br/>raw parquet, one part per fetch")]
  MATZ["Materializer<br/>validate, segment, log-returns"]
  PLAYER["Replay player<br/>synthetic clock, seekable"]
  SUBS["Research subscribers<br/>SANOS / volfi calibration hosts"]
  GRAF["Grafana<br/>dashboards, replay, alerting"]
  PROM["Prometheus<br/>node + kube-state metrics"]
end

QDB[("QuestDB<br/>one store: ticks, daily bars, greeks,<br/>NAV, trades, breaches, audit trail")]

subgraph CORE["trading-core: shared C++ SDK (git submodule)"]
  direction LR
  I1["DataSource<br/>DXLink, Databento"]
  I2["BrokerAdapter<br/>IBKR, Tastytrade"]
  I3["SurfaceModel<br/>eSSVI, SANOS"]
  I4["IVSolver<br/>LBR, volfi"]
  I5["EventPusher<br/>envelope over TCP"]
end

subgraph STRAT["Strategy repos: one pod per strategy, per mode"]
  direction LR
  ST1["index_volstrat<br/>SPX / NDX / RUT / XSP<br/>relative-value vol arb"]
  ST2["equity long-only"]
  ST3["equity long / short"]
  ST4["managed futures<br/>trend and carry"]
  ST5["FX carry"]
  ST6["rates vol"]
end

subgraph INFRA["trading-infra: control plane"]
  direction TB
  API["api-server<br/>envelope ingest, REST + WebSocket<br/>operator identity and audit"]
  RISK["risk-manager<br/>scoped limit groups<br/>broker-truth margin"]
  BOOK["book-manager<br/>cross-strategy positions, NAV"]
  RECO["recorder<br/>to QuestDB, spills and replays"]
  POST["posttrade<br/>performance, accounting, exports"]
end

subgraph EXEC["Execution venues"]
  direction LR
  IBKR["IBKR TWS"]
  TT["Tastytrade"]
end

DBTO --> DBN
VEND --> EQA
DXL --> STRAT
DBN --> MATZ
EQA --> MATZ
MATZ --> QDB
DBN --> PLAYER
PLAYER --> SUBS
SUBS -.->|"models promoted once validated"| CORE

CORE ==>|"linked by every strategy"| STRAT
STRAT ==>|"state, trades, signals, NAV"| API
API --> RECO
RECO --> QDB
API --> BOOK
API <--> RISK
RISK -.->|"pause, flatten, liquidate"| STRAT

STRAT ==>|"orders"| EXEC
EXEC -->|"fills, positions, margin impacts"| STRAT

QDB --> GRAF
QDB --> POST
PROM --> GRAF
GRAF -.->|"audited operator actions"| API

classDef live fill:#0d7a3e,stroke:#0a5c2f,color:#ffffff
classDef planned fill:#39404d,stroke:#7d8590,color:#e6edf3,stroke-dasharray: 5 3
classDef store fill:#1f6feb,stroke:#1158c7,color:#ffffff
classDef ctrl fill:#8250df,stroke:#6639ba,color:#ffffff

class ST1 live
class ST2,ST3,ST4,ST5,ST6 planned
class QDB,DBN,EQA store
class API,RISK,BOOK,RECO,POST ctrl
```

Green: built and running. Dashed grey: not built yet.

**How a strategy slots in.** Every pod, whatever it trades, speaks one protocol and inherits risk, recording, reconciliation and reporting for free:

```mermaid
sequenceDiagram
    autonumber
    participant P as Strategy pod
    participant A as api-server
    participant R as risk-manager
    participant Q as QuestDB
    participant B as Broker

    P->>A: state, trades, signals, NAV, heartbeat
    A->>Q: every event persisted by the recorder
    A->>R: forwarded for limit evaluation
    R-->>P: pause_entries / flatten_and_halt / liquidate
    P->>B: orders via BrokerAdapter
    B-->>P: fills, positions, per-position margin impacts
    P->>P: reconcile book against broker, drop and adopt
    Note over P,B: forced exits on max-hold and DTE run independently of signals
```

### Toolbox

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=C%2B%2B&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/-React-61DAFB?style=flat-square&logo=react&logoColor=black)
![PySpark](https://img.shields.io/badge/-PySpark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)
![SQL](https://img.shields.io/badge/-SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![Node.js](https://img.shields.io/badge/-Node.js-339933?style=flat-square&logo=node.js&logoColor=white)
![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Palantir Foundry](https://img.shields.io/badge/-Palantir_Foundry-101113?style=flat-square&logo=palantir&logoColor=white)
