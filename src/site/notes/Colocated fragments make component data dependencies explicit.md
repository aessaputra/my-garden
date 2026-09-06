---
{"dg-publish":true,"permalink":"/colocated-fragments-make-component-data-dependencies-explicit/","title":"Colocated fragments make component data dependencies explicit","hideInFiletree":true,"tags":["react","architecture"],"noteIcon":"","dg-note-properties":{"title":"Colocated fragments make component data dependencies explicit","categories":["Frameworks"],"tags":["react","architecture"],"created":"2026-09-06","updated":"2026-09-06"}}
---


A component change should reveal its data requirements without forcing developers to inspect unrelated screens.

Relay fragments place those requirements beside components, making the dependency boundary explicit during routine maintenance.

The [Relay overview](https://relay.dev/) explains that fragments join parent queries rather than fetching independently themselves.

The compiler combines these selections and generates artifacts, moving query processing into the build pipeline.

This arrangement reduces manual coordination, but adopting it still requires schema access and compiler integration.

If [[Normalized caches need deliberate update policies\|Normalized caches need deliberate update policies]], explicit selections clarify which records each component actually consumes.

Because [[Optimistic updates predict success rather than confirm it\|Optimistic updates predict success rather than confirm it]], mutation fragments must also reflect changing component requirements.

Choose colocation when maintaining shared data contracts costs more than the additional build discipline required.
