---
{"dg-publish":true,"dg-path":"REST.md","permalink":"/rest/","title":"REST","hideInFiletree":true,"tags":["references","programming","backend","http","architecture","json"],"noteIcon":"","dg-note-properties":{"title":"REST","categories":["APIs"],"type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","backend","http","architecture","json"],"sources":["_raw/articles/rest-user-summary-2026-09-10.md","https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm","https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods","https://developer.mozilla.org/en-US/docs/Web/HTTP/Status"],"created":"2026-09-10","updated":"2026-09-10"}}
---

REST (Representational State Transfer) adalah gaya arsitektur untuk sistem terdistribusi yang dirumuskan Roy Fielding dalam disertasinya pada tahun 2000. Gaya ini didefinisikan lewat seperangkat constraint: client-server, stateless, cacheable, uniform interface, layered system, serta code-on-demand yang bersifat opsional.

Praktik yang biasa disebut "REST API" sehari-hari umumnya hanya mengadopsi sebagian constraint tersebut, yaitu resource URI, HTTP methods, dan status code pada atas protokol [[References/HTTP\|HTTP]].

## Resource dan representasi

Resource adalah konsep yang diidentifikasi oleh URI, misalnya sebuah pesanan atau produk. Representasi adalah data yang dipertukarkan, misalnya dokumen JSON atau XML yang menggambarkan resource pada suatu waktu. Method ditujukan kepada resource, bukan kepada format representasinya, sehingga JSON dan XML adalah format serialisasi, bukan bagian dari definisi REST; konvensi JSON dibahas di [[References/JSON APIs\|JSON APIs]], sementara web service SOAP mengilustrasikan ujung kontrak formal pada Level 0 Richardson Maturity Model; lihat [[References/SOAP\|SOAP]].

## Semantik method dan status

| Method | Safe | Idempoten | Catatan |
| --- | --- | --- | --- |
| GET | Ya | Ya | Hanya membaca; tidak dimaksudkan mengubah state. |
| POST | Tidak | Tidak | Memproses data; membuat resource adalah salah satu kasus umum. |
| PUT | Tidak | Ya | Menggantikan state resource target dengan representasi yang dikirim. |
| DELETE | Tidak | Ya | Menghapus resource target. |
| PATCH | Tidak | Tidak dijamin | Perubahan sebagian; idempotensi bergantung operasi yang didefinisikan. |

Status code menyampaikan hasil: `2xx` sukses seperti `200` dan `201`, `3xx` redirection, `4xx` kesalahan client seperti `400` dan `404`, serta `5xx` kesalahan server. Client membaca status sebelum menafsirkan body.

## Statelessness

Setiap request wajib memuat seluruh informasi yang dibutuhkan server untuk memprosesnya. Server tidak menyimpan konteks antar request, sehingga identitas dan kredensial dikirim pada setiap permintaan. Konsekuensinya, [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] tetap berlaku per request, dan penyimpanan state bergeser ke client atau ke penyimpanan bersama di luar handler request.
## Hubungan dengan lainnya

[[References/APIs\|APIs]] menempatkan REST sebagai satu bentuk HTTP API, bukan definisi seluruh API. [[References/Backend Development\|Backend Development]] merangkum panduan Microsoft bahwa desain resource-oriented sebaiknya tidak menyalin struktur tabel database.

[[References/GraphQL\|GraphQL]] menumpuk operasi pada satu endpoint dengan schema bertipe dan selection set milik client, sementara REST membagi operasi pada banyak URI dengan response yang ditentukan server serta memanfaatkan semantik caching HTTP secara langsung. [[HTTPS protects transit, not application logic\|HTTPS protects transit, not application logic]] tetap berlaku karena REST tidak mengatur kerahasiaan transport.

## Sumber

- [Fielding, Architectural Styles and the Design of Network-based Software Architectures, Bab 5](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm): definisi constraint dan uniform interface.
- [MDN HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods): sifat safe dan idempoten per method.
- [MDN HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status): kelas status dan makna umumnya.
