---
{"dg-publish":true,"dg-path":"Module Bundlers.md","permalink":"/module-bundlers/","title":"Module Bundlers","hideInFiletree":true,"tags":["references","programming","javascript","architecture","performance","optimization"],"noteIcon":"","dg-note-properties":{"title":"Module Bundlers","category":"references","tags":["references","programming","javascript","architecture","performance","optimization"],"sources":["_raw/articles/module-bundlers-research-packet.md"],"created":"2026-09-03","updated":"2026-09-03","confidence":"high"}}
---


Module bundler membaca entry aplikasi, menelusuri dependensi, membangun graph modul, lalu menghasilkan satu atau beberapa artifact untuk browser, server, atau konsumen pustaka.

Pekerjaannya dapat mencakup resolution, transformasi, linking, optimasi, code splitting, minifikasi, source map, pemrosesan aset, hashing, dan penulisan output.

## Mengapa bundler digunakan

Modul memecah program menjadi unit yang dapat diimpor dan diekspor. Browser modern memahami ES modules, tetapi proyek nyata juga memakai paket npm, TypeScript, JSX, CSS, gambar, dan target runtime berbeda.

Bundler menghubungkan kebutuhan itu melalui satu pipeline. Ia dapat menyelesaikan package specifier, mentransformasi sintaks, memproses aset, serta menulis output yang sesuai dengan target distribusi.

Bundler tidak selalu menghasilkan satu file. Aplikasi modern lazim menghasilkan entry chunk, async chunk, shared chunk, CSS, source map, manifest, gambar, dan font.

## Entry dan dependency graph

Entry adalah titik awal traversal. Dari sana, bundler membaca `import`, `export`, `require`, dynamic `import()`, serta referensi aset yang dipahami pipeline.

Setiap hubungan membentuk edge pada dependency graph. Node dapat berupa modul JavaScript, stylesheet, gambar, virtual module, atau format lain yang didukung loader dan plugin.

Graph memungkinkan bundler menentukan urutan proses, modul yang dipakai bersama, kode yang dapat dipisah, serta artifact yang perlu ditulis.

Graph statis tetap memiliki batas. Import dinamis yang tidak dapat dianalisis, penggunaan `eval`, CommonJS dinamis, dan side effect dapat memaksa bundler mempertahankan lebih banyak kode.

## Module resolution

Dependency specifier seperti `./utils.js`, `react`, atau alias proyek harus dipetakan ke file atau modul konkret. Resolution membaca lokasi importer, ekstensi, alias, conditions, dan metadata paket.

Metadata seperti `exports`, `imports`, `browser`, `module`, dan `main` dapat memengaruhi entry paket yang dipilih. Precedence dan semantics tidak selalu identik antarbundler.

Perbedaan resolution dapat membuat kode bekerja pada satu toolchain tetapi gagal pada toolchain lain. Migrasi perlu menguji package exports, symlink, monorepo, alias, dan conditional exports.

## Loaders, transformasi, dan plugins

Loader atau transformer mengubah source menjadi bentuk yang dapat diproses pipeline. Contohnya ialah menghapus tipe TypeScript, mengubah JSX, mengompilasi CSS, atau mengubah gambar menjadi URL.

Plugin dapat ikut campur pada resolution, loading, transformasi, graph, chunk, dan output. [[References/Rollup\|Rollup]] memakai hook seperti `resolveId`, `load`, `transform`, dan `generateBundle`.

Webpack memisahkan loaders untuk transformasi modul dan plugins untuk memperluas proses build. [[References/esbuild\|esbuild]] menyediakan hook `onResolve` dan `onLoad` melalui API plugin.

Fleksibilitas tersebut memperluas supply chain. Plugin berjalan dengan hak proses build, sehingga dapat membaca berkas, environment, atau jaringan sesuai izin runtime dan CI.

## Transformasi bukan type checking

Mengubah TypeScript menjadi JavaScript tidak sama dengan memeriksa tipe. Banyak bundler hanya menghapus anotasi tipe agar pipeline tetap cepat.

Jalankan type checker secara terpisah bila toolchain tidak mengintegrasikannya. Target sintaks juga tidak menjamin semua Web API atau Node.js API tersedia pada runtime tujuan.

Polyfill, transpilation, dan target browser adalah persoalan terkait, tetapi berbeda. Transformasi dapat menurunkan sintaks, sedangkan polyfill menyediakan API runtime yang hilang.

## Tree shaking

Tree shaking mencoba mengecualikan export atau statement yang tidak diperlukan. ES modules membantu karena hubungan `import` dan `export` dapat dianalisis tanpa menjalankan program.

Optimasi ini bergantung pada side effect, bentuk modul, CommonJS, akses dinamis, plugin, dan konfigurasi. Klaim bahwa bundler otomatis menghapus seluruh unused code terlalu kuat.

Webpack memakai unused export detection dan petunjuk `sideEffects`. Rollup memakai live code inclusion, sedangkan [[References/Parcel\|Parcel]] juga mendokumentasikan bailout yang membatasi optimasi.

Konfigurasi side effect yang terlalu agresif dapat menghapus inisialisasi yang dibutuhkan. Verifikasi ukuran output harus disertai pengujian perilaku aplikasi.

## Code splitting

Code splitting membagi output agar kode dapat dimuat berdasarkan entry, route, fitur, atau dynamic `import()`. Tujuannya mengurangi pekerjaan awal dan menunda kode yang belum diperlukan.

Dynamic `import()` membentuk boundary asinkron, tetapi tidak menjamin satu chunk baru. Bundler dapat menggabungkan modul kecil, membuat shared chunk, atau menduplikasi kode berdasarkan strateginya.

Lebih banyak chunk bukan selalu lebih cepat. Request tambahan, urutan pemuatan, kompresi, cache, overhead runtime, dan pola navigasi dapat mengubah hasil.

Strategi chunk perlu diuji dengan cold load, repeat visit, route transition, jaringan lambat, dan cache nyata. Ukuran initial JavaScript hanyalah satu metrik.

## Aset dan output

Bundler dapat mengikuti referensi CSS, gambar, font, worker, dan WebAssembly. Artifact dapat diberi content hash agar perubahan isi menghasilkan URL baru untuk cache browser dan CDN.

Nama stabil masih diperlukan untuk artifact tertentu, seperti entry khusus atau service worker. Manifest dapat memetakan nama logis ke nama output yang memakai hash.

Source map menghubungkan output ke source asli untuk debugging. Ia perlu diuji pada minification, code splitting, error reporting, dan kebijakan publikasi karena dapat mengekspos source.

Untuk pustaka, output dapat mempertahankan external dependencies dan menyediakan format ESM atau CommonJS. Untuk aplikasi, bundler biasanya memasukkan lebih banyak dependensi ke artifact deployment.

## Development dan production

Development mengutamakan startup, incremental rebuild, diagnostic, dan HMR. Production mengutamakan kompatibilitas target, minifikasi, chunking, hashing, dan artifact yang dapat direproduksi.

[[References/Vite\|Vite]] menunjukkan bahwa development server tidak harus membundel seluruh source. Ia menyajikan modul sesuai permintaan melalui native ESM, tetapi tetap melakukan production build.

Webpack dan Parcel menyediakan development server serta rebuild berbasis graph. esbuild menawarkan watch dan serve, tetapi alat tingkat lebih tinggi dapat menambahkan HMR dan integrasi framework.

Cache build menyimpan hasil analisis atau transformasi agar rebuild lebih cepat. Ini berbeda dari browser cache yang menggunakan URL, header HTTP, dan content hash setelah deployment.

## Alat utama

Webpack adalah bundler yang sangat dapat dikonfigurasi. Model entry, output, loaders, plugins, mode, splitting, dan cache cocok untuk aplikasi yang memerlukan integrasi khusus atau ekosistem plugin luas.

Rollup berpusat pada ES modules dan kontrol output. Ia banyak dipakai untuk pustaka, external dependencies, beberapa format distribusi, tree shaking, dan code splitting.

Parcel menggabungkan resolution, transformasi aset, development server, cache, splitting, dan optimasi produksi dengan konfigurasi awal minimal.

esbuild menekankan pipeline cepat, API ringkas, dan dukungan bawaan untuk JavaScript, TypeScript, JSX, CSS, tree shaking, minification, serta source map.

Vite adalah build tool tingkat lebih tinggi. Ia memisahkan native ESM development dari production bundling dan menyediakan HMR serta integrasi framework.

## Memilih bundler

Mulai dari kebutuhan output, bukan popularitas. Nilai format modul, target runtime, aplikasi atau pustaka, SSR, worker, aset, framework, plugin, dan deployment.

Ukur cold build, incremental rebuild, startup server, HMR latency, peak memory, ukuran compressed output, jumlah request, cache hit, dan source map accuracy.

Audit juga stabilitas chunk, kualitas diagnostic, kemampuan debugging plugin, upgrade path, lockfile, serta dukungan conditional exports dan monorepo.

Benchmark vendor berguna untuk memahami arah desain, tetapi tidak menentukan pemenang universal. Jalankan benchmark pada graph, plugin, hardware, dan konfigurasi proyek sendiri.

## Workflow produksi

Kunci versi toolchain dan plugin. Jalankan build bersih di CI, simpan manifest serta metadata versi, lalu verifikasi artifact sebelum deployment.

Pisahkan type checking, linting, testing, dan bundling bila tanggung jawabnya berbeda. Build yang sukses bukan bukti bahwa tipe valid atau perilaku aplikasi benar.

Pantau budget JavaScript dan CSS per route. Gunakan bundle analyzer untuk melihat duplikasi, dependency besar, side effect, dan perubahan chunk sebelum serta sesudah optimasi.

Uji output pada browser dan runtime target, bukan hanya development server. Development dan production dapat memakai jalur transformasi, minification, atau resolution yang berbeda.

## Batas

Native ESM dan import maps memungkinkan aplikasi sederhana berjalan tanpa bundler. Bundling menjadi pilihan arsitektur, bukan persyaratan mutlak browser modern.

Sebaliknya, bundler tidak otomatis mempercepat aplikasi. Chunk buruk, dependency berat, JavaScript berlebih, dan cache salah dapat tetap menghasilkan performa rendah.

Detail default berubah menurut versi. Periksa ulang target, semantics plugin, algoritme chunk, klasifikasi side effect, dan format output saat toolchain ditingkatkan.

## Lihat juga

- [[References/Vite\|Vite]]
- [[References/esbuild\|esbuild]]
- [[References/Rollup\|Rollup]]
- [[References/Parcel\|Parcel]]
- [[References/SWC\|SWC]]
- [[References/JavaScript\|JavaScript]]
- [[References/Package Managers\|Package Managers]]
- [[References/Deployment\|Deployment]]
- [[References/Cache-Control\|Cache-Control]]

## Sumber

- [Concepts: webpack](https://webpack.js.org/concepts/): entry, output, loaders, plugins, mode, dan dependency graph.
- [Dependency Graph: webpack](https://webpack.js.org/concepts/dependency-graph/): pembentukan graph dari hubungan antarmodul.
- [Code Splitting: webpack](https://webpack.js.org/guides/code-splitting/): entry, deduplication, dan dynamic imports.
- [Tree Shaking: webpack](https://webpack.js.org/guides/tree-shaking/): unused exports dan metadata side effects.
- [Cache: webpack](https://webpack.js.org/configuration/cache/): memory dan filesystem cache.
- [Introduction: Rollup](https://rollupjs.org/introduction/): ES modules, bundling, dan tree shaking.
- [Configuration Options: Rollup](https://rollupjs.org/configuration-options/): input, external, output, chunk, dan source map.
- [Dependency Resolution: Parcel](https://parceljs.org/features/dependency-resolution/): specifier, package fields, alias, dan conditions.
- [Code Splitting: Parcel](https://parceljs.org/features/code-splitting/): dynamic import, shared bundle, dan batas request.
- [Production: Parcel](https://parceljs.org/features/production/): minification, tree shaking, hashing, serta cache browser.
- [API: esbuild](https://esbuild.github.io/api/): build, transform, loaders, splitting, target, dan source map.
- [Plugins: esbuild](https://esbuild.github.io/plugins/): hook resolution dan loading.
- [Why Vite](https://vite.dev/guide/why.html): native ESM development, HMR, dan evolusi bundler.
- [Building for Production: Vite](https://vite.dev/guide/build.html): production output, library mode, dan external dependency.
- [JavaScript Modules: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules): ESM, import maps, dan dynamic module loading.
