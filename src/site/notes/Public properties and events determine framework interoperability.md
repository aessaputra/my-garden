---
{"dg-publish":true,"permalink":"/public-properties-and-events-determine-framework-interoperability/","title":"Public properties and events determine framework interoperability","hideInFiletree":true,"tags":["javascript","frameworks","development"],"noteIcon":"","dg-note-properties":{"title":"Public properties and events determine framework interoperability","categories":["Web Components"],"tags":["javascript","frameworks","development"],"created":"2026-09-06","updated":"2026-09-06"}}
---

A browser can support custom elements while a framework still passes data or events unexpectedly.

Attributes serialize strings in markup, whereas properties can carry objects, functions, and other JavaScript values.

[React documents](https://react.dev/reference/react-dom/components#custom-html-elements) runtime rules for choosing attribute assignment, property assignment, and custom event listeners.

Consumers also need stable event names, bubbling rules, composition behavior, methods, reflection, and upgrade timing.

If [[Custom elements make HTML the portability boundary\|HTML provides portability]], [[Templates stay inert while slots preserve consumer ownership\|slot contracts preserve composition]] across diverse framework renderers.

Interoperability claims should therefore name tested framework versions, server-rendering paths, data shapes, and event behavior.

Design the public element contract first, then verify it through native DOM tests and framework-specific integration suites.
