---
{"dg-publish":true,"dg-path":"Bun.md","permalink":"/bun/","title":"Bun","hideInFiletree":true,"tags":["references","javascript","programming","npm","package-manager","development","security"],"noteIcon":"","dg-note-properties":{"title":"Bun","category":"references","tags":["references","javascript","programming","npm","package-manager","development","security"],"sources":["_raw/articles/bun-expanded.md"],"created":"2026-08-30","updated":"2026-08-30","confidence":"high"}}
---

Bun adalah perangkat terpadu untuk mengembangkan aplikasi JavaScript dan TypeScript. Satu executable `bun` menyediakan runtime, pengelola paket, test runner, bundler, transpiler, dan script runner. Pendekatan ini mengurangi kebutuhan untuk merangkai beberapa alat terpisah, tetapi tidak berarti setiap alat dalam ekosistem Node.js dapat diganti tanpa pengujian kompatibilitas.

## Runtime dan JavaScriptCore

Runtime Bun menggunakan JavaScriptCore, mesin JavaScript yang dikembangkan Apple untuk Safari, sedangkan Node.js menggunakan V8. Bun dirancang untuk waktu mulai yang singkat dan eksekusi cepat. Dokumentasi resminya menampilkan benchmark Hello World di Linux dengan waktu mulai 5,2 ms untuk Bun dan 25,1 ms untuk Node.js, tetapi hasil tersebut hanya mengukur skenario kecil pada lingkungan tertentu dan bukan jaminan bahwa semua aplikasi akan mengalami peningkatan serupa.

Bun dapat menjalankan berkas JavaScript, TypeScript, JSX, dan TSX secara langsung. Transpiler bawaan mengubah sintaks TypeScript dan JSX sebelum kode dieksekusi, sehingga banyak proyek tidak memerlukan `ts-node` atau tahap transpile terpisah untuk pengembangan. Transpilasi ini tidak melakukan pemeriksaan tipe penuh; proyek tetap memerlukan TypeScript compiler atau alat lain bila pemeriksaan tipe dan deklarasi tipe menjadi bagian dari proses kualitas.

## Kompatibilitas dengan Node.js

Bun dirancang sebagai pengganti Node.js dengan mendukung resolusi modul bergaya Node, CommonJS, ESM, global seperti `process` dan `Buffer`, serta banyak modul bawaan seperti `fs`, `path`, dan `http`. Kompatibilitas tersebut masih dikembangkan. Dokumentasi Bun mencatat bahwa sejumlah API sudah diterapkan penuh, tetapi beberapa modul dan perilaku memiliki perbedaan, keterbatasan, atau implementasi parsial.

Karena itu, migrasi sebaiknya diuji pada aplikasi nyata, terutama jika proyek menggunakan native addon, API diagnostik, perilaku stream yang spesifik, framework kompleks, atau paket yang bergantung pada detail internal Node.js. Kecepatan runtime tidak menghapus risiko perbedaan semantik dan kompatibilitas.

## Pengelola paket

`bun install` bekerja pada proyek yang memiliki `package.json`, memasang dependensi dari ekosistem npm, dan menghasilkan lockfile `bun.lock`. Pengelola paketnya mendukung `dependencies`, `devDependencies`, `optionalDependencies`, peer dependencies, overrides, resolutions, dan instalasi terfilter dalam monorepo.

Bun menyatakan instalasinya dapat jauh lebih cepat daripada npm pada benchmark resminya. Klaim tersebut perlu dibaca bersama kondisi pengujian karena hasil dipengaruhi cache, jaringan, sistem operasi, struktur dependensi, dan keadaan `node_modules`. Keuntungan praktis sebaiknya diukur pada repositori dan pipeline CI yang benar-benar digunakan.

## Workspaces dan monorepo

Bun mendukung `workspaces` dalam `package.json` untuk mengelola beberapa paket dalam satu repositori. Paket lokal dapat dirujuk dengan protokol `workspace:`, dependensi bersama dapat dideduplikasi, dan opsi `--filter` dapat membatasi instalasi atau eksekusi skrip ke workspace tertentu.

Bun juga mendukung katalog versi terpusat agar beberapa workspace dapat memakai rentang dependensi yang sama. Pada tata letak hoisted, dependensi bersama ditempatkan di akar `node_modules`; workspace tertentu dapat dibuat mandiri ketika alat deployment membutuhkan seluruh dependensi berada di bawah direktori workspace tersebut.

## Bundler dan test runner

Bundler bawaan tersedia melalui `bun build` atau API `Bun.build()`. Bundler ini mendukung JavaScript, TypeScript, JSX, TSX, CSS, pemisahan kode, plugin, mode watch, serta target browser, Bun, dan Node.js. Transformasi bawaannya mencakup penghapusan kode mati dan tree shaking, tetapi bundler tidak dimaksudkan sebagai pengganti `tsc` untuk pemeriksaan tipe atau pembuatan deklarasi tipe.

Bun juga menyertakan test runner yang kompatibel dengan banyak pola Jest, termasuk assertions, mocks, snapshots, DOM, coverage, dan watch mode. Kompatibilitas dengan Jest tidak sebaiknya dianggap identik untuk semua plugin, environment, dan ekstensi, sehingga suite pengujian yang sudah ada tetap perlu dijalankan penuh sebelum migrasi.

## Keamanan instalasi

Skrip lifecycle seperti `postinstall` dapat menjalankan perintah shell saat dependensi dipasang. Bun tidak menjalankan skrip lifecycle arbitrer dari dependensi secara default dan membatasi eksekusinya melalui daftar paket tepercaya. Proyek dapat mengatur `trustedDependencies` secara eksplisit atau memakai `--ignore-scripts` untuk menonaktifkan seluruh skrip instalasi.

Kontrol ini mengurangi satu jalur serangan rantai pasok, tetapi bukan perlindungan menyeluruh. Paket berbahaya masih dapat menimbulkan risiko ketika diimpor atau dijalankan, dan daftar tepercaya juga perlu ditinjau. Lockfile, audit dependensi, pembatasan versi, tinjauan perubahan, dan pengujian artefak tetap diperlukan.

## Kapan Bun layak digunakan

Bun sesuai untuk proyek yang menginginkan runtime dengan waktu mulai singkat, dukungan TypeScript langsung, instalasi paket cepat, dan perangkat pengembangan yang lebih terpadu. Ia juga berguna untuk skrip, layanan baru, prototipe, monorepo, dan pipeline yang dapat memperoleh manfaat dari pengurangan jumlah alat.

Untuk aplikasi Node.js yang matang, pendekatan paling aman adalah mengadopsinya secara bertahap. Tim dapat memulai dengan `bun install`, script runner, test runner, atau bundler sebelum mengganti runtime produksi. Ukur hasil pada beban kerja sendiri, jalankan seluruh pengujian, periksa native addon dan observability, lalu pertahankan jalur kembali ke Node.js sampai kompatibilitas terbukti.

## Lihat juga

- [[References/JavaScript\|JavaScript]]
- [[References/Package Managers\|Package Managers]]
- [[References/npm\|npm]]
- [[References/pnpm\|pnpm]]
- [[References/Vite\|Vite]]
- [[References/esbuild\|esbuild]]

## Sumber

- [Welcome to Bun | Bun Docs](https://bun.com/docs): cakupan runtime, pengelola paket, test runner, bundler, dan tujuan desain Bun.
- [Bun Runtime | Bun Docs](https://bun.com/docs/runtime): JavaScriptCore, dukungan TypeScript dan JSX, script runner, serta benchmark waktu mulai.
- [Node.js Compatibility | Bun Docs](https://bun.com/docs/runtime/nodejs-compat): status implementasi modul dan perbedaan perilaku terhadap Node.js.
- [Bundler | Bun Docs](https://bun.com/docs/bundler): target build, transformasi, jenis konten, dan batas pemeriksaan tipe.
- [bun install | Bun Docs](https://bun.com/docs/pm/cli/install): instalasi dependensi, lockfile, overrides, resolutions, dan lifecycle scripts.
- [Workspaces | Bun Docs](https://bun.com/docs/pm/workspaces): monorepo, protokol `workspace:`, deduplikasi, filter, dan katalog versi.
- [Lifecycle scripts | Bun Docs](https://bun.com/docs/pm/lifecycle): `trustedDependencies`, daftar izin, dan penonaktifan skrip instalasi.
