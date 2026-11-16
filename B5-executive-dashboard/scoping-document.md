# B5: Executive Dashboard Automation, Scoping Document

> Scenario: Executives want an AI-powered dashboard that automatically explains weekly KPI changes.

## 1. Problem Clarification

**Problem statement:**
Executives receive weekly KPI numbers but no reliable account of what changed or whether the change matters, and an AI that simply writes confident-sounding "reasons" risks misleading them. The real problem is to flag which weekly changes are real, break down where they came from (which segments or components), and present possible causes as hypotheses with evidence rather than as facts. The executive team will use this to decide quickly whether a change needs action, investigation, or just monitoring.

**Decision-maker / end user:** Executives at C-level and VP level (CEO, CFO, heads of product, sales, operations). Analysts act as reviewers of the AI output before it reaches executives.

**Decision it informs:** Whether a weekly KPI change needs action now, needs investigation by a team, or only needs monitoring.

**Stance on "explaining":**
- **What changed and where** (which metric, region, segment or component moved) can be stated as fact, because it comes from the data.
- **Why it changed** can only be stated as a labelled hypothesis, unless an experiment or causal analysis supports it. Correlation in time (e.g. "the pricing page launched the same week") is not proof of cause.
- **Is it real?** Weekly KPIs fluctuate by chance, so the system must separate genuine changes from normal noise before explaining anything.
- **Human in the loop:** an analyst reviews the output before executives see it.

## 2. Problem Type Classification

**This is not one ML problem but three questions, each with a different framing:**

| Question | Framing | Why |
|---|---|---|
| **Is the weekly change real?** | Time-series forecasting + anomaly detection (unsupervised) | A KPI has a normal range and rhythm (weekly cycles, seasonality). We estimate what we would expect, then flag changes outside that range. No labels needed. |
| **Where did it come from?** | KPI decomposition (largely arithmetic, not ML) | Split the KPI into components and segments, e.g. revenue = customers x average spend, by region and customer group, and rank the contributors by size. |
| **Why did it change?** | Causal analysis / experimentation | "Because of the new pricing page" is a causal claim. It needs an experiment or a careful causal analysis. Without one, the system can only offer a labelled hypothesis. |

**Supporting component:** a language model writes the plain-English summary. It works only from numbers the first two steps have verified, and does not produce its own causal explanations.

**Not appropriate:**
- **A single model that "explains revenue".** It would find correlations and present them as causes.
- **Supervised learning.** There are no labels for "the real reason a KPI changed".
- **An LLM reading the raw data and explaining freely.** It tends to write fluent, confident stories that are not backed by evidence.

## 3. Data Science Scope

**Available data (likely exists):**
- Ad platforms (e.g. Meta Ads): spend, impressions, clicks, CTR by campaign and creative.
- Marketing analytics / attribution tools (e.g. Triple Whale): ROAS, attributed revenue, conversions.
- Web and app analytics: traffic, sessions, conversion funnel.
- Orders, payments and CRM: revenue, customers, average order value, churn.
- **Context log:** campaign calendar, pricing changes, product launches, outages, holidays. Needed to form hypotheses about "why".

**Modality:** tabular time series (primary), plus short text (campaign notes, launch descriptions).

**Granularity:** daily aggregates by campaign, channel, region and customer segment, reported weekly. Time windows vary with the question: weekly for routine reporting, campaign-level for events such as a Valentine's Day campaign.

**Labels:** none for "the real reason a KPI changed". Past analyst write-ups or incident notes could serve as a weak reference set, and analysts could label a sample of historical changes as real or noise.

**Key data risks:**
- **Conflicting numbers for the same KPI:** CTR in an ad platform often differs from CTR in an attribution tool because of different attribution windows and tracking methods. A team must decide which source is the source of truth per KPI, and the system must not silently pick or mix sources.
- **Inconsistent metric definitions:** "active users", "conversion" or "revenue" may be defined differently across teams.
- **Seasonality and campaigns as confounders:** holidays and promotions move KPIs, so a change may be a normal seasonal effect.
- **Late-arriving or restated data:** numbers for the latest week can change after the report is generated.
- **Tracking breaks:** a broken pixel or tag looks like a real KPI drop.
- **Short history:** few weeks of data make the "normal range" unreliable.
- **Many segments, many false alarms:** checking every region and segment every week will produce chance findings, which are easy to over-interpret.
- **Missing context:** if launches and pricing changes are not recorded, any "why" is guesswork.

## 4. Modelling Approach (High-Level)

**Proposed approaches (a pipeline, not one model):**

1. **Seasonal baseline with prediction intervals, for "is it real?"** Model each KPI's normal behaviour (trend, weekly pattern, holidays and seasonal peaks such as an Advent campaign) and forecast a range for the current week. A value outside the range is flagged as a real change, a value inside is reported as normal fluctuation. Fits because KPIs are time series with strong seasonality and no labels are needed. Should state its confidence when history is short.
2. **KPI decomposition and driver ranking, for "where did it come from?"** Split the KPI into components and segments (e.g. revenue = customers x average spend, by region, channel and customer group) and rank how much each part contributed to the change. Simple, transparent and checkable by an analyst.
3. **Causal checks, for "why?"** Use experiment results (e.g. an A/B test of creatives) where they exist. For changes made without randomisation (pricing page, checkout), use a quasi-experimental comparison such as difference-in-differences against an unaffected group. Without either, the system lists candidate causes from the context log (dates of launches and changes) as **hypotheses only**.

**LLM role (writing, under guardrails):** the LLM receives only verified outputs from steps 1 to 3 plus the context log, and writes a short plain-English summary. Every number in the text must trace back to a computed value. Causes are labelled as hypotheses. An analyst reviews before executives see it.

**Not appropriate:**
- **An LLM analysing raw KPI tables on its own.** It is unreliable at arithmetic on tables and fills gaps with fluent but unsupported stories.
- **A black-box regression "explaining" revenue.** It finds correlations and presents them as causes.
- **Deep-learning forecasters.** A few years of weekly data is too little to train them, and simpler seasonal models are easier to trust and explain.
- **Automatic causal discovery from observational data.** It cannot reliably separate cause from coincidence here.

**How we judge it:** backtest on past weeks to see whether flagged changes match ones analysts later confirmed, and measure the false-alarm rate. Check that every number in the generated summary matches the computed values. Track how often analysts correct or reject the draft.

## 5. Use of LLMs (Reflection)

**How I used the LLM:** as a thinking partner, section by section. It proposed drafts and explained concepts, and I answered its questions first, added my own experience from performance marketing, and decided what to keep.

**Where it helped clarify my thinking:** it separated "what changed and where" (which the data can state as fact) from "why" (which needs an experiment or causal analysis, otherwise it is only a hypothesis). It also showed that "explaining a KPI" is really three questions with different methods: is the change real, where did it come from, and why.

**Where I added to, or the LLM corrected, my thinking:** my own experience shaped the design. I said I would only trust an AI explanation if a human reviewed it, which became the human-in-the-loop requirement. I also described how my team trusted Triple Whale over Meta Ads when the same KPI showed different numbers, which became the single-source-of-truth risk. In the other direction, I first suggested giving an LLM two years of KPIs and asking it to "analyze". The LLM pointed out that this is unreliable, since LLMs make arithmetic mistakes on raw tables and fill gaps with plausible stories. The design was changed so that code and statistics produce the numbers and the LLM only writes up verified results.

## 6. Unknown Methods Discovery

**New technique: difference-in-differences.**

**Why it is relevant:** executives will ask "why did the KPI change?", which is a causal question. Often there is no time or possibility to run a randomised experiment, for example when a change such as a new checkout page is rolled out in one market only. Difference-in-differences gives a more credible estimate of the effect than a simple before/after comparison.

**In my own words:** take two groups, one that got a change and one that did not, and measure both before and after. For example, for Valentine's Day we show new product pictures for a bundle on the German website and keep last year's version in Austria. Conversion goes from 1.8% to 2.5% in Germany (+0.7) and from 1.8% to 2.3% in Austria (+0.5). Austria also rose because it is Valentine's season, so the effect of the new website is the difference of the two differences: 0.7 - 0.5 = +0.2 points. It is better than comparing before and after alone because the comparison group shows what would have happened anyway because of the season. It relies on the assumption that without the change, both groups would have moved the same way.

**Other techniques met along the way:** seasonal baselines with prediction intervals, and KPI decomposition.

**Incorrect assumption and how it was corrected:** I thought difference-in-differences was a kind of seasonal model. It is not: it is a method for estimating the effect of a change when no experiment was run. It handles seasonality because the comparison group experiences the same seasonal effect, but the comparison group and the parallel-trends assumption are what make it work. I also first assumed the "executives" were marketing managers, based on my own background. They are C-level and VP level readers who need a few lines and a decision.
