---
{"dg-publish":true,"dg-path":"Caching.md","permalink":"/caching/","title":"Caching","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Caching","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/caching-user-summary-2026-09-11.md","References/Cache-Control.md"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

Caching menyimpan salinan data atau hasil komputasi agar dapat digunakan kembali tanpa selalu mengambil atau menghitungnya dari awal. Cache dapat berada di memori aplikasi, disk, browser, proxy, CDN, atau layanan cache terpisah.

Cache dapat mengurangi latency karena letaknya lebih dekat dengan pemanggil, tetapi kedekatan geografis bukan syarat. Menghindari query database, transfer data, atau komputasi berulang juga dapat mengurangi waktu respons dan beban sumber asli.

[[References/Redis\|Redis]] dan [[References/Memcached\|Memcached]] dapat dipakai sebagai cache bersama; penggunaannya tetap membutuhkan kebijakan key, masa berlaku, dan eviction.

[[References/Client Side Caching\|Client Side Caching]] membahas penyimpanan di perangkat pengguna melalui HTTP cache dan API browser, berbeda dari layanan cache bersama.

## Cara kerja

- **Cache hit**: entri yang sesuai tersedia dan boleh dipakai menurut kebijakan cache.
- **Cache miss**: tidak ada entri yang dapat dipakai; sistem mengambil atau menghitung hasil dari sumber aslinya, lalu dapat menyimpannya untuk request berikutnya.
- **Cache key**: identitas yang menentukan entri mana yang cocok dengan suatu request atau input.

Contoh hipotetis: aplikasi menyimpan hasil pembacaan katalog produk agar request berikutnya tidak selalu menjalankan query yang sama. Setelah harga berubah, cache perlu diperbarui, dibatalkan, atau dibiarkan kedaluwarsa sesuai toleransi data lama.

## Freshness dan invalidation

Cache membutuhkan kebijakan kapan data boleh digunakan kembali. TTL membatasi masa berlaku menurut aturan sistem; invalidation menandai atau menghapus entri ketika tidak lagi layak digunakan. Eviction mengeluarkan entri, misalnya karena kapasitas terbatas, bukan selalu karena data sudah berubah.

Dalam HTTP, data stale dapat divalidasi ulang sebelum dipakai. [[References/Cache-Control\|Cache-Control]] menjelaskan freshness, validator, serta private dan shared cache. Header HTTP tersebut tidak otomatis mengatur seluruh cache aplikasi.

## Manfaat dan batas

Caching dapat meningkatkan [[References/Web Performance\|Web Performance]] dan mengurangi beban sumber, tetapi tidak menjamin semua request lebih cepat. Cache miss, lookup jaringan, serialisasi, dan pemeliharaan cache menambah biaya. Ukur latency, hit rate, dan beban sumber pada workload yang sebenarnya.

Cache key dan batas akses harus membedakan data yang memang berbeda, termasuk konteks pengguna bila relevan. Jangan menyajikan respons pribadi dari shared cache kepada pengguna lain. [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] tetap berlaku ketika data berasal dari cache.

Cache bukan pengganti penyimpanan utama atau backup. Sistem perlu menentukan toleransi data lama serta perilaku ketika cache kosong atau tidak tersedia.

## Sumber

- [[References/Cache-Control\|Cache-Control]]: catatan vault tentang reuse, freshness, cache key, dan keamanan HTTP caching.
- [RFC 9111: HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111.html): aturan cache khusus HTTP.
