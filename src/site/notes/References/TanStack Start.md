---
{"dg-publish":true,"dg-path":"TanStack Start.md","permalink":"/tan-stack-start/","title":"TanStack Start","hideInFiletree":true,"tags":["references","programming","javascript","typescript","react","frameworks","ssr","deployment","performance"],"noteIcon":"","dg-note-properties":{"title":"TanStack Start","category":"references","tags":["references","programming","javascript","typescript","react","frameworks","ssr","deployment","performance"],"sources":["_raw/articles/tanstack-start-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

TanStack Start adalah framework full-stack [[References/React\|React]] yang dibangun di atas TanStack Router. Framework ini menggabungkan routing bertipe aman, server-side rendering, streaming, server functions, endpoint HTTP, middleware, dan build terpisah untuk browser serta server. Perkakas build-nya mendukung [[References/Vite\|Vite]] dan Rsbuild, sehingga aplikasi tidak terikat pada satu penyedia hosting.

Pada Agustus 2026, TanStack Start masih berstatus Release Candidate untuk v1. Dokumentasi menyebut fiturnya lengkap dan API-nya dianggap stabil, tetapi tetap memperingatkan bahwa bug dan masalah implementasi masih mungkin ditemukan. Status ini perlu dipertimbangkan untuk aplikasi yang menuntut stabilitas jangka panjang atau ekosistem plugin yang matang.

## Router sebagai fondasi

Routing TanStack Start sepenuhnya memakai TanStack Router. Berkas route berada di `src/routes`, sedangkan `src/router.tsx` mengatur router, preloading, cache staleness, dan perilaku global. Generator membuat `routeTree.gen.ts` agar struktur route dan tipe [[TypeScript\|TypeScript]] dapat diinferensikan tanpa menulis tipe URL secara manual.

Konvensi file mendukung index route, segmen dinamis seperti `$postId`, wildcard, nested route, pathless layout, dan route group. Root route di `src/routes/__root.tsx` menyediakan document shell yang berisi elemen `html`, `head`, dan `body`. Komponen `Outlet` menampilkan child route, `HeadContent` mengisi metadata dokumen, sedangkan `Scripts` memuat JavaScript sisi klien.

Model router-first membuat data loading menjadi bagian dari route. `beforeLoad` dapat menyiapkan context atau guard, sedangkan `loader` mengambil data sebelum komponen dirender. Integrasi dengan TanStack Query tersedia untuk kebutuhan cache, prefetching, dan hidrasi yang lebih kompleks, tetapi TanStack Query bukan syarat untuk memakai Start.

## SSR, streaming, dan mode rendering

SSR aktif secara default. Pada permintaan awal, route yang cocok menjalankan `beforeLoad` dan `loader` di server, merender HTML, lalu browser menghidrasi hasil tersebut menjadi aplikasi interaktif. Streaming memungkinkan bagian dokumen dikirim secara progresif ketika data atau komponen belum semuanya siap.

Selective SSR memberi kontrol per route melalui opsi `ssr`. Nilai `true` menjalankan data loading dan rendering komponen di server. Nilai `false` memindahkan keduanya ke browser. Mode `'data-only'` menjalankan `beforeLoad` dan `loader` di server, tetapi merender komponen hanya di klien. Konfigurasi ini berguna untuk route yang bergantung pada `localStorage`, `canvas`, atau API browser lainnya.

TanStack Start juga mendukung static prerendering. Route statis dapat ditemukan otomatis, sementara route dinamis dapat disediakan secara eksplisit atau ditemukan melalui perayapan tautan. Pendekatan ISR-nya memakai header cache HTTP dan perilaku CDN seperti `stale-while-revalidate`, bukan cache regenerasi yang terikat pada satu platform.

SPA mode tersedia ketika aplikasi tidak memerlukan HTML penuh dari server. Build menghasilkan document shell statis, lalu route dirender di browser. Mode ini menyederhanakan hosting dan tetap dapat dipadukan dengan server functions atau server routes, tetapi konten awal bergantung pada JavaScript dan umumnya kurang ideal untuk crawler dibanding SSR atau prerendering.

## Batas eksekusi server dan browser

Kode TanStack Start bersifat isomorphic secara default. Artinya, modul biasa dan route loader dapat masuk ke bundle server maupun browser. Loader berjalan di server saat SSR, lalu dapat berjalan lagi di browser pada navigasi berikutnya. Rahasia, akses database, atau kode filesystem tidak boleh ditempatkan langsung di loader tanpa batas eksekusi yang benar.

`createServerFn` mendefinisikan logika server yang dapat dipanggil dari loader, komponen, hook, atau event handler. Saat dipanggil dari browser, Start mengubah pemanggilan tersebut menjadi RPC dan menjaga tipe input serta output. Validasi runtime tetap diperlukan karena data melintasi jaringan. Server function ditujukan untuk pemakaian internal aplikasi dan dibatasi ke same-origin; endpoint publik atau lintas origin lebih tepat dibuat sebagai server route.

Untuk utilitas yang tidak boleh masuk ke lingkungan lain, Start menyediakan `createServerOnlyFn`, `createClientOnlyFn`, dan `createIsomorphicFn`. Pemisahan ini penting karena type safety tidak otomatis mencegah kebocoran rahasia. Batas server harus ditegakkan melalui API eksekusi dan struktur impor, bukan hanya melalui konvensi nama.

## Server routes dan middleware

Server routes dibuat di direktori route yang sama dengan halaman. Properti `server.handlers` pada `createFileRoute` memetakan metode HTTP ke handler berbasis Web `Request` dan `Response`. Satu file bahkan dapat menyediakan UI dan handler server untuk path yang sama. Fitur ini sesuai untuk API publik, webhook, autentikasi, form submission, atau integrasi yang membutuhkan kontrol HTTP langsung.

Middleware dapat membungkus permintaan SSR, server routes, dan server functions. Request middleware menangani seluruh permintaan server, sedangkan server function middleware menambahkan validator serta logika sebelum dan sesudah RPC di sisi klien maupun server. Penggunaan umumnya mencakup autentikasi, otorisasi, logging, observability, [[References/Content Security Policy\|Content Security Policy]], context, dan penanganan error.

Guard pada route bukan batas keamanan data. Server function atau server route yang membaca data privat tetap harus memverifikasi sesi dan otorisasi pada endpoint-nya. Data context yang dikirim browser juga harus dianggap tidak tepercaya, divalidasi bentuknya, lalu diperiksa hak aksesnya sebelum dipakai sebagai kunci query.

## CSS, metadata, dan SEO

TanStack Start mendukung pola CSS yang disediakan bundler. Global CSS dan CSS Modules dapat dikaitkan dengan route, ditemukan melalui manifest build, dan disertakan pada HTML SSR sebelum hidrasi. Import `?url` memberi kontrol eksplisit melalui `head()`. CSS inlining tersedia sebagai fitur eksperimental untuk mengurangi request awal, dengan trade-off berupa HTML yang lebih besar, cache CSS yang tidak terpisah, dan kebutuhan penyesuaian CSP.

Route dapat menghasilkan title, meta description, canonical URL, Open Graph tag, dan structured data melalui properti `head`. SSR dan prerendering membantu crawler menerima HTML yang siap dibaca, tetapi framework tidak menjamin peringkat pencarian. Kualitas konten, struktur internal, performa, aksesibilitas, sitemap, dan kebijakan indeksasi tetap harus dirancang oleh aplikasi.

## Deployment

Build Vite atau Rsbuild dapat diarahkan ke Cloudflare Workers, Netlify, Railway, Vercel, Node.js, Docker, Bun, dan target lain melalui integrasi yang sesuai. Nitro menyediakan lapisan deployment untuk banyak runtime, tetapi dokumentasi menyebut plugin `nitro/vite` masih aktif dikembangkan. Dukungan nyata dapat berbeda menurut runtime, plugin, dan fitur platform.

Kebebasan deployment tidak berarti tanpa konfigurasi. Streaming, cache CDN, runtime API, environment variables, asset handling, dan server entry harus diuji pada target produksi. Untuk SPA murni, host juga perlu mengarahkan URL yang tidak cocok ke document shell tanpa mengganggu static assets atau endpoint server.

## Kapan TanStack Start tepat digunakan

TanStack Start sesuai untuk tim React dan TypeScript yang menginginkan routing sebagai pusat arsitektur, tipe yang kuat untuk path dan search params, SSR selektif, server functions, middleware komposabel, serta pilihan deployment yang luas. Framework ini menarik untuk aplikasi kompleks yang membutuhkan kontrol eksplisit terhadap data loading dan batas rendering.

Biayanya adalah model eksekusi isomorphic yang harus dipahami, tanggung jawab lebih besar atas cache dan deployment, serta status framework yang masih Release Candidate. Aplikasi SPA tanpa kebutuhan server dapat memakai TanStack Router atau [[References/Vite\|Vite]] secara langsung. Tim yang lebih memilih konvensi terpadu dan ekosistem produksi yang lebih matang dapat membandingkannya dengan [[References/Next.js\|Next.js]].

React Server Components tersedia sebagai fitur eksperimental, bukan fondasi wajib TanStack Start. Aplikasi produksi sebaiknya tidak menganggap API eksperimental setara dengan fitur v1 yang stabil.

## Lihat juga

- [[References/React\|React]]
- [[TypeScript\|TypeScript]]
- [[References/Vite\|Vite]]
- [[References/Next.js\|Next.js]]
- [[References/JavaScript\|JavaScript]]
- [[References/Web Hosting\|Web Hosting]]
- [[References/Content Security Policy\|Content Security Policy]]

## Sumber

- [TanStack Start Overview](https://tanstack.com/start/latest/docs/framework/react/overview.md): status Release Candidate, fondasi TanStack Router, build tool, SSR, server functions, dan deployment.
- [Routing](https://tanstack.com/start/latest/docs/framework/react/guide/routing.md): file-based routing, route tree, root route, nested route, metadata, dan code splitting.
- [Execution Model](https://tanstack.com/start/latest/docs/framework/react/guide/execution-model.md): kode isomorphic, loader, batas server dan browser, serta API khusus lingkungan.
- [Server Functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions.md): RPC internal, validasi, same-origin, CSRF, dan organisasi kode server.
- [Server Routes](https://tanstack.com/start/latest/docs/framework/react/guide/server-routes.md): endpoint HTTP, file-route conventions, handler, dan penggunaan API eksternal.
- [Middleware](https://tanstack.com/start/latest/docs/framework/react/guide/middleware.md): request middleware, server function middleware, context, autentikasi, dan otorisasi.
- [Selective Server-Side Rendering](https://tanstack.com/start/latest/docs/framework/react/guide/selective-ssr.md): konfigurasi SSR per route dengan mode penuh, data-only, atau client-only.
- [SPA mode](https://tanstack.com/start/latest/docs/framework/react/guide/spa-mode.md): document shell, deployment statis, server functions, dan trade-off tanpa SSR.
- [Static Prerendering](https://tanstack.com/start/latest/docs/framework/react/guide/static-prerendering): pembuatan HTML statis, penemuan route, dan perayapan tautan.
- [Incremental Static Regeneration](https://tanstack.com/start/latest/docs/framework/react/guide/isr): cache HTTP, CDN, revalidasi, dan `stale-while-revalidate`.
- [CSS Styling](https://tanstack.com/start/latest/docs/framework/react/guide/css-styling.md): pola import CSS, asset route, SSR, Early Hints, dan CSS inlining eksperimental.
- [SEO](https://tanstack.com/start/latest/docs/framework/react/guide/seo): head management, structured data, sitemap, robots.txt, SSR, dan prerendering.
- [Hosting](https://tanstack.com/start/latest/docs/framework/react/guide/hosting): target deployment, integrasi platform, Node.js, Docker, Bun, dan status Nitro.
- [TanStack Start v1 Release Candidate](https://tanstack.com/blog/announcing-tanstack-start-v1): pengumuman RC, cakupan v1, dan posisi React Server Components.
