# Order Lifecycle Diagram

```mermaid
flowchart TB
    subgraph controlled["Adara-originated controlled execution"]
        human["Discretionary / human-directed"]
        automated["Automated strategy"]
        intent["Trading intent"]
        validation["Validation"]
        compliance["Pre-trade compliance"]
        stopped["Stopped before exchange"]
        confirmation["Human confirmation where applicable"]
        submission["Exchange submission"]
        monitoring["Execution / fill monitoring"]

        human --> intent
        automated --> intent
        intent --> validation
        validation --> compliance
        compliance -->|"FAIL"| stopped
        compliance -->|"PASS / automated"| submission
        compliance -->|"PASS / discretionary where required"| confirmation
        confirmation --> submission
        submission --> monitoring
    end

    subgraph external["Externally originated activity"]
        externalOrder["External exchange order"]
        sync["Synchronization / observation"]
        externalOrder --> sync
    end

    context["Retained decision context<br/>origin + portfolio/account state + strategy/compliance context"]
    history["Persistent order history & provenance"]
    evidence["Order-level compliance evidence"]
    review["Historical review / reporting / analytics"]

    intent -.->|"decision context begins"| context
    monitoring --> history
    monitoring --> context
    compliance -->|"evaluation context"| context
    sync --> history
    context --> history
    submission -.-> evidence
    evidence --> review
    history --> review
```

The diagram distinguishes controlled Adara-originated execution from externally originated activity.

The retained decision-context node is deliberately separate from the final order record: the architectural objective is to preserve enough historical state to explain why an order was allowed and, for automated activity, why the strategy acted.
