---
{"dg-publish":true,"permalink":"/cross-platform-pw-as-still-need-capability-based-fallbacks/","title":"Cross-platform PWAs still need capability-based fallbacks","hideInFiletree":true,"tags":["javascript","programming"],"noteIcon":"","dg-note-properties":{"title":"Cross-platform PWAs still need capability-based fallbacks","categories":["Web APIs"],"tags":["javascript","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---


One web codebase can reach several platforms without giving every user the same browser capabilities.

I choose progressive enhancement so essential workflows remain usable when optional platform integrations are unavailable.

[MDN recommends feature detection and fallbacks](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app) for advanced APIs used across different browsers.

Installation interfaces, notification permissions, and background execution therefore belong in the product compatibility plan explicitly.

Shared implementation can reduce duplicated work, but compatibility testing still contributes to the total delivery cost.

If [[Installing a PWA does not guarantee offline operation\|Installing a PWA does not guarantee offline operation]], platform reach alone cannot establish feature parity.

Because [[Offline reliability depends on explicit cache policy\|Offline reliability depends on explicit cache policy]], unsupported background features need visible recovery alternatives.

Choose the web platform when its tested capabilities satisfy the actual workflows users depend on.
