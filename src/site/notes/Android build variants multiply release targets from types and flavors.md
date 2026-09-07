---
{"dg-publish":true,"permalink":"/android-build-variants-multiply-release-targets-from-types-and-flavors/","title":"Android build variants multiply release targets from types and flavors","hideInFiletree":true,"tags":["deployment","programming"],"noteIcon":"","dg-note-properties":{"title":"Android build variants multiply release targets from types and flavors","categories":["Deployment Strategies"],"tags":["deployment","programming"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Android build variants are the cross product of build types and product
flavors, configured through Gradle and the Android Gradle plugin. [Build
configuration](https://developer.android.com/build) defines types for lifecycle
stages, such as debug and release, and flavors for market editions, such as
free and paid.

Each variant can override manifest entries, dependencies, signing, and
shrinking rules. Debug signs automatically with a known key, while release
demands an explicit signing configuration before store distribution.

Shrinking with R8 applies per variant, so size optimization follows the exact
artifact being shipped. Adding one type or flavor multiplies the variant
matrix, which raises configuration and testing cost.

This complements [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]]:
variants serve delivery obligations without describing product behavior. The
[[References/Mobile Apps\|Mobile Apps]] packaging notes frame the same split at platform level.

Define types for stages, flavors for editions, and test the variants actually
shipped.
