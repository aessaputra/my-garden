---
{"dg-publish":true,"permalink":"/field-data-should-judge-users-while-lab-data-diagnoses-causes/","title":"Field data should judge users while lab data diagnoses causes","hideInFiletree":true,"tags":["performance","testing","development"],"noteIcon":"","dg-note-properties":{"title":"Field data should judge users while lab data diagnoses causes","categories":["Web Performance","Tests"],"tags":["performance","testing","development"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Laboratory tests provide repeatable conditions, while field telemetry records the heterogeneous environments users actually encounter.

Those datasets can disagree because devices, networks, caches, locations, interactions, traffic, and page behavior differ.

[Web.dev distinguishes](https://web.dev/articles/lab-and-field-data-differences) controlled lab conditions from measurements gathered during real visits.

Field percentiles should judge outcomes, whereas traces and synthetic tests help isolate causes before deployment.

If [[Fast loading requires reducing both latency and main-thread work\|lab experiments isolate causes]], [[Responsiveness must be measured across the entire visit\|field measurements reveal production outcomes]] across user cohorts.

Segment results by route, device, geography, and release to avoid hiding vulnerable cohorts inside averages.

Use laboratory evidence for diagnosis, field evidence for impact, and both sources for regression control.
