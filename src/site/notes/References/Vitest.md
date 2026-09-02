---
{"dg-publish":true,"dg-path":"Vitest.md","permalink":"/vitest/","title":"Vitest","hideInFiletree":true,"tags":["references","programming","javascript","typescript","testing"],"noteIcon":"","dg-note-properties":{"title":"Vitest","category":"references","tags":["references","programming","javascript","typescript","testing"],"sources":["_raw/articles/vitest-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Vitest adalah framework pengujian JavaScript dan TypeScript yang ditenagai [[References/Vite\|Vite]]. Ia memakai konfigurasi, transformer, resolver, dan plugin Vite sehingga jalur transformasi untuk pengembangan, *build*, dan pengujian dapat berbagi konfigurasi yang sama. Walaupun dirancang sebagai *test runner* native untuk proyek Vite, Vitest juga dapat dipakai pada proyek lain dan untuk kode backend.

## Integrasi dengan Vite

Test runner yang berdiri sendiri biasanya memerlukan konfigurasi tambahan untuk TypeScript, JSX, alias modul, dan plugin. Pada proyek Vite, Vitest mengurangi duplikasi tersebut dengan membaca `vite.config.*` secara bawaan. Proyek tetap dapat menyediakan `vitest.config.*` jika pengaturan pengujian perlu dipisahkan.

Integrasi ini memberi dukungan ESM, TypeScript, JSX, dan PostCSS melalui jalur transformasi Vite. Dalam *watch mode*, Vitest membaca graf modul untuk menjalankan ulang pengujian yang berkaitan dengan berkas yang berubah, bukan selalu mengulang seluruh suite.

## API dan fitur

Vitest menyediakan assertion melalui `expect`, snapshot yang kompatibel dengan Jest, serta fungsi mocking pada objek `vi`. Chai juga tersedia sebagai pustaka assertion bawaan. Kompatibilitas Jest mempermudah migrasi, tetapi perilakunya tidak sepenuhnya identik. Vitest tidak mengaktifkan global API secara bawaan, dan beberapa detail module mocking serta automocking berbeda.

Fitur utamanya mencakup:

- *watch mode* dengan pemilihan test berdasarkan graf modul;
- snapshot dan mocking;
- coverage melalui V8 atau Istanbul;
- eksekusi paralel dan test konkuren dengan `.concurrent`;
- filtering, test projects, dan sharding untuk CI;
- lingkungan Node, `jsdom`, atau `happy-dom`;
- in-source testing dan pengujian tipe.

Vitest menjalankan berkas pengujian dalam beberapa proses secara bawaan dan mengisolasi lingkungan setiap berkas. Paralelisme membantu suite yang sesuai, tetapi test yang berbagi state global, port, database, atau sistem berkas tetap memerlukan pembersihan yang benar.

## Instalasi

Vitest dipasang sebagai *development dependency*:

```bash
pnpm add -D vitest
```

Vitest 4 memerlukan Vite 6 atau lebih baru dan Node.js 20 atau lebih baru. Skrip dasar di `package.json` dapat ditulis sebagai berikut:

```json
{
  "scripts": {
    "test": "vitest",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage"
  }
}
```

`vitest` berjalan dalam *watch mode* pada lingkungan pengembangan. `vitest run` menjalankan suite satu kali dan lebih sesuai untuk CI. Secara bawaan, nama berkas pengujian memuat `.test.` atau `.spec.`.

## Contoh dasar

```ts
// sum.ts
export function sum(a: number, b: number) {
  return a + b;
}
```

```ts
// sum.test.ts
import { expect, test } from "vitest";
import { sum } from "./sum";

test("menjumlahkan dua angka", () => {
  expect(sum(1, 2)).toBe(3);
});
```

Vitest dapat dikonfigurasi melalui blok `test` atau berkas khusus `vitest.config.ts`:

```ts
import { defineConfig } from "vitest/config";

export default defineConfig({
  test: {
    environment: "node",
    coverage: {
      include: ["src/**/*.{js,jsx,ts,tsx}"],
    },
  },
});
```

Untuk test komponen yang memerlukan DOM, ubah `environment` menjadi `jsdom` atau `happy-dom` setelah memasang paket terkait.

## Coverage

Coverage dijalankan dengan `vitest run --coverage` dan memakai provider V8 atau Istanbul. Pada Vitest 4, provider V8 menggunakan *remapping* berbasis AST. Jika `coverage.include` tidak ditentukan, laporan hanya memasukkan berkas yang dimuat selama test. Tetapkan pola `coverage.include` agar laporan juga memperhitungkan berkas sumber yang belum tersentuh pengujian.

## Pertimbangan migrasi

Migrasi dari Jest biasanya mudah pada assertion dan struktur test, tetapi tidak boleh diasumsikan sebagai penggantian tanpa perubahan. Periksa penggunaan global API, module factory, automocking, timer, dan reset mock. API global dapat diaktifkan lewat konfigurasi, tetapi impor eksplisit dari `vitest` membuat dependensi test lebih jelas.

Sebutan "cepat" juga bukan jaminan universal. Durasi aktual dipengaruhi jumlah berkas, pola impor, plugin, biaya setup, I/O, dan paralelisme. Keuntungan Vitest paling jelas ketika proyek sudah memakai Vite dan dapat menggunakan konfigurasi serta plugin yang sama.

## Lihat juga

- [[References/Vite\|Vite]]
- [[References/JavaScript\|JavaScript]]
- [[References/Package Managers\|Package Managers]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [Getting Started: Vitest](https://vitest.dev/guide/): instalasi, persyaratan versi, pola nama berkas, perintah, dan konfigurasi dasar.
- [Why Vitest](https://vitest.dev/guide/why): alasan test runner native Vite, integrasi transformasi, dan kompatibilitas Jest.
- [Features: Vitest](https://vitest.dev/guide/features.html): watch mode, paralelisme, snapshot, mocking, coverage, sharding, dan lingkungan test.
- [Migration Guide: Vitest](https://vitest.dev/guide/migration.html): persyaratan Vitest 4, perubahan coverage, dan perbedaan migrasi dari Jest.
