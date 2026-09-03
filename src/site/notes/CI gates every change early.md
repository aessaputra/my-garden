---
{"dg-publish":true,"permalink":"/ci-gates-every-change-early/","title":"CI gates every change early","hideInFiletree":true,"tags":["testing","ci-cd","devops"],"noteIcon":"","dg-note-properties":{"title":"CI gates every change early","categories":["Tests"],"tags":["testing","ci-cd","devops"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A bug surfaces in staging one day before release, forcing overtime and panicked rollback.

Earlier automated tests would have caught it small and cheap while context stayed fresh.

I run automated checks on every change through CI pipelines so mistakes surface immediately.

Harness [describes CI testing as pipeline quality gates](https://www.harness.io/blog/testing-methodologies-for-cd-pipelines) asking safe to proceed at each stage.

Unit tests run per commit, integration tests per merge, and E2E tests before release.

Pass means proceed and fail means stop, so broken code never slips through quietly.

If [[E2E tests stay small and rare\|E2E tests stay small and rare]] sets proportion, then [[BDD agrees on examples before code\|BDD agrees on examples before code]] supplies release checks.

I use it when suites run fast enough that every failure clearly names its owner.
