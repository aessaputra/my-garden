---
{"dg-publish":true,"permalink":"/dom-turns-documents-into-scriptable-objects/","title":"DOM turns documents into scriptable objects","hideInFiletree":true,"tags":["javascript","programming"],"noteIcon":"","dg-note-properties":{"title":"DOM turns documents into scriptable objects","categories":["Web APIs"],"tags":["javascript","programming"],"sources":["_raw/articles/web-apis-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04"}}
---

A page on screen and its HTML source look different, yet scripts need one stable handle.

I read the DOM as a live tree of nodes that mirrors the document for code.

MDN [defines the DOM as an object model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) for changing structure, style, and page content.

Selectors like querySelector return node lists that scripts can inspect, modify, or remove.

The DOM stays independent of any single language, which keeps its design portable across runtimes.

Because [[Web APIs live in the runtime, not the language\|Web APIs live in the runtime, not the language]], the DOM stays scriptable everywhere, and [[History API restores back button behavior in SPAs\|History API restores back button behavior in SPAs]] leans on those live updates.

I manipulate nodes, never raw markup strings, whenever structure must stay consistent.
