---
{"dg-publish":true,"permalink":"/responsiveness-must-be-measured-across-the-entire-visit/","title":"Responsiveness must be measured across the entire visit","hideInFiletree":true,"tags":["performance","development","javascript"],"noteIcon":"","dg-note-properties":{"title":"Responsiveness must be measured across the entire visit","categories":["Web Performance"],"tags":["performance","development","javascript"],"created":"2026-09-06","updated":"2026-09-06"}}
---

A page can render quickly, then become sluggish after hydration, navigation, filtering, or repeated user actions.

INP observes click, tap, and keyboard latency throughout a visit instead of sampling initial loading alone.

[Web.dev defines](https://web.dev/articles/inp) responsiveness through the delay from interaction until the browser can present the next frame.

Long tasks, expensive handlers, synchronous layout, and excessive rendering keep the main thread unavailable for input.

[[Fast loading requires reducing both latency and main-thread work\|Fast loading]] prepares interaction, while [[Field data should judge users while lab data diagnoses causes\|field measurement]] reveals real-world cohorts that remain slow.

A good initial render therefore cannot compensate for persistent delays during the task users came to complete.

Profile the slowest meaningful interactions, divide long work, and verify improvement across the complete session.
