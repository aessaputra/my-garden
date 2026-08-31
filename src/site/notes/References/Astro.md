---
{"dg-publish":true,"dg-path":"Astro.md","permalink":"/astro/","title":"Astro","hideInFiletree":true,"tags":["references","programming","javascript","typescript","frameworks","ssr","deployment","performance"],"dg-note-properties":{"title":"Astro","category":"references","tags":["references","programming","javascript","typescript","frameworks","ssr","deployment","performance"],"sources":["_raw/articles/astro-expanded.md"],"created":"2026-08-29","updated":"2026-08-30","confidence":"high"}}
---

Astro adalah framework web untuk situs berbasis konten seperti blog, dokumentasi, situs pemasaran, portofolio, dan etalase e-commerce. Astro bermula sebagai static site generator, tetapi versi modern juga dapat merender route saat permintaan masuk. Perbedaan utamanya dari framework aplikasi yang berpusat pada browser adalah pendekatan server-first: komponen dirender menjadi [[References/HTML\|HTML]], lalu [[References/JavaScript\|JavaScript]] sisi klien hanya dikirim untuk bagian yang memang interaktif.

Pada Agustus 2026, rilis terkini yang ditemukan adalah Astro 7.2. Astro 7 memakai [[References/Vite\|Vite]] 8 dan Rolldown, compiler `.astro` berbasis Rust, pipeline Markdown dan MDX Sätteri, queued rendering, Advanced Routing, serta route caching. Incremental static builds hadir di 7.2 sebagai fitur eksperimental, jadi perilakunya masih dapat berubah.

## Islands architecture

Astro memakai islands architecture untuk memisahkan konten statis dari bagian interaktif atau personal. Sebagian besar halaman tetap berupa HTML biasa. Komponen yang membutuhkan JavaScript menjadi client island dan dihidrasi secara terpisah. Teknik ini juga disebut partial atau selective hydration.

Komponen Astro dan komponen framework UI dirender menjadi HTML tanpa runtime browser secara default. Direktif `client:*` mengaktifkan hidrasi untuk komponen tertentu. `client:load` memuat komponen segera, `client:idle` menunggu browser senggang, `client:visible` menunggu komponen masuk viewport, `client:media` mengikuti media query, sedangkan `client:only` melewati rendering server. Pilihan direktif menentukan kapan JavaScript komponen dikirim dan dijalankan.

Island tidak membuat semua jenis aplikasi otomatis ringan. Komponen interaktif tetap membawa kode komponen dan runtime framework yang diperlukan. Jika sebagian besar halaman membutuhkan state bersama dan hidrasi luas, keuntungan pendekatan islands mengecil. Arsitektur ini paling efektif ketika interaktivitas hanya menempati sebagian kecil halaman.

Astro juga memiliki server islands. Direktif `server:defer` memisahkan komponen dinamis atau personal dari render utama. Halaman dapat menampilkan fallback lebih dulu, lalu browser meminta HTML island dari endpoint khusus. Pola ini memungkinkan shell dan konten utama diprerender atau dicache lebih agresif tanpa menunggu data seperti avatar pengguna atau status keranjang. Server islands memerlukan adapter dan membawa persoalan operasional seperti serialisasi props, cache request, serta sinkronisasi encryption key pada rolling deployment.

## Komponen dan framework UI

Berkas `.astro` memakai sintaks yang dekat dengan HTML, dengan component script di antara pagar `---` untuk logika server. Astro tidak memiliki runtime reaktif di browser untuk komponen `.astro`; event interaktif dapat dibuat melalui elemen HTML, `<script>`, Web Components, atau komponen framework UI.

Integrasi resmi tersedia untuk [[References/React\|React]], Preact, [[References/Vue.js\|Vue.js]], [[References/Svelte\|Svelte]], SolidJS, dan Alpine.js. Beberapa framework dapat dipakai dalam satu halaman selama komponen-komponen itu dirangkai dari berkas `.astro`. Namun, setiap island tetap mengikuti aturan framework asalnya. Komponen React tidak dapat langsung mengimpor komponen `.astro`, dan props untuk komponen terhidrasi harus dapat diserialisasi.

Script biasa di komponen Astro mendapat pemrosesan TypeScript, bundling import, `type="module"`, deduplication, dan inlining untuk script kecil. Interaktivitas sederhana karena itu tidak selalu memerlukan React atau framework UI lain.

## Routing dan rendering

Astro memakai file-based routing dari direktori `src/pages`. Berkas `.astro`, `.md`, dan `.mdx` menjadi route sesuai nama dan lokasinya. Segmen dinamis memakai `[param]`, sedangkan `[...path]` menangkap beberapa tingkat URL. Dalam static output, route dinamis harus menyediakan `getStaticPaths()` agar Astro mengetahui halaman yang dibangun.

Prerendering statis adalah perilaku default. Hasil build dapat dilayani sebagai berkas dari CDN atau web server sederhana. Route tertentu dapat memilih on-demand rendering dengan `export const prerender = false` setelah proyek memasang adapter. Untuk aplikasi yang sebagian besar dinamis, konfigurasi `output: 'server'` membalik default sehingga route dirender saat diminta, sementara halaman tertentu masih dapat diprerender.

On-demand rendering mendukung cookie, respons personal, endpoint API, dan HTML streaming. Kemampuan runtime bergantung pada adapter untuk Node.js, Cloudflare, Netlify, Vercel, atau target lain. Perbedaan adapter perlu diperiksa karena API platform dan fitur deployment tidak selalu sama.

Astro 7 menambahkan Advanced Routing melalui `src/fetch.ts`. Entry point ini dapat menjalankan pipeline Astro lengkap atau menyusun handler untuk actions, middleware, pages, dan i18n. Integrasi dengan Hono juga tersedia. Fitur ini memperluas Astro melampaui static site generator sederhana, tetapi menambah area konfigurasi yang tidak diperlukan oleh situs statis biasa.

## Konten Markdown dan Content Collections

Astro mendukung GitHub Flavored Markdown, frontmatter YAML atau TOML, dan MDX melalui integrasi. Berkas Markdown di `src/pages` dapat langsung menjadi halaman. Untuk kumpulan konten terstruktur, Content Collections memberi loader, schema, validasi, autocomplete, dan tipe [[TypeScript\|TypeScript]].

Build-time collections cocok untuk konten yang relatif stabil dan dapat dioptimalkan saat build. Live content collections mengambil data saat permintaan masuk untuk CMS, API, database, stok, atau data pengguna yang sering berubah. Pilihan live menghindari rebuild, tetapi data diambil pada setiap request kecuali aplikasi menambahkan cache. Live collections juga tidak memiliki dukungan MDX dan image optimization yang sama dengan build-time collections.

Astro 7 memakai Sätteri sebagai processor Markdown dan MDX default. Pipeline Rust ini menggantikan Unified sebagai default, tetapi Unified tetap tersedia untuk proyek yang bergantung pada ekosistem plugin remark atau rehype.

## Aset dan navigasi

Komponen `Image` dan `Picture` dapat mengoptimalkan gambar lokal atau remote yang diizinkan, menghasilkan beberapa format atau ukuran, dan membuat gambar responsif. Aset di `src` dapat diproses saat build, sedangkan berkas di `public` disalin tanpa optimasi. Native `<img>` tetap dapat dipakai ketika pemrosesan Astro tidak dibutuhkan.

Navigasi default Astro adalah perpindahan dokumen standar melalui elemen `<a>`. View Transitions browser dapat menambahkan animasi tanpa mengubah model multi-page application. Komponen `ClientRouter` bersifat opsional untuk client-side routing, state persisten, dan fitur transisi yang lebih luas. Memakainya berarti script dan state tertentu perlu diinisialisasi ulang setelah navigasi.

## Deployment dan batas penggunaan

Situs statis Astro menghasilkan output ke `dist` dan dapat di-host di layanan static hosting. On-demand rendering, server islands, sessions, dan fitur server membutuhkan adapter yang sesuai. Dokumentasi menyediakan panduan untuk banyak penyedia, tetapi dukungan adapter resmi berfokus pada Node.js, Cloudflare, Netlify, dan Vercel.

Astro tepat untuk proyek dengan porsi konten besar dan interaktivitas lokal: blog, dokumentasi, landing page, situs pemasaran, katalog, media, serta storefront yang sebagian besar dapat dicache. Dukungan beberapa framework juga berguna saat tim ingin memakai komponen yang sudah ada tanpa mengubah seluruh situs menjadi SPA.

Astro kurang cocok ketika hampir seluruh layar merupakan aplikasi interaktif dengan state klien yang saling terhubung, navigasi client-only, dan komunikasi intensif antarkomponen. Proyek seperti itu dapat lebih sederhana dengan [[References/Vite\|Vite]], [[References/Next.js\|Next.js]], atau [[References/TanStack Start\|TanStack Start]], tergantung kebutuhan routing dan server. Klaim "zero JavaScript by default" menjelaskan baseline Astro, bukan jaminan bahwa aplikasi akhir bebas JavaScript atau selalu lebih cepat.

## Lihat juga

- [[References/HTML\|HTML]]
- [[References/JavaScript\|JavaScript]]
- [[TypeScript\|TypeScript]]
- [[References/React\|React]]
- [[References/Vue.js\|Vue.js]]
- [[References/Vite\|Vite]]
- [[References/Next.js\|Next.js]]
- [[References/TanStack Start\|TanStack Start]]
- [[References/Web Hosting\|Web Hosting]]

## Sumber

- [Why Astro?](https://docs.astro.build/en/concepts/why-astro/): fokus content-driven, server-first, zero JavaScript by default, dan batas penggunaan.
- [Islands architecture](https://docs.astro.build/en/concepts/islands/): client islands, partial hydration, server islands, dan isolasi komponen.
- [Front-end frameworks](https://docs.astro.build/en/guides/framework-components/): integrasi React, Vue, Svelte, Solid, hidrasi, mixing framework, dan serialisasi props.
- [Routing](https://docs.astro.build/en/guides/routing/): file-based routing, route statis dan dinamis, `getStaticPaths`, redirect, rewrite, dan Advanced Routing.
- [Markdown in Astro](https://docs.astro.build/en/guides/markdown-content/): Markdown, MDX, frontmatter, processor Sätteri dan Unified, serta individual Markdown pages.
- [Content collections](https://docs.astro.build/en/guides/content-collections/): loader, schema, build-time collections, live collections, serta batas runtime.
- [On-demand rendering](https://docs.astro.build/en/guides/on-demand-rendering/): prerendering default, SSR per route, server output, adapter, dan streaming.
- [Server islands](https://docs.astro.build/en/guides/server-islands/): `server:defer`, fallback, endpoint island, caching, props, dan encryption key.
- [Scripts and event handling](https://docs.astro.build/en/guides/client-side-scripts/): script processing, TypeScript, bundling, event listener, dan Web Components.
- [Images](https://docs.astro.build/en/guides/images/): `Image`, `Picture`, responsive images, aset lokal, remote, dan `public`.
- [View transitions](https://docs.astro.build/en/guides/view-transitions/): navigasi MPA, browser-native transitions, `ClientRouter`, state, dan fallback.
- [Deploy your Astro Site](https://docs.astro.build/en/guides/deploy/): build output, continuous deployment, CLI, dan penyedia hosting.
- [Astro 7.0](https://astro.build/blog/astro-7/): Vite 8, compiler Rust, Sätteri, queued rendering, Advanced Routing, dan route caching.
- [Astro 7.2](https://astro.build/blog/astro-720/): incremental static builds eksperimental dan perubahan operasional terbaru.
