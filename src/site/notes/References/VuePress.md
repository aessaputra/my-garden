---
{"dg-publish":true,"dg-path":"VuePress.md","permalink":"/vue-press/","title":"VuePress","hideInFiletree":true,"tags":["references","programming","javascript","vue","frameworks","deployment","performance"],"dg-note-properties":{"title":"VuePress","category":"references","tags":["references","programming","javascript","vue","frameworks","deployment","performance"],"sources":["_raw/articles/vuepress-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

VuePress adalah static site generator yang berpusat pada Markdown dan menggunakan [[References/Vue.js\|Vue.js]] untuk membangun dokumentasi, blog, serta situs konten statis. Setiap berkas Markdown menjadi halaman. Pada proses build, VuePress memprerender setiap rute menjadi HTML statis; setelah dimuat di browser, situs berjalan sebagai single-page application berbasis Vue dan Vue Router.

Model ini membuat VuePress cocok ketika konten terutama ditulis sebagai dokumen, tetapi sesekali membutuhkan komponen interaktif. Ia bukan framework full-stack dan tidak menyediakan backend request-time, database, atau autentikasi server. Kebutuhan tersebut memerlukan layanan terpisah atau framework seperti [[References/Nuxt\|Nuxt]].

## Status versi

VuePress memiliki dua garis versi yang perlu dibedakan. VuePress 1 mencapai versi stabil 1.9.10, sedangkan pengembangan modern berlangsung pada VuePress 2. Pada 31 Agustus 2026, rilis terbaru yang ditemukan adalah `2.0.0-rc.31` dan proyek masih berstatus Release Candidate.

Dokumentasi menyatakan VuePress 2 sudah dapat dipakai untuk membangun situs, tetapi config dan API belum sepenuhnya stabil. Upgrade antarversi RC dapat membawa breaking change kecil maupun perubahan konfigurasi. Untuk proyek produksi, pin versi dependensi, simpan lockfile, uji build di CI, dan baca changelog sebelum memperbarui.

## Cara kerja

Alur VuePress terdiri dari dua mode:

1. Dalam mode pengembangan, VuePress menjalankan development server dengan hot reload dan menyajikan situs sebagai SPA.
2. Dalam mode build, VuePress membuat versi server-rendered lalu mengunjungi setiap rute secara virtual untuk menghasilkan berkas HTML statis.

Rute berasal dari lokasi relatif berkas Markdown. `README.md` atau `index.md` pada akar menjadi `/`, sedangkan `guide/getting-started.md` secara bawaan menjadi `/guide/getting-started.html`. Frontmatter pada awal berkas dapat mengatur metadata halaman seperti `title`, `description`, `lang`, dan layout.

```text
docs/
├── README.md
├── guide/
│   ├── README.md
│   └── getting-started.md
└── .vuepress/
    └── config.ts
```

Direktori `.vuepress` menampung konfigurasi dan kode khusus situs. Hasil build default berada di `.vuepress/dist`, sedangkan direktori sementara dan cache dibuat terpisah di bawah `.vuepress`.

## Markdown dan komponen Vue

VuePress memproses Markdown dengan `markdown-it`, mengubahnya menjadi HTML, lalu memperlakukannya sebagai template Vue Single-File Component. Karena itu, halaman dapat memakai sintaks template dan komponen Vue langsung di dalam Markdown.

Dukungan Markdown mencakup tabel, strikethrough, heading anchors, table of contents, transformasi tautan internal, impor potongan kode, serta ekstensi melalui plugin `markdown-it`. Syntax highlighting untuk code block disediakan melalui plugin seperti PrismJS atau Shiki, bukan kemampuan inti yang selalu aktif.

Komponen Vue memberi ruang untuk demo interaktif, diagram, playground, atau widget dokumentasi. Namun, setiap interaktivitas menambah JavaScript dan potensi biaya hidrasi. Kode komponen juga harus aman ketika dirender saat build: akses langsung ke `window`, `document`, atau API browser perlu dibatasi agar tidak dijalankan dalam lingkungan Node.

## Bundler

VuePress 2 memisahkan core dari bundler. Proyek memilih dan memasang salah satu dari dua bundler resmi:

- `@vuepress/bundler-vite` untuk alur berbasis [[References/Vite\|Vite]].
- `@vuepress/bundler-webpack` untuk alur berbasis Webpack.

Bundler dipilih secara eksplisit dalam konfigurasi. Vite umum dipakai untuk proyek baru, sedangkan dukungan Webpack berguna bagi proyek yang bergantung pada konfigurasi atau plugin Webpack. Pilihan bundler tidak mengubah model konten utama: Markdown tetap menjadi halaman Vue yang diprerender saat build.

## Theme dan layout

Theme bertanggung jawab atas layout, style, navbar, sidebar, navigasi antardokumen, tampilan halaman, serta fitur presentasi lain. VuePress menyediakan default theme untuk situs dokumentasi. Proyek juga dapat memakai community theme atau membuat local theme.

Karena pemisahan tersebut, klaim bahwa VuePress selalu menghasilkan layout responsif perlu dikualifikasi. Responsivitas mengikuti theme dan kustomisasi CSS yang digunakan, bukan jaminan generator inti. Default theme menyediakan fondasi dokumentasi, mode warna, navbar, sidebar, metadata pembaruan, serta titik ekstensi, tetapi hasil akhir tetap perlu diuji untuk aksesibilitas dan berbagai ukuran layar.

## Plugin dan pencarian

Plugin memperluas proses build maupun perilaku klien. Plugin dapat menambah analitik, search, sitemap, progressive web app, container Markdown, atau integrasi lain. VuePress menyediakan plugin resmi, sementara ekosistem juga memuat plugin komunitas dan plugin lokal.

Pencarian bukan fitur inti yang otomatis aktif pada semua proyek. Plugin `@vuepress/plugin-search` membuat indeks lokal dari judul dan heading halaman, lalu memuat indeks tersebut di browser. Pendekatan ini ringan dan tidak memerlukan request ke layanan eksternal, tetapi ukuran indeks dapat memperlambat pemuatan pada situs besar. Untuk dokumentasi besar, Algolia DocSearch atau mesin pencarian khusus lebih sesuai.

Theme yang kompatibel menampilkan komponen search yang didaftarkan plugin. Karena itu, keberadaan kotak pencarian bergantung pada kombinasi plugin dan theme, bukan hanya instalasi core VuePress.

## SEO dan performa

Prerendering memberi setiap rute HTML yang dapat dibaca tanpa menunggu aplikasi Vue membangun seluruh halaman di browser. Ini dapat membantu crawlability dan tampilan konten awal. Hasilnya tetap bukan jaminan SEO atau performa otomatis.

Kualitas produksi dipengaruhi oleh metadata, struktur heading, canonical URL, internal linking, ukuran aset, komponen interaktif, JavaScript pihak ketiga, theme, cache, dan hosting. Plugin SEO atau sitemap dapat membantu, tetapi konfigurasi dan konten tetap perlu diperiksa. Ukur hasil build dengan [[References/Lighthouse\|Lighthouse]] dan data lapangan, bukan dengan asumsi bahwa semua static site pasti cepat.

## Build dan deployment

Perintah `vuepress build docs` menghasilkan aset statis di `docs/.vuepress/dist`. Folder tersebut dapat diterbitkan ke [[References/GitHub Pages\|GitHub Pages]], GitLab Pages, Netlify, Vercel, Firebase Hosting, atau static hosting lain.

Jika situs ditempatkan pada subpath repository di GitHub Pages, opsi `base` harus mengikuti nama repository, misalnya `/repository/`. Nilai yang salah dapat membuat tautan dan aset gagal dimuat. Pipeline deployment sebaiknya memasang dependensi dari lockfile, menjalankan build, memeriksa broken link, lalu menerbitkan hanya folder output.

Karena hasilnya statis, pembaruan konten memerlukan build dan deployment baru. Konten yang harus berubah pada setiap request atau berdasarkan sesi pengguna tidak dapat diselesaikan oleh SSG saja.

## VuePress dan VitePress

VuePress dan VitePress memiliki asal yang berdekatan, tetapi sekarang merupakan proyek berbeda. VitePress dikelola tim Vue dan terintegrasi erat dengan Vite. VuePress 2 dikelola tim VuePress, mendukung Vite maupun Webpack, dan menekankan ekosistem theme serta plugin yang luas.

Pilih berdasarkan kebutuhan, bukan hanya kemiripan nama. VuePress sesuai bila proyek membutuhkan fleksibilitas bundler, theme, dan plugin. VitePress dapat lebih langsung untuk dokumentasi Vue yang menginginkan integrasi Vite dan pendekatan lebih terfokus. Migrasi antarkeduanya tetap memerlukan penyesuaian konfigurasi, theme, plugin, dan kemungkinan sintaks Markdown khusus.

## Kapan VuePress tepat digunakan

VuePress cocok untuk dokumentasi pustaka, manual internal yang diterbitkan sebagai situs, knowledge base publik, blog teknis, landing page berbasis konten, dan situs sederhana yang sebagian besar dapat dibangun lebih awal. Nilai utamanya terletak pada kombinasi Markdown, prerendering, komponen Vue, theme, dan plugin.

Pertimbangkan alat lain ketika aplikasi membutuhkan backend terpadu, personalisasi request-time, data yang berubah tanpa rebuild, atau kontrol aplikasi full-stack. Status RC VuePress 2 juga menjadi faktor operasional: proyek yang sangat menghindari perubahan API perlu mengevaluasi toleransi upgrade dan alternatif yang lebih stabil.

## Lihat juga

- [[References/Vue.js\|Vue.js]]
- [[References/Vite\|Vite]]
- [[References/JavaScript\|JavaScript]]
- [[References/Nuxt\|Nuxt]]
- [[References/GitHub Pages\|GitHub Pages]]
- [[References/Lighthouse\|Lighthouse]]

## Sumber

- [Introduction](https://vuepress.github.io/guide/introduction.html): definisi, model SPA dan SSG, prerendering, serta hubungan VuePress dengan VitePress.
- [Getting Started](https://vuepress.github.io/guide/getting-started.html): status RC, prasyarat, struktur proyek, development server, dan build output.
- [Page](https://vuepress.github.io/guide/page.html): pemetaan Markdown ke halaman dan rute serta penggunaan frontmatter.
- [Markdown](https://vuepress.github.io/guide/markdown.html): markdown-it, ekstensi sintaks, komponen Vue, dan syntax highlighting melalui plugin.
- [Bundler](https://vuepress.github.io/guide/bundler.html): dukungan Vite dan Webpack serta konfigurasi bundler.
- [Theme](https://vuepress.github.io/guide/theme.html): default theme, community theme, dan local theme.
- [Plugin](https://vuepress.github.io/guide/plugin.html): plugin resmi, komunitas, dan lokal.
- [search](https://ecosystem.vuejs.press/plugins/search/search.html): pencarian lokal, pembuatan indeks, dan batas skala.
- [Deployment](https://vuepress.github.io/guide/deployment.html): output build, base path, dan target static hosting.
- [VuePress Core Releases](https://github.com/vuepress/core/releases): rilis `2.0.0-rc.31` dan perubahan terbaru.
