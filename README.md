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

## Status

Work in progress.
