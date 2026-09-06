---
{"dg-publish":true,"permalink":"/dynamic-inputs-require-runtime-validation-despite-static-types/","title":"Dynamic inputs require runtime validation despite static types","hideInFiletree":true,"tags":["programming","security","testing"],"noteIcon":"","dg-note-properties":{"title":"Dynamic inputs require runtime validation despite static types","categories":["Validation Practices"],"tags":["programming","security","testing"],"created":"2026-09-06","updated":"2026-09-06"}}
---

Network responses, files, environment variables, and user input arrive as values rather than trusted declarations.

Python documentation states that its [runtime does not enforce annotations](https://docs.python.org/3/library/typing.html), leaving enforcement to external tools.

TypeScript likewise preserves JavaScript runtime behavior instead of validating incoming data automatically during execution.

Static confidence becomes unsound when unchecked values are asserted, cast, or represented through permissive escape types.

If [[Inference should remove annotation noise without hiding public contracts\|explicit public contracts]] define expectations, [[Parameterized queries separate data from commands\|Parameterized queries separate data from commands]] illustrates defensive treatment of external values.

Runtime schemas, parsing, range checks, and rejection paths must establish facts before typed application logic proceeds.

Validate at trust boundaries, then let static checking preserve those validated assumptions throughout internal code.
