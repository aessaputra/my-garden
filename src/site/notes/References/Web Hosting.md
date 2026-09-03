---
{"dg-publish":true,"dg-path":"Web Hosting.md","permalink":"/web-hosting/","title":"Web Hosting","hideInFiletree":true,"tags":["network","hosting","devops","guide"],"noteIcon":"","dg-note-properties":{"title":"Web Hosting","category":"references","tags":["network","hosting","devops","guide"],"sources":["_raw/articles/web-hosting-namecheap.md"],"created":"2026-08-21","updated":"2026-09-02"}}
---

Web hosting adalah proses menyewa atau membeli ruang di server untuk menampung website di World Wide Web.

Konten website seperti HTML, CSS, gambar, dan database disimpan di server agar dapat diakses online. Setiap website yang dikunjungi di-host pada sebuah server.

Sumber: Namecheap.

## Cara kerja web hosting

File website di-upload dari komputer lokal ke web server. Resource seperti RAM, hard drive space, dan bandwidth dialokasikan ke website yang memakainya.

DNS menghubungkan browser ke server yang menyimpan website. Saat alamat web dimasukkan, komputer memperoleh IP address server melalui DNS, lalu browser menampilkan halaman.

## Bahasa analogi: hosting = sewa kantor

- Shared hosting ≈ workstation di co-working space: murah, berbagi fasilitas (dapur, printer), tidak bisa renovasi besar. Cocok untuk website kecil.
- VPS ≈ kantor dalam business park: punya tetangga tapi tidak tergantung, bisa kustomisasi sendiri.
- Dedicated server ≈ menyewa seluruh gedung: mahal, kontrol penuh, untuk website yang mementingkan reliabilitas dan performa.

## Jenis hosting

### Shared Web Hosting
Banyak website di satu server yang sama. Murah dan mudah setup, cocok untuk situs baru, personal, dan bisnis kecil-menengah. Tidak cocok untuk situs besar dengan trafic tinggi.

### VPS Hosting (Virtual Private Server / VDS)
Virtual server tampak seperti dedicated server meski host fisiknya melayani banyak website. Client mendapat akses penuh untuk mengatur konfigurasi.

VPS menjadi pilihan antara shared dan dedicated hosting. Model ini cocok bagi pengguna yang memerlukan fleksibilitas dedicated tanpa seluruh biayanya.

### Dedicated Hosting
Menyewa seluruh server. Mahal, dipakai saat trafic besar atau butuh kontrol server penuh (software, sistem keamanan, administrasi). Membutuhkan keahlian teknis untuk mengelola sendiri.

### Cloud Hosting
Cloud hosting berjalan pada beberapa web server yang saling terhubung. Infrastruktur ini scalable dan andal, dengan bandwidth, disk space, serta domain sesuai paket.

Model ini cocok untuk aplikasi yang intensif resource atau memiliki banyak aset gambar. Biayanya dapat jauh lebih tinggi.

### Reseller Hosting
Pemilik akun memakai jatah hard drive dan bandwidth untuk meng-host website pihak ketiga. Pemilik tersebut bertindak sebagai reseller.

Model ini cocok bagi entrepreneur yang menjual resource untuk pendapatan berulang atau pemilik banyak domain yang ingin membuat paket hosting sendiri.

## Cara memilih host

### Pertimbangan teknis
- Bandwidth allowance: jumlah byte untuk transfer situs ke pengunjung. Situs baru tanpa video/musik biasanya < 3 GB/bulan. Free host sering pasang limit harian/bulanan; kelebihan trafic = website dinonaktifkan atau ditagih.
- File size limits: free host sering membatasi ukuran file upload.
- PHP/Perl: pastikan bisa install tanpa persetujuan host.
- .htaccess: dibutuhkan untuk custom error pages, proteksi hotlinking, password-protect folder.
- SSH: berguna untuk maintain database MySQL dan CMS/blog.
- FTP: cara populer upload file. Pastikan ada FTP access atau cara upload lain.
- Control panel (cPanel): dashboard untuk manage email, password, konfigurasi server dasar.
- Multiple domains: bisa host beberapa domain dalam satu akun (add-on domain), cek biayanya.
- Email: cek bisa setup email di domain sendiri (info@domain.com) sebelum signup.
- Technical support: cari 24/7/365, baca review real customer.

### Biaya
- Shared hosting: $10-$150/tahun untuk situs dasar.
- Plan lebih tinggi mulai $150 ke atas.
- Bayar tahunan lebih murah dari bulanan.
- Waspada: harga signup murah, renewal bisa jauh lebih mahal.

### Uptime
Reliabilitas 24/7 hanya realistis di plan berbayar. Cek uptime history dan garansi uptime sebelum memilih. Situs sering down/lemot = kehilangan pengunjung dan pendapatan.

## Web hosting vs domain hosting

- Domain name: identitas website (lihat [[References/Domain Name\|Domain Name]]).
- Web hosting: tempat menyimpan file website.
- DNS menghubungkan keduanya: domain → IP address server → file website.

## Lihat juga

- [[References/Deployment\|Deployment]]: proses membawa artifact dan konfigurasi ke environment yang dapat diakses pengguna
- [[References/Domain Name\|Domain Name]]: domain sebagai identitas; hosting sebagai rumah file website
- [[References/HTTP\|HTTP]]: protokol yang melayani file website ke browser
- [[References/How Does Internet Work\|How Does Internet Work]]: TCP/IP, server dan koneksi