---
{"dg-publish":true,"dg-path":"Mobile Apps.md","permalink":"/mobile-apps/","title":"Mobile Apps","hideInFiletree":true,"tags":["references","programming","architecture","security"],"noteIcon":"","dg-note-properties":{"title":"Mobile Apps","categories":["Frameworks"],"tags":["references","programming","architecture","security"],"sources":["_raw/articles/mobile-apps-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Mobile apps adalah aplikasi untuk smartphone, tablet, dan perangkat genggam. [Distribusi Apple](https://developer.apple.com/distribute/) mencakup aplikasi untuk iPhone dan iPad.

## Native dan cross-platform

Native dalam perbandingan ini berarti implementasi khusus platform. [Android](https://developer.android.com/guide/components/fundamentals) memakai SDK untuk mengemas kode Kotlin, Java, atau C++ bersama resource aplikasi.

Cross-platform berbagi kode antar sistem operasi. [Flutter](https://docs.flutter.dev/resources/architectural-overview) menggabungkan reuse dengan akses layanan platform, bukan menghapus perbedaannya.

[React Native](https://reactnative.dev/docs/platform-specific-code) menyediakan deteksi platform dan file khusus iOS atau Android. Karena itu, satu codebase tidak berarti seluruh implementasi identik.

Rincian model UI berada di [[References/React Native\|React Native]] dan [[References/Flutter\|Flutter]]. Keduanya tetap dibahas terpisah; istilah cross-platform tidak menyatakan keduanya memakai arsitektur rendering yang sama.

## Kemampuan perangkat dan izin

Akses kamera dan lokasi dibatasi izin pada [Android](https://developer.android.com/guide/components/fundamentals). Kebutuhan lokasi, termasuk penggunaan GPS, bukan jaminan akses sensor atau akurasi tertentu.

Izin bukan selalu dialog runtime: sebagian diberikan saat instalasi menurut [Permissions on Android](https://developer.android.com/guide/topics/permissions/overview).

Izin dapat ditolak atau dicabut. [Panduan Android](https://developer.android.com/training/permissions/requesting) meminta pemeriksaan akses dan graceful degradation agar fitur lain tetap dapat digunakan.

[Apple](https://developer.apple.com/ios/) menyediakan notifikasi lokal dan push untuk informasi tepat waktu. Keberadaan API tidak membuktikan setiap pesan pasti diterima pengguna.

## Distribusi dan lifecycle

[Google Play](https://developer.android.com/guide/components/fundamentals) menghasilkan APK yang disesuaikan dengan perangkat dari distribusi app bundle. Jalur distribusi berbeda dari keputusan berbagi source code.

[Apple](https://developer.apple.com/distribute/) menawarkan App Store serta alternatif bergantung wilayah. Kehadiran di toko bukan definisi universal aplikasi mobile.

[Android](https://developer.android.com/guide/components/fundamentals) dapat menghentikan proses background untuk kebutuhan resource. Aplikasi tidak boleh diasumsikan terus berjalan hanya karena telah diinstal.

[[References/Progressive Web Apps\|Progressive Web Apps]] memberi konteks alternatif berbasis web, sedangkan [[References/Tauri\|Tauri]] membahas pendekatan webview. Keduanya bukan sinonim untuk semua aplikasi mobile.

## Batas riset

Tujuh dokumen primer diambil penuh melalui 9Router pada 6 September 2026. Detail izin bersumber dari Android, bukan bukti perilaku identik pada semua versi iOS.

Tidak dilakukan pengujian perangkat, benchmark performa, pengukuran biaya, atau pengajuan toko. Rekomendasi pemisahan integrasi merupakan sintesis, bukan klaim bahwa suatu framework selalu unggul.

Bukti, kutipan, tanggal, konflik metadata, dan teks lengkap tersimpan dalam [[_raw/articles/mobile-apps-research-packet\|paket riset Mobile Apps]].

## Lanjutan

- [[Shared mobile code still needs platform boundaries\|Shared mobile code still needs platform boundaries]]
- [[Mobile permissions are revocable feature dependencies\|Mobile permissions are revocable feature dependencies]]
- [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]]
