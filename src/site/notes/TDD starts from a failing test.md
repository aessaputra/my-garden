---
{"dg-publish":true,"permalink":"/tdd-starts-from-a-failing-test/","title":"TDD starts from a failing test","hideInFiletree":true,"tags":["testing","programming"],"noteIcon":"","dg-note-properties":{"title":"TDD starts from a failing test","categories":["Tests"],"tags":["testing","programming"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

Finished discount code resists tests, so each small change feels risky and slow.

I write one failing test first, then add the simplest code until that test passes.

Fowler [describes TDD as Red Green Refactor](https://martinfowler.com/bliki/TestDrivenDevelopment.html) repeated in tiny increments from Kent Beck.

I start from a small test list to know what to build and when to stop adding code.

Code born from tests usually stays testable because coverage is designed in, not patched.

If [[Small unit tests give fast feedback\|Small unit tests give fast feedback]] is the target, then [[Integration tests catch contract mismatch\|Integration tests catch contract mismatch]] covers seams.

I use it for clear new logic, not for exploring unstable UI ideas.
