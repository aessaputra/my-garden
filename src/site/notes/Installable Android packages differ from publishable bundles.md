---
{"dg-publish":true,"permalink":"/installable-android-packages-differ-from-publishable-bundles/","title":"Installable Android packages differ from publishable bundles","hideInFiletree":true,"tags":["deployment","architecture"],"noteIcon":"","dg-note-properties":{"title":"Installable Android packages differ from publishable bundles","categories":["Deployment Strategies"],"tags":["deployment","architecture"],"created":"2026-09-07","updated":"2026-09-07"}}
---

An APK installs and runs on devices, while an Android App Bundle only publishes
to a store and cannot install directly. [Application
fundamentals](https://developer.android.com/guide/components/fundamentals)
define the APK as the runtime archive and the AAB as project content plus
metadata awaiting later APK generation.

When distributed through Google Play, servers derive optimized APKs containing
just the code and resources one device needs. One uploaded bundle therefore
becomes many installed variants without developer-side repackaging.

Release builds additionally require explicit signing, unlike debug builds. This
keeps the publishable artifact distinct from the installable one in both
pipeline and trust.

Because [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]], packaging
choices must not dictate implementation structure. The [[References/Mobile Apps\|Mobile Apps]]
distribution section gives the wider channel context for this split.

Publish a bundle for the store, sign releases deliberately, and verify what
each device actually installs.
