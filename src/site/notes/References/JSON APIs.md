---
{"dg-publish":true,"dg-path":"JSON APIs.md","permalink":"/json-ap-is/","title":"JSON APIs","hideInFiletree":true,"tags":["references","programming","backend","http","json"],"noteIcon":"","dg-note-properties":{"title":"JSON APIs","categories":["APIs"],"type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","backend","http","json"],"sources":["_raw/articles/json-apis-user-summary-2026-09-10.md","https://www.rfc-editor.org/rfc/rfc8259","https://jsonapi.org/format/"],"created":"2026-09-10","updated":"2026-09-10"}}
---

JSON (JavaScript Object Notation) adalah format pertukaran data berbasis teks yang distandarkan dalam RFC 8259. Namanya menyebut JavaScript, tetapi format ini independen bahasa: ia hanya mendefinisikan sintaks nilai, yaitu object, array, string, number, boolean, dan null.

JSON menyeragamkan serialisasi data, bukan kontrak API itu sendiri. Dua pihak yang bertukar JSON masih perlu menyepakati field, makna, dan perilaku error secara terpisah.

## Dua makna "JSON API"

Istilah "JSON API" dipakai dalam dua arti yang perlu dibedakan:

1. **JSON over HTTP secara umum.** API yang mempertukarkan representasi JSON lewat [[References/HTTP\|HTTP]]. Ini praktik dominan, bukan spesifikasi: bentuk dokumen ditentukan penyedia API.
2. **JSON:API, spesifikasi tertentu.** Konvensi bermedia type `application/vnd.api+json` dari jsonapi.org yang membakukan struktur dokumen: resource object, relationships, errors, pagination, dan compound document. Tujuannya agar client dapat generik terhadap banyak server.

## Kosakata entity, bundle, dan field

Frasa "entity types, bundles, dan fields" berasal dari model data Drupal dan dimunculkan modul JSON:API Drupal. Itu implementasi CMS tertentu di atas konvensi, bukan bagian spesifikasi inti JSON:API maupun JSON. Rujukan dokumentasi Drupal tercantum di Sumber namun tidak terverifikasi penuh saat ingest karena tantangan bot.

## Sintaks bukan validasi

JSON hanya menjamin keabsahan sintaks. Nilai yang masuk tetap perlu divalidasi terhadap skema atau aturan aplikasi; [[Dynamic inputs require runtime validation despite static types\|Dynamic inputs require runtime validation despite static types]] menjelaskan mengapa pemeriksaan runtime tetap diperlukan di batas sistem.

## Hubungan dengan halaman lain

[[References/APIs\|APIs]] menempatkan format data sebagai detail kontrak HTTP API, sedangkan [[References/REST\|REST]] mengatur gaya arsitektur di atasnya; JSON berada pada lapisan representasi. Di sisi penyimpanan, [[References/JSON dan Data Tidak Terstruktur di Postgres\|JSON dan Data Tidak Terstruktur di Postgres]] membahas tipe JSON di database, konteks yang berbeda dari pertukaran antarsistem.

## Sumber

- [RFC 8259, The JavaScript Object Notation (JSON) Data Interchange Format](https://www.rfc-editor.org/rfc/rfc8259): definisi format dan tipe nilai.
- [JSON:API Specification v1.1](https://jsonapi.org/format/): media type, resource object, relationships, errors, pagination.
- [Dokumentasi modul JSON:API Drupal](https://www.drupal.org/docs/8/modules/json-api): konteks entity types, bundles, fields (tidak terverifikasi penuh, bot challenge).
