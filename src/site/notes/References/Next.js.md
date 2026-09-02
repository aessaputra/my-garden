---
{"dg-publish":true,"dg-path":"Next.js.md","permalink":"/next-js/","title":"Next.js","hideInFiletree":true,"tags":["references","programming","javascript","typescript","react","frameworks","ssr","deployment","performance"],"dg-note-properties":{"title":"Next.js","category":"references","tags":["references","programming","javascript","typescript","react","frameworks","ssr","deployment","performance"],"sources":["_raw/articles/nextjs-expanded.md"],"created":"2026-08-29","updated":"2026-09-01","confidence":"high"}}
---

Next.js adalah framework [[References/React\|React]] untuk membangun aplikasi web full-stack. React menyediakan model komponen untuk antarmuka, sedangkan Next.js menambahkan routing, rendering di server, pengambilan dan perubahan data, endpoint HTTP, optimasi aset, serta pilihan deployment. Next.js dapat digunakan untuk situs statis, aplikasi yang dirender saat permintaan masuk, maupun aplikasi interaktif yang memadukan pekerjaan server dan browser.

## App Router dan Pages Router

Next.js memiliki dua sistem routing. App Router adalah arsitektur utama untuk aplikasi baru dan memakai React Server Components, Suspense, serta Server Functions. Pages Router merupakan sistem lama yang masih didukung. Istilah "API Routes" merujuk pada endpoint di Pages Router, sedangkan App Router memakai Route Handlers.

Pada App Router, struktur folder di direktori `app` menentukan URL. Berkas `page.tsx` membuat halaman, `layout.tsx` menyediakan UI bersama, dan folder bersarang membentuk segmen URL. Folder bernama `[slug]` membuat segmen dinamis, misalnya `app/blog/[slug]/page.tsx` untuk halaman artikel berdasarkan slug.

Layout tetap dipertahankan saat pengguna berpindah halaman. State dan elemen interaktif di layout tidak perlu dibuat ulang untuk setiap navigasi. Konvensi lain seperti `loading.tsx`, `error.tsx`, dan `not-found.tsx` menyediakan UI pemuatan serta penanganan kegagalan pada tingkat route.

## Rendering di server dan browser

Page dan layout pada App Router adalah Server Components secara default. Komponen ini dapat mengambil data dekat dengan database atau API, memakai rahasia yang tidak boleh dikirim ke browser, dan menghasilkan UI dengan lebih sedikit JavaScript sisi klien. Bagian yang memerlukan state, event handler, lifecycle, custom hook, atau API browser ditandai dengan direktif `'use client'`.

Pada pemuatan pertama, Next.js menghasilkan HTML agar konten dapat tampil sebelum seluruh interaktivitas aktif. React Server Component Payload kemudian menyelaraskan pohon komponen, sedangkan JavaScript melakukan hidrasi pada Client Components. Batas `'use client'` sebaiknya ditempatkan sedekat mungkin dengan bagian interaktif karena semua impor dalam graf modul tersebut masuk ke bundle klien.

Rendering server bukan satu mode tunggal. Prerendering terjadi saat build atau revalidasi dan hasilnya dapat disimpan dalam cache. Dynamic Rendering berjalan saat ada permintaan. Static export menghasilkan HTML, CSS, dan JavaScript yang dapat dilayani oleh hosting statis, tetapi fitur yang memerlukan server tidak tersedia dalam mode ini.

## Navigasi dan performa

Komponen `Link` menyediakan navigasi sisi klien dan prefetching. Route statis dapat dimuat di latar belakang ketika tautannya memasuki viewport. Route dinamis dapat diprefetch sebagian jika memiliki `loading.tsx`, sehingga layout bersama dan skeleton tersedia sebelum pengguna membuka halaman.

Streaming memungkinkan server mengirim bagian halaman yang sudah siap tanpa menunggu seluruh route selesai dirender. Next.js membungkus konten route yang memakai `loading.tsx` dalam batas Suspense, lalu mengganti fallback dengan konten sebenarnya ketika data tersedia. Kombinasi prefetching, streaming, dan transisi sisi klien membuat aplikasi yang dirender server tetap terasa responsif.

Next.js juga membagi JavaScript dan CSS berdasarkan route. Browser tidak harus mengunduh seluruh kode aplikasi untuk membuka satu halaman. Fast Refresh memperbarui komponen selama pengembangan dan berusaha mempertahankan state lokal ketika perubahan aman diterapkan.

## Data, cache, dan endpoint HTTP

Server Components dapat mengambil data langsung pada server. Server Functions dan Server Actions menangani perubahan data serta pengiriman formulir tanpa mengharuskan pengembang membuat endpoint terpisah untuk setiap operasi. Next.js juga menyediakan mekanisme cache dan revalidasi untuk mengendalikan kapan data atau UI digunakan ulang.

Route Handlers membuat endpoint HTTP melalui berkas `route.ts` di dalam direktori `app`. Handler memakai Web `Request` dan `Response` API serta mendukung metode `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, dan `OPTIONS`. Sebuah segmen route tidak dapat memiliki `page.tsx` dan `route.ts` pada tingkat yang sama karena keduanya akan menempati URL yang sama.

## CSS, gambar, font, dan metadata

Next.js mendukung Global CSS, CSS Modules, Tailwind CSS, Sass, stylesheet eksternal, dan beberapa pendekatan CSS-in-JS. Saat build produksi, CSS dipecah dan diminifikasi berdasarkan kebutuhan route. CSS Modules memberi cakupan lokal pada nama class agar tidak bertabrakan dengan komponen lain.

Komponen `next/image` dapat menyajikan gambar sesuai ukuran perangkat, memakai format modern, melakukan lazy loading, dan mengurangi layout shift. `next/font` menyimpan font sebagai aset statis yang dilayani dari domain aplikasi sehingga browser tidak perlu meminta font Google dari domain eksternal.

Metadata API menerima objek `metadata` statis atau fungsi `generateMetadata` untuk data dinamis. Next.js membentuk elemen `head` yang relevan dan menyediakan konvensi berkas untuk favicon, `robots.txt`, sitemap, serta Open Graph image. Fitur ini membantu mesin pencari dan layanan sosial memahami halaman, tetapi kualitas SEO tetap bergantung pada isi, struktur HTML, aksesibilitas, kecepatan, dan strategi indeksasi aplikasi.

## Deployment

Aplikasi Next.js dapat dijalankan sebagai server Node.js, container Docker, static export, atau melalui adapter platform. Server Node.js dan Docker mendukung seluruh fitur. Static export lebih sederhana untuk di-host, tetapi tidak mendukung fitur yang membutuhkan runtime server. Dukungan adapter berbeda menurut kemampuan platform dan tingkat kompatibilitas implementasinya.

Vercel memberi integrasi langsung karena mengembangkan Next.js, tetapi Next.js tidak terbatas pada Vercel. Aplikasi dapat di-self-host pada penyedia yang mendukung Node.js atau Docker. Pilihan deployment harus mengikuti fitur yang dipakai, terutama streaming, cache bersama, image optimization, revalidasi, dan fungsi server.

## Kapan Next.js tepat digunakan

Next.js sesuai untuk aplikasi React yang membutuhkan routing terpadu, rendering server atau statis, metadata per halaman, endpoint backend ringan, dan optimasi aset bawaan. Contohnya meliputi situs konten, e-commerce, dashboard, SaaS, dan aplikasi web dengan halaman publik yang perlu mudah ditemukan mesin pencari.

Framework ini membawa biaya berupa model cache dan rendering yang perlu dipahami, pemisahan eksplisit antara lingkungan server dan browser, serta ketergantungan deployment pada fitur runtime yang dipilih. Aplikasi client-only sederhana yang tidak memerlukan kemampuan tersebut dapat memakai perkakas yang lebih ringan seperti [[References/Vite\|Vite]]. Next.js lebih tepat ketika manfaat arsitektur full-stack dan rendering campuran sebanding dengan kompleksitasnya.

## Lihat juga

- [[References/React\|React]]
- [[References/Vercel\|Vercel]]
- [[References/Vite\|Vite]]
- [[References/JavaScript\|JavaScript]]
- [[TypeScript\|TypeScript]]
- [[References/CSS\|CSS]]
- [[References/Web Hosting\|Web Hosting]]

## Sumber

- [Next.js Documentation Index](https://nextjs.org/docs/llms.txt): indeks dokumentasi resmi Next.js 16.3.3 dan cakupan fitur App Router.
- [Layouts and Pages](https://nextjs.org/docs/app/getting-started/layouts-and-pages): file-system routing, page, layout, nested route, dynamic segment, dan `Link`.
- [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components): batas server dan browser, RSC Payload, hidrasi, serta penggunaan `'use client'`.
- [Linking and Navigating](https://nextjs.org/docs/app/getting-started/linking-and-navigating): prerendering, dynamic rendering, prefetching, streaming, dan transisi sisi klien.
- [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers): endpoint HTTP pada App Router, metode yang didukung, dan perilaku cache.
- [CSS](https://nextjs.org/docs/app/getting-started/css): Global CSS, CSS Modules, Tailwind CSS, Sass, CSS-in-JS, dan pemrosesan produksi.
- [Image Optimization](https://nextjs.org/docs/app/getting-started/images): optimasi ukuran, format, lazy loading, dan stabilitas layout pada `next/image`.
- [Metadata and OG images](https://nextjs.org/docs/app/getting-started/metadata-and-og-images): metadata statis dan dinamis serta konvensi aset untuk SEO dan berbagi sosial.
- [Static Exports](https://nextjs.org/docs/app/guides/static-exports): pembuatan HTML per route dan batas fitur tanpa server.
- [Deploying](https://nextjs.org/docs/app/getting-started/deploying): deployment melalui Node.js, Docker, static export, dan adapter.
- [Fast Refresh](https://nextjs.org/docs/architecture/fast-refresh): pembaruan komponen selama pengembangan dan pemeliharaan state lokal.
