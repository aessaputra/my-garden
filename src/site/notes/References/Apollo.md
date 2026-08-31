---
{"dg-publish":true,"dg-path":"Apollo.md","permalink":"/apollo/","title":"Apollo","hideInFiletree":true,"tags":["references","programming","backend","schema","performance","security"],"dg-note-properties":{"title":"Apollo","category":"references","tags":["references","programming","backend","schema","performance","security"],"sources":["https://www.apollographql.com/docs/react/get-started","https://www.apollographql.com/docs/react/caching/overview","https://www.apollographql.com/docs/react/data/queries","https://www.apollographql.com/docs/react/data/mutations","https://www.apollographql.com/docs/react/data/subscriptions","https://www.apollographql.com/docs/apollo-server/getting-started","https://www.apollographql.com/docs/apollo-server/data/resolvers","https://www.apollographql.com/docs/apollo-server/security/authentication","https://www.apollographql.com/docs/apollo-server/integrations/plugins","https://www.apollographql.com/docs/federation","https://www.apollographql.com/docs/graphos/platform/schema-management","https://www.apollographql.com/docs/graphos/platform/schema-management/checks","https://www.apollographql.com/docs/graphos/platform/insights/operation-metrics"],"created":"2026-08-29","updated":"2026-08-29"}}
---

Apollo adalah keluarga perangkat untuk membangun, mengonsumsi, menggabungkan, dan mengoperasikan API [[References/GraphQL\|GraphQL]]. Komponennya dapat dipakai terpisah. Apollo Client berjalan di sisi aplikasi, Apollo Server mengeksekusi API GraphQL, Apollo Federation menyatukan beberapa API melalui router, sedangkan GraphOS menyediakan layanan komersial untuk registry schema, pemeriksaan perubahan, delivery, dan observability.

Menyebut Apollo sebagai satu "platform GraphQL" memang praktis, tetapi mudah menutupi batas antarkomponen. Menggunakan Apollo Client tidak mengharuskan backend memakai Apollo Server. Apollo Server juga dapat melayani client GraphQL lain. Federation dan GraphOS baru relevan ketika arsitektur dan kebutuhan operasional membutuhkannya.

## Apollo Client

Apollo Client adalah pustaka pengelolaan data untuk aplikasi JavaScript. Integrasi resminya paling menonjol pada [[References/React\|React]], tetapi inti client menangani eksekusi operasi, jaringan melalui Apollo Link, error, cache, dan state lokal. `ApolloProvider` menempatkan instance client dalam context React. Hook `useQuery`, `useMutation`, dan `useSubscription` menghubungkan operasi GraphQL dengan lifecycle UI.

Konfigurasi dasar memerlukan link dan cache:

```javascript
import { ApolloClient, HttpLink, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  link: new HttpLink({ uri: "/graphql" }),
  cache: new InMemoryCache(),
});
```

`useQuery` biasanya menjalankan query saat komponen dirender dan mengekspos status seperti `loading`, `error`, `data`, dan `networkStatus`. `useLazyQuery` menunda eksekusi sampai fungsi pemicunya dipanggil. Fetch policy menentukan apakah hasil dibaca dari cache, network, atau kombinasi keduanya. Polling dan `refetch` dapat memperbarui data, tetapi keduanya menambah request dan bukan pengganti strategi freshness yang jelas.

`useMutation` mengembalikan fungsi eksekusi dan status hasil. Mutation tidak otomatis berjalan saat render. Setelah backend berubah, UI dapat diselaraskan dengan refetch query, mengembalikan objek yang berubah agar cache menormalisasikannya, atau memperbarui cache secara eksplisit. Optimistic response dapat mempercepat respons yang dirasakan pengguna, tetapi aplikasi tetap harus menangani penolakan server dan rollback.

Subscription cocok untuk perubahan kecil yang membutuhkan latensi rendah, bukan mekanisme default untuk setiap data yang mungkin berubah. Apollo Client mendukung WebSocket dan multipart response melalui HTTP sesuai kemampuan endpoint. Polling atau refetch sering lebih sederhana bila pembaruan tidak harus langsung.

## Normalized cache

`InMemoryCache` menyimpan hasil query sebagai graph objek yang dinormalisasi. Secara default, objek diidentifikasi dari `__typename` dan `id` atau `_id`. Field objek bertingkat diganti dengan referensi ke entri cache. Ketika response baru membawa cache ID yang sama, field yang masuk digabungkan dengan data yang sudah ada.

Normalisasi mengurangi duplikasi dan memungkinkan beberapa komponen melihat objek yang sama. Namun, cache tidak memahami identitas domain tanpa konfigurasi yang memadai. Objek tanpa identifier stabil dapat tertanam pada parent. Pagination, key argument, merge list, dan hasil mutation sering memerlukan type policy. Cache yang salah konfigurasi dapat menampilkan data lama, menimpa page, atau mencampur hasil dengan argument berbeda.

Apollo Client juga dapat menyimpan state lokal, tetapi ini bukan alasan otomatis untuk memindahkan seluruh state aplikasi ke cache GraphQL. State server, state form, state navigasi, dan state UI memiliki lifecycle berbeda. Pilihan penyimpanan sebaiknya mengikuti ownership dan kebutuhan sinkronisasi.

## Apollo Server

Apollo Server adalah server GraphQL untuk Node.js. Paket `@apollo/server` menangani request GraphQL dan memakai paket `graphql` untuk parsing, validation, serta execution. Aplikasi memberikan type definitions dan resolver map. Resolver mengisi nilai satu field dan dapat membaca database, memanggil API lain, atau menjalankan business logic.

```javascript
const server = new ApolloServer({
  typeDefs,
  resolvers,
});
```

Resolver menerima `parent`, `args`, `contextValue`, dan `info`. `contextValue` dibuat per request dan lazim membawa identitas pengguna, data source, atau dependency lain. Authentication dapat dilakukan saat context dibentuk. Authorization harus tetap diterapkan pada business logic atau lapisan kebijakan yang tepat, bukan dianggap selesai karena schema bertipe atau karena user tersedia di context.

Plugin Apollo Server mengaitkan kode dengan lifecycle server dan request, termasuk parsing, validation, execution, error, serta pengiriman response. Plugin berguna untuk logging, tracing, cache, dan shutdown. Hook yang terlalu berat dapat menambah latency ke setiap operasi, sehingga overhead dan data sensitif yang direkam perlu diaudit.

Apollo Server bukan database, ORM, schema registry, ataupun alat federasi lengkap dengan sendirinya. Performa masih bergantung pada desain resolver, batching, cache, query backend, demand control, timeout, dan observability. Risiko N+1 tetap ada ketika resolver mengambil data per item tanpa batching yang sesuai.

## Federation dan router

Apollo Federation menggabungkan beberapa API menjadi satu graph. Setiap API penyusun disebut subgraph, hasil komposisinya disebut supergraph, dan client mengirim operasi ke router. Router menyusun query plan, memanggil subgraph yang diperlukan, lalu menggabungkan hasil menjadi satu response.

Federation memisahkan ownership schema antartim sambil mempertahankan satu kontrak untuk client. Ia tidak menghapus koordinasi. Perubahan entity key, nullability, ownership field, dan dependency lintas subgraph tetap dapat menimbulkan kegagalan komposisi atau runtime. Tambahan network hop juga dapat meningkatkan latency. Federation paling masuk akal ketika batas domain dan ownership tim cukup jelas; monolit GraphQL sering lebih sederhana untuk sistem kecil.

Subgraph tidak harus memakai Apollo Server selama implementasinya kompatibel dengan Federation. Client juga tidak perlu mengetahui pembagian internal tersebut. Demi keamanan dan konsistensi orkestrasi, subgraph biasanya tidak dibuka sebagai endpoint publik yang dilewati client.

## GraphOS

GraphOS adalah layanan terkelola Apollo untuk operasi graph. Schema registry menyimpan schema dan variant. Rover CLI atau API dapat memublikasikan perubahan. Schema checks memeriksa validitas dan komposisi, lint, serta dampak perubahan terhadap operasi historis. Pemeriksaan berbasis traffic dapat menunjukkan bahwa perubahan yang secara teori breaking tidak mengenai client aktif, tetapi hasilnya hanya sebaik cakupan telemetry dan periode retensinya.

Insights menampilkan metrik operasi dan field seperti request rate, latency, error, serta penggunaan field. Data ini membantu diagnosis dan penghapusan field lama. Pengumpulan metrics harus dikonfigurasi, dan detail fitur serta retensi bergantung pada paket layanan. GraphOS bukan syarat menjalankan Apollo Client, Apollo Server, atau Federation; tim dapat memilih registry, router, dan observability lain.

## Batas dan keputusan adopsi

Apollo mengurangi pekerjaan integrasi ketika tim memilih konvensi yang sama untuk client, server, schema, dan operasi. Harga yang dibayar adalah API pustaka yang luas, kebijakan cache yang perlu dipahami, komponen infrastruktur tambahan, serta potensi ketergantungan pada layanan vendor.

Nilai "lebih cepat" perlu dipisahkan. Cache dan optimistic UI dapat memperbaiki perceived latency. Router dapat mengurangi koordinasi di client. Schema checks dapat mempercepat review perubahan. Tidak satu pun menjamin request backend lebih cepat atau sistem lebih scalable tanpa pengukuran. Mulai dari komponen yang menyelesaikan masalah nyata, ukur hasilnya, lalu tambahkan Federation atau GraphOS ketika kebutuhan ownership dan operasi sudah muncul.

## Sumber

1. Get Started with Apollo Client — Apollo GraphQL: https://www.apollographql.com/docs/react/get-started
2. Caching in Apollo Client — Apollo GraphQL: https://www.apollographql.com/docs/react/caching/overview
3. Queries — Apollo GraphQL: https://www.apollographql.com/docs/react/data/queries
4. Mutations in Apollo Client — Apollo GraphQL: https://www.apollographql.com/docs/react/data/mutations
5. Subscriptions — Apollo GraphQL: https://www.apollographql.com/docs/react/data/subscriptions
6. Get Started with Apollo Server — Apollo GraphQL: https://www.apollographql.com/docs/apollo-server/getting-started
7. Resolvers — Apollo GraphQL: https://www.apollographql.com/docs/apollo-server/data/resolvers
8. Authentication and Authorization — Apollo GraphQL: https://www.apollographql.com/docs/apollo-server/security/authentication
9. Creating Apollo Server Plugins — Apollo GraphQL: https://www.apollographql.com/docs/apollo-server/integrations/plugins
10. Introduction to Apollo Federation — Apollo GraphQL: https://www.apollographql.com/docs/federation
11. GraphOS Schema Management — Apollo GraphQL: https://www.apollographql.com/docs/graphos/platform/schema-management
12. Schema Checks — Apollo GraphQL: https://www.apollographql.com/docs/graphos/platform/schema-management/checks
13. Operation Metrics in GraphOS — Apollo GraphQL: https://www.apollographql.com/docs/graphos/platform/insights/operation-metrics
