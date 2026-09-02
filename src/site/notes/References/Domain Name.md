---
{"dg-publish":true,"dg-path":"Domain Name.md","permalink":"/domain-name/","title":"Domain Name","hideInFiletree":true,"tags":["references","network","dns","security","guide"],"noteIcon":"","dg-note-properties":{"title":"Domain Name","category":"references","tags":["references","network","dns","security","guide"],"sources":["_raw/articles/domain-name-expanded.md"],"created":"2026-08-21","updated":"2026-08-30","confidence":"high"}}
---

Domain name adalah nama hierarkis yang digunakan untuk mengenali sumber daya di Internet. Nama seperti `example.com` lebih mudah digunakan manusia daripada alamat IP, sementara [[References/DNS\|DNS]] menghubungkan nama tersebut dengan rekaman seperti alamat server, server surat elektronik, atau server nama.

Domain tidak sama dengan website. Registrasi domain memberi hak untuk memakai sebuah nama selama periode tertentu, sedangkan website memerlukan konten dan layanan [[References/Web Hosting\|hosting]]. Satu domain juga dapat dipakai untuk website, alamat email, API, atau layanan lain melalui konfigurasi DNS yang berbeda.

## Struktur nama domain

Nama domain tersusun dari label yang dipisahkan oleh titik dan dibaca dari kanan ke kiri. Pada `developer.mozilla.org`, `.org` adalah *top-level domain* atau TLD, `mozilla` adalah label yang berada tepat di bawahnya, dan `developer` adalah subdomain.

Setiap label DNS memiliki panjang maksimum 63 oktet. Bentuk tradisionalnya menggunakan huruf ASCII, angka, dan tanda hubung, dengan tanda hubung yang tidak ditempatkan pada awal atau akhir label. Keseluruhan nama membentuk hierarki administrasi, bukan sekadar rangkaian kata untuk pemasaran.

TLD berada di tingkat tertinggi setelah akar DNS. Root Zone Database IANA mencatat delegasi TLD generik seperti `.com` dan TLD kode negara seperti `.id` atau `.uk`, beserta organisasi pengelolanya. Istilah *second-level domain* atau SLD merujuk pada label tepat di sebelah kiri TLD, tetapi struktur registrasi berbeda antar-TLD. Pada `example.co.uk`, misalnya, nama yang dapat didaftarkan biasanya berada di bawah `co.uk`, sehingga pembacaan praktisnya tidak cukup hanya dengan menghitung posisi label.

Subdomain berada di bawah domain yang sudah dikendalikan registran. Pemilik `example.com` dapat membuat `docs.example.com`, `api.example.com`, atau tingkat yang lebih dalam tanpa mendaftarkan setiap subdomain sebagai domain baru, selama konfigurasi DNS dan kebijakan layanannya mendukung.

## Domain, hostname, URL, dan DNS

Domain name menyatakan posisi dalam ruang nama DNS. *Hostname* adalah nama domain yang digunakan untuk mengenali host atau layanan tertentu. Dalam praktik web, keduanya sering tampak sama, tetapi tidak setiap domain harus menunjuk langsung ke sebuah mesin tunggal.

[[References/HTTP\|URL]] memuat lokasi sumber daya secara lebih lengkap. Pada `https://www.example.com/docs/start`, `https` adalah skema, `www.example.com` adalah host, dan `/docs/start` adalah *path*. Domain hanya salah satu bagian dari URL.

DNS adalah sistem yang menyimpan dan menjawab informasi tentang domain. Saat browser membutuhkan `www.example.com`, resolver memeriksa cache lalu mencari jawaban melalui hierarki DNS sampai memperoleh rekaman dari sumber otoritatif. Perubahan rekaman tidak diterima semua resolver pada saat yang sama karena jawaban lama dapat tetap tersimpan sampai masa cache atau TTL berakhir.

## Pihak dalam registrasi domain

Registran adalah orang atau organisasi yang memegang hak penggunaan domain sesuai perjanjian registrasi. Registrar adalah perusahaan tempat registran mendaftarkan dan mengelola domain. Registry mengelola basis data domain di bawah TLD tertentu serta menerima permintaan registrasi dari registrar.

DNS host mengelola rekaman zona, sedangkan web host menyediakan komputasi, penyimpanan, dan alamat jaringan untuk website. Satu perusahaan dapat menawarkan beberapa peran sekaligus, tetapi pemisahan ini penting. Memindahkan hosting tidak selalu mengharuskan transfer registrar. Registran sering cukup mengganti *nameserver* atau rekaman DNS.

Registrasi bukan pembelian permanen. Registran membayar hak penggunaan selama masa tertentu dan harus memperbaruinya untuk mempertahankan domain. Harga, syarat transfer, periode registrasi, biaya pemulihan, dan kebijakan pembaruan mengikuti perjanjian dengan registrar serta aturan registry yang berlaku.

## Memilih dan mendaftarkan domain

Nama yang baik mudah dibaca, tidak mudah salah ketik, dan tidak menyerupai merek pihak lain. TLD perlu dipilih berdasarkan audiens, kebijakan registry, dan biaya total, bukan hanya harga tahun pertama. Beberapa TLD memiliki persyaratan kelayakan atau aturan penamaan tersendiri.

Sebelum registrasi, periksa ketersediaan dan data registrasi melalui layanan registrar atau RDAP. RDAP dibuat sebagai pengganti protokol WHOIS dan menyediakan akses terstruktur ke data registrasi domain. Sejak 28 Januari 2025, registry dan registrar gTLD tidak lagi diwajibkan menyediakan WHOIS, kecuali untuk `.com`, `.name`, dan `.post`, tetapi tetap diwajibkan menyediakan RDAP sesuai profil gTLD.

Data publik dapat dibatasi atau disamarkan karena kebijakan privasi. Hasil RDAP juga tidak membuktikan kepemilikan merek dan tidak menjamin bahwa sebuah nama aman digunakan secara hukum. Pemeriksaan merek tetap diperlukan untuk penggunaan bisnis.

Setelah registrasi, atur *nameserver* atau rekaman DNS sesuai layanan yang digunakan. Rekaman `A` dan `AAAA` menghubungkan nama ke alamat IPv4 dan IPv6, `CNAME` membuat alias, `MX` menentukan server email, sedangkan `TXT` sering dipakai untuk verifikasi dan kebijakan email. Detail rekaman dan proses resolusi dibahas di [[References/DNS\|DNS]].

## Siklus hidup dan kedaluwarsa

Registrasi biasanya berlangsung antara satu sampai sepuluh tahun. Registrar harus mengirim pengingat pembaruan sekitar satu bulan dan satu minggu sebelum masa berlaku berakhir, tetapi registran tetap perlu memantau tanggal kedaluwarsa dan memastikan data pembayaran serta kontak selalu mutakhir.

Domain yang kedaluwarsa tidak selalu langsung tersedia untuk orang lain. Alurnya bergantung pada TLD dan registrar. Untuk gTLD yang mengikuti kebijakan ICANN, domain yang telah dihapus dapat memasuki *Redemption Grace Period* selama 30 hari, kemudian berstatus `pendingDelete` selama lima hari sebelum dilepas untuk registrasi kembali. Pemulihan pada masa *redemption* dapat dikenai biaya tambahan.

Aktifkan pembaruan otomatis untuk domain penting, tetapi jangan menjadikannya satu-satunya pengaman. Simpan pengingat terpisah, periksa metode pembayaran, dan pastikan alamat email pemulihan tidak bergantung pada domain yang sama. Jika domain gagal, email berbasis domain tersebut juga dapat berhenti bekerja tepat ketika akses akun perlu dipulihkan.

## Domain internasional dan risiko kemiripan

*Internationalized Domain Names* atau IDN memungkinkan label menggunakan karakter di luar ASCII. Aplikasi menampilkan bentuk Unicode atau U-label kepada pengguna, sedangkan DNS menggunakan bentuk kompatibel ASCII atau A-label yang diawali `xn--` dan dihasilkan dengan algoritma Punycode.

Karakter dari sistem tulisan berbeda dapat terlihat sangat mirip. Kemiripan ini dapat dimanfaatkan untuk domain peniru dan *phishing*, sehingga pemeriksaan visual saja tidak cukup. Untuk domain bernilai tinggi, pertimbangkan registrasi defensif atas variasi yang paling masuk akal, pantau penerbitan sertifikat dan penyalahgunaan merek, serta ajarkan pengguna memeriksa asal tautan sebelum memasukkan kredensial.

## Keamanan pengelolaan domain

Pengambilalihan akun registrar dapat mengalihkan website dan email tanpa menyentuh server aplikasi. Gunakan registrar tepercaya, kata sandi unik, pengelola kata sandi, autentikasi multifaktor, dan alamat email pemulihan yang tetap dapat diakses jika domain bermasalah.

*Registrar lock* membantu mencegah perubahan, transfer, atau penghapusan tanpa izin. Untuk domain yang sangat penting, *registry lock* dapat memberi lapisan persetujuan tambahan jika registry dan registrar mendukungnya. Simpan kode pemulihan di luar akun email domain dan batasi jumlah orang yang memiliki akses administratif.

DNSSEC menambahkan tanda tangan digital pada data zona agar resolver yang memvalidasi dapat mendeteksi jawaban DNS yang telah dipalsukan atau diubah. DNSSEC tidak mengenkripsi kueri, tidak mencegah pengambilalihan akun registrar, dan tidak menggantikan [[References/HTTPS\|HTTPS]]. Penerapannya juga harus dikelola dengan benar saat mengganti penyedia DNS agar rantai kepercayaan tidak terputus.

*Phishing* sering memakai pemberitahuan pembaruan palsu, nama pengirim yang menyerupai registrar, atau domain yang tampak serupa. ICANN tidak mengelola registrasi pengguna secara langsung dan tidak menagih registran untuk pembaruan domain. Verifikasi pesan melalui panel atau kanal resmi registrar, bukan melalui tautan di email yang mencurigakan.

## Checklist operasional

1. Pilih registrar dengan dukungan MFA, kontrol akses, riwayat keamanan yang baik, dan biaya pembaruan yang jelas.
2. Catat registran, registrar, registry, penyedia DNS, serta kontak pemulihan untuk setiap domain.
3. Aktifkan *registrar lock*, MFA, pembaruan otomatis, dan notifikasi perubahan.
4. Gunakan alamat email pemulihan yang tidak bergantung pada domain yang dikelola.
5. Audit *nameserver*, rekaman DNS, akun pengguna, dan tanggal kedaluwarsa secara berkala.
6. Aktifkan DNSSEC jika seluruh rantai penyedia mendukung dan proses pergantian kunci dipahami.
7. Dokumentasikan prosedur transfer, pergantian penyedia DNS, dan pemulihan insiden sebelum diperlukan.

## Lihat juga

- [[References/DNS\|DNS]]: hierarki resolver, server akar, TLD, server otoritatif, dan cache.
- [[References/Web Hosting\|Web Hosting]]: layanan komputasi dan penyimpanan untuk menerbitkan website.
- [[References/HTTP\|HTTP]]: protokol aplikasi yang digunakan browser dan server web.
- [[References/HTTPS\|HTTPS]]: perlindungan koneksi HTTP dengan TLS.
- [[References/How Does Internet Work\|How Does Internet Work]]: alamat IP, DNS, dan dasar komunikasi Internet.

## Sumber

- [MDN: What is a Domain Name?](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name): definisi, struktur label, subdomain, registrasi, dan cache DNS.
- [IANA: Root Zone Database](https://www.iana.org/domains/root/db): delegasi TLD generik dan kode negara.
- [ICANN: Domain Name Industry](https://www.icann.org/resources/pages/domain-name-industry-2017-06-20-en): peran registran, registrar, reseller, DNS host, web host, dan registry.
- [ICANN: Life Cycle of a Typical gTLD Domain Name](https://www.icann.org/en/contracted-parties/accredited-registrars/resources/gtld-lifecycle): tahapan siklus hidup gTLD.
- [ICANN: Information for RDAP Users](https://www.icann.org/en/contracted-parties/registry-operators/registration-data-access-protocol/information-for-rdap-users-31-08-2018-en): RDAP sebagai pengganti WHOIS dan kewajiban layanan gTLD.
- [ICANN: Protect Your Domain Name](https://www.icann.org/en/blogs/details/do-you-have-a-domain-name-heres-what-you-need-to-know-30-4-2020-en): MFA, registrar lock, phishing, dan DNSSEC.
- [ICANN: Domain Name Renewals and Expiration](https://www.icann.org/resources/pages/domain-name-renewal-expiration-faqs-2018-12-07-en): pembaruan, kedaluwarsa, redemption, dan `pendingDelete`.
- [RFC 5890: IDNA Definitions and Framework](https://www.rfc-editor.org/rfc/rfc5890.html): A-label, U-label, Punycode, batas label, dan risiko karakter serupa.
