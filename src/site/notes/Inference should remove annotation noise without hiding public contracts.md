---
{"dg-publish":true,"permalink":"/inference-should-remove-annotation-noise-without-hiding-public-contracts/","title":"Inference should remove annotation noise without hiding public contracts","hideInFiletree":true,"tags":["programming","development","testing"],"noteIcon":"","dg-note-properties":{"title":"Inference should remove annotation noise without hiding public contracts","categories":["Code Quality"],"tags":["programming","development","testing"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Modern type checkers infer many local types from initializers, control flow, calls, and surrounding context.

[TypeScript recommends](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) fewer annotations where inference already communicates the intended type precisely.

That economy keeps implementation code readable, but exported functions still benefit from deliberate and stable contracts.

Flow even requires selected boundary annotations so modules can be checked independently and predictably.

[[Type checking catches contradictions before execution, not every runtime failure\|Type checking catches contradictions before execution, not every runtime failure]] verifies compatibility through static analysis.

[[Dynamic inputs require runtime validation despite static types\|Dynamic inputs require runtime validation despite static types]] protects external boundaries through explicit parsing and checks.

Explicit boundary types connect those layers by documenting assumptions that callers and validators must preserve.

Infer obvious locals, annotate architectural boundaries, and review every escape hatch that erases useful information.
