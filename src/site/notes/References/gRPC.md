---
{"dg-publish":true,"dg-path":"gRPC.md","permalink":"/g-rpc/","title":"gRPC","hideInFiletree":true,"tags":["references","programming","backend","http"],"noteIcon":"","dg-note-properties":{"title":"gRPC","categories":["APIs"],"type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","backend","http"],"sources":["_raw/articles/grpc-user-summary-2026-09-10.md","https://grpc.io/docs/what-is-grpc/introduction/","https://protobuf.dev/overview/"],"created":"2026-09-10","updated":"2026-09-10"}}
---

gRPC adalah framework RPC open source untuk pemanggilan method pada server jarak jauh seolah objek lokal. Kontrak layanan ditulis dalam Protocol Buffers (file `.proto`), termasuk nama layanan, method, parameter, dan tipe return.

Dari kontrak tersebut compiler menghasilkan stub client dan kerangka server. Developer mengimplementasikan logika server dan memanggil stub dari client; detail serialisasi dan jaringan ditangani framework.

## Protobuf dan transport

Protocol Buffers berperan ganda sebagai bahasa definisi antarmuka (IDL) sekaligus format pertukaran pesan biner. Format ini netral bahasa dan umumnya lebih kecil serta lebih cepat diproses daripada JSON teks, dengan dukungan evolusi skema.

Transport default gRPC adalah HTTP/2, sehingga streaming dua arah, multiplexing, dan header compression tersedia. gRPC tidak terikat JSON sebagai format pesan.

## Empat pola interaksi

| Pola | Alur |
| --- | --- |
| Unary | Satu request, satu response. |
| Server streaming | Satu request, aliran response. |
| Client streaming | Aliran request, satu response. |
| Bidirectional streaming | Aliran request dan response secara independen. |

Pola streaming cocok untuk update berkelanjutan, transfer besar, atau percakapan dua arah; unary cukup untuk request-response sederhana.

## Perbedaan dengan pendekatan lain

Berbeda dari [[References/REST\|REST]] yang memetakan operasi ke URI resource dan [[References/HTTP\|HTTP]] method, gRPC memanggil method bernama pada layanan; kontraknya eksplisit per method, bukan per resource. Berbeda dari [[References/JSON APIs\|JSON APIs]] yang bertukar dokumen teks, gRPC umumnya bertukar pesan biner hasil code generation, dan berbeda dari [[References/GraphQL\|GraphQL]] yang mengeksekusi query client terhadap schema, gRPC tidak menyediakan introspection dan query dinamis yang sama.

[[References/APIs\|APIs]] menempatkan gRPC sebagai bentuk RPC API, bukan sekadar HTTP API berbasis resource.
