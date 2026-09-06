---
{"dg-publish":true,"permalink":"/optimistic-updates-predict-success-rather-than-confirm-it/","title":"Optimistic updates predict success rather than confirm it","hideInFiletree":true,"tags":["react","architecture"],"noteIcon":"","dg-note-properties":{"title":"Optimistic updates predict success rather than confirm it","categories":["Frameworks"],"tags":["react","architecture"],"created":"2026-09-06","updated":"2026-09-06"}}
---


An immediately updated counter shows a prediction, not proof that the server accepted the action.

Relay writes optimistic data before receiving a response, allowing selected components to display anticipated changes immediately.

The [mutation guide](https://relay.dev/docs/guided-tour/updating-data/graphql-mutations/) documents rollback on failure and replacement with server data after success.

Concurrent optimistic changes deserve special care when their predicted values depend on previously observed store values.

A later prediction can preserve an incorrect count after an earlier operation fails and rolls back.

If [[Normalized caches need deliberate update policies\|Normalized caches need deliberate update policies]], optimistic reconciliation must respect shared records and overlapping operations.

Because [[Colocated fragments make component data dependencies explicit\|Colocated fragments make component data dependencies explicit]], changing fragment selections also changes the optimistic payload contract.

Use immediate feedback only with visible failure handling and tests covering overlapping actions and rollback.
