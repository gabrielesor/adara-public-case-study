# Logical Architecture Diagram

This diagram presents Adara as interacting areas of logical responsibility. External market-data, exchange, and AI-service boundaries are shown only where they clarify the flow of market state, trading intent, execution information, retained evidence, and AI-assisted interaction.

```mermaid
flowchart TB
    providers["Public market-data providers"]
    exchanges["Digital-asset exchanges"]
    aiService["OpenAI API"]

    subgraph adara["ADARA — Public logical responsibilities"]
        console["Web Console"]
        aially["AiAlly / AI-Assisted Interaction"]
        tools["Curated Adara Capabilities<br/>(MCP tool boundary)"]
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
        console -->|"Natural-language interaction"| aially
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
    aially -->|"Responses requests / application context"| aiService
    aiService -->|"Streaming responses"| aially
    aiService -->|"Remote MCP tool requests"| tools
    tools -->|"Selected capability results"| aiService
```

The boxes in this diagram represent public logical responsibilities, not a one-to-one map of deployable components, processes, JARs, hosts, or AWS resources.

The diagram is not a deployment, network, or source-code/module view. Multiple responsibilities may cooperate within the platform, and the drawing intentionally omits implementation-sensitive topology, interfaces, and security configuration.

AiAlly is an AI-assisted user and operational interface, not algorithmic trading logic. The Responses API mediates remote MCP tool requests and receives results from the curated Adara capability boundary; the diagram does not depict AiAlly independently invoking a local MCP tool. The MCP node publishes no tool names, count, permissions, or configuration. The OpenAI boundary represents the Responses/MCP replacement successfully validated in pre-production; production rollout is pending. The Assistants API generation is a historical production implementation that is currently unavailable following retirement of the upstream API and is not shown as a current runtime path.
