---
{"dg-publish":true,"permalink":"/web-ap-is-live-in-the-runtime-not-the-language/","title":"Web APIs live in the runtime, not the language","hideInFiletree":true,"tags":["javascript","programming"],"noteIcon":"","dg-note-properties":{"title":"Web APIs live in the runtime, not the language","categories":["Web APIs"],"tags":["javascript","programming"],"sources":["_raw/articles/web-apis-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

JavaScript alone cannot touch the page, the network, or the camera without help from its host.

I treat every Web API as a runtime capability, never as syntax I can assume everywhere.

MDN [separates browser APIs from third party APIs](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction) by where their code ships and runs.

Browser APIs expose the device and the page through objects the runtime owns and maintains.

Third party APIs arrive as libraries instead, so versioning and availability stay the caller's burden.

If [[DOM turns documents into scriptable objects\|DOM turns documents into scriptable objects]] works for pages, [[fetch() treats network as promises with explicit control\|fetch() treats network as promises with explicit control]] extends it to servers.

I check the runtime first whenever an API feels missing or behaves differently.
