---
{"dg-publish":true,"dg-path":"Client Side Caching.md","permalink":"/client-side-caching/","title":"Client Side Caching","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Client Side Caching","aliases":["Client-Side Caching"],"categories":["Web Technologies"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/client-side-caching-user-summary-2026-09-11.md","References/Cache-Control.md","References/Service Workers.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Client-side caching menyimpan salinan data atau resource pada perangkat pengguna agar dapat digunakan kembali. Pada aplikasi web, cara ini dapat mengurangi transfer jaringan, waktu pemuatan, dan beban server ketika salinan lokal masih layak dipakai.

Berbeda dari cache bersama seperti Redis atau Memcached, cache ini berada di sisi client. [[References/Caching\|Caching]] menjelaskan prinsip umum hit, miss, freshness, dan invalidation.

## Mekanisme pada browser

| Mekanisme | Pengelolaan |
| --- | --- |
| HTTP cache | Browser mengelola response mengikuti aturan HTTP, termasuk header freshness dan validator. |
| Cache Storage | Aplikasi menyimpan pasangan Request/Response melalui Cache API dan mengatur kebijakan reuse serta pembersihannya. |
| localStorage | Penyimpanan string sinkron; aplikasi mengatur format, masa berlaku, dan invalidation sendiri. |
| IndexedDB | Penyimpanan data terstruktur asynchronous untuk kebutuhan aplikasi yang lebih besar atau kompleks. |

[[References/Cache-Control\|Cache-Control]] membahas HTTP cache, termasuk perbedaan `no-cache` yang membutuhkan validasi sebelum reuse dan `no-store` yang melarang penyimpanan oleh cache HTTP yang mematuhinya.

[[References/Service Workers\|Service Workers]] dapat mencegat request dan memilih respons dari jaringan atau Cache Storage. Service worker tidak otomatis menyimpan semua resource. Cache Storage juga dapat diakses dari halaman, bukan hanya service worker.

## Freshness dan pembaruan

Aturan HTTP cache tidak otomatis memberi expiry pada entri Cache Storage, localStorage, atau IndexedDB. Aplikasi perlu menetapkan versi, TTL bila diperlukan, serta kapan data dibersihkan atau diperbarui.

Aset dengan nama berbasis content hash dapat memakai cache lama karena versi baru memiliki URL berbeda. Data dinamis membutuhkan kebijakan sesuai toleransi staleness, misalnya validasi ulang atau pengambilan dari jaringan sebelum memakai fallback lokal.

Offline bukan jaminan dari penyimpanan lokal saja. Resource yang diperlukan harus tersedia, dan aplikasi harus menangani data yang belum tersimpan, kuota, penghapusan oleh pengguna, serta eviction browser.

## Keamanan dan batas

Jangan menggunakan kembali data pribadi setelah pergantian akun tanpa memeriksa konteksnya. Pisahkan entri berdasarkan pengguna bila relevan dan bersihkan data sensitif saat logout sesuai kebutuhan aplikasi.

localStorage dapat dibaca JavaScript pada origin yang sama, sehingga bukan tempat yang kebal terhadap XSS. Penyimpanan lokal tidak menggantikan authorization server atau perlindungan data sensitif.

Manfaat performa bergantung pada cache hit dan biaya pengelolaan. Penyimpanan sinkron seperti localStorage juga dapat menghambat main thread; ukur dampaknya, bukan menganggap seluruh akses lokal selalu lebih cepat.

## Sumber

- [[References/Cache-Control\|Cache-Control]]: aturan HTTP caching dan batas Cache API.
- [[References/Service Workers\|Service Workers]]: request interception, Cache Storage, lifecycle, dan batas offline.
- [MDN: HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching): referensi HTTP cache.
