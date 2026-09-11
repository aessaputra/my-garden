---
{"dg-publish":true,"dg-path":"GraphQL.md","permalink":"/graph-ql/","title":"GraphQL","hideInFiletree":true,"tags":["references","programming","backend","schema"],"noteIcon":"","dg-note-properties":{"title":"GraphQL","categories":["APIs"],"type":"reference","tags":["references","programming","backend","schema"],"sources":["_raw/articles/graphql-expanded.md","_raw/articles/graphql-user-summary-2026-09-11.md","https://graphql.org/learn/introduction/","https://graphql.org/learn/schema/","https://graphql.org/learn/serving-over-http/","https://graphql.org/learn/performance/"],"created":"2026-08-29","updated":"2026-09-11"}}
---

GraphQL adalah bahasa query dan model eksekusi untuk API. Awalnya dikembangkan Facebook, kini GraphQL merupakan standar terbuka. Client memilih field yang dibutuhkan dari schema bertipe, lalu server menjalankan resolver untuk mengambil datanya.

## Cara kerja

Implementasi HTTP umumnya memakai satu endpoint, seperti `/graphql`. Client mengirim operasi berikut:

- **Query**: membaca data.
- **Mutation**: mengubah data.
- **Subscription**: menerima pembaruan saat event terjadi.

Contoh query berikut hanya meminta nama produk, jika field tersebut tersedia dalam schema:

```graphql
query {
  product(id: "42") {
    name
  }
}
```

Schema menentukan field, tipe, dan operasi yang tersedia. GraphQL bukan database dan tidak mewajibkan transport HTTP.

## Kapan berguna

GraphQL cocok ketika beberapa client membutuhkan bentuk data berbeda atau satu tampilan mengambil banyak data yang saling berhubungan. Pemilihan field dapat mengurangi **over-fetching** (data berlebih) dan **under-fetching** (data belum cukup).

Dibanding [[References/REST\|REST]], GraphQL memberi client lebih banyak kendali atas bentuk response. Namun, GraphQL tidak otomatis lebih cepat atau lebih sederhana. REST tetap memadai untuk banyak API berbasis resource.

## Batasan

- Query fleksibel tetap perlu pagination dan pembatasan biaya agar tidak membebani server.
- Resolver dapat memicu [[References/N plus one problem\|N plus one problem]]; satu request API tidak berarti satu query database.
- Schema bertipe tidak menggantikan authorization. Client juga perlu memeriksa `errors` dalam response, bukan hanya status HTTP.

## Sumber

- [Introduction to GraphQL](https://graphql.org/learn/introduction/)
- [Schemas and Types](https://graphql.org/learn/schema/)
- [Serving over HTTP](https://graphql.org/learn/serving-over-http/)
- [Performance](https://graphql.org/learn/performance/)
