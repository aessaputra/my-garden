---
{"dg-publish":true,"permalink":"/schema-changes-need-compatibility-until-old-clients-retire/","title":"Schema changes need compatibility until old clients retire","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"Schema changes need compatibility until old clients retire","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---

A database schema can outlive the application release that introduced it because old and new clients may run during the same rollout. Removing a column before its last consumer retires turns a valid schema change into an application failure.

The [expand and contract pattern](https://www.prisma.io/dataguide/types/relational/expand-and-contract-pattern) introduces the new structure alongside the old one, accommodates writes, migrates existing data, validates results, and shifts reads before removing the legacy structure. New constraints must not reject writes that old clients still legitimately make. [[Database constraints enforce shared invariants at the write boundary\|Database constraints enforce shared invariants at the write boundary]] explains why that compatibility includes writes, not merely query shape.

Preserving the old columns alone does not guarantee a lossless rollback: new data may have meaning the old representation cannot express. Retire the old structure only after its consumers have moved and the recovery implications are accepted.

This qualifies [[References/Deployment\|Deployment]]: application rollout and schema retirement are separate decisions. It does not promise zero downtime. [PostgreSQL ALTER TABLE](https://www.postgresql.org/docs/18/sql-altertable.html) can acquire restrictive locks or rewrite data, so operational testing remains necessary even when both application versions understand the schema.
