---
{"dg-publish":true,"permalink":"/hardware-ap-is-demand-permission-and-secure-context/","title":"Hardware APIs demand permission and secure context","hideInFiletree":true,"tags":["javascript","security"],"noteIcon":"","dg-note-properties":{"title":"Hardware APIs demand permission and secure context","categories":["Web APIs"],"tags":["javascript","security"],"sources":["_raw/articles/web-apis-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A video call button that silently fails on HTTP teaches users to distrust the whole product.

I request camera and microphone through constraints and handle denial as a normal outcome.

MDN [documents getUserMedia permission behavior](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia) including rejection over insecure contexts and blocked policies.

The promise resolves with a MediaStream on success and rejects with named errors otherwise.

Permissions Policy can block sources entirely, so feature checks must precede every call.

If [[Web APIs live in the runtime, not the language\|Web APIs live in the runtime, not the language]] explains availability, [[fetch() treats network as promises with explicit control\|fetch() treats network as promises with explicit control]] contrasts with these gated reads.

I design a graceful fallback before requesting any device the page may not receive.
