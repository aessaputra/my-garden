---
{"dg-publish":true,"dg-path":"Vercel.md","permalink":"/vercel/","title":"Vercel","hideInFiletree":true,"tags":["references","deployment","devops","performance"],"dg-note-properties":{"title":"Vercel","category":"references","tags":["references","deployment","devops","performance"],"sources":["_raw/articles/vercel-expanded.md","_raw/articles/vercel-9router-fetch-correction-2026-09-01.md"],"created":"2026-09-01","updated":"2026-09-01","confidence":"high"}}
---

Vercel adalah platform terkelola untuk membangun, menerbitkan, dan menjalankan aplikasi web. Platform ini memadukan proses build, deployment, CDN, komputasi terkelola, domain, keamanan, serta observabilitas.

Vercel sering diasosiasikan dengan frontend dan situs statis. Cakupannya kini lebih luas: aplikasi full-stack dan backend yang kompatibel dapat berjalan melalui Vercel Functions, sedangkan aset dan respons cache dilayani melalui CDN.

## Model deployment

Deployment adalah hasil build yang berhasil. Setiap deployment memperoleh URL unik dan bersifat terpisah, sehingga perubahan dapat diperiksa sebelum domain produksi diarahkan ke versi tersebut.

Vercel menyediakan lingkungan Local, Preview, dan Production. Preview dipakai untuk pengujian atau kolaborasi tanpa mengubah situs publik. Tim Pro dan Enterprise dapat menambah lingkungan khusus seperti staging atau QA.

Integrasi Git mendukung GitHub, GitLab, Bitbucket, dan Azure DevOps. Push ke branch dapat memicu Preview Deployment, sedangkan perubahan pada production branch memicu Production Deployment.

Deployment juga dapat dibuat melalui Vercel CLI, REST API, Deploy Hooks, atau unggahan folder. Git merupakan jalur umum, bukan satu-satunya cara memakai platform.

Rollback mengalihkan domain produksi ke deployment lama yang sudah tersedia. Ini mempercepat pemulihan, tetapi tidak otomatis mengembalikan migrasi database, data eksternal, atau perubahan layanan pihak ketiga.

## Framework dan hasil build

Vercel mendeteksi banyak framework dan menyiapkan build serta infrastruktur yang sesuai. Dukungan mencakup frontend, full-stack, dan sejumlah framework backend, tetapi kelengkapan fitur berbeda menurut framework dan adapter.

Aset statis dapat disajikan langsung dari CDN. Route dinamis dapat dijalankan sebagai Functions. Pada framework yang didukung, konfigurasi routing, rendering, dan caching dapat diterjemahkan menjadi infrastruktur platform saat build.

[[References/Next.js\|Next.js]] memiliki integrasi paling langsung karena dikembangkan oleh Vercel. Namun, Vercel bukan syarat untuk menjalankan Next.js, dan Vercel tidak terbatas pada aplikasi Next.js.

## Functions dan Fluid compute

Vercel Functions menjalankan kode server tanpa pengelolaan server langsung. Runtime, region, durasi maksimum, dan memori dapat dikonfigurasi dalam batas paket dan runtime yang digunakan.

Fluid compute mengoptimalkan konkurensi dengan memungkinkan satu instance menangani beberapa request. Model ini dapat mengurangi cold start dan biaya pada pola tertentu, tetapi instance hangat dapat dipakai ulang antar-request.

State pengguna tidak boleh disimpan pada variabel global atau module scope. State persisten, koordinasi, sesi, dan pub/sub sebaiknya berada pada database atau layanan eksternal yang dirancang untuk kebutuhan tersebut.

Functions tetap memiliki batas ukuran, memori, durasi, payload, file descriptor, dan concurrency. Request yang melewati durasi maksimum dapat berakhir dengan `FUNCTION_INVOCATION_TIMEOUT`.

## CDN, routing, dan cache

Setiap request ke deployment melewati CDN Vercel. Lapisan ini menangani routing, caching, compression, dan kontrol keamanan sebelum request mencapai kode aplikasi.

CDN dapat menyimpan aset statis, halaman, dan respons API yang memenuhi kebijakan cache. Cache hit mengurangi latensi dan beban origin.

Performa akhir tetap bergantung pada cacheability, lokasi data, kode aplikasi, dan layanan eksternal.

Vercel mendukung `Cache-Control`, `CDN-Cache-Control`, dan `Vercel-CDN-Cache-Control`. Header `x-vercel-cache` membantu menunjukkan apakah respons berasal dari cache.

Caching respons personal tanpa pemisahan cache key dan header yang tepat dapat membocorkan data antar-pengguna. Route autentikasi dan respons dengan `Set-Cookie` perlu diuji pada jalur produksi, bukan hanya di server lokal.

CDN Vercel dan [[References/Cloudflare\|Cloudflare]] sama-sama dapat menangani cache, routing, keamanan, serta compute. Arsitektur dan model produknya berbeda, sehingga keputusan tidak cukup didasarkan pada label CDN atau edge saja.

## Keamanan dan observabilitas

Deployment Protection membatasi pihak yang dapat membuka URL Preview atau Production sesuai metode dan cakupan yang dipilih. Perlindungan deployment tidak menggantikan autentikasi dan otorisasi aplikasi.

Platform juga menyediakan mitigasi DDoS dan kontrol keamanan jaringan. Fitur, retensi, kemampuan konfigurasi, serta ketersediaannya berbeda menurut paket.

Observability menampilkan traffic dan performa proyek melalui event serta insight. Fitur dasar tersedia pada semua paket, sedangkan Observability Plus menambah metrik, limit, dan retensi pada tim berbayar yang memenuhi syarat.

## Biaya dan trade-off

Biaya dapat mencakup Active CPU, provisioned memory, invocation, transfer data, image optimization, build, observability, dan sumber daya lain. Kuota, kredit, harga per region, serta batas paket dapat berubah.

Platform terkelola mengurangi pekerjaan pada server, deployment, sertifikat, dan distribusi global. Sebagai gantinya, aplikasi bergantung pada model runtime, cache, limit, harga, dan fitur spesifik platform.

Vercel tepat untuk tim yang menghargai Preview Deployment, integrasi Git, deployment immutable, CDN terpadu, dan runtime terkelola.

Static hosting sederhana mungkin cukup bila aplikasi hanya menghasilkan berkas statis tanpa kebutuhan server.

Backend stateful, proses sangat panjang, protokol khusus, atau kontrol jaringan mendalam perlu diperiksa terhadap batas platform. Gunakan dokumentasi limit dan uji produksi sebelum memilih Vercel sebagai target utama.

## Workflow yang aman

1. Hubungkan repository atau gunakan CLI, lalu pastikan framework dan perintah build terdeteksi benar.
2. Pisahkan environment variable Local, Preview, dan Production. Jangan memasukkan secret ke bundle browser.
3. Uji Preview Deployment, termasuk route dinamis, cache, autentikasi, dan koneksi database.
4. Promosikan deployment yang telah diperiksa. Siapkan rollback dan strategi migrasi data yang terpisah.
5. Pantau error, latensi, penggunaan Functions, cache status, serta biaya aktual setelah rilis.

## Lihat juga

- [[References/Next.js\|Next.js]]
- [[References/Cloudflare\|Cloudflare]]
- [[Content Delivery Network\|Content Delivery Network]]
- [[References/Cache-Control\|Cache-Control]]
- [[References/Web Hosting\|Web Hosting]]
- [[References/GitHub Pages\|GitHub Pages]]

## Sumber

- [Deploying to Vercel](https://vercel.com/docs/deployments): deployment unik, metode deployment, URL, dan pengelolaan rilis.
- [Deploying Git Repositories](https://vercel.com/docs/git): integrasi Git, Preview Deployment, Production Deployment, dan rollback.
- [Environments](https://vercel.com/docs/deployments/environments): Local, Preview, Production, Custom Environments, dan environment variables.
- [Frameworks on Vercel](https://vercel.com/docs/frameworks): deteksi framework, integrasi build, dan matriks dukungan infrastruktur.
- [Vercel Functions](https://vercel.com/docs/functions): komputasi terkelola, Fluid compute, region, concurrency, dan harga berbasis sumber daya.
- [Vercel Functions Limits](https://vercel.com/docs/functions/limitations): batas runtime, ukuran, durasi, memori, payload, dan kegagalan timeout.
- [Vercel CDN](https://vercel.com/docs/cdn): routing, caching, compute, jaringan global, dan proteksi default.
- [Vercel CDN Cache](https://vercel.com/docs/caching/cdn-cache): cache response, header khusus CDN, invalidasi, dan `x-vercel-cache`.
- [Global Network and Regions](https://vercel.com/docs/regions): hubungan PoP, region compute, cache, dan data locality.
- [Pricing on Vercel](https://vercel.com/docs/pricing): metrik penggunaan, kredit paket, dan sumber daya yang dapat ditagihkan.
- [Deployment Protection](https://vercel.com/docs/security/deployment-protection): metode serta cakupan perlindungan URL deployment.
- [Observability](https://vercel.com/docs/observability): event, insight, akses per paket, Observability Plus, dan retensi.
