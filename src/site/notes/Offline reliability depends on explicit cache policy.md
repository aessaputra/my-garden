---
{"dg-publish":true,"permalink":"/offline-reliability-depends-on-explicit-cache-policy/","title":"Offline reliability depends on explicit cache policy","hideInFiletree":true,"tags":["javascript","programming"],"noteIcon":"","dg-note-properties":{"title":"Offline reliability depends on explicit cache policy","categories":["Web APIs"],"tags":["javascript","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---


A cached application shell is useful, but users also need predictable behavior when requested data is missing.

I define offline reliability through explicit cache policy rather than the mere presence of a worker.

[MDN explains cache strategy tradeoffs](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation): faster cached responses can preserve outdated content.

A network first policy instead prioritizes fresh responses, using cached content when network access fails.

Choose policies per resource, state what remains available, and make unavailable actions visible to users.

Since [[Installing a PWA does not guarantee offline operation\|Installing a PWA does not guarantee offline operation]], verify disconnected tasks independently from installation.

If [[Cross-platform PWAs still need capability-based fallbacks\|Cross-platform PWAs still need capability-based fallbacks]], background retries cannot be the only recovery path.

Reliable offline behavior starts with a bounded promise that the application can demonstrably keep.
