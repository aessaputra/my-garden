---
{"dg-publish":true,"dg-path":"Yarn.md","permalink":"/yarn/","title":"Yarn","hideInFiletree":true,"tags":["javascript","programming","npm","package-manager","development"],"noteIcon":"","dg-note-properties":{"title":"Yarn","category":"references","tags":["javascript","programming","npm","package-manager","development"],"sources":["_raw/articles/yarn-expanded.md","https://yarnpkg.com/getting-started/qa","https://yarnpkg.com/features/pnp","https://yarnpkg.com/features/caching","https://yarnpkg.com/features/workspaces","https://yarnpkg.com/configuration/yarnrc"],"created":"2026-08-30","updated":"2026-08-30","confidence":"high"}}
---

Yarn adalah pengelola paket untuk proyek JavaScript. Versi pertamanya dikembangkan oleh Sebastian McKenzie ketika bekerja di Facebook, tetapi Yarn bukan produk yang kini dioperasikan oleh Facebook. Proyek ini berada di organisasi GitHub tersendiri dan dikembangkan sebagai proyek *open source*.

Seperti [[References/npm\|npm]], Yarn memasang paket dari ekosistem npm dan mencatat dependensi proyek. Perbedaannya terutama terletak pada strategi instalasi, pengelolaan cache, dukungan monorepo, dan mekanisme resolusi paket. Konsep dasarnya mengikuti komponen yang dijelaskan dalam [[References/Package Managers\|Package Managers]], seperti manifest, registry, resolver, cache, linker, dan *lockfile*.

## Konsistensi dan integritas instalasi

Yarn menggunakan berkas `yarn.lock` untuk mempertahankan versi dependensi yang konsisten di berbagai mesin. Pada lingkungan CI, Yarn Modern secara bawaan menolak perubahan terhadap *lockfile*, sehingga pemasangan yang tidak sesuai dengan kondisi repositori dapat dihentikan lebih awal.

Yarn juga memeriksa *checksum* paket dalam cache. Melalui `checksumBehavior`, Yarn dapat menolak ketidaksesuaian, memperbarui nilai, menghapus paket untuk diunduh ulang, atau mengabaikan pemeriksaan tersebut. Mode yang dipilih perlu mengikuti kebutuhan keamanan dan pemulihan cache proyek.

## Cache, offline mirror, dan zero-installs

Cache mengurangi kebutuhan untuk mengunduh paket yang sama berulang kali. Tim dapat menonaktifkan cache global agar arsip paket disimpan di `.yarn/cache` dan disertakan dalam repositori. Pola ini membentuk *offline mirror*, sehingga setiap commit tetap dapat dipasang ketika registry tidak tersedia.

Jika *offline mirror* digabungkan dengan Plug'n'Play, proyek dapat menerapkan *zero-installs*. Dalam pola ini, perpindahan cabang umumnya tidak memerlukan `yarn install`. Dependensi native tertentu tetap memerlukan instalasi karena tidak dapat langsung dijalankan dari arsip cache.

## Plug'n'Play

Pada Yarn Modern, Plug'n'Play atau PnP merupakan strategi pemasangan bawaan. Alih-alih membuat direktori `node_modules`, PnP menghasilkan pemuat `.pnp.cjs` yang memetakan pohon dependensi dan lokasi paket di disk.

Pendekatan ini mengurangi operasi penyalinan berkas, mendeteksi *ghost dependencies*, dan menghasilkan pesan kesalahan dependensi yang lebih informatif. Proyek tetap dapat memakai strategi instalasi tradisional ketika kompatibilitas dengan `node_modules` diperlukan.

Migrasi proyek lama ke PnP dapat memerlukan penyesuaian. Sebagian paket atau skrip mungkin bergantung pada perilaku `node_modules` yang tidak dinyatakan dalam manifest. Masalah seperti ini biasanya terlihat sebagai *ghost dependency* atau asumsi terhadap struktur direktori.

## Workspaces dan Constraints

Yarn menyediakan *workspaces* untuk mengelola beberapa paket dalam satu proyek dan menautkan referensi antarpaket. Fitur ini cocok untuk monorepo yang menyimpan aplikasi, pustaka internal, dan konfigurasi bersama dalam satu repositori.

Constraints dapat menegakkan aturan lintas *workspace*, misalnya menyelaraskan versi dependensi, melarang paket tertentu, atau memastikan bidang seperti `license` dan `engines.node` tersedia secara konsisten. Pemeriksaan ini membantu mencegah konfigurasi antar paket menyimpang seiring pertumbuhan monorepo.

## Yarn Classic dan Yarn Modern

Yarn Classic merujuk pada lini versi 1.x. Versi ini masih digunakan, tetapi dokumentasi resmi menganjurkan migrasi ke Yarn Modern untuk memperoleh strategi pemasangan baru, arsitektur plugin, dukungan *workspaces* yang lebih matang, dan fitur seperti Constraints.

Rilis modern tidak lagi didistribusikan melalui paket `yarn` di npm. Instalasinya dilakukan melalui Corepack atau perintah `yarn set version`. Pemisahan ini mencegah perubahan besar pada Yarn Modern langsung memengaruhi lingkungan yang masih bergantung pada instalasi global Yarn Classic.

## Lihat juga

- [[References/Package Managers\|Package Managers]]: konsep umum manifest, resolver, cache, linker, dan *lockfile*
- [[References/npm\|npm]]: pengelola paket bawaan ekosistem Node.js
- [[References/JavaScript\|JavaScript]]: bahasa dan ekosistem modul yang dikelola Yarn
- [[References/Git\|Git]]: sistem kontrol versi untuk menyimpan `yarn.lock` dan cache proyek bila memakai *zero-installs*

## Sumber

- [Questions & Answers](https://yarnpkg.com/getting-started/qa): sejarah proyek, perbedaan Yarn Classic dan Yarn Modern, serta metode instalasi modern.
- [Plug'n'Play](https://yarnpkg.com/features/pnp): cara kerja PnP, manfaat, batas kompatibilitas, dan perlindungan terhadap *ghost dependencies*.
- [Cache strategies](https://yarnpkg.com/features/caching): cache global, *offline mirror*, dan pola *zero-installs*.
- [Workspaces](https://yarnpkg.com/features/workspaces): pengelolaan monorepo dan penerapan Constraints lintas paket.
- [Settings `.yarnrc.yml`](https://yarnpkg.com/configuration/yarnrc): konfigurasi *checksum*, cache, dan instalasi immutable.
