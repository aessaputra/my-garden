---
{"dg-publish":true,"dg-path":"Cache-Control.md","permalink":"/cache-control/","title":"Cache-Control","hideInFiletree":true,"tags":["references","programming","performance","network","security"],"dg-note-properties":{"title":"Cache-Control","type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","performance","network","security"],"sources":["https://www.rfc-editor.org/rfc/rfc9111.html","https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control","https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching","https://www.rfc-editor.org/rfc/rfc5861.html","https://www.rfc-editor.org/rfc/rfc8246.html","https://www.rfc-editor.org/rfc/rfc9213.html","https://developers.cloudflare.com/cache/concepts/cache-control/","https://developers.cloudflare.com/cache/concepts/cdn-cache-control/","https://developers.cloudflare.com/cache/concepts/cache-responses/","https://developers.cloudflare.com/cache/how-to/purge-cache/","https://developer.mozilla.org/en-US/docs/Web/API/Cache"],"created":"2026-08-29","updated":"2026-08-29"}}
---

`Cache-Control` adalah field header [[References/HTTP\|HTTP]] untuk menyampaikan directive cache pada request dan response. Directive ini memengaruhi apakah response boleh disimpan, berapa lama response dianggap fresh, kapan cache harus melakukan validasi ulang, dan apakah aturan berlaku pada private cache seperti browser atau shared cache seperti proxy dan [[References/Cloudflare\|CDN]]. Header ini mengatur perilaku cache yang mematuhinya, bukan jaminan bahwa resource pasti tersimpan atau website otomatis lebih cepat.

## Model cache HTTP

Cache menyimpan response agar request berikutnya dapat dipenuhi tanpa selalu mengunduh seluruh representasi dari origin. Response yang masih fresh dapat digunakan kembali sesuai aturan cache. Setelah stale, response biasanya perlu divalidasi atau diambil ulang sebelum digunakan.

Freshness eksplisit lazimnya ditentukan oleh `max-age`, `s-maxage`, atau `Expires`. Jika tidak ada freshness lifetime eksplisit, cache dapat menghitung freshness secara heuristik untuk response yang memang cacheable. Karena itu, ketiadaan `Cache-Control` tidak selalu berarti response tidak akan disimpan.

`Age` menunjukkan perkiraan waktu, dalam detik, sejak response dibuat atau berhasil divalidasi oleh origin. Nilai ini membantu menjelaskan mengapa sisa freshness di browser atau CDN dapat lebih pendek daripada angka `max-age` yang terlihat. Cache key juga dipengaruhi oleh metode dan target URI; field `Vary` dapat menambahkan bagian request tertentu, seperti encoding atau bahasa, ke proses pemilihan response.

## Directive response utama

### `max-age`

`max-age=N` menetapkan jumlah detik response dianggap fresh sejak waktu pembuatannya. Ini bukan timer yang selalu dimulai ketika browser menerima file. Umur yang sudah dihabiskan pada cache upstream ikut diperhitungkan.

Aset ber-hash yang URL-nya berubah ketika konten berubah dapat memakai freshness panjang. HTML atau data yang berubah tanpa mengganti URL biasanya membutuhkan masa fresh lebih pendek atau mekanisme validasi agar versi lama tidak bertahan terlalu lama.

### `s-maxage`

`s-maxage=N` menetapkan freshness untuk shared cache dan mengungguli `max-age` atau `Expires` di cache tersebut. Private cache mengabaikannya. Directive ini berguna ketika browser dan CDN memerlukan kebijakan berbeda, misalnya browser harus sering memeriksa ulang sementara CDN boleh menyimpan response lebih lama.

### `public` dan `private`

`public` menyatakan bahwa shared cache boleh menyimpan response, termasuk dalam keadaan yang biasanya membatasi shared caching. Directive ini bukan perintah agar response pasti disimpan.

`private` membatasi penyimpanan response pada private cache. Browser masih boleh menyimpannya, tetapi shared cache tidak boleh. `private` bukan sinonim `no-store` dan bukan kontrol otorisasi. Data sensitif yang tidak boleh tertinggal di cache memerlukan kebijakan yang lebih ketat.

### `no-cache`

`no-cache` tidak berarti “jangan simpan”. Cache boleh menyimpan response, tetapi harus memvalidasinya dengan origin sebelum reuse. Jika validator seperti `ETag` atau `Last-Modified` tersedia dan representasi belum berubah, origin dapat mengembalikan status `304 Not Modified` tanpa body penuh.

Directive ini cocok ketika penyimpanan lokal masih bermanfaat, tetapi aplikasi membutuhkan pemeriksaan freshness sebelum setiap reuse. Untuk field tertentu, sintaks bernilai dapat membatasi kewajiban validasi pada field tersebut, walau dukungannya tidak selalu seragam.

### `no-store`

`no-store` meminta cache agar tidak menyimpan response dan, pada request, tidak menyimpan response hasil request tersebut. Directive ini ditujukan untuk mencegah penyimpanan yang disengaja, terutama pada response sensitif. Ia tidak menghapus salinan lama yang sudah tersimpan sebelum directive diterapkan dan tidak menggantikan penghapusan data aplikasi, pembatalan sesi, atau perlindungan transport.

### Revalidasi

`must-revalidate` melarang cache menggunakan response stale tanpa validasi yang berhasil. Jika origin tidak dapat dijangkau, cache harus menghasilkan error daripada diam-diam memakai response stale. `proxy-revalidate` menerapkan pembatasan serupa hanya pada shared cache.

`stale-while-revalidate=N` mengizinkan cache menyajikan response stale selama interval terbatas sambil melakukan validasi ulang secara asynchronous. `stale-if-error=N` mengizinkan response stale dipakai ketika request baru menghasilkan kegagalan tertentu. Keduanya dapat meningkatkan ketahanan dan latency, tetapi juga memperpanjang waktu pengguna melihat data lama. Gunakan hanya pada konten yang toleran terhadap staleness.

### Directive lain

`immutable` menyatakan bahwa response tidak akan berubah selama masih fresh sehingga client tidak perlu melakukan validasi ulang akibat reload biasa. Directive ini paling aman untuk aset dengan URL versioned atau content-hashed.

`no-transform` meminta intermediary agar tidak mengubah representasi, misalnya mengonversi format gambar. `must-understand` meminta cache hanya menyimpan response jika ia memahami persyaratan caching berdasarkan status response; directive ini digunakan bersama `no-store` sebagai fallback bagi cache yang tidak memahaminya.

## Directive pada request

Client juga dapat mengirim `Cache-Control`. `no-cache` meminta cache melakukan validasi sebelum menggunakan response tersimpan. `no-store` meminta cache tidak menyimpan request atau response terkait, tetapi tidak menjamin penghapusan entry lama. `max-age`, `min-fresh`, dan `max-stale` membatasi umur atau tingkat staleness yang dapat diterima. `only-if-cached` meminta hasil dari cache dan tidak mengizinkan akses jaringan; bila tidak ada response yang sesuai, cache biasanya mengembalikan `504 Gateway Timeout`.

Request directive tidak selalu dapat membatalkan aturan response. Contohnya, permintaan client untuk menerima response stale tidak mengizinkan reuse jika response mewajibkan revalidasi.

## Pola kebijakan

### Aset statis dengan nama ber-hash

```http
Cache-Control: public, max-age=31536000, immutable
```

Pola ini sesuai untuk file yang URL-nya berubah setiap konten berubah. Jangan menerapkannya pada URL stabil jika proses deployment tidak memiliki versioning atau purge yang andal.

### Dokumen yang harus selalu diperiksa

```http
Cache-Control: no-cache
```

Response boleh disimpan dan digunakan kembali setelah validasi. Pola ini berbeda dari memaksa pengunduhan body penuh setiap kali.

### Response privat pengguna

```http
Cache-Control: private, no-cache
```

Shared cache tidak boleh menyimpan response, sedangkan browser dapat menyimpan dan memvalidasinya. Jika body mengandung data sensitif yang tidak boleh tersimpan, gunakan `no-store` berdasarkan model ancaman aplikasi.

### Kebijakan browser dan CDN yang berbeda

```http
Cache-Control: public, max-age=60, s-maxage=600, stale-while-revalidate=30
```

Browser memperoleh freshness singkat, shared cache memperoleh freshness lebih panjang, lalu dapat menyajikan response stale secara terbatas ketika validasi ulang berlangsung. Kelayakannya bergantung pada toleransi aplikasi terhadap data lama.

## CDN dan header khusus

Shared cache dapat menerapkan kebijakan platform di samping standar HTTP. Cloudflare mendukung header `CDN-Cache-Control` dan `Cloudflare-CDN-Cache-Control` untuk memisahkan aturan CDN dari aturan yang diterima browser. Header yang lebih spesifik dapat memungkinkan satu response memiliki TTL browser, TTL CDN umum, dan TTL khusus Cloudflare yang berbeda.

Perilaku akhir tetap bergantung pada cacheability response, status code, metode, konfigurasi cache, aturan platform, cookie, otorisasi, dan plan penyedia. Header origin tidak selalu cukup untuk membuat response dinamis menjadi cacheable. Periksa dokumentasi CDN yang digunakan, bukan menganggap semua intermediary menafsirkan extension atau override secara identik.

## Invalidasi dan deployment

TTL panjang mengurangi request ke origin, tetapi memperbesar risiko konten lama bertahan setelah deployment. Tiga mekanisme utama adalah URL versioning, revalidation, dan purge. URL content-hashed biasanya paling mudah diprediksi untuk aset immutable. Purge berguna untuk koreksi mendesak, tetapi tidak memperbaiki private cache yang sudah menerima freshness panjang dan tidak boleh menjadi satu-satunya strategi versioning.

Cache HTTP juga berbeda dari Cache Storage yang dikelola [[References/Service Workers\|Service Workers]]. Header HTTP memandu HTTP cache, sedangkan Cache API memberi aplikasi kendali eksplisit atas pasangan `Request` dan `Response`; entry Cache API tidak otomatis kedaluwarsa berdasarkan `Cache-Control`. Aplikasi yang memakai keduanya perlu menetapkan ownership dan invalidasi dengan jelas agar dua lapisan tidak mempertahankan versi berbeda.

## Diagnosis

Gunakan panel Network di [[References/Chrome DevTools\|Chrome DevTools]] untuk memeriksa request dan response header, nilai `Age`, status `304`, transfer size, serta penanda sumber seperti memory cache atau disk cache. Lakukan pengujian dalam kondisi cache normal; opsi “Disable cache” mengubah perilaku dan hanya cocok untuk perbandingan terkontrol.

Pada CDN, periksa header status cache yang disediakan platform. Standar `Cache-Status` menyediakan format terstruktur untuk menjelaskan apakah response merupakan hit, miss, stale, atau hasil revalidation, tetapi penerapan dan nama header vendor tetap bervariasi.

Ketika hasil tidak sesuai dugaan, periksa berurutan: cacheability metode dan status, directive response, `Age`, validator, `Vary`, cookie dan authorization, aturan CDN, lalu service worker. Jangan menyimpulkan header gagal hanya dari satu reload karena reload, DevTools, extension, dan beberapa lapisan cache dapat mengubah jalur request.

## Batas dan risiko

Caching mengurangi latency dan beban origin hanya ketika response aman digunakan kembali dan cache hit benar-benar terjadi. TTL yang terlalu pendek dapat menghasilkan banyak validasi; TTL terlalu panjang dapat menyajikan konten usang. Shared caching untuk response personal dapat membocorkan data lintas pengguna jika cache key atau directive salah.

`Cache-Control` bukan mekanisme autentikasi, enkripsi, atau penghapusan data. `private` tidak mencegah browser menyimpan data. `no-store` mengurangi penyimpanan melalui cache yang patuh, tetapi tidak mengendalikan screenshot, extension, log, backup, atau storage aplikasi lain. Kebijakan cache harus dipadukan dengan kontrol akses, HTTPS, pemisahan response publik dan privat, serta pengujian pada browser dan CDN yang sebenarnya.

## Sumber

1. RFC 9111: HTTP Caching — RFC Editor: https://www.rfc-editor.org/rfc/rfc9111.html
2. Cache-Control header — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cache-Control
3. HTTP caching — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching
4. RFC 5861: HTTP Cache-Control Extensions for Stale Content — RFC Editor: https://www.rfc-editor.org/rfc/rfc5861.html
5. RFC 8246: HTTP Immutable Responses — RFC Editor: https://www.rfc-editor.org/rfc/rfc8246.html
6. RFC 9213: The Cache-Status HTTP Response Header Field — RFC Editor: https://www.rfc-editor.org/rfc/rfc9213.html
7. Origin Cache-Control — Cloudflare Docs: https://developers.cloudflare.com/cache/concepts/cache-control/
8. CDN-Cache-Control — Cloudflare Docs: https://developers.cloudflare.com/cache/concepts/cdn-cache-control/
9. Cloudflare cache responses — Cloudflare Docs: https://developers.cloudflare.com/cache/concepts/cache-responses/
10. Purge cache — Cloudflare Docs: https://developers.cloudflare.com/cache/how-to/purge-cache/
11. Cache interface — MDN Web Docs: https://developer.mozilla.org/en-US/docs/Web/API/Cache
