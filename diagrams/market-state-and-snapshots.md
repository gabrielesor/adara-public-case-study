# Market State & Snapshot Path

This diagram shows two deliberately different data paths: tick-driven market processing and aggregate portfolio-state materialization.

```mermaid
flowchart LR
    exchanges["Digital-asset exchanges"]
    providers["Market-data providers"]
    ticks["Tick ingestion & normalization"]
    memory["Low-latency in-memory market state"]
    strategies["Automated strategies"]

    accounts["Remote account / balance state"]
    acquisition["Independent state acquisition"]
    snapshot["Persistent + in-memory portfolio snapshot"]
    portfolio["Portfolio / dashboard / applicable compliance reads"]
    live["Live-refresh bypass"]

    exchanges -->|"price updates"| ticks
    providers -->|"price updates"| ticks
    ticks --> memory
    memory --> strategies

    exchanges -->|"account state"| accounts
    accounts --> acquisition
    acquisition -->|"periodic materialization"| snapshot
    snapshot --> portfolio

    accounts --> live
    live -->|"fresh state when requested/configured"| portfolio
```

The tick path exists to process changing market state as it arrives.

The snapshot path exists because repeatedly rebuilding aggregate portfolio state from many external accounts would unnecessarily couple dashboard and control reads to remote latency and availability.

The live-refresh path preserves an explicit way to prioritize freshness when required.
