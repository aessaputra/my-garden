---
{"dg-publish":true,"dg-path":"Static Site Generators.md","permalink":"/static-site-generators/","title":"Static Site Generators","hideInFiletree":true,"tags":["references","programming","frameworks","deployment","performance","devops"],"noteIcon":"","dg-note-properties":{"title":"Static Site Generators","categories":["Frameworks"],"tags":["references","programming","frameworks","deployment","performance","devops"],"sources":["_raw/articles/static-site-generators-research-packet.md"],"created":"2026-09-06","updated":"2026-09-06","confidence":"high"}}
---

Static site generator atau SSG adalah alat yang menggabungkan konten, data, konfigurasi, dan template saat build untuk menghasilkan [[References/HTML\|HTML]] serta aset siap deploy. Server produksi umumnya hanya menyajikan artifact tersebut, bukan merender halaman setiap kali ada request.

[Cloudflare](https://www.cloudflare.com/learning/performance/static-site-generator/) menjelaskan SSG sebagai kompromi antara penulisan HTML manual dan CMS dinamis. Template tetap dapat digunakan kembali, tetapi halaman dibuat sebelum pengunjung memintanya.

## Alur kerja

Konten biasanya ditulis dalam Markdown, HTML, atau format data. Generator membaca frontmatter, layout, include, collection, dan konfigurasi, lalu menulis halaman serta aset ke direktori output.

Build dapat berjalan secara lokal atau melalui [[CI\|CI]]. Artifact kemudian dikirim ke [[References/GitHub Pages\|GitHub Pages]], [[References/Netlify\|Netlify]], [[References/Vercel\|Vercel]], object storage, CDN, atau server web biasa. Perubahan konten memerlukan build baru agar output ikut berubah.

## Contoh

[[Jekyll\|Jekyll]] memakai Ruby, Liquid, markup, dan layout. [Dokumentasi Jekyll](https://jekyllrb.com/docs/) menempatkannya sebagai generator blog-aware yang membentuk situs statis dari teks.

Hugo ditulis dalam Go dan menyediakan template, taxonomy, multilingual content, serta pipeline aset. [Quick start Hugo](https://gohugo.io/getting-started/quick-start/) menunjukkan artifact publikasi ditulis ke direktori `public`.

[[References/Eleventy\|Eleventy]] berbasis JavaScript dan mendukung banyak template language. [Dokumentasi Eleventy](https://www.11ty.dev/docs/) menulis keluaran statis ke `_site`, sedangkan runtime JavaScript browser tidak disisipkan secara bawaan.

## Kecocokan

SSG cocok untuk blog, dokumentasi, portofolio, landing page, dan situs konten yang dapat diketahui sebelum deployment. Model ini juga baik ketika perubahan melalui Git dan review lebih penting daripada editing langsung di database.

SSG kurang memadai sebagai satu-satunya lapisan untuk dashboard privat, personalisasi per request, inventaris sangat volatil, autentikasi server, atau transaksi. Fitur tersebut memerlukan JavaScript browser, API, function, CMS, SSR, atau kombinasi hybrid.

## Performa, keamanan, dan hosting

HTML siap kirim dapat mengurangi compute request dan mempermudah cache CDN. Namun, output statis tidak otomatis cepat. JavaScript besar, gambar buruk, font, CSS, dan script pihak ketiga tetap menentukan pengalaman nyata.

Tidak adanya database dan renderer pada request memperkecil sebagian runtime surface. Risiko frontend, supply chain, pipeline deployment, secret, form, API, serta konfigurasi header tetap harus dikendalikan.

[[References/GitHub Pages\|GitHub Pages]] dapat menerbitkan berkas statis atau artifact dari custom Actions. Dokumentasinya menegaskan bahwa runtime PHP, Ruby, dan Python tidak tersedia, meskipun bahasa tersebut dapat dipakai selama build.

## Batas

Sumber utama berasal dari dokumentasi vendor dan platform. Bukti mendukung mekanisme umum, bukan klaim bahwa satu generator selalu tercepat, paling aman, atau terbaik untuk semua situs.

## Atomik Evergreen

- [[Static generation moves rendering work to build time\|Static generation moves rendering work to build time]]
- [[Static output broadens hosting choices\|Static output broadens hosting choices]]
- [[Content stability should determine static generation\|Content stability should determine static generation]]
- [[Static generation narrows runtime exposure without removing frontend risk\|Static generation narrows runtime exposure without removing frontend risk]]
