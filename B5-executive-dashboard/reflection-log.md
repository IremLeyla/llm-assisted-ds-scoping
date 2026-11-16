# B5 Reflection Log

Running notes for Sections 5 and 6 of the scoping document. Add entries as they happen.

## Where the LLM helped clarify my thinking

- Showed that "what changed and where" can be stated from data, while "why" needs evidence beyond correlation, so an AI should present causes as hypotheses.
- Suggested splitting a KPI into components (e.g. revenue = customers x average spend) and asking first whether the change is real or just weekly noise.

## Where I contributed, challenged or refined

- My own instinct: I would only trust an AI-written explanation if a human reviewed it. This became the human-in-the-loop requirement.
- From my performance marketing experience: the same KPI (CTR, ROAS) showed different numbers in Meta Ads and in Triple Whale, and my team decided to trust Triple Whale as the single source of truth. This became a key data risk: the dashboard must not silently pick or mix sources.
- I said the explanation "has no real substance" behind it, which pointed to the correlation vs causation problem.

## Incorrect assumptions and how they were corrected

- **Assumption:** the executives are marketing managers (this came from my own background in performance marketing). **Correction:** executives usually means C-level and VP level (CEO, CFO, heads of product, sales, operations), who need a few lines and a decision, not campaign-level detail.

- **Assumption:** difference-in-differences is a kind of seasonal model. **Correction:** it is a method for estimating the effect of a change without an experiment. It copes with seasonality only because the comparison group shares the same season.
- **Assumption:** an LLM can analyse raw KPI tables directly. **Correction:** compute the numbers with code and statistics, and let the LLM write up only verified results.

## Methods I did not know before

- **Difference-in-differences:** compares the change in a group that got an intervention with the change in a comparison group that did not, to separate the effect from what would have happened anyway (e.g. season). Relies on the parallel-trends assumption.
- Seasonal baselines with prediction intervals, and KPI decomposition (met, not explored in depth).
