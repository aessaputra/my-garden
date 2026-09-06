---
{"dg-publish":true,"dg-path":"Server-Side Rendering.md","permalink":"/server-side-rendering/","title":"Server-Side Rendering","hideInFiletree":true,"tags":["references","ssr","architecture","performance"],"noteIcon":"","dg-note-properties":{"title":"Server-Side Rendering","categories":["Rendering Strategies"],"tags":["references","ssr","architecture","performance"],"sources":["_raw/articles/server-side-rendering-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Server-side rendering atau SSR menghasilkan HTML pada server sebagai respons terhadap navigasi, lalu mengirimkannya ke browser. Definisi ini membedakannya dari client-side rendering, yang menunggu JavaScript membangun tampilan utama di perangkat pengguna.

[Web.dev](https://web.dev/articles/rendering-on-the-web) menjelaskan bahwa SSR dapat menghindari round trip tambahan untuk data dan templating sebelum respons tiba. Namun, rendering dinamis menambah kerja server dan dapat menaikkan Time to First Byte.

## Alur permintaan

1. Browser meminta URL.
2. Server mengambil data yang diperlukan dan merender HTML.
3. Browser menerima, mem-parsing, dan menampilkan HTML.
4. JavaScript klien dimuat bila halaman membutuhkan interaktivitas.
5. Framework melakukan hydration atau aktivasi setara pada bagian interaktif.

HTML awal dan interaktivitas bukan tahap yang sama. [Dokumentasi Next.js](https://nextjs.org/docs/app/getting-started/server-and-client-components) menyebut HTML sebagai pratinjau non-interaktif, lalu JavaScript menghidrasi Client Components dengan event handler.

Nuxt menyebut pola bawaan ini universal rendering. Kode Vue berjalan pada server untuk menghasilkan HTML, kemudian berjalan di browser untuk mengikat listener. [Panduan rendering Nuxt](https://nuxt.com/docs/4.x/guide/concepts/rendering) juga menyediakan client-side, prerendering, dan hybrid rendering.

## Dampak performa

SSR sering mempercepat kemunculan konten karena browser menerima struktur halaman lebih awal. Manfaat ini dapat terasa pada perangkat lambat ketika aplikasi client-heavy sebelumnya menahan tampilan sampai JavaScript selesai diunduh dan dijalankan.

Klaim bahwa SSR selalu lebih cepat tidak tepat. Server harus menjalankan logika, menunggu data, dan menghasilkan respons. [Definisi TTFB web.dev](https://web.dev/articles/ttfb) menempatkan latensi backend sebelum metrik visual seperti FCP dan LCP.

Hydration juga dapat mempertahankan biaya JavaScript di browser. Bundle besar, third-party scripts, dan mismatch antara keluaran server dengan client dapat menunda interaksi. [Panduan hydration Nuxt](https://nuxt.com/docs/4.x/guide/best-practices/hydration) memperingatkan bahwa mismatch dapat mengganggu fungsi dan memicu render ulang.

Caching mengurangi render berulang untuk respons yang aman digunakan bersama. Streaming mengurangi pemblokiran oleh satu data source lambat. [Dokumentasi streaming Next.js](https://nextjs.org/docs/app/guides/streaming) menjelaskan pengiriman bagian HTML ketika siap, bukan menunggu seluruh dokumen selesai.

## SEO dan crawler

SSR membantu crawlability karena konten dan link tersedia pada respons HTML. Ini berguna bagi crawler yang tidak menjalankan JavaScript atau memiliki antrean rendering terpisah.

SSR tidak menjamin ranking. [Google Search Central](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics?hl=en) dapat merender JavaScript, tetapi tetap menyarankan server-side atau pre-rendering. Status HTTP, robots, canonical, metadata, internal links, kualitas konten, dan performa tetap menentukan hasil.

## Pilihan rendering

SSR dinamis tepat untuk halaman publik yang bergantung pada data per permintaan. Prerendering lebih efisien bagi konten stabil. Client-side rendering dapat cukup untuk dashboard privat. Hybrid rendering memilih strategi berbeda per rute.

[[References/Next.js\|Next.js]] menyediakan Server Components, Client Components, prerendering, dynamic rendering, caching, dan streaming. [[References/Nuxt\|Nuxt]] menyediakan universal rendering, CSR, prerendering, serta route rules untuk hybrid rendering. Keduanya adalah contoh, bukan batas pilihan framework SSR.

Keputusan sebaiknya mengikuti volatilitas data, kebutuhan personalisasi, crawlability, target interaksi, cacheability, kapasitas server, dan lingkungan deployment. Ukur TTFB, FCP atau LCP, serta INP di lapangan sebelum menyimpulkan strategi terbaik.

## Batas

Sumber utama berasal dari dokumentasi framework dan panduan Google. Bukti mendukung mekanisme dan trade-off, bukan peringkat performa universal antara Next.js, Nuxt, SSR, SSG, dan CSR.

## Atomik Evergreen

- [[Rendered HTML does not make a page interactive\|Rendered HTML does not make a page interactive]]
- [[SSR moves work instead of removing it\|SSR moves work instead of removing it]]
- [[Server-rendered HTML helps crawling but cannot guarantee SEO\|Server-rendered HTML helps crawling but cannot guarantee SEO]]
- [[Route needs should choose the rendering mode\|Route needs should choose the rendering mode]]
