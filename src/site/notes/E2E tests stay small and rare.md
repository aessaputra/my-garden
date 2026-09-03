---
{"dg-publish":true,"permalink":"/e2-e-tests-stay-small-and-rare/","title":"E2E tests stay small and rare","hideInFiletree":true,"tags":["testing","ci-cd"],"noteIcon":"","dg-note-properties":{"title":"E2E tests stay small and rare","categories":["Tests"],"tags":["testing","ci-cd"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

Two hundred browser tests run two hours each release, then three fail randomly with no code changed.

They feel convincing because they mimic real clicks, yet the suite grows slow and ignored.

I keep many unit tests below, some integration tests midstream, and very few E2E tests.

CircleCI [summarizes the pyramid with clear ratios](https://circleci.com/blog/testing-pyramid) as seventy unit, twenty integration, and ten E2E.

I reserve E2E tests for login, payment, and checkout so releases stay fast and calm.

If [[Small unit tests give fast feedback\|Small unit tests give fast feedback]] grows thick and [[Integration tests catch contract mismatch\|Integration tests catch contract mismatch]] holds, E2E stays light.

I choose this shape whenever daily speed matters without losing release confidence.
