---
{"dg-publish":true,"permalink":"/small-unit-tests-give-fast-feedback/","title":"Small unit tests give fast feedback","hideInFiletree":true,"tags":["testing","programming"],"noteIcon":"","dg-note-properties":{"title":"Small unit tests give fast feedback","categories":["Tests"],"tags":["testing","programming"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A one thousand rupiah discount error can silently corrupt every checkout receipt for a whole day.

I test the smallest unit in isolation so logic mistakes surface within seconds, not days.

Atlassian [describes unit tests as the fast foundation](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing) teams rerun every single day.

I use mocks for APIs and databases so tests stay deterministic without depending on the network.

Frequent small tests build courage to refactor because every regression shows up immediately.

If [[E2E tests stay small and rare\|E2E tests stay small and rare]] shapes the suite, then [[CI gates every change early\|CI gates every change early]] turns speed into habit.

I keep unit tests largest whenever fast cheap signal matters more than realism.
