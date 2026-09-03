---
{"dg-publish":true,"dg-path":"GraphQL.md","permalink":"/graph-ql/","title":"GraphQL","hideInFiletree":true,"tags":["references","programming","backend","schema","performance","security"],"noteIcon":"","dg-note-properties":{"title":"GraphQL","type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","backend","schema","performance","security"],"sources":["https://spec.graphql.org/September2025/","https://graphql.org/learn/introduction/","https://graphql.org/learn/schema/","https://graphql.org/learn/queries/","https://graphql.org/learn/validation/","https://graphql.org/learn/execution/","https://graphql.org/learn/response/","https://graphql.org/learn/serving-over-http/","https://graphql.org/learn/performance/","https://graphql.org/learn/security/","https://graphql.org/learn/authorization/","https://graphql.org/community/foundation/"],"created":"2026-08-29","updated":"2026-08-29"}}
---

GraphQL adalah bahasa query untuk API sekaligus model eksekusi server berdasarkan schema bertipe. Client memilih field yang diperlukan, lalu server memvalidasi operasi terhadap schema dan menjalankan resolver untuk menghasilkan response dengan bentuk yang mengikuti selection set. GraphQL tidak terikat pada database, bahasa pemrograman, atau transport tertentu.

GraphQL dibuat di Facebook pada 2012 dan dibuka sebagai proyek open source pada 2015. Sejak 2019, pengembangan spesifikasi dan aset ekosistemnya berada dalam tata kelola GraphQL Foundation serta kelompok kerja lintas organisasi. Karena itu, GraphQL kini merupakan standar terbuka, bukan API khusus Facebook.

## Masalah yang diselesaikan

Pada API berbasis endpoint, bentuk response biasanya ditetapkan server. Satu endpoint dapat mengembalikan field yang tidak dibutuhkan client, sedangkan suatu tampilan mungkin perlu memanggil beberapa endpoint untuk mengumpulkan data terkait. GraphQL memberi client selection set pada tingkat field sehingga response memuat field yang diminta dan relasi dapat ditelusuri dalam satu operasi.

Kemampuan tersebut dapat mengurangi over-fetching dan sebagian under-fetching pada batas API. Namun, GraphQL tidak menjamin request selalu lebih kecil atau lebih cepat daripada REST. Client tetap dapat meminta terlalu banyak data, schema yang buruk dapat memerlukan beberapa round trip, dan resolver yang naif dapat memicu banyak akses ke sumber data. REST juga dapat dirancang dengan endpoint khusus, field filtering, embedding, atau format lain yang sesuai kebutuhan client.

GraphQL berguna ketika beberapa client memiliki kebutuhan data berbeda, domain memiliki banyak relasi, atau UI berubah lebih cepat daripada endpoint. Mobile dapat memperoleh manfaat dari kontrol payload, tetapi label “mobile” sendiri bukan alasan cukup untuk mengadopsinya. API sederhana, transfer file, atau sistem yang sudah cocok dengan semantik resource dan caching [[References/HTTP\|HTTP]] mungkin tidak memperoleh manfaat yang sebanding dengan kompleksitas tambahan.

## Schema dan type system

Schema mendeskripsikan kemampuan API. GraphQL memiliki enam jenis named type: Object, Scalar, Enum, Interface, Union, dan Input Object. Modifier List (`[]`) menyatakan kumpulan, sedangkan Non-Null (`!`) menyatakan nilai yang tidak boleh `null`. Secara default, field GraphQL nullable.

Setiap schema wajib menyediakan root operation untuk `query`. Root `mutation` dan `subscription` bersifat opsional. Schema Definition Language (SDL) dapat mendefinisikan kontrak secara independen dari bahasa implementasi:

```graphql
type Product {
  id: ID!
  name: String!
  price: Float!
}

type Query {
  product(id: ID!): Product
}
```

Strong typing membantu editor, validasi, introspection, dokumentasi, dan code generation. Akan tetapi, type system hanya menjamin aturan yang dinyatakan schema. `String` tidak otomatis aman untuk HTML, `ID` tidak otomatis memiliki otorisasi, dan nilai bisnis tetap perlu divalidasi serta disanitasi.

## Operasi dan eksekusi

GraphQL mengenal tiga jenis operasi:

- `query` membaca data;
- `mutation` menjalankan perubahan lalu mengambil hasil;
- `subscription` mempertahankan permintaan jangka panjang dan menghasilkan data saat event terjadi.

Sebuah query dimulai dari root dan memilih field hingga Scalar atau Enum:

```graphql
query GetProduct($id: ID!) {
  product(id: $id) {
    id
    name
    price
  }
}
```

Nilai dinamis sebaiknya dikirim melalui variables, bukan interpolasi string. Alias dapat mengganti nama key response, fragment memakai ulang selection set, directive mengubah inclusion atau perilaku yang didukung implementasi, dan introspection memungkinkan client memeriksa type system.

Server mem-parse document, memvalidasinya terhadap schema, lalu mengeksekusi field melalui resolver. Resolver dapat membaca database, memanggil service lain, atau menghitung nilai. GraphQL adalah lapisan orkestrasi; ia bukan database dan tidak otomatis mengoptimalkan akses ke sumber data.

Mutation top-level dieksekusi secara serial menurut spesifikasi, tetapi efek internal dan resolver turunannya tetap perlu dirancang dengan benar. Subscription mendefinisikan semantik operasi jangka panjang, sementara WebSocket, Server-Sent Events, atau transport lain ditentukan oleh implementasi dan protokol di luar spesifikasi bahasa inti.

## Response dan error

Response GraphQL dapat memiliki key top-level `data`, `errors`, dan `extensions`. Request error seperti syntax atau validation error terjadi sebelum eksekusi sehingga `data` tidak tersedia. Field error dapat menghasilkan partial response: field yang gagal menjadi `null` sesuai aturan nullability, sementara field lain tetap dikembalikan bersama `errors`.

Konsekuensinya, status HTTP `200` tidak selalu berarti seluruh operasi berhasil. Client harus memeriksa body GraphQL, bukan hanya status transport. Non-Null juga perlu digunakan dengan hati-hati karena kegagalan pada field non-null dapat merambat dan membuat parent menjadi `null`.

## GraphQL melalui HTTP

Spesifikasi bahasa GraphQL tidak mewajibkan transport tertentu. Untuk query dan mutation stateless, implementasi umumnya memakai satu endpoint seperti `/graphql`. `POST` mendukung query dan mutation; `GET` dapat dipakai untuk query jika server mendukungnya. Media type response modern adalah `application/graphql-response+json`, dengan JSON sebagai format yang lazim.

Satu endpoint tidak berarti semua request identik bagi cache. Query melalui `GET`, response header yang tepat, dan persisted document dapat mendukung [[References/Cache-Control\|HTTP caching]] atau CDN. Normalized client cache memakai identitas objek seperti kombinasi `__typename` dan `id`. Strategi ini perlu dirancang; GraphQL tidak memberikan cache otomatis hanya karena schema memiliki `ID`.

API GraphQL perlu mempertahankan semantik transport. Authentication biasanya selesai sebelum eksekusi GraphQL. Authorization sebaiknya berada pada business logic yang dipanggil resolver agar aturan yang sama berlaku bagi seluruh jalur akses data.

## Performance dan skalabilitas

Selection set memberi client kendali payload, tetapi juga memberi kendali atas biaya. Query yang dalam, lebar, memiliki banyak alias, atau meminta list tanpa batas dapat menghabiskan CPU, memori, dan akses backend. Batasi ukuran page, depth, breadth, jumlah operasi per batch, serta kompleksitas berdasarkan biaya field.

Masalah N+1 muncul ketika satu resolver list diikuti request backend terpisah untuk setiap item. Batching dan caching per request, misalnya dengan DataLoader, dapat menggabungkan pemuatan. Solusi harus mengikuti karakter sumber data; DataLoader bukan pengganti index database, query plan, atau observability.

Persisted atau trusted documents mengganti document penuh dengan identifier. Untuk first-party client, allowlist trusted documents juga membatasi operasi yang dapat dijalankan. Public API yang menerima operasi pihak ketiga tetap memerlukan demand control karena operasinya tidak dapat diketahui seluruhnya saat build.

Monitor latency per operation dan field, error, ukuran response, cache hit, serta beban sumber data. Beri nama operasi agar trace dan log dapat dikelompokkan. GraphQL mengubah unit observability dari sekadar URL menjadi operation, field, resolver, dan dependency.

## Keamanan

Gunakan HTTPS, timeout, batas ukuran request, dan kebijakan cache untuk data sensitif. Validasi schema tidak menggantikan sanitasi input, authorization, atau perlindungan business logic. Mask detail error production agar stack trace dan informasi backend tidak bocor.

Menonaktifkan introspection dapat mengurangi kemudahan discovery pada API first-party, tetapi bukan kontrol keamanan utama. Schema masih dapat ditebak melalui error atau perilaku endpoint. Authorization, trusted documents bila sesuai, pagination, depth dan breadth limits, complexity analysis, serta rate limiting berbasis biaya memberi perlindungan yang lebih langsung.

## Evolusi schema

GraphQL mendorong evolusi tambahan: tambahkan type atau field baru tanpa mengubah client lama, tandai field lama dengan `@deprecated`, ukur penggunaannya, lalu hapus setelah tidak dipakai. “Versionless” bukan berarti bebas breaking change. Menghapus atau mengganti nama field, memperketat nullability, mengubah arti, atau mengubah perilaku resolver tetap dapat merusak client.

Schema sebaiknya mencerminkan domain dan use case, bukan menyalin tabel database secara mentah. Kontrak yang stabil, ownership yang jelas, dokumentasi field, policy deprecation, dan pemeriksaan perubahan schema diperlukan ketika banyak tim berbagi graph.

## Sumber

1. GraphQL Specification, September 2025 — GraphQL Specification Project: https://spec.graphql.org/September2025/
2. Introduction to GraphQL — GraphQL Foundation: https://graphql.org/learn/introduction/
3. Schemas and Types — GraphQL Foundation: https://graphql.org/learn/schema/
4. Queries — GraphQL Foundation: https://graphql.org/learn/queries/
5. Validation — GraphQL Foundation: https://graphql.org/learn/validation/
6. Execution — GraphQL Foundation: https://graphql.org/learn/execution/
7. Response — GraphQL Foundation: https://graphql.org/learn/response/
8. Serving over HTTP — GraphQL Foundation: https://graphql.org/learn/serving-over-http/
9. Performance — GraphQL Foundation: https://graphql.org/learn/performance/
10. Security — GraphQL Foundation: https://graphql.org/learn/security/
11. Authorization — GraphQL Foundation: https://graphql.org/learn/authorization/
12. What is the GraphQL Foundation? — GraphQL Foundation: https://graphql.org/community/foundation/
