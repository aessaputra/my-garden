---
{"dg-publish":true,"permalink":"/android-components-are-separate-system-entry-points-with-distinct-lifecycles/","title":"Android components are separate system entry points with distinct lifecycles","hideInFiletree":true,"tags":["programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"Android components are separate system entry points with distinct lifecycles","categories":["Frameworks"],"tags":["programming","architecture"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Android exposes four application components, and each one is a separate entry
point the system can invoke. Activities present a single screen, Services run
background work without UI, receivers answer system announcements, and
providers share structured data across apps.

Every component carries its own lifecycle, so creation and destruction rules
never transfer between kinds. [Application fundamentals](https://developer.android.com/guide/components/fundamentals)
describe started versus bound Services and cross-app Activity invocation as
distinct mechanisms.

I would choose the component by trigger and lifetime first, then write the UI
or worker inside it. Modern guidance favors a single Activity hosting many
destinations, per the [architecture guide](https://developer.android.com/topic/architecture).

Since [[Shared mobile code still needs platform boundaries\|Shared mobile code still needs platform boundaries]], these entry
points belong in platform adapters rather than portable logic. And because
[[Mobile permissions are revocable feature dependencies\|Mobile permissions are revocable feature dependencies]], each entry point
must recheck protected access on use.

Design the boundary the system sees, then share only the rules behind it.
