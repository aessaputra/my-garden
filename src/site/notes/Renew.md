---
{"dg-publish":true,"dg-permalink":"craft/renew","permalink":"/craft/renew/","title":"Renew: A Wallos Client","hideInFiletree":true,"tags":["craft"],"noteIcon":"","dg-note-properties":{"title":"Renew: A Wallos Client","tags":["craft"],"created":"2026-09-19","updated":"2026-09-19"}}
---

![renew-x8s0IFP6.webp](/img/user/Attachments/renew-x8s0IFP6.webp)

![renew-60pVRiJD.webp](/img/user/Attachments/renew-60pVRiJD.webp)

![renew-94WfRJqh.webp\|center\|300](/img/user/Attachments/renew-94WfRJqh.webp)

Renew adalah antarmuka untuk mengecek biaya langganan dan jadwal pembayaran tanpa membuka panel administrasi Wallos. Data langganan tetap dikelola oleh Wallos sebagai backend.

Di Renew, pengguna bisa mencari langganan aktif dan menyaringnya berdasarkan kategori atau metode pembayaran. Detail tiap langganan bisa dibuka, datanya diedit, atau langganannya dihapus. Saat menambah maupun mengedit langganan, pengguna juga bisa mengatur interval pembayaran dan pengingat.

Renew dibangun dengan SvelteKit, Svelte, TypeScript, dan Tailwind CSS. Tampilannya menyesuaikan ukuran layar, dengan tema gelap yang mengikuti pengaturan perangkat. Semua permintaan ke Wallos melewati server, sehingga API key tetap di server dan tidak dikirim ke browser.

Login menggunakan OpenID Connect (OIDC), dengan akses terbatas untuk satu pemilik. Dukungan PWA mencakup cache aset dan halaman offline. Pengelolaan langganan tetap membutuhkan koneksi internet.

Untuk mengatur kategori, mata uang, dan kanal notifikasi, pengguna tetap perlu membuka Wallos. Renew hanya menangani pengelolaan langganan sehari-hari, bukan seluruh fitur Wallos.

## Tautan
* [[https://renew.aes.my.id \| Renew]]
* [[https://github.com/ellite/wallos\|Wallos]]