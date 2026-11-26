---
{"dg-publish":true,"dg-path":"Caching.md","permalink":"/caching/","title":"Caching","hideInFiletree":true,"tags":["references","programming","performance"],"noteIcon":"","dg-note-properties":{"title":"Caching","categories":["Software Systems"],"type":"reference","tags":["references","programming","performance"],"sources":["_raw/articles/caching-user-summary-2026-09-11.md","References/Cache-Control.md","_raw/articles/cloudflare-what-is-caching-2026-11-26.md","_raw/articles/networkencyclopedia-cache-server-2026-11-26.md","_raw/articles/nordvpn-caching-server-2026-11-26.md"],"created":"2026-09-11","updated":"2026-11-26","confidence":"medium"}}
---

Caching menyimpan salinan data atau hasil komputasi agar dapat digunakan kembali tanpa selalu mengambil atau menghitungnya dari awal. Cache dapat berada di memori aplikasi, disk, browser, proxy, CDN, atau layanan cache terpisah.

Cache dapat mengurangi latency karena letaknya lebih dekat dengan pemanggil, tetapi kedekatan geografis bukan syarat. Menghindari query database, transfer data, atau komputasi berulang juga dapat mengurangi waktu respons dan beban sumber asli.

[[References/Redis\|Redis]] dan [[References/Memcached\|Memcached]] dapat dipakai sebagai cache bersama; penggunaannya tetap membutuhkan kebijakan key, masa berlaku, dan eviction.

[[References/Client Side Caching\|Client Side Caching]] membahas penyimpanan di perangkat pengguna melalui HTTP cache dan API browser, berbeda dari layanan cache bersama.

## Cara kerja

- **Cache hit**: entri yang sesuai tersedia dan boleh dipakai menurut kebijakan cache.
- **Cache miss**: tidak ada entri yang dapat dipakai; sistem mengambil atau menghitung hasil dari sumber aslinya, lalu dapat menyimpannya untuk request berikutnya.
- **Cache key**: identitas yang menentukan entri mana yang cocok dengan suatu request atau input.

Contoh hipotetis: aplikasi menyimpan hasil pembacaan katalog produk agar request berikutnya tidak selalu menjalankan query yang sama. Setelah harga berubah, cache perlu diperbarui, dibatalkan, atau dibiarkan kedaluwarsa sesuai toleransi data lama.

## Freshness dan invalidation

Cache membutuhkan kebijakan kapan data boleh digunakan kembali. TTL membatasi masa berlaku menurut aturan sistem; invalidation menandai atau menghapus entri ketika tidak lagi layak digunakan. Eviction mengeluarkan entri, misalnya karena kapasitas terbatas, bukan selalu karena data sudah berubah.

Dalam HTTP, data stale dapat divalidasi ulang sebelum dipakai. [[References/Cache-Control\|Cache-Control]] menjelaskan freshness, validator, serta private dan shared cache. Header HTTP tersebut tidak otomatis mengatur seluruh cache aplikasi.

## Cache di web

**Browser cache** menyimpan HTML, CSS, JavaScript, dan gambar di perangkat pengguna. Entri bertahan sampai TTL habis, cache penuh, atau pengguna membersihkannya. Ini mempercepat kunjungan berikutnya, tetapi tidak membantu kunjungan pertama dan bisa menyajikan aset usang bila nama file tidak di-fingerprint.

**CDN** menyimpan salinan di PoP yang dekat dengan pengguna. Request datang ke edge; bila hit, edge langsung menjawab; bila miss, edge mengambil dari origin, menyimpan hasilnya, lalu menjawab. TTL dan header cache menentukan masa simpan, ditambah purge manual atau eviction untuk konten yang jarang diminta. [[References/Cloudflare\|Cloudflare]] adalah contoh jaringan anycast, tetapi setiap PoP tetap mengisi cache-nya sendiri: anycast tidak berarti semua lokasi sudah memiliki objek.

**DNS cache** menyimpan hasil lookup di resolver dan browser; **search engine cache** menyimpan snapshot halaman. Keduanya memakai prinsip yang sama pada lapisan berbeda.

**Cache server korporat** menyimpan halaman yang sering diminta di sisi WAN agar link ke ISP tidak dipenuhi traffic berulang. Pada era proxy dan firewall, produk seperti Microsoft Proxy Server, ISA Server, dan Forefront TMG menggabungkan fungsi ini. Lini produk itu sudah dihentikan dan dukungan Forefront TMG berakhir 14 April 2020, jadi bagian sejarah tersebut tidak relevan untuk deployment baru; padanan modernnya adalah reverse proxy dan CDN caching ([[References/Proxy Server\|Proxy Server]]).

Cache juga bukan hanya infrastruktur jaringan. Cache aplikasi in-process, [[References/Redis\|Redis]] atau [[References/Memcached\|Memcached]], buffer pool database, dan materialized view menyimpan hasil di dalam atau dekat layanan yang membutuhkannya.

## Manfaat dan batas

Caching dapat meningkatkan [[References/Web Performance\|Web Performance]] dan mengurangi beban sumber, tetapi tidak menjamin semua request lebih cepat. Cache miss, lookup jaringan, serialisasi, dan pemeliharaan cache menambah biaya. Ukur latency, hit rate, dan beban sumber pada workload yang sebenarnya.

Cache key dan batas akses harus membedakan data yang memang berbeda, termasuk konteks pengguna bila relevan. Jangan menyajikan respons pribadi dari shared cache kepada pengguna lain. [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]] tetap berlaku ketika data berasal dari cache.

Cache publik menambah permukaan serangan: **cache poisoning** menyisipkan respons beracun agar tersaji ke pengguna lain, dan **cache deception** memancing halaman privat disimpan sebagai aset statis. Validasi input cache key dan jangan cache respons terautentikasi di lapisan bersama.

Cache bukan pengganti penyimpanan utama atau backup. Sistem perlu menentukan toleransi data lama serta perilaku ketika cache kosong atau tidak tersedia.

## Koreksi dan catatan sumber

1. **Cloudflare: "cached content remains in the CDN cache as long as users continue to request it".** Ini bertentangan dengan bagian TTL di artikel yang sama. TTL, eviction, dan purge yang menentukan; popularitas hanya memengaruhi peluang objek masih ada.
2. **Cloudflare: TTL "stored in the HTTP header".** Terlalu sederhana. TTL diekspresikan lewat `Cache-Control: max-age`, `Expires`, atau header khusus CDN; `Age` melaporkan usia objek, bukan menetapkan TTL.
3. **Cloudflare: 340 kota/PoP.** Snapshot vendor per November 2026, bukan angka umum.
4. **Cloudflare: "anycast berarti konten yang sama dapat dikirim dari data center mana pun".** Mengabaikan cache miss per PoP; lokasi tanpa salinan tetap harus mengambil ke origin.
5. **Network Encyclopedia: artikel 2019, diperbarui 2021.** Sebagian besar isinya sejarah Microsoft Proxy Server, ISA Server, dan Forefront TMG. Produknya berhenti 2012 dan dukungan berakhir 14 April 2020, jadi bukan rujukan deployment saat ini.
6. **Network Encyclopedia: framing "cache server".** Berasal dari era WAN korporat; padanan modernnya reverse proxy dan CDN caching.
7. **NordVPN: "dedicated network server or service".** Terlalu sempit. Cache bisa berupa komponen in-process tanpa server terpisah.
8. **NordVPN: "database caching menyimpan query".** Yang disimpan adalah hasil query, row/page buffer, atau hasil materialized view, bukan teks query.
9. **NordVPN: "sit between the user and the host server".** Tidak berlaku untuk browser cache di perangkat pengguna dan cache internal aplikasi.

## Sumber

- [[References/Cache-Control\|Cache-Control]]: catatan vault tentang reuse, freshness, cache key, dan keamanan HTTP caching.
- [RFC 9111: HTTP Caching](https://www.rfc-editor.org/rfc/rfc9111.html): aturan cache khusus HTTP.
- [What is caching?](https://www.cloudflare.com/en-gb/learning/cdn/what-is-caching/), Cloudflare Learning Center, diakses 26 November 2026.
- [Cache Server](https://networkencyclopedia.com/cache-server/), Network Encyclopedia (2019, diperbarui 2021), diakses 26 November 2026.
- [Caching server](https://nordvpn.com/cybersecurity/glossary/caching-server/), NordVPN Glossary, diakses 26 November 2026.
