---
{"dg-publish":true,"dg-path":"Web Hosting.md","permalink":"/web-hosting/","title":"Web Hosting","hideInFiletree":true,"tags":["network","hosting","devops","guide"],"dg-note-properties":{"title":"Web Hosting","category":"references","tags":["network","hosting","devops","guide"],"sources":["_raw/articles/web-hosting-namecheap.md"],"created":"2026-08-21","updated":"2026-08-21"}}
---

Web hosting adalah proses menyewa atau membeli ruang di server untuk menampung website di World Wide Web. Konten website (HTML, CSS, gambar, database) harus disimpan di server agar bisa diakses online. Setiap website yang pernah dikunjungi di-host di sebuah server.

Sumber: Namecheap.

## Cara kerja web hosting

File website di-upload dari komputer lokal ke web server. Resource server (RAM, hard drive space, bandwidth) dialokasikan ke website yang memakainya. DNS memastikan browser terhubung ke server yang tepat menyimpan file website: saat seseorang mengetik alamat web, komputernya terhubung ke server via IP address dari DNS, lalu browser menampilkan halaman.

## Bahasa analogi: hosting = sewa kantor

- Shared hosting ≈ workstation di co-working space: murah, berbagi fasilitas (dapur, printer), tidak bisa renovasi besar. Cocok untuk website kecil.
- VPS ≈ kantor dalam business park: punya tetangga tapi tidak tergantung, bisa kustomisasi sendiri.
- Dedicated server ≈ menyewa seluruh gedung: mahal, kontrol penuh, untuk website yang mementingkan reliabilitas dan performa.

## Jenis hosting

### Shared Web Hosting
Banyak website di satu server yang sama. Murah dan mudah setup, cocok untuk situs baru, personal, dan bisnis kecil-menengah. Tidak cocok untuk situs besar dengan trafic tinggi.

### VPS Hosting (Virtual Private Server / VDS)
Virtual server yang tampak seperti dedicated server padahal melayani banyak website. Client punya akses penuh konfigurasi, lebih dekat ke dedicated. Stepping stone antara shared dan dedicated. Cocok untuk yang mau fleksibilitas dedicated tanpa biaya tinggi.

### Dedicated Hosting
Menyewa seluruh server. Mahal, dipakai saat trafic besar atau butuh kontrol server penuh (software, sistem keamanan, administrasi). Membutuhkan keahlian teknis untuk mengelola sendiri.

### Cloud Hosting
Beroperasi di banyak web server saling terhubung: infrastruktur terjangkau, scalable, andal. Biasanya bandwidth unmetered, disk space besar, unlimited domains. Cocok untuk aplikasi intensif resource atau banyak aset gambar. Bisa jauh lebih mahal.

### Reseller Hosting
Akun owner memakai jatah hard drive dan bandwidth untuk meng-host website pihak ketiga. Owner jadi 'reseller'. Cocok untuk entrepreneur yang mau berbagi resource sambil dapat pendapatan berulang, atau pemilik banyak domain yang mau membuat paket hosting sendiri.

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

- [[References/Domain Name\|Domain Name]]: domain sebagai identitas; hosting sebagai rumah file website
- [[References/HTTP\|HTTP]]: protokol yang melayani file website ke browser
- [[References/How Does Internet Work\|How Does Internet Work]]: TCP/IP, server dan koneksi