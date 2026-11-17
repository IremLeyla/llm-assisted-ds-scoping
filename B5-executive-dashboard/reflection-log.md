# B5 Reflection Log: Working with an LLM as a Thinking Partner

I used an LLM (Claude) to draft, explain and challenge. The decisions, the domain knowledge and the final text are mine. This log shows how that worked in practice.

## Decisions I made

- **Required human review.** AI-written explanations must be reviewed by an analyst before executives see them. This became the human-in-the-loop requirement.
- **Raised the source-of-truth problem from real work.** In performance marketing, the same KPIs (CTR, ROAS) showed different numbers in Meta Ads and in Triple Whale, and my team trusted Triple Whale. It became a key data risk: the dashboard must not silently pick or mix sources.
- **Proposed a seasonal baseline:** comparing against the same period last year (for example the Advent calendar campaign).
- **Pointed to the correlation problem.** I said an explanation without evidence "has no real substance", which led to presenting causes as hypotheses.
- **Chose the method to learn:** difference-in-differences. I wrote the Valentine's Day example in my own words (Germany with new product pictures vs Austria with old ones) and calculated the effect myself.
- **Confirmed a realistic history length** (one to four years of weekly data) and accepted the **mock weekly summary** as a key output.
- **Reviewed every section** and decided what stayed in the document.

## What the LLM added

- Separated what can be stated from data ("what changed and where") from what needs evidence beyond correlation ("why").
- Suggested splitting a KPI into components (revenue = customers x average spend) and asking first whether a change is real or weekly noise.
- Explained difference-in-differences and its parallel-trends assumption.

## Where I was corrected, and what changed

| My first assumption | Correction | Effect on the document |
|---|---|---|
| The executives are marketing managers (my own background). | "Executives" usually means C-level and VP level, who need a few lines and a decision, not campaign detail. | Problem statement and the mock summary are written for that audience. |
| Difference-in-differences is a kind of seasonal model. | It estimates the effect of a change without an experiment. It copes with seasonality only because the comparison group shares the season. | Method section states the comparison-group requirement and its limit. |
| An LLM can analyse raw KPI tables directly. | Compute numbers with code and statistics; let the LLM write up only verified results. | Section 5 limits the LLM to wording verified numbers. |

I also made a small arithmetic slip in my difference-in-differences example (+0.7 and +0.5, so the effect is +0.2), which I corrected.

## What I learned

- **Difference-in-differences:** compares the change in a group that got an intervention with the change in a comparison group that didn't, to separate the effect from what would have happened anyway. It relies on parallel trends and needs a comparison group.
- **Seasonal baselines with prediction intervals** and **KPI decomposition** (understood at concept level, not implemented).
- **How to use an LLM well:** bring domain knowledge, ask for reasons, and decide what an AI should and should not be trusted to do.
