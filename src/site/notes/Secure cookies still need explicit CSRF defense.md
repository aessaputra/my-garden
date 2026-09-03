---
{"dg-publish":true,"permalink":"/secure-cookies-still-need-explicit-csrf-defense/","title":"Secure cookies still need explicit CSRF defense","hideInFiletree":true,"tags":["security","auth"],"noteIcon":"","dg-note-properties":{"title":"Secure cookies still need explicit CSRF defense","categories":["Web Security"],"tags":["security","auth"],"sources":["_raw/articles/web-security-knowledge-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A logged in user opens a malicious page, and the browser fires an authenticated transfer request.

I treat Secure, HttpOnly, and SameSite cookies as necessary hygiene, never as CSRF protection by itself.

OWASP [places CSRF inside broken access control](https://owasp.org/Top10/2025/0x00_2025-Introduction) in the 2025 categories, so risky actions need explicit token checks.

The server must verify an unpredictable anti-CSRF token on every state changing request it accepts.

If [[HTTPS protects transit, not application logic\|HTTPS protects transit, not application logic]] secures the channel, then [[Strict CSP contains injected scripts by default\|Strict CSP contains injected scripts by default]] shrinks what a forged request can achieve.

I set hardened cookie flags and verify tokens before honoring any risky action.
