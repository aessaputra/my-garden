---
{"dg-publish":true,"permalink":"/alternative-text-must-preserve-purpose-not-merely-exist/","title":"Alternative text must preserve purpose, not merely exist","hideInFiletree":true,"tags":["development","ui","testing"],"noteIcon":"","dg-note-properties":{"title":"Alternative text must preserve purpose, not merely exist","categories":["Web Accessibility"],"tags":["development","ui","testing"],"created":"2026-09-06","updated":"2026-09-06"}}
---

An image passes an attribute check while still withholding the information a user actually needs.

Alternative text must represent the image's purpose within its surrounding content, rather than mechanically describing every visible detail.

The [WAI decision tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) separates informative, functional, textual, complex, and decorative images because each demands different treatment.

A linked logo names its destination, a chart needs its conclusion, and a decorative flourish usually needs `alt=""`.

When [[Semantics make accessibility resilient across interfaces\|semantics expose relationships]], [[Keyboard access exposes pointer-only architecture\|operable controls preserve actions]] that imagery may otherwise conceal.

Captions, nearby text, and accessible names can provide the equivalent when repeating them in `alt` would create noise.

Automated checks can detect missing attributes, but cannot reliably judge accuracy, relevance, redundancy, or contextual purpose.

Write the alternative after identifying what changes for the reader when the image becomes unavailable.
