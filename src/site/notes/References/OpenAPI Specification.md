---
{"dg-publish":true,"dg-path":"OpenAPI Specification.md","permalink":"/open-api-specification/","title":"OpenAPI Specification","hideInFiletree":true,"tags":["references","programming","backend"],"noteIcon":"","dg-note-properties":{"title":"OpenAPI Specification","aliases":["Open API Spec","OAS"],"categories":["APIs"],"type":"reference","tags":["references","programming","backend"],"sources":["_raw/articles/openapi-user-summary-2026-09-11.md","_raw/articles/documentation-generation-with-ai-research-packet.md","_raw/articles/backend-development-evidence-addendum-2026-09-07.md","https://spec.openapis.org/oas/latest.html"],"created":"2026-09-11","updated":"2026-09-11","confidence":"medium"}}
---

OpenAPI Specification (OAS) adalah standar terbuka untuk mendeskripsikan HTTP API dalam YAML atau JSON. Kontraknya dapat dibaca manusia dan diproses tool tanpa harus melihat implementasi server.

Sering digunakan untuk [[References/REST\|REST]], tetapi OpenAPI tidak mensyaratkan semua batasan arsitektur REST. Spesifikasinya dahulu bernama Swagger; kini Swagger juga merujuk pada ekosistem tool yang bekerja dengan OpenAPI.

## Isi kontrak

- **Endpoint dan operasi**: path serta HTTP method yang tersedia.
- **Input dan output**: parameter, request body, response, status code, dan schema data.
- **Keamanan**: skema autentikasi dan persyaratan akses yang dideskripsikan API.
- **Metadata**: judul, versi API, deskripsi, dan alamat server.

YAML atau JSON adalah format dokumen OpenAPI, bukan pembatas format payload API yang dideskripsikannya.

## Kegunaan dan batasan

Tool dapat memakai kontrak untuk menghasilkan dokumentasi interaktif, client SDK, dan bahan pengujian. Kontrak yang sama membantu tim menyepakati desain [[References/APIs\|APIs]] sebelum implementasi dibuat.

[[References/Documentation Generation with AI\|Documentation Generation with AI]] menempatkan kontrak sebagai dasar dokumentasi, bukan tebakan dari potongan kode. Namun, dokumen OpenAPI tidak otomatis menjamin server mengikuti kontrak atau menerapkan autentikasi dengan benar. Kesesuaian implementasi tetap perlu diuji.

## Sumber

- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
