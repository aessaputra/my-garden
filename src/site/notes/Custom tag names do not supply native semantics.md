---
{"dg-publish":true,"permalink":"/custom-tag-names-do-not-supply-native-semantics/","title":"Custom tag names do not supply native semantics","hideInFiletree":true,"tags":["javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Custom tag names do not supply native semantics","categories":["Web Components"],"tags":["javascript","development","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Naming an element like a button does not give it native keyboard behavior or accessibility semantics.

A compact custom tag can hide implementation complexity while leaving essential interaction responsibilities entirely with its author.

[HTML explains](https://html.spec.whatwg.org/multipage/custom-elements.html) that browsers and accessibility tools do not recognize button semantics from a custom name.

Authors must provide appropriate names, states, focus handling, and keyboard interactions whenever native behavior is not inherited.

If [[Semantics make accessibility resilient across interfaces\|semantics preserve meaning]], [[Keyboard access exposes pointer-only architecture\|keyboard testing]] must verify behavior beyond a component's visual appearance.

ElementInternals supports default accessibility semantics and form integration, but those hooks still require a complete implementation.

Prefer native controls within reusable components when possible, and justify replacements through demonstrated interaction requirements rather than shorter markup.
