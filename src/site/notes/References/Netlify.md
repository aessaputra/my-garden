---
{"dg-publish":true,"dg-path":"Netlify.md","permalink":"/netlify/","title":"Netlify","hideInFiletree":true,"tags":["references","deployment","devops","performance"],"noteIcon":"","dg-note-properties":{"title":"Netlify","category":"references","tags":["references","deployment","devops","performance"],"sources":["_raw/articles/netlify-research-packet.md","_raw/articles/netlify-research-packet-correction-2026-09-02.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Netlify adalah platform terkelola untuk membangun, menerbitkan, dan menjalankan situs serta aplikasi web.

Platform ini memadukan build, deployment, CDN, komputasi serverless, domain, keamanan, routing, dan observabilitas dalam satu alur kerja.

Netlify dahulu identik dengan static site hosting. [Primitive platform](https://docs.netlify.com/start/core-concepts/primitives) kini mendukung aplikasi dinamis dan full-stack, meski kemampuan framework bergantung pada adapter.

## Model deployment

[Continuous deployment](https://docs.netlify.com/deploy/create-deploys) menghubungkan repository Git ke proyek. Push dapat menjalankan perintah build dan menerbitkan hasilnya ke jaringan Netlify.

GitHub, GitLab, Bitbucket, dan Azure DevOps dapat memicu workflow ini. Deployment juga dapat dibuat lewat CLI, API, drag and drop, atau metode manual. Git merupakan jalur utama, bukan kewajiban.

Setiap [atomic deploy](https://docs.netlify.com/deploy/deploy-overview) adalah versi lengkap. Versi baru baru menjadi live setelah seluruh perubahan siap, sehingga HTML dan aset tidak tercampur dengan versi sebelumnya.

Atomic deploy mempermudah rollback ke versi lama. Rollback tersebut tidak otomatis membalikkan migrasi database, perubahan data, atau konfigurasi layanan eksternal.

## Preview dan kolaborasi

[Deploy Preview](https://docs.netlify.com/deploy/deploy-types/deploy-previews) memberi URL unik untuk perubahan pada pull atau merge request. Tim dapat memeriksa UI, route, dan integrasi sebelum publikasi produksi.

Preview tetap dapat mengakses secret, data, atau backend bila environment salah dikonfigurasi. Akses preview dan environment variable perlu dipisahkan dari produksi.

Branch deploy menyediakan URL persisten untuk branch tertentu. Production deploy berasal dari production branch, sedangkan published deploy adalah versi yang saat ini dilayani pada domain utama.

## Build dan konfigurasi

Netlify mendeteksi banyak framework dan menjalankan build sesuai konfigurasi. Hasil statis diterbitkan ke CDN, sedangkan kebutuhan server dapat diterjemahkan ke Functions atau primitive platform lain.

Berkas [`netlify.toml`](https://docs.netlify.com/build/configure-builds/file-based-configuration) menyimpan build command, publish directory, redirect, header, dan konfigurasi per konteks bersama kode.

Konfigurasi berbasis file membantu reproduksibilitas dan review melalui version control. Namun, `netlify.toml` bukan representasi lengkap karena beberapa pengaturan hanya tersedia melalui UI, CLI, atau API.

## Functions dan Edge Functions

[Netlify Functions](https://docs.netlify.com/build/functions/overview) menjalankan kode server untuk endpoint API, server-side rendering, dan event lain tanpa pengelolaan server langsung.

Runtime Functions bersifat ephemeral dan diskalakan oleh platform. State persisten tetap harus ditempatkan pada database atau penyimpanan eksternal, bukan diasumsikan bertahan di memori proses.

[Edge Functions](https://docs.netlify.com/build/edge-functions/overview) menjalankan JavaScript atau TypeScript di network edge. Kegunaannya meliputi autentikasi, personalisasi, redirect, rewrite, dan middleware request.

Functions dan Edge Functions bukan runtime yang identik. Batas waktu, API, lokasi eksekusi, kompatibilitas pustaka, caching, dan ketersediaan paket perlu diperiksa sebelum memilih salah satunya.

## CDN, cache, dan routing

Aset statis dilayani melalui jaringan edge. Menurut [dokumentasi caching](https://docs.netlify.com/build/caching/caching-overview), aset statis dicache dan otomatis diinvalidasi saat deploy mengubah konten.

Respons dari Functions, Edge Functions, dan proxy tidak dicache secara default. Cache dinamis memerlukan header yang tepat serta pengujian agar respons personal tidak digunakan ulang untuk pengguna lain.

Header `Netlify-CDN-Cache-Control`, `CDN-Cache-Control`, dan `Cache-Control` memberi cakupan kontrol berbeda. Hubungannya dengan browser dan CDN lain perlu dipahami sebelum menetapkan freshness.

Aturan [redirect dan rewrite](https://docs.netlify.com/manage/routing/redirects/redirect-options) dapat dijalankan di edge. Rewrite status `200` juga lazim dipakai sebagai fallback route untuk single-page application.

## Domain dan keamanan

Netlify menyediakan [HTTPS terkelola](https://docs.netlify.com/manage/domains/secure-domains-with-https/https-ssl) dengan pembuatan dan pembaruan sertifikat otomatis. Custom domain dan sertifikat khusus juga didukung.

HSTS dapat memaksa browser memakai HTTPS. Opsi `includeSubDomains` dan `preload` harus diterapkan hati-hati karena kesalahan dapat memutus akses subdomain dan sulit dibalik.

[Checklist keamanan Netlify](https://docs.netlify.com/resources/checklists/security-checklist) merekomendasikan perlindungan preview, MFA, pengelolaan secret, CSP, security header, dan peninjauan dependency.

Perlindungan platform tidak menggantikan autentikasi, otorisasi, validasi input, keamanan dependency, serta pembatasan akses data dalam aplikasi.

## Observabilitas

Deploy log membantu menemukan kegagalan build. Log Functions dan Edge Functions membantu diagnosis runtime, sedangkan metrik dapat menghubungkan deployment dengan error atau latensi.

[Function Metrics](https://docs.netlify.com/manage/monitoring/function-metrics) mencakup invocation, error rate, dan durasi. Retensi, detail, dan ketersediaan observabilitas berbeda menurut paket.

## Trade-off

Netlify mengurangi pekerjaan untuk server, sertifikat, deployment atomik, preview, dan distribusi global. Keuntungan ini ditukar dengan ketergantungan pada runtime, limit, harga, serta konfigurasi spesifik platform.

Platform ini cocok untuk situs statis, frontend modern, dan aplikasi full-stack yang sesuai dengan Functions atau Edge Functions.

Server stateful, proses sangat panjang, protokol khusus, atau kontrol jaringan mendalam perlu diperiksa terhadap limit aktual. Harga dan batas paket dapat berubah, sehingga dokumentasi terbaru tetap menjadi acuan.

Netlify, [[References/Vercel\|Vercel]], [[References/Cloudflare\|Cloudflare]], dan [[References/GitHub Pages\|GitHub Pages]] memiliki irisan pada deployment web. Bandingkan kebutuhan runtime, preview, cache, observabilitas, biaya, dan tingkat kontrol, bukan label hosting semata.

## Workflow yang aman

- Hubungkan repository atau gunakan CLI, lalu verifikasi build command dan publish directory.
- Simpan konfigurasi yang sesuai di `netlify.toml`, tetapi audit pengaturan yang hanya berada di UI.
- Pisahkan environment variable preview dan produksi. Jangan menaruh secret di bundle browser.
- Uji Deploy Preview, route dinamis, autentikasi, cache, Functions, dan koneksi data.
- Siapkan rollback aplikasi serta prosedur terpisah untuk migrasi data.
- Pantau build, error runtime, latensi, penggunaan, dan biaya setelah publikasi.

## Lihat juga

- [[References/Deployment\|Deployment]]
- [[References/Render\|Render]]
- [[References/Railway\|Railway]]
- [[References/Vercel\|Vercel]]
- [[References/Cloudflare\|Cloudflare]]
- [[References/GitHub Pages\|GitHub Pages]]
- [[References/Web Hosting\|Web Hosting]]
- [[References/Cache-Control\|Cache-Control]]
- [[References/HTTPS\|HTTPS]]

## Sumber

- [Netlify Platform Primitives](https://docs.netlify.com/start/core-concepts/primitives): cakupan runtime dan layanan platform.
- [Create deploys](https://docs.netlify.com/deploy/create-deploys): Git, continuous deployment, CLI, dan deploy manual.
- [Deploy overview](https://docs.netlify.com/deploy/deploy-overview): atomic deploy, branch, production, dan published deploy.
- [Deploy Previews](https://docs.netlify.com/deploy/deploy-types/deploy-previews): preview pull request dan merge request.
- [File-based configuration](https://docs.netlify.com/build/configure-builds/file-based-configuration): `netlify.toml` dan batas cakupannya.
- [Functions overview](https://docs.netlify.com/build/functions/overview): runtime serverless, scaling, deployment, dan rollback.
- [Edge Functions overview](https://docs.netlify.com/build/edge-functions/overview): eksekusi edge, middleware, dan personalisasi.
- [Caching overview](https://docs.netlify.com/build/caching/caching-overview): cache statis, cache dinamis, header, dan invalidasi.
- [Redirect options](https://docs.netlify.com/manage/routing/redirects/redirect-options): redirect, rewrite, status, dan pemrosesan edge.
- [HTTPS](https://docs.netlify.com/manage/domains/secure-domains-with-https/https-ssl): sertifikat terkelola, custom certificate, dan HSTS.
- [Netlify security checklist](https://docs.netlify.com/resources/checklists/security-checklist): kontrol platform dan kewajiban keamanan aplikasi.
- [Function Metrics](https://docs.netlify.com/manage/monitoring/function-metrics): invocation, error, durasi, dan batas retensi.
