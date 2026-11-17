# ML Problem Scoping: Two Case Studies

What do you do when leadership says "can we use AI for this?" and nobody has defined the problem yet? These two case studies show how I turn a vague request into a clear, justified problem definition **before any modelling begins**: who the user is, what decision the result supports, what the data can and cannot tell us, which methods fit, and where an LLM should (and should not) be used.

This was a bootcamp assignment (neuefische, "Applied LLMs for Business Cases"). The brief: given a vaguely defined problem where leadership wants to "use AI", scope it as a Product Manager working with a Data Science / ML team. No model is built, and no clean labelled data exists at the start. Work is judged on clarity of thinking, scope and justification.

**About me:** I come from performance marketing (campaign analytics, attribution tools, seasonal comparisons) and am moving into data and ML product work. That background shapes the examples in both cases.

**Also in this repo:** [All slides as one PDF](presentations.pdf) (16 pages).

## Case studies

| Case | The vague request | Scoping document | Slides |
|---|---|---|---|
| **A6: User onboarding drop-off** | "Many users abandon onboarding. We have lots of event data, but don't know where or why." | [scoping-document.md](A6-onboarding-dropoff/scoping-document.md) | [Slides](https://iremleyla.github.io/ml-problem-scoping-case-studies/A6-onboarding-dropoff/presentation.html) |
| **B5: Executive dashboard automation** | "Executives want an AI dashboard that explains weekly KPI changes automatically." | [scoping-document.md](B5-executive-dashboard/scoping-document.md) | [Slides](https://iremleyla.github.io/ml-problem-scoping-case-studies/B5-executive-dashboard/presentation.html) |

## Key decisions at a glance

**A6: Onboarding drop-off**
- Defined "abandonment" explicitly instead of assuming it, and treated "users drop off at one page" as a hypothesis to test.
- Framed it as **diagnose first, predict later**: unsupervised and representation learning on event sequences rather than a classifier trained on an unvalidated label.
- Kept data at event level to preserve the order of events, and flagged censoring, leakage and GDPR risks.
- Learned and applied **survival analysis** to handle users who haven't dropped off *yet*.

**B5: Executive dashboard**
- Split the problem into three questions: is the change real, where did it come from, and why did it happen.
- Compute the numbers with statistics and code; the LLM only **writes up verified results**, and a human reviews the text before executives see it.
- Flagged the source-of-truth problem (the same KPI differs between ad and attribution tools) as a core data risk.
- Learned and applied **difference-in-differences** to estimate the effect of a change when an experiment isn't possible.

## Structure of each scoping document

1. Problem clarification
2. Problem type classification
3. Data science scope
4. Modelling approach (high level)
5. Use of LLMs (reflection)
6. Unknown methods discovery

## How I worked

I used an LLM (Claude) as a thinking partner: it drafted, explained unfamiliar methods and challenged my framing. I made the decisions and brought my own background in performance marketing (attribution, seasonality, campaign comparisons). The reflection logs document my decisions, where I was corrected, and what I learned:

- [A6 reflection log](A6-onboarding-dropoff/reflection-log.md)
- [B5 reflection log](B5-executive-dashboard/reflection-log.md)
