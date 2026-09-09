---
{"dg-publish":true,"permalink":"/database-constraints-enforce-shared-invariants-at-the-write-boundary/","title":"Database constraints enforce shared invariants at the write boundary","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"Database constraints enforce shared invariants at the write boundary","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---


Database constraints enforce declared data rules wherever ordinary writes reach the table, rather than depending on every application path to repeat a check. In a hypothetical order system, a foreign key can reject an unknown customer even when an import bypasses the usual form.

[PostgreSQL constraints](https://www.postgresql.org/docs/18/ddl-constraints.html) distinguish unique, non-null primary keys from foreign keys that preserve referential integrity. A nullable foreign key still permits missing references. Likewise, `CHECK (quantity > 0)` needs `NOT NULL` if missing quantities are invalid: a check evaluating to null passes.

This supports [[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]]: constraints define admissible states, while a transaction groups dependent changes. Neither mechanism invents the business rule. Declare the invariant first, choose a constraint that expresses it, and handle rejection explicitly.

The boundary matters. PostgreSQL row checks cannot safely enforce arbitrary conditions over other rows or tables. Rules that cannot be expressed by supported constraints need an appropriate transaction and concurrency strategy, not merely a pre-write application check. [[Transaction isolation determines what concurrent operations may observe\|Transaction isolation determines what concurrent operations may observe]] explains why the read used for that check also matters.
