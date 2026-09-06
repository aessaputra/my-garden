---
{"dg-publish":true,"permalink":"/templates-stay-inert-while-slots-preserve-consumer-ownership/","title":"Templates stay inert while slots preserve consumer ownership","hideInFiletree":true,"tags":["javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Templates stay inert while slots preserve consumer ownership","categories":["Web Components"],"tags":["javascript","development","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Repeated component markup needs a reusable structure without rendering every definition immediately into the document.

Template content remains inert until code clones or adopts it, making structure available without immediate presentation.

[MDN explains](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_templates_and_slots) that template contents remain unrendered while JavaScript can reference and reuse them.

Slots place consumer-owned light DOM into documented insertion points, preserving composition without transferring node ownership.

When [[Shadow DOM reduces collisions without eliminating integration work\|shadow boundaries contain structure]], [[Custom elements make HTML the portability boundary\|custom elements provide lifecycle behavior]] around that composition.

Neither primitive supplies reactivity, sanitization, state management, diffing, or an application rendering architecture automatically.

Document named slots, fallback content, accepted semantics, and update behavior before treating templates as a component API.
