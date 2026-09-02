---
{"dg-publish":true,"dg-path":"GitHub Pages.md","permalink":"/git-hub-pages/","title":"GitHub Pages","hideInFiletree":true,"tags":["references","programming","hosting","deployment","ci-cd","devops","security"],"dg-note-properties":{"title":"GitHub Pages","category":"references","tags":["references","programming","hosting","deployment","ci-cd","devops","security"],"sources":["_raw/articles/github-pages-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

GitHub Pages adalah layanan static site hosting milik [[GitHub\|GitHub]]. Layanan ini mengambil berkas [[References/HTML\|HTML]], [[References/CSS\|CSS]], dan [[References/JavaScript\|JavaScript]] dari repository, dapat menjalankan proses build, lalu menerbitkan hasilnya sebagai situs web. Kode aplikasi sisi server tidak dijalankan saat pengunjung membuka halaman, sehingga GitHub Pages cocok untuk dokumentasi proyek, blog, portofolio, landing page, dan situs lain yang dapat dibangun menjadi berkas statis.

GitHub Pages tersedia untuk repository publik pada GitHub Free. Repository privat dapat digunakan pada paket tertentu, tetapi situs Pages perlu diperlakukan sebagai publik kecuali organisasi memakai fitur private publishing yang sesuai. Secret, data pribadi, dan kredensial tidak boleh ikut masuk ke artifact atau repository yang diterbitkan.

## Jenis situs dan struktur URL

GitHub Pages membedakan dua jenis situs:

- User atau organization site memakai repository bernama `<owner>.github.io`. Setiap akun hanya dapat memiliki satu situs jenis ini, dengan alamat awal `<owner>.github.io`.
- Project site terkait dengan repository tertentu. Setiap repository dapat memiliki satu project site, biasanya tersedia di `<owner>.github.io/<repository>`.

Perbedaan URL berpengaruh pada konfigurasi static site generator. Project site berada di subpath, sehingga base URL untuk aset dan tautan internal perlu memuat nama repository. Kesalahan base path sering menghasilkan halaman utama yang tampil, tetapi CSS, JavaScript, gambar, atau route lain gagal dimuat.

## Sumber penerbitan

Ada dua pola penerbitan utama. Pola pertama adalah deploy dari branch. GitHub mengambil berkas dari root atau folder `/docs` pada branch yang dipilih. Entry file seperti `index.html`, `index.md`, atau `README.md` harus berada di tingkat teratas sumber penerbitan. Perubahan pada branch memicu workflow deployment Pages.

Pola kedua memakai custom [[GitHub Actions\|GitHub Actions]] workflow. Workflow melakukan checkout, menjalankan build bila perlu, mengunggah artifact melalui `actions/upload-pages-artifact`, lalu menerbitkannya dengan `actions/deploy-pages`. Job deployment memerlukan izin `pages: write` dan `id-token: write`, serta environment yang umumnya bernama `github-pages`.

Custom workflow lebih sesuai untuk [[References/Astro\|Astro]], [[References/Vite\|Vite]], framework dokumentasi, atau static site generator selain Jekyll. Workflow dapat menginstal dependency, menjalankan test, membangun situs, dan hanya mengunggah direktori output. Artifact akhir harus memuat entry file di root dan tidak boleh bergantung pada server runtime.

## Jekyll dan static site generator lain

[[Jekyll\|Jekyll]] memiliki dukungan bawaan di GitHub Pages. Jekyll mengubah Markdown dan HTML menjadi situs statis melalui layout, tema, Liquid template, dan plugin yang didukung. Saat penerbitan berasal langsung dari branch, Pages memakai Jekyll secara default.

Jekyll bukan syarat penggunaan GitHub Pages. Situs HTML biasa dapat diterbitkan tanpa generator. Generator lain sebaiknya dibangun melalui GitHub Actions, lalu hasil statisnya diunggah sebagai artifact. Jika hasil build dikirim langsung ke branch, berkas `.nojekyll` mencegah Pages menjalankan pemrosesan Jekyll pada output tersebut.

Batas plugin pada build Jekyll bawaan perlu diperhatikan. Plugin yang tidak didukung tidak dapat dijalankan oleh Pages. Solusinya adalah membangun situs melalui workflow sendiri dan menerbitkan output akhirnya.

## Custom domain dan HTTPS

Situs dapat memakai domain `github.io` atau custom domain. Subdomain seperti `www.example.com` dan `docs.example.com` menggunakan record `CNAME` yang menunjuk ke `<owner>.github.io`. Apex domain seperti `example.com` menggunakan record `A`, `AAAA`, `ALIAS`, atau `ANAME` sesuai dukungan penyedia [[References/DNS\|DNS]]. GitHub menyarankan konfigurasi `www` bersama apex domain agar redirect dan pengelolaan sertifikat lebih stabil.

Custom domain harus ditambahkan ke pengaturan repository sebelum DNS diarahkan. Domain juga sebaiknya diverifikasi pada GitHub untuk mengurangi risiko domain takeover. Wildcard DNS seperti `*.example.com` tidak disarankan karena dapat membuka subdomain yang belum diklaim.

Semua situs Pages mendukung [[References/HTTPS\|HTTPS]], termasuk custom domain yang dikonfigurasi dengan benar. Opsi Enforce HTTPS mengalihkan request HTTP ke HTTPS. Sertifikat custom domain disediakan setelah pemeriksaan DNS berhasil. Aset yang masih memakai URL HTTP dapat menyebabkan mixed content meskipun halaman utamanya sudah memakai HTTPS.

## Batas teknis dan kebijakan

GitHub Pages hanya melayani situs statis. PHP, Python, Ruby, database server, dan proses backend lain tidak tersedia sebagai runtime request. Fitur dinamis harus memakai JavaScript di browser atau API eksternal. Formulir, autentikasi, pencarian server-side, dan penyimpanan data karena itu membutuhkan layanan lain.

Dokumentasi GitHub menetapkan batas berikut untuk GitHub.com:

- repository sumber direkomendasikan tidak lebih dari 1 GB;
- situs yang diterbitkan tidak boleh lebih dari 1 GB;
- deployment berhenti jika berjalan lebih dari 10 menit;
- bandwidth memiliki soft limit 100 GB per bulan;
- build memiliki soft limit 10 kali per jam, kecuali build dan publish dilakukan melalui custom GitHub Actions workflow.

GitHub Pages bukan layanan gratis untuk menjalankan toko online, SaaS, atau situs yang terutama memproses transaksi komersial. Layanan ini juga tidak sesuai untuk transaksi sensitif seperti pengiriman password atau nomor kartu. Situs dengan kebutuhan backend, kontrol cache tingkat lanjut, traffic besar, atau aturan deployment khusus lebih tepat ditempatkan pada [[References/Web Hosting\|Web Hosting]] atau platform deployment lain.

## Kapan tepat digunakan

GitHub Pages tepat ketika source dan situs ingin dikelola dalam alur Git yang sama. Commit dan pull request menjadi riwayat perubahan konten, sementara Actions menangani build dan deployment. Model ini sederhana untuk dokumentasi open source, halaman proyek, resume, blog statis, dan demo frontend tanpa backend.

Layanan ini kurang tepat untuk aplikasi yang memerlukan SSR saat request, data rahasia di server, scheduled process, database internal, atau endpoint API. Framework seperti Astro masih dapat dipakai selama menghasilkan static output. Sebaliknya, mode server pada [[References/Next.js\|Next.js]] atau [[References/TanStack Start\|TanStack Start]] tidak dapat dijalankan langsung di GitHub Pages.

## Lihat juga

- [[GitHub\|GitHub]]
- [[GitHub Actions\|GitHub Actions]]
- [[References/Git\|Git]]
- [[References/Web Hosting\|Web Hosting]]
- [[References/DNS\|DNS]]
- [[References/HTTPS\|HTTPS]]
- [[References/HTML\|HTML]]
- [[References/Astro\|Astro]]
- [[References/Vite\|Vite]]
- [[Jekyll\|Jekyll]]

## Sumber

- [What is GitHub Pages?](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages): definisi, tipe situs, repository, URL default, dan custom domain.
- [Creating a GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site): entry file, static site generator, `.nojekyll`, dan batas runtime server-side.
- [Configuring a publishing source for your GitHub Pages site](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site): penerbitan dari branch atau GitHub Actions.
- [Using custom workflows with GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages): artifact, permission, environment, build, dan deployment actions.
- [GitHub Pages limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits): ukuran, waktu deployment, bandwidth, frekuensi build, dan batas penggunaan.
- [About GitHub Pages and Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll): build Jekyll, Markdown, Liquid, tema, plugin, dan rekomendasi Actions.
- [About custom domains and GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages): tipe domain, pewarisan domain, verifikasi, dan risiko takeover.
- [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site): pengaturan DNS, CNAME, apex domain, dan wildcard record.
- [Securing your GitHub Pages site with HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https): dukungan HTTPS, enforcement, sertifikat, DNS, dan mixed content.
- [Troubleshooting 404 errors for GitHub Pages sites](https://docs.github.com/en/pages/getting-started-with-github-pages/troubleshooting-404-errors-for-github-pages-sites): entry file, root artifact, DNS, repository, dan broken links.
