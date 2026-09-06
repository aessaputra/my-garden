---
{"dg-publish":true,"permalink":"/normalized-caches-need-deliberate-update-policies/","title":"Normalized caches need deliberate update policies","hideInFiletree":true,"tags":["react","architecture"],"noteIcon":"","dg-note-properties":{"title":"Normalized caches need deliberate update policies","categories":["Frameworks"],"tags":["react","architecture"],"created":"2026-09-06","updated":"2026-09-06"}}
---


A shared record can keep several screens aligned, but normalization cannot infer every intended application change.

Relay stores records by identity and merges returned fields, rather than treating every query response independently.

Its [runtime design](https://relay.dev/docs/principles-and-architecture/runtime-architecture/) describes an in-memory graph with targeted notifications for changed results.

Stable identities make merging reliable, while relationship changes can still require directives or explicit updater logic.

Fetch policies separately determine cache reuse and network access; locally available data does not prove server freshness.

If [[Colocated fragments make component data dependencies explicit\|Colocated fragments make component data dependencies explicit]], returned selections define the information available for reconciliation.

Because [[Optimistic updates predict success rather than confirm it\|Optimistic updates predict success rather than confirm it]], temporary values need reconciliation with authoritative server responses.

Treat normalized storage as coordination infrastructure, with deliberate policies for freshness, relationships, and mutation effects.
