---
{"dg-publish":true,"permalink":"/custom-element-initialization-must-survive-reconnection/","title":"Custom element initialization must survive reconnection","hideInFiletree":true,"tags":["javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Custom element initialization must survive reconnection","categories":["Web Components"],"tags":["javascript","development","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---

A custom element can reconnect repeatedly, so initialization must distinguish permanent setup from connection-dependent work.

Moving an existing widget can trigger cleanup and setup again, even when its application identity remains unchanged.

[HTML requires](https://html.spec.whatwg.org/multipage/custom-elements.html) guarding genuinely one-time initialization because connection callbacks may run more than once.

Register observers and external subscriptions deliberately, release connection-bound resources on disconnection, and restore them when needed.

Because [[Custom elements make HTML the portability boundary\|HTML elements cross environments]], [[Public properties and events determine framework interoperability\|public contracts]] must remain stable across reconnections.

A permanent initialized flag cannot replace reconnection logic when cleanup has already removed necessary subscriptions or observers.

Test removal and reinsertion alongside initial mounting, rather than treating the first successful render as sufficient lifecycle coverage.
