---
{"dg-publish":true,"permalink":"/strict-csp-contains-injected-scripts-by-default/","title":"Strict CSP contains injected scripts by default","hideInFiletree":true,"tags":["security","programming","http"],"noteIcon":"","dg-note-properties":{"title":"Strict CSP contains injected scripts by default","categories":["Web Security"],"tags":["security","programming","http"],"sources":["_raw/articles/web-security-knowledge-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

An attacker hides a script inside a comment field, and every visitor silently runs it.

I treat strict Content Security Policy as the containment layer that stops injected scripts from executing.

MDN [requires the policy in a response header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) on every response, written as semicolon separated directives.

Strict policies use a fresh nonce or hash per response instead of broad origin lists attackers can abuse.

If [[Parameterized queries separate data from commands\|Parameterized queries separate data from commands]] guards the server, then [[Secure cookies still need explicit CSRF defense\|Secure cookies still need explicit CSRF defense]] guards the session it issues.

I ship nonce based CSP knowing it layers over validation rather than replacing it.
