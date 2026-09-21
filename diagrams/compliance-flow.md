# Compliance Flow Diagram

This diagram presents two related but distinct compliance responsibilities: pre-trade enforcement for orders originated through Adara, and portfolio monitoring with scheduled daily reporting.

```mermaid
flowchart TB
    subgraph pretrade["Flow 1 — Pre-trade controlled execution"]
        direction TB
        intent["Adara-originated trading intent"]
        context["Order + current portfolio context"]
        normalize["Normalize values / prepare compliance context"]
        evaluate["Evaluate applicable controls / enforced limits"]
        stopped["Stop before exchange submission"]
        eligible["Eligible controlled execution"]
        submission["Exchange submission"]
        orderEvidence["Order-level compliance PDF<br/>(successful controlled path)"]
        orderEmail["Email distribution"]
        orderRetention["Server-side retention"]

        intent --> context
        context --> normalize
        normalize --> evaluate
        evaluate -->|"FAIL"| stopped
        evaluate -->|"PASS"| eligible
        eligible --> submission
        submission -.->|"Associated evidence"| orderEvidence
        orderEvidence --> orderEmail
        orderEvidence --> orderRetention
    end

    subgraph portfolio["Flow 2 — Portfolio monitoring and daily reporting"]
        direction TB
        portfolioState["Portfolio state"]
        warningMonitoring["Early-warning threshold monitoring"]
        warning["Multi-channel warning"]
        scheduledEvaluation["Scheduled portfolio compliance evaluation"]
        dailyEvidence["Daily portfolio-level compliance PDF"]
        dailyEmail["Email distribution"]
        dailyRetention["Server-side retention"]

        portfolioState --> warningMonitoring
        warningMonitoring -->|"Warning threshold crossed"| warning
        portfolioState --> scheduledEvaluation
        scheduledEvaluation --> dailyEvidence
        dailyEvidence --> dailyEmail
        dailyEvidence --> dailyRetention
    end
```

Pre-trade enforcement applies to discretionary and automated orders originated through Adara. A failed applicable control stops the controlled path before exchange submission; only the successful path reaches submission. The dotted evidence relationship associates the order-level PDF with the successfully controlled path without prescribing exact transactional sequencing.

Early-warning thresholds are separate from enforced limits. Crossing a warning threshold can trigger a multi-channel warning but is not presented as necessarily being a formal compliance-rule violation.

Order-level compliance evidence and the scheduled daily portfolio-level compliance PDF are distinct reporting views. Each has its own email-distribution and server-retention responsibility; the portfolio report is not represented as a sum of individual-order PDFs.

This is a public responsibility view, not a detailed rules engine, implementation sequence, or legal compliance model. It does not define thresholds, formulas, recipients, escalation procedures, or regulatory status.
