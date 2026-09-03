---
{"dg-publish":true,"permalink":"/parameterized-queries-separate-data-from-commands/","title":"Parameterized queries separate data from commands","hideInFiletree":true,"tags":["security","programming"],"noteIcon":"","dg-note-properties":{"title":"Parameterized queries separate data from commands","categories":["Web Security"],"tags":["security","programming"],"sources":["_raw/articles/web-security-knowledge-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A login form accepts `' OR '1'='1` and hands the attacker every row of the users table.

I treat parameterized queries as the boundary that keeps untrusted input as data, never as executable structure.

OWASP [counts injection among the 2025 categories](https://owasp.org/Top10/2025/0x00_2025-Introduction), covering SQL, NoSQL, operating system commands, and cross-site scripting.

Allowlist validation and context aware escaping help, but neither ever replaces parameterization.

If [[Strict CSP contains injected scripts by default\|Strict CSP contains injected scripts by default]] limits script impact, then [[Updates and logging close the loop attackers exploit\|Updates and logging close the loop attackers exploit]] catches what slips past input handling.

I parameterize every untrusted input first, then add validation as a deliberate second layer.
