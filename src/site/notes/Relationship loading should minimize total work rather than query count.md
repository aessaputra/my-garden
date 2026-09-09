---
{"dg-publish":true,"permalink":"/relationship-loading-should-minimize-total-work-rather-than-query-count/","title":"Relationship loading should minimize total work rather than query count","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"Relationship loading should minimize total work rather than query count","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---


Reducing repeated database round trips does not require forcing every relationship into one giant join. The loading strategy should preserve the required result while controlling network calls, returned rows, and materialization work.

[SQLAlchemy](https://docs.sqlalchemy.org/en/20/orm/queryguide/relationships.html) distinguishes joined loading from select-in loading. Joining a collection multiplies result rows; select-in loading fetches related records for a group of parent keys. The latter can use several bounded batches rather than exactly two queries. A small many-to-one relationship and a large collection therefore need not share the same loading plan.

This resolves [[References/N plus one problem\|N plus one problem]] at the access-pattern level: replace repeated per-item fetching with deliberate relationship loading. A useful regression check compares query growth as the parent list grows, then measures latency and result volume instead of treating query count as the final outcome.

The boundary matters. [SQLite](https://www.sqlite.org/np1queryprob.html) runs in the application process, so small queries do not incur client/server message round trips. Its counterexample qualifies the general warning, not the need to inspect expensive queries. [[References/Relational Databases\|Relational Databases]] supplies the complementary concern: indexes and execution plans still determine work inside each query.
