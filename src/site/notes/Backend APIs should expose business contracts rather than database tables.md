---
{"dg-publish":true,"permalink":"/backend-ap-is-should-expose-business-contracts-rather-than-database-tables/","title":"Backend APIs should expose business contracts rather than database tables","hideInFiletree":true,"tags":["backend","programming"],"noteIcon":"","dg-note-properties":{"title":"Backend APIs should expose business contracts rather than database tables","categories":["Backend Systems"],"tags":["backend","programming"],"created":"2026-09-07","updated":"2026-09-07"}}
---


An order endpoint should describe an order, not force clients to understand internal storage tables.

I would define the public contract around business operations before choosing how persistence represents them internally.

[Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design) advises against APIs that mirror database structure because clients should not depend on implementation details.

Request fields, response shapes, and failure behavior deserve documentation alongside the resource names clients already use.

If [[Integration tests catch contract mismatch\|integration tests expose mismatches]], then [[Dynamic inputs require runtime validation despite static types\|boundary validation]] keeps the contract enforceable when external values arrive.

This separation creates room for storage changes without promising that every API evolution remains backward compatible.

Treat the API as a deliberate public agreement, then test changes against real consumer expectations.
