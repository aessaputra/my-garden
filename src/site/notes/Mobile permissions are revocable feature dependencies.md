---
{"dg-publish":true,"permalink":"/mobile-permissions-are-revocable-feature-dependencies/","title":"Mobile permissions are revocable feature dependencies","hideInFiletree":true,"tags":["security","testing"],"noteIcon":"","dg-note-properties":{"title":"Mobile permissions are revocable feature dependencies","categories":["Security Practices"],"tags":["security","testing"],"created":"2026-09-06","updated":"2026-09-06"}}
---


A mobile feature cannot treat an earlier permission grant as permanent access to private data.

[Android guidance](https://developer.android.com/training/permissions/requesting) requires checking permission again whenever protected application functionality is needed.

The same guidance recommends contextual requests and graceful degradation after denial or revocation. Availability belongs in the feature contract, rather than an exceptional crash path.

I would separate the user's goal from the preferred sensor, then define what remains possible without access. Manual location entry is one possible design option, not an Android requirement.

Because [[Shared mobile code still needs platform boundaries\|Shared mobile code still needs platform boundaries]], permission adapters must preserve platform rules. Likewise, [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]] prevents installation from becoming an access assumption.

Tests should cover granted, denied, and revoked states before claiming a workflow remains usable. These notes describe documented Android behavior, not verified permission equivalence across iOS versions.

Design permission loss as a supported state, rather than demanding consent to continue unrelated work.
