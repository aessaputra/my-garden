---
{"dg-publish":true,"permalink":"/applied-migrations-preserve-history-when-corrections-move-forward/","title":"Applied migrations preserve history when corrections move forward","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"Applied migrations preserve history when corrections move forward","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---


Editing an already-applied migration changes the recorded instructions without changing the database that previously ran them. A new environment may then build a different schema from what appears to be the same history.

[Flyway versioned migrations](https://documentation.red-gate.com/flyway/flyway-concepts/migrations/versioned-migrations) record versions and checksums and recommend adding a new migration once a change has reached a permanent downstream environment. The reusable principle is to correct shared history by moving forward, not silently rewriting what other databases already consumed.

This supports [[References/Database Migrations\|Database Migrations]] as a reproducible change process, but the execution ledger is not proof that every script is safe to rerun. [Flyway repeatable migrations](https://documentation.red-gate.com/flyway/flyway-concepts/migrations/repeatable-migrations) deliberately run again when their checksum changes; their authors must make repeated execution safe.

Keep the distinction explicit: an ordered migration history controls which changes are due, while idempotency describes what repeating an operation does. Neither validates the business meaning of a data transformation. [[Schema changes need compatibility until old clients retire\|Schema changes need compatibility until old clients retire]] adds the separate requirement that applications remain able to use the intermediate schema.
