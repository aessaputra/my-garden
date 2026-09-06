---
{"dg-publish":true,"dg-path":"TypeScript.md","permalink":"/type-script/","title":"TypeScript","hideInFiletree":true,"tags":["references","programming","javascript","development","testing"],"noteIcon":"","dg-note-properties":{"title":"TypeScript","categories":["Programming Languages","Developer Tools"],"tags":["references","programming","javascript","development","testing"],"sources":["_raw/articles/typescript-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

TypeScript adalah bahasa pemrograman dan static type checker yang memperluas JavaScript dengan sintaks tipe. Program TypeScript diperiksa sebelum eksekusi, kemudian menghasilkan JavaScript untuk runtime yang sudah ada.

[Dokumentasi resminya](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html) menyebut TypeScript sebagai typed superset of JavaScript. Artinya, sintaks JavaScript legal dalam TypeScript. Namun, kode JavaScript yang valid tetap dapat menghasilkan type error karena TypeScript menambahkan aturan penggunaan nilai.

## Pemeriksaan statis

Checker menggabungkan annotation, inference, declaration files, generic, union, narrowing, dan control flow analysis. Ia memeriksa assignment, argument, return value, property access, serta operasi lain sebelum program dijalankan.

Annotation dapat ditulis pada variable, parameter, return value, object, callback, dan public API. Banyak local type tidak perlu ditulis karena [TypeScript melakukan inference](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) dari initializer dan konteks.

Pemeriksaan ini mempercepat feedback dan mendukung editor tooling, tetapi tidak membuktikan algoritme benar. TypeScript juga tidak memvalidasi response jaringan, file, environment variable, atau input pengguna saat runtime.

## JavaScript dan emit

TypeScript mempertahankan perilaku runtime JavaScript. Type annotation dan konstruksi type-only dihapus ketika output JavaScript dibuat. Transformasi sintaks dapat menargetkan runtime lama, tetapi tipe tidak menjadi pemeriksaan runtime otomatis.

Proyek dapat mengadopsinya bertahap. [`allowJs`](https://www.typescriptlang.org/tsconfig/allowJs.html) menerima file JavaScript dalam project, sedangkan [`checkJs`](https://www.typescriptlang.org/tsconfig/checkJs.html) melaporkan error pada file tersebut. File dapat dikonversi secara bertahap ke `.ts` atau `.tsx`.

## Structural typing

Type compatibility didasarkan pada struktur anggota, bukan identitas deklarasi. Dua object dapat kompatibel bila shape yang dibutuhkan tersedia dengan tipe sesuai, meskipun keduanya tidak berbagi deklarasi nominal.

[Structural subtyping](https://www.typescriptlang.org/docs/handbook/type-compatibility.html) sesuai dengan object literal, function, dan library JavaScript. Namun, dokumentasi TypeScript juga mencatat beberapa aturan yang sengaja tidak sound untuk mendukung pola JavaScript umum.

## Strictness

Kekuatan pemeriksaan bergantung pada konfigurasi dan coverage. `any` menonaktifkan pemeriksaan lanjutan pada nilai terkait. Type assertion, ignore directive, declaration yang salah, atau module tanpa tipe juga dapat melemahkan signal.

Flag [`strict`](https://www.typescriptlang.org/tsconfig/strict.html) mengaktifkan keluarga pemeriksaan yang memberi jaminan lebih kuat. Pengaturan seperti `noImplicitAny` mencegah fallback implisit ke `any`, sedangkan strictness lain memperketat nullability dan function compatibility.

Maintainability tidak muncul otomatis dari pemakaian TypeScript. Manfaatnya bergantung pada kontrak API yang jelas, konfigurasi bersama, pemeriksaan CI, runtime validation, tests, dependency types, dan review terhadap escape hatch.

TypeScript berhubungan langsung dengan [[References/JavaScript\|JavaScript]], [[References/Type Checkers\|Type Checkers]], [[References/Linters dan Formatters\|Linters dan Formatters]], [[References/Module Bundlers\|Module Bundlers]], serta build tool seperti [[References/Vite\|Vite]] dan [[References/SWC\|SWC]]. Transpiler atau bundler yang menerima sintaks TypeScript belum tentu menjalankan type checking.

## Lanjutan

- [[TypeScript extends JavaScript without changing runtime behavior\|TypeScript extends JavaScript without changing runtime behavior]]
- [[Structural typing matches JavaScript shapes without proving identity\|Structural typing matches JavaScript shapes without proving identity]]
- [[Strict TypeScript settings turn annotations into stronger feedback\|Strict TypeScript settings turn annotations into stronger feedback]]
