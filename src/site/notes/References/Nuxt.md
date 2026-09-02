---
{"dg-publish":true,"dg-path":"Nuxt.md","permalink":"/nuxt/","title":"Nuxt","hideInFiletree":true,"tags":["references","programming","javascript","typescript","vue","frameworks","architecture","ssr","deployment","performance"],"dg-note-properties":{"title":"Nuxt","category":"references","tags":["references","programming","javascript","typescript","vue","frameworks","architecture","ssr","deployment","performance"],"sources":["_raw/articles/nuxt-expanded.md"],"created":"2026-08-31","updated":"2026-08-31","confidence":"high"}}
---

Nuxt adalah framework full-stack open source berbasis [[References/Vue.js\|Vue.js]] untuk membangun situs dan aplikasi web. Vue menyediakan model komponen antarmuka, sedangkan Nuxt menambahkan struktur proyek berbasis konvensi, routing, pengambilan data, server API, beberapa strategi rendering, optimasi build, dan deployment melalui server engine Nitro.

Nuxt sering dikaitkan dengan SEO dan performa, tetapi keduanya bukan hasil otomatis dari pemilihan framework. HTML yang dirender sebelum mencapai browser dapat membantu crawler dan mempercepat tampilan konten awal. Hasil produksi tetap dipengaruhi oleh arsitektur aplikasi, query data, cache, aset, JavaScript pihak ketiga, hidrasi, dan infrastruktur deployment.

## Struktur aplikasi dan konvensi

Pada Nuxt 4, kode antarmuka utama berada di direktori `app/`. Direktori seperti `app/pages/`, `app/layouts/`, `app/components/`, `app/composables/`, `app/middleware/`, dan `app/plugins/` memiliki fungsi khusus yang dikenali framework. Direktori `server/` menampung API routes, server routes, middleware, plugins, dan utility sisi server.

Konvensi ini mengurangi konfigurasi manual, tetapi tidak menghilangkan kebutuhan desain aplikasi. Proyek tetap perlu menetapkan batas antara kode browser, server, dan kode bersama, serta mengelola autentikasi, otorisasi, cache, observabilitas, dan dependensi.

## Routing dan navigasi

Setiap komponen Vue dalam `app/pages/` menjadi rute berdasarkan nama dan susunan berkas. Nama dalam kurung siku membentuk parameter dinamis, sementara susunan direktori dapat membentuk rute bertingkat.

```text
app/pages/
├── index.vue
├── about.vue
└── blog/
    └── [slug].vue
```

Struktur tersebut menghasilkan rute `/`, `/about`, dan `/blog/:slug`. Routing Nuxt dibangun di atas Vue Router. Code splitting aktif secara bawaan melalui dynamic import per halaman sehingga browser tidak perlu menerima seluruh kode aplikasi saat membuka satu rute.

Komponen `NuxtLink` menangani navigasi internal tanpa pemuatan ulang halaman penuh setelah aplikasi dihidrasi. Ketika tautan internal memasuki viewport, Nuxt dapat melakukan prefetch terhadap komponen dan payload halaman tujuan agar navigasi berikutnya lebih cepat.

## Model rendering

Nuxt memakai universal rendering secara bawaan. Pada permintaan awal, kode Vue dijalankan di server untuk menghasilkan HTML. Browser kemudian memuat JavaScript dan melakukan hidrasi agar antarmuka menjadi interaktif. Pendekatan ini memberi konten awal yang dapat ditampilkan sebelum seluruh JavaScript selesai berjalan, tetapi juga menambah biaya server dan hidrasi.

Client-side rendering dapat diaktifkan secara global dengan `ssr: false`. Dalam mode ini, browser membangun antarmuka setelah JavaScript dimuat. Mode tersebut dapat sesuai untuk aplikasi internal tertentu, tetapi halaman publik mungkin mengalami tampilan awal yang lebih lambat dan pengindeksan yang lebih bergantung pada kemampuan crawler menjalankan JavaScript.

Nuxt juga mendukung prerendering untuk menghasilkan HTML statis saat build. Perintah `nuxt generate` sesuai untuk situs yang dapat disajikan tanpa runtime server, tetapi server endpoints tidak tersedia dalam keluaran statis.

Hybrid rendering mengatur perilaku per rute melalui `routeRules`. Satu aplikasi dapat melakukan prerender pada halaman pemasaran, menggunakan cache stale-while-revalidate atau incremental static regeneration pada katalog, menjalankan universal rendering pada halaman dinamis, dan menonaktifkan SSR untuk area administrasi.

```ts
export default defineNuxtConfig({
  routeRules: {
    '/': {prerender: true},
    '/products/**': {swr: 3600},
    '/blog/**': {isr: 3600},
    '/admin/**': {ssr: false}
  }
});
```

Dukungan `isr` bergantung pada platform yang mendukung integrasi CDN terkait. Hybrid rendering juga tidak tersedia ketika seluruh aplikasi dibangun dengan `nuxt generate`, karena hasilnya tidak menyertakan server yang dapat menerapkan aturan runtime.

## Pengambilan data dan hidrasi

Nuxt menyediakan `$fetch`, `useFetch`, dan `useAsyncData`. `$fetch` merupakan utility permintaan jaringan umum. `useFetch` membungkus `$fetch` untuk pola pengambilan data yang terintegrasi dengan rendering Nuxt, sedangkan `useAsyncData` memberi kontrol lebih rinci terhadap operasi asinkron.

Dalam universal rendering, pemanggilan `$fetch` langsung pada setup komponen dapat menyebabkan permintaan dijalankan di server lalu diulang saat hidrasi di browser. `useFetch` dan `useAsyncData` menghindari duplikasi tersebut dengan meneruskan hasil server melalui payload Nuxt. Browser menggunakan payload itu ketika menghidrasi halaman.

```vue
<script setup lang="ts">
const {data, error, status} = await useFetch('/api/articles');
</script>
```

Payload mengurangi pengambilan ulang data, tetapi data yang diserialisasi harus aman untuk dikirim ke browser. Rahasia, token server, atau objek yang tidak seharusnya terlihat oleh pengguna tidak boleh dimasukkan ke payload halaman.

## Nitro dan kemampuan full-stack

Nitro adalah server engine Nuxt. Berkas dalam `server/api/` menghasilkan endpoint API, sedangkan `server/routes/` dapat menghasilkan route server non-API. Nitro menangani build server dan menyediakan target deployment untuk server Node atau Deno, serverless, edge runtime, serta prerendering statis.

Kemampuan lintas target tidak berarti semua runtime identik. Dukungan API Node, filesystem, streaming, cache persisten, batas ukuran fungsi, dan durasi eksekusi dapat berbeda. Aplikasi harus diuji pada lingkungan yang menyerupai target deployment sebenarnya.

## Ekstensibilitas dan performa

Nuxt dapat diperluas melalui modules, plugins, composables, dan layers. Modules mengintegrasikan fitur pada tingkat framework, plugins menginisialisasi fungsi ketika aplikasi Nuxt dibuat, composables membungkus logika Vue yang dapat digunakan kembali, dan layers membagikan konfigurasi serta struktur aplikasi antarproyek.

Fitur performa bawaan mencakup code splitting per rute, prefetch tautan, lazy loading komponen, lazy hydration, dan penerusan data server melalui payload. Modul resmi juga tersedia untuk optimasi gambar, font, dan skrip pihak ketiga.

Optimasi tersebut harus dipilih berdasarkan pengukuran. Prefetch yang terlalu agresif dapat membuang bandwidth, plugin mahal dapat menghambat hidrasi, dan dependensi yang tidak digunakan dapat memperbesar bundle. Build produksi dapat dianalisis dengan perintah Nuxt analyze, Nuxt DevTools, Chrome DevTools, Lighthouse, atau alat pemantauan lapangan.

## Kapan Nuxt tepat digunakan

Nuxt sesuai ketika proyek Vue membutuhkan routing terpadu, rendering server atau statis, endpoint backend, metadata halaman, strategi cache per rute, dan deployment lintas runtime. Contohnya mencakup situs konten, e-commerce, dashboard, SaaS, serta aplikasi dengan halaman publik dan area interaktif.

Untuk widget kecil atau aplikasi client-only sederhana, [[References/Vue.js\|Vue.js]] bersama [[References/Vite\|Vite]] dapat memberi struktur yang lebih ringan. Nuxt lebih tepat ketika manfaat konvensi full-stack, beberapa strategi rendering, dan Nitro sebanding dengan kompleksitas server, hidrasi, caching, serta deployment yang harus dikelola.

## Lihat juga

- [[References/Vue.js\|Vue.js]]
- [[References/Vite\|Vite]]
- [[References/JavaScript\|JavaScript]]
- [[References/Next.js\|Next.js]]
- [[References/SvelteKit\|SvelteKit]]
- [[References/Web Hosting\|Web Hosting]]

## Sumber

- [Nuxt 4 Introduction](https://nuxt.com/docs/4.x/getting-started/introduction): definisi framework, konvensi, SSR bawaan, Nitro, modules, dan target deployment.
- [Rendering Modes](https://nuxt.com/docs/4.x/guide/concepts/rendering): universal rendering, client-side rendering, prerendering, hybrid rendering, route rules, SWR, dan ISR.
- [Routing](https://nuxt.com/docs/4.x/getting-started/routing): file-based routing, dynamic import, code splitting, NuxtLink, dan prefetch.
- [Data Fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching): `$fetch`, `useFetch`, `useAsyncData`, payload, dan pencegahan permintaan ganda saat hidrasi.
- [Nuxt Performance](https://nuxt.com/docs/4.x/guide/best-practices/performance): optimasi bawaan, lazy loading, lazy hydration, modules performa, profiling, dan masalah umum.
- [Nuxt Directory Structure](https://nuxt.com/docs/4.x/directory-structure): struktur `app/`, `server/`, `shared/`, `modules/`, dan `layers/`.
