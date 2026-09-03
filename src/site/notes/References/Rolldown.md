---
{"dg-publish":true,"dg-path":"Rolldown.md","permalink":"/rolldown/","title":"Rolldown","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance","optimization"],"noteIcon":"","dg-note-properties":{"title":"Rolldown","category":"references","tags":["references","programming","javascript","architecture","performance","optimization"],"sources":["_raw/articles/rolldown-research-packet.md"],"created":"2026-09-03","updated":"2026-09-03","confidence":"high"}}
---

Rolldown adalah *bundler* JavaScript dan TypeScript berbasis Rust yang dirancang sebagai pengganti terpadu esbuild dan [[References/Rollup\|Rollup]] di dalam arsitektur [[References/Vite\|Vite]]. Proyek ini dikembangkan oleh VoidZero, mencapai tahap Release Candidate dengan API publik yang dinyatakan stabil, dan didokumentasikan di [situs resmi Rolldown](https://rolldown.rs).

Ruang lingkup fitur Rolldown lebih dekat ke esbuild daripada Rollup murni, tetapi antarmuka plugin dan bentuk konfigurasi meniru ekosistem Rollup. Kombinasi ini menjadikan Rolldown jembatan antara performa native dan kompatibilitas plugin yang sudah mapan.

## Fitur utama

Transformasi bawaan mencakup TypeScript, JSX, dan *syntax lowering* sampai ES2015 melalui Oxc, sebagaimana didokumentasikan di [panduan fitur](https://rolldown.rs/guide/notable-features). Interop CJS dan ESM ditangani secara native sehingga tidak memerlukan plugin *commonjs* terpisah, dan resolusi modul bawaan menghormati *paths* pada `tsconfig.json`.

Kontrol *code splitting* granular tersedia melalui opsi `output.codeSplitting`, yang menggantikan nama lama `advancedChunks`. Fitur *define* dan *inject* berbasis AST memberi cakupan lebih luas dari plugin *replace* biasa, sementara tipe modul dan *watch mode* masih bertanda eksperimental.

## Integrasi dengan Vite

Vite memakai Rolldown sebagai *bundler* masa depannya melalui paket jalur coba `rolldown-vite` yang bersifat *drop-in*, seperti dijelaskan di [panduan integrasi Vite](https://vite.dev/guide/rolldown). Motivasi utama migrasi adalah unifikasi *prebundling* dan *production build* dalam satu mesin, sehingga perbedaan perilaku antara mode dev dan produksi menyusut.

Opsi lama `manualChunks` tetap didukung tetapi deprecated; penggantinya adalah konfigurasi granular ala webpack `splitChunks`. Batasan pada mode tertentu dan fitur eksperimental berarti adopsi produksi sebaiknya diuji per proyek.

## Performa dan benchmark

Benchmark resmi tim Rolldown menempatkan kecepatan bundling setara esbuild dan belasan hingga puluhan kali lebih cepat dari Rollup pada *workload* ribuan modul. [Pengumuman RC](https://voidzero.dev/posts/announcing-rolldown-rc) mencatat lebih dari 3.400 commit sejak beta, ratusan fitur dan perbaikan, serta kelulusan 900 lebih tes Rollup dan 670 lebih tes esbuild.

Angka tersebut berasal dari pengukuran internal pada *workload* tertentu, bukan jaminan universal. Pengguna sebaiknya membandingkan pada graf modul, plugin, dan perangkat keras proyek sendiri sebelum mengharapkan percepatan serupa.

## Batas dan risiko

Mode seperti *module types* dan *watch mode* masih eksperimental dan dapat berubah setelah 1.0. Penggantian nama opsi dari `advancedChunks` ke `codeSplitting` perlu diperhatikan saat migrasi. Materi utama yang diakses pada paket riset ini berfokus pada modul JS dan TS; dukungan aset non-JS tidak terdokumentasi luas di sana.

Ekosistem plugin Rollup mayoritas kompatibel, tetapi ada kasus tertentu yang belum didukung penuh, dengan daftar limitasi yang dirujuk ke PR GitHub yang terus diperbarui.

## Lihat juga

- [[Rolldown menyatukan kecepatan Rust dengan kompatibilitas API Rollup\|Rolldown menyatukan kecepatan Rust dengan kompatibilitas API Rollup]]
- [[References/Rollup\|Rollup]]
- [[References/Vite\|Vite]]
- [[References/Module Bundlers\|Module Bundlers]]
- [[References/esbuild\|esbuild]]
- [[References/JavaScript\|JavaScript]]
- [[References/Deployment\|Deployment]]

## Sumber

- [Introduction: Rolldown](https://rolldown.rs/guide/introduction): tujuan unifikasi, performa, kompatibilitas plugin, dan ruang lingkup fitur.
- [Homepage: Rolldown](https://rolldown.rs): positioning, benchmark 19k modul, dan status proyek VoidZero.
- [Repositori resmi Rolldown](https://github.com/rolldown/rolldown): kode sumber, lisensi, dan rilis proyek.
- [Announcing Rolldown 1.0 RC: VoidZero](https://voidzero.dev/posts/announcing-rolldown-rc): stabilitas API, fitur kunci, statistik commit, dan kompatibilitas tes.
- [Rolldown Integration: Vite](https://vite.dev/guide/rolldown): motivasi migrasi, jalur coba rolldown-vite, dan batasan.
- [Getting Started: Rolldown](https://rolldown.rs/guide/getting-started): instalasi, tier platform, fallback WASM, dan CLI.
- [Notable Features: Rolldown](https://rolldown.rs/guide/notable-features): platform presets, transform, CJS, resolusi, define, inject, dan splitting.
- [Why bundlers: Rolldown](https://rolldown.rs/in-depth/why-bundlers): argumen kebutuhan bundling di era HTTP/2 dan ESM native.
