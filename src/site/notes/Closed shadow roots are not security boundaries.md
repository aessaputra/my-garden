---
{"dg-publish":true,"permalink":"/closed-shadow-roots-are-not-security-boundaries/","title":"Closed shadow roots are not security boundaries","hideInFiletree":true,"tags":["javascript","development","programming"],"noteIcon":"","dg-note-properties":{"title":"Closed shadow roots are not security boundaries","categories":["Web Components"],"tags":["javascript","development","programming"],"created":"2026-09-06","updated":"2026-09-06"}}
---

A closed shadow root hides one access path, but it does not isolate potentially hostile JavaScript execution.

Code that creates the root receives a reference, while ordinary page queries remain outside its internal tree.

[MDN warns](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Using_shadow_DOM) that closed mode is not a strong security mechanism against outside interference.

The practical benefit is protection against accidental coupling, not confidentiality for secrets placed inside component markup.

Since [[Shadow DOM reduces collisions without eliminating integration work\|encapsulation limits collisions]], [[Public properties and events determine framework interoperability\|public contracts]] should replace reliance on internal nodes.

Choosing closed mode therefore requires considering debugging and integration costs without claiming an additional security guarantee.

Keep sensitive authorization decisions outside browser component internals, and evaluate untrusted execution using a genuine security boundary.
