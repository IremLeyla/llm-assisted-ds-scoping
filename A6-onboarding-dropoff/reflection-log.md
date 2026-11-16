# A6 Reflection Log

Running notes for Sections 5 and 6 of the scoping document. Add entries as they happen.

## Where the LLM helped clarify my thinking

- Showed that "abandonment" has several possible definitions (did not finish onboarding, no second session within 7 days, never did the first key action), and that choosing one is part of the problem.

## Where I challenged or corrected the LLM / where I was corrected

- I first read the problem-type list as "what I would recommend the product team do" (ranking / recommendation, A/B testing). The list actually describes the type of ML problem. Correction: A/B testing is causal analysis and fits as a later validation phase, not the discovery phase.

## Incorrect assumptions and how they were corrected

- **Assumption:** users drop off at one specific page (e.g. the third). **Correction:** treat it as a hypothesis, and let the data show where and in how many patterns users leave.
- **Assumption:** one row per user is the right granularity. **Correction:** raw data is event-level, and aggregating to one row per user too early loses the order of events, which is what path analysis needs.
- **Assumption:** a supervised classifier is the natural first model. **Correction:** it depends on a label we have not validated, and it answers "who", not "where / why".

## Methods I did not know before

- **Survival analysis:** models time until drop-off and keeps not-yet-dropped users as censored instead of counting them as failures.
- Process mining / Markov path analysis, and representation learning for event sequences (met, not explored in depth).
