---
{"dg-publish":true,"permalink":"/ionic-separates-web-interfaces-from-native-integration/","title":"Ionic separates web interfaces from native integration","hideInFiletree":true,"tags":["programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"Ionic separates web interfaces from native integration","categories":["Frameworks"],"tags":["programming","architecture"],"created":"2026-09-07","updated":"2026-09-07"}}
---


An Ionic screen shares web interface code, while native capabilities remain a separate integration responsibility.

[Ionic documentation](https://ionicframework.com/docs) places controls, gestures, and animations in the frontend rather than the native runtime.

[Capacitor plugins](https://capacitorjs.com/docs/plugins/creating-plugins) connect JavaScript with native APIs, including custom implementations when existing integrations are insufficient.

This separation makes interface reuse useful without pretending every device exposes identical capabilities or behavior.

If [[Shared mobile code still needs platform boundaries\|Shared mobile code still needs platform boundaries]], native adapters deserve explicit ownership alongside shared screens.

Because [[Mobile permissions are revocable feature dependencies\|Mobile permissions are revocable feature dependencies]], those adapters also need deliberate unavailable and denied states.

I would evaluate the hardest native integration before committing the entire product to shared interface code.

Choose Ionic for reusable web interfaces, then budget native integration as real engineering work.
