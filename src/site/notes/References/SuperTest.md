---
{"dg-publish":true,"dg-path":"SuperTest.md","permalink":"/super-test/","title":"SuperTest","hideInFiletree":true,"tags":["references","testing","programming"],"noteIcon":"","dg-note-properties":{"title":"SuperTest","aliases":["supertest"],"categories":["Tests"],"type":"reference","tags":["references","testing","programming"],"sources":["_raw/articles/supertest-github-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

SuperTest adalah library Node.js untuk testing HTTP server lewat fluent API — "HTTP assertions made easy via superagent". Ia memberi abstraksi high-level untuk testing HTTP dengan opsi turun ke lower-level API superagent bila diperlukan. Maintained untuk Forward Email dan Lad (repo kini di `forwardemail/supertest`, v7.3.0 saat ingest).

`request()` menerima `http.Server` atau `Function`; bila server belum listening, ia di-bind otomatis ke ephemeral port sehingga test tidak perlu mengatur port. Instal via `npm install supertest --save-dev` dan pakai dengan test framework apa pun — dengan Mocha, `done` bisa di-pass langsung ke `.expect()`.

## Posisi di strategi testing

SuperTest menguji batas HTTP antar komponen: cocok untuk [[Integration tests catch contract mismatch\|integration test]] API (status, header, body, cookie) dalam [[References/Testing Your Apps\|piramida pengujian]]. Ia bukan pengganti unit test untuk logika kecil, bukan pula E2E browser — untuk alur klik-ketik pengguna, pakai Playwright atau [[References/Cypress\|Cypress]].

## Fitur: HTTP/2 dan cookie assertions

HTTP/2 diaktifkan via `request(app, { http2: true })` atau `request.agent(...)`. Assertions cookie tersedia lewat `request.cookies` (`.set`, `.reset`, `.new`, `.renew`, `.contain`, `.not`) untuk domain, path, expiry, flag secure/httpOnly; cookie signed butuh inisialisasi `cookies(secret)`.

## Batas

SuperTest hanya melihat lapisan HTTP — tidak mengeksekusi JavaScript browser atau mengukur rendering. Throughput/latency aktual bergantung pada aplikasi, bukan klaim generik. Lisensi MIT.

## Sumber

- [SuperTest di GitHub](https://github.com/ladjs/supertest) (redirect ke forwardemail/supertest)
