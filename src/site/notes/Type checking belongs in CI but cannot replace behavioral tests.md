---
{"dg-publish":true,"permalink":"/type-checking-belongs-in-ci-but-cannot-replace-behavioral-tests/","title":"Type checking belongs in CI but cannot replace behavioral tests","hideInFiletree":true,"tags":["programming","testing","ci-cd"],"noteIcon":"","dg-note-properties":{"title":"Type checking belongs in CI but cannot replace behavioral tests","categories":["Tests","Code Quality"],"tags":["programming","testing","ci-cd"],"created":"2026-09-06","updated":"2026-09-06"}}
---

A type checker only prevents regressions when every developer and automation job applies consistent rules.

[Mypy recommends](https://mypy.readthedocs.io/en/stable/existing_code.html) pinning versions, sharing configuration, checking identical files, and running analysis in CI.

Gradual adoption works when teams establish a passing baseline, then tighten coverage without flooding reviews.

Permissive settings, ignored modules, missing dependency types, and broad escape hatches weaken the resulting signal.

If [[CI gates every change early\|CI gates every change early]] enforces analysis, [[Integration tests catch contract mismatch\|Integration tests catch contract mismatch]] still exercises real component connections.

[[E2E tests stay small and rare\|E2E tests stay small and rare]] then samples critical journeys that static models cannot execute.

Gate type errors early, but retain layered tests for behavior, integration, infrastructure, and user outcomes.
