---
{"dg-publish":true,"permalink":"/android-art-executes-dex-bytecode-not-java-se-class-files/","title":"Android ART executes DEX bytecode, not Java SE class files","hideInFiletree":true,"tags":["programming","architecture"],"noteIcon":"","dg-note-properties":{"title":"Android ART executes DEX bytecode, not Java SE class files","categories":["Programming Languages"],"tags":["programming","architecture"],"created":"2026-09-07","updated":"2026-09-07"}}
---

Android applications compile toward DEX bytecode, which the Android runtime executes on devices. This pipeline differs from standard Java SE deployment despite shared language syntax.

I treat Java language knowledge as portable, while runtime assumptions must be rechecked per platform. Class file versions, garbage collector options, and security tooling do not transfer automatically.

The [runtime documentation](https://source.android.com/docs/core/runtime) states ART executes the DEX format and its bytecode specification.

If [[Shared mobile code still needs platform boundaries\|shared code still needs boundaries]], Android Java needs [[Mobile permissions are revocable feature dependencies\|explicit permission states]] on every device.

Share business rules across platforms, then isolate runtime and permission integration deliberately. Portability claims should name the tested runtime instead of assuming identical execution.
