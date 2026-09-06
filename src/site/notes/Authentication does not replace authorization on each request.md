---
{"dg-publish":true,"permalink":"/authentication-does-not-replace-authorization-on-each-request/","title":"Authentication does not replace authorization on each request","hideInFiletree":true,"tags":["backend","programming"],"noteIcon":"","dg-note-properties":{"title":"Authentication does not replace authorization on each request","categories":["Backend Systems"],"tags":["backend","programming"],"created":"2026-09-07","updated":"2026-09-07"}}
---


A logged-in customer may request another customer's order, so identity alone cannot justify access.

The server must evaluate whether this caller may perform this operation on this specific resource.

[OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html) requires permission validation on every request, regardless of how the request was initiated.

I would centralize default denial while keeping resource ownership and operation-specific rules explicit in application policy.

If [[Dynamic inputs require runtime validation despite static types\|inputs need validation]], then [[Integration tests catch contract mismatch\|integration tests]] should also exercise rejected operations across actual service boundaries.

A hidden button provides interface guidance, but it cannot enforce permissions against independently constructed requests.

Test forbidden actions as carefully as successful ones, including access attempts against another user's resources.
