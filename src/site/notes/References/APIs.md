---
{"dg-publish":true,"dg-path":"APIs.md","permalink":"/ap-is/","title":"APIs","hideInFiletree":true,"tags":["references","programming","http"],"noteIcon":"","dg-note-properties":{"title":"APIs","categories":["Software Interfaces"],"tags":["references","programming","http"],"sources":["_raw/articles/apis-user-summary-2026-09-10.md","References/Backend Development.md","References/Web APIs.md"],"created":"2026-09-10","updated":"2026-09-11","confidence":"medium"}}
---

API (Application Programming Interface) adalah antarmuka yang menetapkan cara software menggunakan kemampuan software lain. Kontraknya menjelaskan operasi yang tersedia, input yang diterima, output, serta perilaku kegagalan.

Pemanggil tidak perlu memahami seluruh implementasi internal. Namun, ia tetap harus mengikuti kontrak, termasuk batas penggunaan dan aturan akses. Standardisasi berlaku pada antarmuka yang disepakati, bukan berarti semua API memiliki standar yang sama.

## API tidak selalu memakai HTTP

API library dapat berupa fungsi atau kelas yang dipanggil dalam proses yang sama. [[References/Web APIs\|Web APIs]] membahas antarmuka browser seperti DOM, Fetch, dan History. API layanan jarak jauh memungkinkan komunikasi antarsistem melalui jaringan.

Endpoint, HTTP methods, JSON, dan XML adalah detail yang lazim pada HTTP API, bukan syarat definisi seluruh API. [[References/Backend Development\|Backend Development]] membahas logika server di balik antarmuka tersebut. Gaya arsitektur REST memandu HTTP API berbasis resource; lihat [[References/REST\|REST]]. Format JSON dan konvensi JSON:API dibahas di [[References/JSON APIs\|JSON APIs]]; SOAP menempati lapisan yang sama dengan format XML, lihat [[References/SOAP\|SOAP]]. RPC dengan kontrak biner ditangani gRPC; lihat [[References/gRPC\|gRPC]].

## Bagian HTTP API

| Bagian | Peran |
| --- | --- |
| Endpoint | Alamat tujuan request untuk resource atau operasi tertentu. |
| Method | Menyatakan maksud request menurut semantik HTTP. |
| Parameters dan body | Membawa input sesuai kontrak. |
| Headers | Membawa metadata, misalnya jenis konten dan informasi autentikasi. |
| Status dan response | Menyampaikan hasil atau kegagalan operasi. |

Dalam [[References/HTTP\|HTTP]], GET meminta representasi resource. POST meminta resource memproses data yang dikirim. PUT meminta pembuatan atau penggantian state resource target dengan representasi yang diberikan. POST tidak selalu berarti membuat data baru.

JSON dan XML adalah format representasi data, bukan protokol komunikasi. API juga dapat mengembalikan teks, gambar, atau format lain. Format dan field yang diwajibkan harus dinyatakan dalam kontrak.

## Contoh hipotetis

`GET /products/42` dapat meminta data produk dengan ID 42. Server dapat mengembalikan status `200` beserta JSON, atau `404` bila produk tidak ditemukan. Alamat dan perilaku ini adalah ilustrasi, bukan endpoint yang telah diuji.

Pemanggil perlu memeriksa hasil request, bukan hanya mencoba membaca body. Catatan [[fetch() treats network as promises with explicit control\|fetch() treats network as promises with explicit control]] membahas pemeriksaan response pada Fetch API.

## Desain kontrak

[[References/OpenAPI Specification\|OpenAPI Specification]] mendeskripsikan kontrak HTTP API dalam YAML atau JSON agar dapat dipakai untuk dokumentasi, client generation, dan pengujian.

[[Backend APIs should expose business contracts rather than database tables\|Backend APIs should expose business contracts rather than database tables]] menjelaskan mengapa bentuk API sebaiknya tidak mengikuti struktur penyimpanan secara langsung. Perubahan internal dapat tetap tersembunyi selama kontrak publik dipertahankan.

[[References/Authentication\|Authentication]] membahas kredensial dan validasi identitas pemanggil API, termasuk batas API key, OAuth, dan JWT.

Kontrak juga tidak menggantikan keamanan. [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] menjelaskan mengapa identitas yang valid belum membuktikan izin mengakses setiap resource.

## Sumber

- [[References/Backend Development\|Backend Development]]: catatan vault mengenai kontrak, request, response, dan authorization.
- [[References/Web APIs\|Web APIs]]: catatan vault mengenai API browser dan antarmuka runtime.
