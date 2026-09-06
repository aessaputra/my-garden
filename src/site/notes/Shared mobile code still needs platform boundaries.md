---
{"dg-publish":true,"permalink":"/shared-mobile-code-still-needs-platform-boundaries/","title":"Shared mobile code still needs platform boundaries","hideInFiletree":true,"tags":["programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"Shared mobile code still needs platform boundaries","categories":["Frameworks"],"tags":["programming","architecture"],"created":"2026-09-06","updated":"2026-09-06"}}
---


Sharing mobile code reduces duplication, but platform boundaries still need deliberate ownership and testing.

[React Native](https://reactnative.dev/docs/platform-specific-code) explicitly supports platform detection and separate files when implementations diverge.

[Flutter](https://docs.flutter.dev/resources/architectural-overview) likewise combines shared application code with platform embedders and native interoperability.

I would share stable business rules first, then isolate integrations whose behavior depends on operating systems. This keeps platform exceptions visible rather than disguising them as portable logic.

If [[Mobile permissions are revocable feature dependencies\|Mobile permissions are revocable feature dependencies]], shared features need explicit failure states. Meanwhile, [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]] keeps packaging separate from implementation choices.

A camera workflow should therefore name its platform adapter, permission handling, and expected unavailable state. These are design recommendations, not evidence that one framework always costs less.

Choose reuse boundaries around verified behavior, not the promise of identical code everywhere.
