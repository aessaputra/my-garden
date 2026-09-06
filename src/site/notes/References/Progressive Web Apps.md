---
{"dg-publish":true,"dg-path":"Progressive Web Apps.md","permalink":"/progressive-web-apps/","title":"Progressive Web Apps","hideInFiletree":true,"tags":["references","javascript","programming","storage"],"noteIcon":"","dg-note-properties":{"title":"Progressive Web Apps","categories":["Web APIs"],"tags":["references","javascript","programming","storage"],"sources":["_raw/articles/progressive-web-apps-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Progressive Web App (PWA) adalah aplikasi web yang dapat dipasang dan ditingkatkan dengan kemampuan seperti offline serta notifikasi. UI memakai HTML, CSS, dan JavaScript; browser engine tetap menjalankannya.

Menurut [MDN tentang PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app), pendekatan ini menggabungkan distribusi web dengan sejumlah integrasi perangkat.

## Komponen dan cara kerja

Web app manifest berisi metadata aplikasi, termasuk nama, ikon, URL awal, dan mode tampilan. [[References/Service Workers\|Service Workers]] dapat menangani request, caching, serta event background secara terpisah dari UI.

Service worker bukan syarat universal instalasi. [Panduan instalasi MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable) memisahkan installability dari pengalaman offline.

PWA juga tidak harus berupa single-page app. [Penjelasan arsitektur MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app) memperbolehkan aplikasi dengan banyak halaman.

## Instalasi

Untuk promosi instalasi berbasis manifest, Chromium memerlukan nama, ikon 192px dan 512px, `start_url`, serta `display` atau `display_override`. `prefer_related_applications` harus tidak ada atau bernilai `false`.

Produksi memakai [[References/HTTPS\|HTTPS]]; localhost atau loopback tersedia untuk pengembangan. Persyaratan ini mengikuti [dokumentasi instalasi](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable).

Antarmuka pemasangan berbeda antarbrowser dan OS. Sebagian browser juga dapat memasang website biasa tanpa manifest; hal tersebut berbeda dari pemenuhan kriteria promosi PWA dalam dokumentasi MDN.

Aplikasi tetap perlu berfungsi sebagai website ketika instalasi tidak tersedia. [MDN menjelaskan perbedaan jalur instalasi](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Making_PWAs_installable).

## Offline dan caching

Offline memerlukan caching dan fallback eksplisit; lihat [panduan offline MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation).

Cache first berisiko basi; network first mencoba jaringan dahulu. [Pemilihan strategi](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation) bergantung pada kebutuhan aplikasi.

Rekomendasi desain: bedakan konten yang dapat dibaca offline dari tindakan yang memerlukan server. Tampilkan status tertunda dengan jelas, lalu uji recovery saat koneksi kembali.

## Push dan background

Push memerlukan push service dan koneksi perangkat; lihat [alur push MDN](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation).

Worker dapat dihentikan browser; retry background dibatasi menurut [batas background operation](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation).

Dalam pengumuman iOS/iPadOS 16.4, WebKit memperkenalkan push untuk Home Screen web apps. Izin diminta setelah interaksi langsung pengguna, bukan secara diam-diam.

Ketentuan tersebut bersumber dari [pengumuman WebKit Februari 2023](https://webkit.org/blog/13878/web-push-for-web-apps-on-ios-and-ipados/); ini bukan matriks dukungan lengkap untuk setiap versi perangkat saat ini.

## Kecocokan dan batas

Satu codebase dan distribusi melalui URL dapat mengurangi duplikasi implementasi. Namun, sumber yang ditinjau tidak membuktikan PWA selalu lebih murah daripada native setelah pengujian dan pemeliharaan dihitung.

Kemampuan browser berbeda; [MDN menganjurkan feature detection dan fallback](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/What_is_a_progressive_web_app).

Rekomendasi evaluasi: uji instalasi, peluncuran offline, cache kosong, konten basi, penolakan izin, serta recovery jaringan pada perangkat target. Pilih PWA berdasarkan kebutuhan yang terbukti terpenuhi, bukan label lintas platform.

## Lanjutan

- [[Installing a PWA does not guarantee offline operation\|Installing a PWA does not guarantee offline operation]]
- [[Offline reliability depends on explicit cache policy\|Offline reliability depends on explicit cache policy]]
- [[Cross-platform PWAs still need capability-based fallbacks\|Cross-platform PWAs still need capability-based fallbacks]]
