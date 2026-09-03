---
{"dg-publish":true,"dg-path":"ESLint.md","permalink":"/es-lint/","title":"ESLint","hideInFiletree":true,"tags":["references","programming","javascript","typescript"],"noteIcon":"","dg-note-properties":{"title":"ESLint","category":"references","tags":["references","programming","javascript","typescript"],"sources":["_raw/articles/eslint-expanded.md"],"created":"2026-08-29","updated":"2026-09-03","confidence":"high"}}
---

ESLint adalah *linter* JavaScript yang dapat dikonfigurasi untuk menemukan dan memperbaiki masalah pada kode. Pemeriksaannya mencakup potensi galat saat program berjalan, pelanggaran praktik pengembangan, dan ketidakkonsistenan gaya penulisan. Dengan pemeriksaan otomatis, tim dapat menerapkan standar yang sama sebelum perubahan kode masuk ke tahap peninjauan atau integrasi.

## Cara kerja ESLint

ESLint memeriksa kode berdasarkan sekumpulan aturan. Setiap aturan menetapkan kondisi yang harus dipenuhi dan tingkat pelanggarannya, sedangkan sebagian aturan juga menyediakan opsi konfigurasi tersendiri. Proyek dapat memakai ratusan aturan bawaan, menambahkan aturan dari *plugin*, atau membuat aturan khusus untuk kebutuhan internal.

Sebelum aturan dijalankan, *parser* mengubah kode sumber menjadi *abstract syntax tree* yang dapat dianalisis ESLint. ESLint menggunakan Espree untuk JavaScript standar, sementara *parser* khusus diperlukan untuk sintaks nonstandar seperti TypeScript. Dukungan terhadap React, Angular, TypeScript, dan format lain umumnya diberikan melalui *plugin*, *parser*, atau *processor* yang sesuai.

Aturan dapat diberi tingkat `off`, `warn`, atau `error`. Pemisahan ini memungkinkan tim membedakan aturan yang dinonaktifkan, temuan yang perlu diperhatikan, dan pelanggaran yang harus menggagalkan pemeriksaan. Konfigurasi juga dapat dibatasi menurut pola berkas sehingga kode aplikasi, pengujian, skrip konfigurasi, dan paket dalam *monorepo* tidak harus memakai aturan yang identik.

## Konfigurasi proyek

Konfigurasi modern ESLint disimpan dalam berkas seperti `eslint.config.js`, `eslint.config.mjs`, atau `eslint.config.cjs` di akar proyek dan mengekspor larik objek konfigurasi. Setiap objek dapat menentukan `files`, `ignores`, `extends`, `plugins`, `languageOptions`, dan `rules`, sehingga konfigurasi dasar dapat digabung dengan pengecualian yang lebih spesifik.

Konfigurasi siap pakai dapat dibagikan sebagai paket npm. Pendekatan ini membantu organisasi menerapkan kumpulan aturan yang sama di beberapa repositori tanpa menyalin konfigurasi secara manual. Urutan objek tetap penting karena konfigurasi yang muncul kemudian dapat menyesuaikan penerapan aturan untuk kelompok berkas tertentu.

Contoh konfigurasi dasar:

```js
import js from "@eslint/js";
import { defineConfig } from "eslint/config";

export default defineConfig([
  js.configs.recommended,
  {
    files: ["**/*.js"],
    ignores: ["dist/**"],
    rules: {
      "no-unused-vars": "error",
      "no-console": "warn",
    },
  },
]);
```

Konfigurasi tersebut memakai aturan rekomendasi ESLint, memeriksa berkas JavaScript, mengabaikan keluaran di direktori `dist`, dan menetapkan tingkat pelanggaran untuk dua aturan. Pemilihan aturan sebaiknya mengikuti risiko proyek, bukan sekadar mengaktifkan sebanyak mungkin aturan.

## Perbaikan otomatis dan integrasi

Perintah `npx eslint .` menjalankan pemeriksaan pada proyek dari terminal. Opsi `--fix` menerapkan perbaikan yang tersedia langsung pada berkas, sedangkan `--fix-dry-run` menghitung hasil perbaikan tanpa menulis perubahan ke sistem berkas. Tidak semua temuan aman untuk diperbaiki otomatis. ESLint membedakan *fix* yang dirancang agar tidak mengubah logika aplikasi dari *suggestion* yang dapat memengaruhi logika dan hanya tersedia melalui integrasi editor.

ESLint dapat dipasang di editor agar temuan muncul saat kode ditulis, lalu dijalankan kembali melalui skrip pembangunan, *pre-commit hook*, atau CI untuk pemeriksaan yang konsisten. Dalam CI, opsi `--max-warnings` dapat membatasi jumlah peringatan yang diizinkan, dan kode keluar ESLint membedakan pemeriksaan berhasil, pelanggaran aturan, serta masalah konfigurasi atau galat internal. Integrasi editor dan alat komunitas tidak semuanya dipelihara oleh tim ESLint, sehingga kompatibilitas versinya perlu diperiksa secara terpisah.

## Dukungan TypeScript dan JSX

Untuk TypeScript, proyek `typescript-eslint` menyediakan *parser*, *plugin*, dan konfigurasi bersama yang menghubungkan TypeScript dengan ESLint. Konfigurasi rekomendasinya dapat diterapkan pada ekstensi JavaScript, JSX, TypeScript, dan TSX melalui pola `files` yang sesuai. JSX sendiri dapat diproses sebagai sintaks JavaScript, sedangkan aturan khusus React biasanya berasal dari *plugin* ekosistem.

Sebagian aturan TypeScript memakai informasi tipe untuk mendeteksi masalah yang tidak terlihat dari satu berkas saja. Mode ini meminta TypeScript menganalisis proyek, sehingga pemeriksaannya lebih kuat tetapi lebih lambat daripada aturan tanpa informasi tipe. Dokumentasi `typescript-eslint` menyarankan konfigurasi bertipe melalui preset seperti `recommendedTypeChecked` dan `parserOptions.projectService: true`, dengan konsekuensi biaya waktu yang lebih terasa pada proyek besar.

## Penggunaan dalam tim

ESLint paling efektif ketika konfigurasi diperlakukan sebagai kebijakan kode yang ditinjau bersama. Tim perlu menentukan aturan yang berkaitan dengan galat sebagai `error`, memakai `warn` untuk adopsi bertahap, dan memberi alasan pada pengecualian yang benar-benar diperlukan. Perbaikan otomatis dapat dijalankan saat menyimpan berkas, tetapi CI tetap perlu menjalankan pemeriksaan penuh agar hasil tidak bergantung pada pengaturan editor masing-masing pengembang.

Dengan konfigurasi yang terukur, ESLint membantu menemukan masalah lebih awal dan menjaga konsistensi tanpa menggantikan pengujian, pemeriksaan tipe, atau peninjauan kode. Perannya adalah analisis statis berbasis aturan: melengkapi lapisan jaminan kualitas lain, bukan mengambil alih semuanya.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Linters dan Formatters\|Linters dan Formatters]]
- [[References/Prettier\|Prettier]]
- [[References/Biome\|Biome]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [Core Concepts: ESLint](https://eslint.org/docs/latest/use/core-concepts): definisi, aturan, perbaikan, plugin, parser, processor, dan integrasi.
- [Configuration Files: ESLint](https://eslint.org/docs/latest/use/configure/configuration-files): format konfigurasi modern, objek konfigurasi, pola berkas, dan pengabaian.
- [Command Line Interface Reference: ESLint](https://eslint.org/docs/latest/use/command-line-interface): penggunaan CLI, perbaikan otomatis, peringatan, dan kode keluar.
- [Integrations: ESLint](https://eslint.org/docs/latest/use/integrations): integrasi komunitas dengan editor, alat pembangunan, kontrol sumber, dan CI.
- [Getting Started: typescript-eslint](https://typescript-eslint.io/getting-started): pemasangan dan konfigurasi ESLint untuk JavaScript, JSX, TypeScript, dan TSX.
- [Linting with Type Information: typescript-eslint](https://typescript-eslint.io/getting-started/typed-linting): aturan berbasis informasi tipe, konfigurasi, dan biaya performa.
