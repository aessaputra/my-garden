---
{"dg-publish":true,"permalink":"/semantics-make-accessibility-resilient-across-interfaces/","title":"Semantics make accessibility resilient across interfaces","hideInFiletree":true,"tags":["development","ui","programming"],"noteIcon":"","dg-note-properties":{"title":"Semantics make accessibility resilient across interfaces","categories":["Web Accessibility"],"tags":["development","ui","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Accessible interfaces begin with meaning that software can interpret, not visual resemblance alone.

Native elements expose roles, names, states, and relationships through browser accessibility APIs with established behavior.

[W3C accessibility principles](https://www.w3.org/WAI/fundamentals/accessibility-principles/) explain how content, browsers, assistive technologies, and authoring tools depend upon this shared information.

ARIA can supplement missing semantics, but it does not add keyboard behavior, focus management, or reliable interaction automatically.

When [[Keyboard access exposes pointer-only architecture\|keyboard operation exposes assumptions]], [[Alternative text must preserve purpose, not merely exist\|purposeful alternatives preserve meaning]] across diverse interfaces.

Semantic structure also supports headings, form labels, landmarks, adaptable presentation, and clearer automated diagnostics.

Custom controls remain justified when native elements cannot express required behavior, provided their complete interaction contract is implemented and tested.

Choose the strongest native semantic foundation first, then add ARIA only for information HTML cannot express.
