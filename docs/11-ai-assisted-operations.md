# AI-Assisted Operations — AiAlly

AiAlly is a later AI-assisted interface layer over Adara, distinct from the core trading architecture and Adara's proprietary automated strategies.

Most of the platform described in this case study — market data, portfolio state, order management, compliance, exchange integration, persistence, and production operations — was designed and implemented before generative AI became part of the development workflow.

## First generation

The first AiAlly generation used the OpenAI Assistants API and was deployed in the production Adara environment.

That implementation is now unavailable following retirement of the upstream API.

## Replacement architecture

The replacement uses:

- OpenAI Responses API;
- remote MCP integration;
- a curated set of Adara capabilities exposed as tools;
- optional File Search / RAG over product documentation;
- streamed responses;
- page/application context;
- conversation continuity where configured; and
- tracing and usage diagnostics.

It has been implemented and validated in pre-production. Production rollout is pending.

## MCP boundary

Remote MCP provides the standardized tool boundary through which the Responses interaction can access selected Adara capabilities.

AiAlly does not become the owner of portfolio, compliance, order-management, or trading rules. Those responsibilities remain deterministic platform capabilities underneath the AI interface.

## Retrieval and grounding

Document retrieval can complement MCP results with product knowledge from Adara documentation.

This creates two distinct source types:

- **operational context/capabilities** through MCP; and
- **documented product knowledge** through retrieval.

The public case study does not expose private prompts, tool inventories, endpoints, authorization configuration, or private source documents.

## Separation from automated trading

AiAlly is not a proprietary trading strategy and is not presented as a buy/sell decision engine.

Automated trading existed independently of AiAlly and is documented in [Strategy Platform](08-strategy-platform.md).

## Development authorship context

Generative AI was used during the design and implementation of the replacement AiAlly architecture and has also been used as an engineering assistant by the junior contributor, especially in UI work.

This does not change the authorship history of the core platform, which substantially predates generative-AI-assisted development.

## Status

| Generation | Public status |
| --- | --- |
| Assistants API integration | Historical production implementation; currently unavailable after upstream retirement |
| Responses API + remote MCP + optional retrieval | Implemented and validated in pre-production; production rollout pending |

## Public boundary

Private prompts, credentials, endpoints, tool inventories, user identifiers, trace data, retrieved private documents, and production operational data are not published.

[← Previous](10-operational-scale.md) | [Case Study Home](../README.md) | [Next →](12-current-development-and-rd.md)
