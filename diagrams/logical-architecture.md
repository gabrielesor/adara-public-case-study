# Logical Architecture Diagram

This diagram presents Adara as interacting areas of logical responsibility. External market-data and exchange boundaries are shown only where they clarify the flow of market state, trading intent, execution information, and retained evidence.

```mermaid
flowchart TB
    providers["Public market-data providers"]
    exchanges["Digital-asset exchanges"]

    subgraph adara["ADARA — Public logical responsibilities"]
        console["Web Console"]
        market["Market Data & Normalization"]
        portfolio["Portfolio & Valuation"]
        trading["Trading & Order Management"]
        strategies["Strategy Lifecycle"]
        compliance["Compliance"]
        analysis["Analysis & Reporting"]
        administration["Administration & Notifications"]
        state["Persistent Operational State<br/>(MySQL relational persistence)"]

        console -->|"Operational interaction"| portfolio
        console -->|"Discretionary trading intent"| trading
        console -->|"Review and reporting"| analysis
        console -->|"Administration"| administration
        market -->|"Normalized market state"| portfolio
        market -->|"Real-time market state"| strategies
        market -->|"Trading context"| trading
        portfolio -->|"Portfolio context"| trading
        portfolio -->|"Exposure and valuation state"| compliance
        strategies -->|"Automated trading intent"| trading
        trading -->|"Validated order state"| compliance
        compliance -->|"Submission outcome"| trading
        compliance -->|"Retained evidence"| state
        trading -->|"Order and execution state"| state
        portfolio -->|"Portfolio state"| state
        state -->|"Retained operational data"| analysis
        analysis -->|"Reports and notifications"| administration
        administration -->|"Administrative state"| state
    end

    providers -->|"Streaming market data"| market
    exchanges -->|"Market data"| market
    exchanges -->|"Execution information"| trading
    trading -->|"Orders that pass applicable controls"| exchanges
```

The boxes in this diagram represent public logical responsibilities, not a one-to-one map of deployable components, processes, JARs, hosts, or AWS resources.

The diagram is not a deployment, network, or source-code/module view. Multiple responsibilities may cooperate within the platform, and the drawing intentionally omits implementation-sensitive topology, interfaces, and security configuration.
