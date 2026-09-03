---
{"dg-publish":true,"permalink":"/history-api-restores-back-button-behavior-in-sp-as/","title":"History API restores back button behavior in SPAs","hideInFiletree":true,"tags":["javascript","network"],"noteIcon":"","dg-note-properties":{"title":"History API restores back button behavior in SPAs","categories":["Web APIs"],"tags":["javascript","network"],"sources":["_raw/articles/web-apis-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A single page shop swaps products without reloads, then its Back button jumps somewhere unexpected.

I push a history entry for each view so Back returns to the previous rendered state.

MDN [shows pushState fixing SPA navigation](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API) by synthesizing entries the browser can traverse.

The popstate event fires when the active entry changes, letting scripts rerender the matching view.

State travels inside the entry object, so reloads and shares need separately designed fallbacks.

If [[fetch() treats network as promises with explicit control\|fetch() treats network as promises with explicit control]] loads the data, [[DOM turns documents into scriptable objects\|DOM turns documents into scriptable objects]] renders it without a reload.

I pair every content swap with a history entry whenever Back must keep working.
