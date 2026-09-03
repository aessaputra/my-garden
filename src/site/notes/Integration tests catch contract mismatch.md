---
{"dg-publish":true,"permalink":"/integration-tests-catch-contract-mismatch/","title":"Integration tests catch contract mismatch","hideInFiletree":true,"tags":["testing","programming"],"noteIcon":"","dg-note-properties":{"title":"Integration tests catch contract mismatch","categories":["Tests"],"tags":["testing","programming"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

Login succeeds but the username never appears because a data format changed quietly yesterday.

All unit tests stay green, so the fault lives in integration between modules, not inside functions.

I run integration tests with real data so format mismatch surfaces before it reaches production.

Semaphore [places integration tests in the middle](https://semaphore.io/blog/testing-pyramid) to check code against databases without UI clicks.

They run slower than unit tests yet far faster than full E2E tests across browsers.

If [[Small unit tests give fast feedback\|Small unit tests give fast feedback]] covers logic, then [[CI gates every change early\|CI gates every change early]] checks integration on each merge.

I use them whenever a new feature touches databases or other APIs before release.
