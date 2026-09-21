# Order Lifecycle Diagram

This diagram separates orders originated through Adara's controlled execution path from exchange activity that originated outside Adara and is later synchronized or observed.

```mermaid
flowchart TB
    subgraph controlled["Adara-originated controlled execution"]
        direction TB
        human["Discretionary / human-directed trading"]
        automated["Automated strategy"]
        intent["Trading intent"]
        validation["Input and context validation"]
        compliance["Pre-trade compliance"]
        stopped["Stopped before exchange submission"]
        continuation["Eligible to continue"]
        confirmation["Human confirmation<br/>where applicable"]
        submission["Exchange submission"]
        submittedExchange["Digital-asset exchange boundary"]
        monitoring["Monitoring / execution state"]
        evidence["Order-level compliance evidence<br/>(Adara-originated orders)"]

        human --> intent
        automated --> intent
        intent --> validation
        validation --> compliance
        compliance -->|"FAIL"| stopped
        compliance -->|"PASS"| continuation
        continuation -->|"Confirmation not applicable"| submission
        continuation -->|"Discretionary confirmation where applicable"| confirmation
        confirmation --> submission
        submission --> submittedExchange
        submittedExchange --> monitoring
        compliance --> evidence
    end

    subgraph external["Externally originated activity"]
        direction TB
        externalActivity["External order activity"]
        observedExchange["Digital-asset exchange boundary"]
        synchronization["Synchronization / observation"]

        externalActivity --> observedExchange
        observedExchange --> synchronization
    end

    history["Persistent order history & provenance"]
    outputs["Historical review / reporting / analytics"]

    monitoring --> history
    synchronization --> history
    evidence --> outputs
    history --> outputs
```

The controlled pre-trade path applies to discretionary and automated orders originated through Adara. A failed applicable compliance result stops that path before exchange submission. Human confirmation is conditional and applies where required for discretionary activity; the diagram does not imply confirmation of each automated-strategy order.

Externally originated activity follows a separate observation path. It may be synchronized from an exchange and retained with provenance, but it is not represented as having passed through Adara's validation or pre-trade compliance controls before reaching the exchange.

This is a responsibility and lifecycle view, not an exact implementation state machine. The repeated exchange-boundary labels distinguish submitted and externally originated activity without identifying different venues or prescribing deployment components.
