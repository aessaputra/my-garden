---
{"dg-publish":true,"permalink":"/structural-typing-matches-java-script-shapes-without-proving-identity/","title":"Structural typing matches JavaScript shapes without proving identity","hideInFiletree":true,"tags":["programming","javascript","development"],"noteIcon":"","dg-note-properties":{"title":"Structural typing matches JavaScript shapes without proving identity","categories":["Type Systems"],"tags":["programming","javascript","development"],"created":"2026-09-06","updated":"2026-09-06"}}
---

TypeScript compares object members rather than requiring declarations to share an explicit nominal identity.

[The compatibility handbook](https://www.typescriptlang.org/docs/handbook/type-compatibility.html) calls this structural subtyping, matching common JavaScript object and function patterns.

Objects with the required compatible members can satisfy interfaces even when they never declared them.

This flexibility improves interoperability, yet selected compatibility rules deliberately permit unsound but common patterns.

[[TypeScript extends JavaScript without changing runtime behavior\|Runtime continuity]] explains the design, while [[Strict TypeScript settings turn annotations into stronger feedback\|strict settings]] reject some risky assignments.

Because shape agreement cannot validate provenance or behavior, runtime boundaries and tests retain separate responsibilities.

Prefer structural compatibility for composition, then tighten function variance and review casts where assumptions become consequential.
