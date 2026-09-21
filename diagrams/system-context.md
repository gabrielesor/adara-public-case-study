# System Context Diagram

This diagram places Adara within its public system context. Adara is represented as one system boundary; the surrounding nodes are external actors or systems that exchange information with it.

```mermaid
flowchart LR
    users["Authorized operational users"]
    providers["Public market-data providers"]
    exchanges["Digital-asset exchanges"]
    recipients["Notification / compliance-report recipients"]
    monitoring["External uptime monitoring"]
    aiService["OpenAI API"]
    adara["ADARA"]

    users -->|"Interact through the web console"| adara
    providers -->|"Provide market data"| adara
    exchanges -->|"Provide market and execution information"| adara
    adara -->|"Submit orders"| exchanges
    adara -->|"Send notifications and compliance reports"| recipients
    monitoring -->|"Observe the production web endpoint"| adara
    adara -->|"Responses requests / remote MCP results"| aiService
    aiService -->|"Streaming responses / remote MCP requests"| adara
```

This is a system-context view, not a network or deployment topology. It does not identify organisations, exchanges, data providers, recipients, monitoring providers, infrastructure services, private endpoints, authorization, or communication protocols. The web console, AiAlly, and persistent operational state are inside the Adara system boundary and are therefore not shown as external systems.

The OpenAI API boundary represents AiAlly's external AI-service interaction. In the replacement, the Responses API mediates remote MCP requests to selected capabilities inside the Adara boundary and receives their results before the response is streamed back. The first-generation Assistants API integration is a historical production implementation that is currently unavailable following retirement of the upstream API; the Responses API and remote MCP replacement has been successfully validated in pre-production, with production rollout pending. The diagram does not imply that the replacement is currently operating in production.
