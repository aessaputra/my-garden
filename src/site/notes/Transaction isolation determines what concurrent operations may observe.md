---
{"dg-publish":true,"permalink":"/transaction-isolation-determines-what-concurrent-operations-may-observe/","title":"Transaction isolation determines what concurrent operations may observe","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"Transaction isolation determines what concurrent operations may observe","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---


A transaction boundary does not by itself specify which concurrent changes a read can observe. Isolation defines that behavior, so it must match the invariant the application needs to preserve.

In [PostgreSQL 18](https://www.postgresql.org/docs/18/transaction-iso.html), the default `READ COMMITTED` level gives each ordinary query a snapshot taken when that query begins. Two queries inside one transaction can therefore return different results after another transaction commits. [MySQL 8.4 InnoDB](https://dev.mysql.com/doc/refman/8.4/en/innodb-transaction-isolation-levels.html) instead defaults to `REPEATABLE READ`; engine defaults are not interchangeable contracts.

[[Database transactions keep dependent writes from becoming partial outcomes\|Database transactions keep dependent writes from becoming partial outcomes]] concerns all-or-nothing changes, not a promise that every read shares one snapshot. PostgreSQL `SERIALIZABLE` constrains committed outcomes to those of some serial execution, but the application must be prepared to retry an entire transaction after a serialization failure.

The design implication is to specify the forbidden concurrent outcome before selecting isolation or locking. [[Database constraints enforce shared invariants at the write boundary\|Database constraints enforce shared invariants at the write boundary]] remains complementary: isolation coordinates concurrent operations, while constraints reject declared invalid states. Neither label alone proves the correctness of arbitrary business logic.
