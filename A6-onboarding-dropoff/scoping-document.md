# A6: User Onboarding Drop-off, Scoping Document

> Scenario: Many users abandon the app during onboarding. We have high-dimensional event data from early sessions, but no clear explanation for where or why users disengage.

## Summary

- **Problem:** users abandon onboarding, but "abandonment" is undefined, there are no labels, and nobody knows where or why users leave.
- **Approach:** diagnose before predicting. Cluster user paths from event logs, map real screen-to-screen transitions, and use survival analysis for users who haven't left *yet*. The "why" needs interviews, surveys and experiments on top.
- **Key decision:** no classifier until the definition of abandonment is validated, because it would learn our guess and say who leaves, not where.

## 1. Problem Clarification

**Problem statement:**
We don't know where in the onboarding flow users disengage or which behavioural patterns precede abandonment, because "abandonment" has never been defined. The goal is to describe and diagnose how users drop off by discovering and characterising distinct onboarding trajectories, including the ones that end in abandonment, rather than to predict individual users. The onboarding product team will use this to decide which steps to redesign or test first.

**Decision-maker / end user:** Onboarding product team (secondary audience: growth / lifecycle marketing).

**Decision it informs:** Which onboarding screens or steps to fix, cut or A/B test first.

**Working definition of abandonment (to be tested, not assumed):**
A user has abandoned onboarding if they started it, did not reach the final step, and did not return to finish it within 7 days (default, to be validated against the data). Alternatives to compare: no second session within 7 days; never performing the first key action.

**Hypothesis, not assumption:** users may drop off around a specific screen (e.g. the third page). The data should show where, and there may be several drop-off patterns.

**Framing choice:** Diagnose first, predict later. With no labels and no agreed definition there is nothing reliable to predict yet, and the product team needs to know which step to fix, not only who will leave.

## 2. Problem Type Classification

**Primary framing: unsupervised learning**, supported by **representation learning**.

**Why it fits:**
- There are no labels and no agreed definition of abandonment, so the first job is to discover structure: which paths through onboarding exist, and which of them end in drop-off.
- The event data is high-dimensional (taps, scrolls, dwell time, screen sequences). Representation learning compresses each session into a compact form that clustering can work with.
- The product team first needs to know where and how users leave, which is a descriptive question, not a predictive one. The "why" needs extra evidence beyond event logs (see Section 4).

**Later phases:**
- **Causal analysis / experimentation:** once we know which step is the problem, A/B test fixes (wording, layout, fewer steps) to confirm they improve completion.
- **Supervised classification:** only after the definition of abandonment is validated. A classifier built on an unvalidated label would learn our guess, and it would tell us who leaves, not where or why.

**Not appropriate (yet):** time-series forecasting (we are not predicting a volume over time) and ranking / recommendation (nothing to recommend to individual users in this problem).

## 3. Data Science Scope

**Available data (likely exists):**
- Event logs from early sessions: taps, scrolls, dwell time, screen views, timestamps.
- Session metadata: session duration, screen sequence.
- Install and device metadata: phone type, OS, country, install source.
- Contact identifiers (e.g. email) collected during sign-up, and any reminder messages sent for unfinished onboarding.

**Modality:** event logs / sequences (primary), plus tabular metadata.

**Granularity:** raw data is event-level. Events are grouped into sessions, then into one onboarding trajectory per user for analysis. Collapsing to one row per user too early would lose the order of events.

**Labels:** none exist. They could be created from a working definition of abandonment (see Section 1). An existing "unfinished onboarding" reminder rule, if the company has one, is a candidate weak label to compare against.

**Key data risks:**
- **Censoring:** recent users may not have had time to return, so they look like drop-offs when they are not.
- **Leakage:** using behaviour from after onboarding to explain behaviour during it.
- **Sparsity:** many users leave after one or two screens, giving very short sequences.
- **Bias:** differences by OS, app version, install source, or flow changes and A/B tests running during the period.
- **Quality:** duplicated or missing events, inconsistent timestamps, instrumentation changes between app versions.
- **Invisible users:** people who install but never open the app generate no events, so they are missing from event logs and must be tracked via install data.
- **Privacy:** device, country and email are personal data (GDPR). Analysis should use pseudonymised IDs and exclude direct identifiers.

## 4. Modelling Approach (High-Level)

**Proposed approaches:**

1. **Sequence clustering with learned representations.** Compress each user's onboarding trajectory (screen sequence, dwell times, interaction counts) into a compact vector, then cluster to find distinct user archetypes, e.g. "opens the first screen and leaves" or "completes everything but the last step". Fits because the data is unlabelled, high-dimensional and sequential.
2. **Process mining / Markov path analysis.** Build a map of actual transitions between screens with their probabilities. Reveals loops, skipped steps and back-and-forth that a simple funnel hides, and shows where each archetype exits.
3. **Survival analysis.** Model time until a user drops off. Handles censoring (users who have not had time to return) and lets us compare drop-off timing across clusters and definitions of abandonment.

**Answering "why":** event logs show behaviour, not motivation. Our methods find where users leave and which patterns they follow, but not why. To answer "why" we would add user interviews, short exit surveys and session replays for each archetype, and then test the resulting ideas with A/B experiments.

**How we judge the results (no labels):**
- Check that clusters are stable when we re-run on different samples of users.
- Compare clusters with the existing "unfinished onboarding" reminder rule, if there is one, as a weak label.
- Have the product team review whether the archetypes make sense and are actionable.

**Not appropriate (yet):**
- **Supervised classifier as the first step.** Needs a validated abandonment label, and would predict who leaves rather than explain where or why.
- **Clustering on raw event counts.** Very high-dimensional, ignores event order, and distances become unreliable. Needs a sequence-aware representation first.
- **A/B testing as the first step.** We do not yet know which step to test. It comes after discovery.
- **Install-only users** (no events) cannot be analysed with event logs. They need separate funnel analysis from install data.

**Follow-up phase:** A/B test fixes for the steps and archetypes identified, to confirm they improve completion.

## 5. Use of LLMs (Reflection)

**How I used the LLM:** as a thinking partner, not an answer machine. I worked section by section. The LLM proposed drafts and explained unfamiliar concepts, and I decided which framing to keep, rewrote what did not fit, and tried to answer its questions before hearing its view.

**Where it helped clarify my thinking:** it showed me that "abandonment" has several possible definitions (did not finish onboarding, no second session within 7 days, never did the first key action), so defining it is part of the problem. It also explained that the problem-type list describes the kind of ML question being asked, not what the product team will do. That moved my first idea (ranking / recommendation, A/B testing) to its correct place: A/B testing is a later validation phase.

**Where I challenged or refined its output:** my own experience refined the data scope. I pointed out that apps send reminders when onboarding is unfinished, which led us to treat that reminder rule as a candidate weak label. I also suggested user groups such as "install but never open the app". Working through that example together, we realised those users produce no events, so they are invisible in event logs and need to be tracked through install data.

## 6. Unknown Methods Discovery

**New technique: survival analysis.**

**Why it is relevant:** many users have not dropped off yet, they simply have not had enough time. Counting them as drop-offs would make the problem look worse than it is. Survival analysis handles exactly this and also shows *when* users leave, not only where.

**In my own words:** survival analysis measures how long users stay in onboarding before they drop off. Users who have not dropped off *yet* are kept in the data as "still here so far", not counted as failures, so recent users do not make the drop-off rate look worse than it really is. For example, if 200 of 300 unfinished users installed only yesterday, they may still finish tomorrow, so only about 100 are real drop-offs so far.

**Other techniques met along the way:** process mining / Markov path analysis, and representation learning for event sequences.

**Incorrect assumption and how it was corrected:** I assumed users drop off at one specific page (for example the third). That is a hypothesis, not a fact. Correction: let the data show where users leave, because there may be several drop-off points and several different patterns.
