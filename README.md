# LLM-Assisted Data / ML Problem Scoping

Two scoping documents written as a Product Manager working with a Data Science / ML team. Each takes a vaguely defined "use AI" request and turns it into a clear, justified problem definition **before any modelling begins**. An LLM was used as a thinking partner, not as a source of ready-made answers.

## Scenarios

| Scenario | Prompt | Document |
|---|---|---|
| **A6: User Onboarding Drop-off** | Many users abandon the app during onboarding. High-dimensional event data exists, but no clear explanation of where or why users disengage. | [A6-onboarding-dropoff/scoping-document.md](A6-onboarding-dropoff/scoping-document.md) |
| **B5: Executive Dashboard Automation** | Executives want an AI-powered dashboard that automatically explains weekly KPI changes. | [B5-executive-dashboard/scoping-document.md](B5-executive-dashboard/scoping-document.md) |

## Structure of each scoping document

1. Problem clarification
2. Problem type classification
3. Data science scope
4. Modelling approach (high level)
5. Use of LLMs (reflection)
6. Unknown methods discovery

## Constraints assumed

- No clean, labelled training data exists at the start.
- Focus is on scoping and justification, not building a model.

## How I worked

An LLM (Claude) acted as a thinking partner: it proposed drafts, explained unfamiliar methods and challenged my framing. I made the decisions, brought in my own experience from performance marketing (for example the single-source-of-truth problem with ad and attribution tools, seasonal comparisons for campaigns, and the Valentine's Day example), and decided what stayed in each document. The reflection logs list my contributions, the corrections I received and the incorrect assumptions I had:

- [A6 reflection log](A6-onboarding-dropoff/reflection-log.md)
- [B5 reflection log](B5-executive-dashboard/reflection-log.md)

## Status

Both scoping documents are complete drafts. Presentation slides: [A6 slides](https://iremleyla.github.io/llm-assisted-ds-scoping/A6-onboarding-dropoff/presentation.html), [B5 slides](https://iremleyla.github.io/llm-assisted-ds-scoping/B5-executive-dashboard/presentation.html).
