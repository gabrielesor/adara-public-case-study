# System Context Diagram

This diagram places Adara within its public system context. Adara is represented as one system boundary; the surrounding nodes are external actors or systems that exchange information with it.

```mermaid
flowchart LR
    users["Authorized operational users"]
    providers["Public market-data providers"]
    exchanges["Digital-asset exchanges"]
    recipients["Notification / compliance-report recipients"]
    monitoring["External uptime monitoring"]
    adara["ADARA"]

    users -->|"Interact through the web console"| adara
    providers -->|"Provide market data"| adara
    exchanges -->|"Provide market and execution information"| adara
    adara -->|"Submit orders"| exchanges
    adara -->|"Send notifications and compliance reports"| recipients
    monitoring -->|"Observe the production web endpoint"| adara
```

This is a system-context view, not a network or deployment topology. It does not identify organisations, exchanges, data providers, recipients, monitoring providers, infrastructure services, or communication protocols. The web console and persistent operational state are inside the Adara system boundary and are therefore not shown as external systems.
