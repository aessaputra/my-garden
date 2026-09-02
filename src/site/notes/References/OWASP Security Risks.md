---
{"dg-publish":true,"dg-path":"OWASP Security Risks.md","permalink":"/owasp-security-risks/","title":"OWASP Security Risks","hideInFiletree":true,"tags":["references","security","programming","testing","devops"],"dg-note-properties":{"title":"OWASP Security Risks","category":"references","tags":["references","security","programming","testing","devops"],"sources":["_raw/articles/owasp-top-10-2025-expanded.md"],"created":"2026-08-29","updated":"2026-08-29","confidence":"high"}}
---

Open Worldwide Application Security Project (OWASP) menerbitkan OWASP Top 10 sebagai dokumen kesadaran bagi pengembang dan praktisi keamanan aplikasi web. Edisi 2025 merangkum konsensus luas mengenai sepuluh kategori risiko yang dinilai paling serius, tetapi daftar ini bukan standar verifikasi lengkap dan bukan pengganti penilaian risiko yang disesuaikan dengan organisasi.

Teks ringkas yang menyebut serangan injeksi, autentikasi rusak, paparan data, dan dependensi rentan menggambarkan istilah dari beberapa edisi lama. Dalam OWASP Top 10:2025, *Broken Authentication* menjadi *Authentication Failures*, sedangkan *Sensitive Data Exposure* diperlakukan sebagai dampak yang dapat muncul dari *Cryptographic Failures* atau *Broken Access Control*. Kategori *Vulnerable and Outdated Components* diperluas menjadi *Software Supply Chain Failures*.

## Cara membaca OWASP Top 10

OWASP Top 10 adalah titik awal untuk membangun kesadaran dan prioritas, bukan daftar seluruh kelemahan yang mungkin ada pada aplikasi. Metodologi 2025 memakai data pengujian aplikasi, pemetaan CWE, data CVE dan CVSS, serta survei komunitas untuk memasukkan risiko yang mungkin belum dapat diuji secara luas. Delapan kategori dipilih dari data dan dua kategori berasal dari survei komunitas karena hasil pengujian cenderung mencerminkan kelemahan yang sudah dapat dideteksi oleh alat dan metode yang tersedia.

Peringkat global tidak otomatis menentukan urutan perbaikan pada setiap sistem. OWASP menyatakan bahwa agen ancaman, eksposur aplikasi, dan dampak bisnis dapat berbeda walaupun dua organisasi memakai perangkat lunak yang sama. Tim perlu menggabungkan Top 10 dengan inventaris aset, klasifikasi data, model ancaman, arsitektur, konteks bisnis, dan persyaratan regulasi.

## OWASP Top 10:2025

### A01: Broken Access Control

Kontrol akses rusak terjadi ketika pengguna dapat bertindak di luar izin yang diberikan, misalnya membaca data milik pengguna lain, memanggil fungsi administratif, mengubah identifier objek, atau mengakses endpoint API tanpa pemeriksaan otorisasi yang memadai. Kategori 2025 juga mencakup CSRF, SSRF, *forced browsing*, manipulasi metadata, serta konfigurasi CORS yang memberi akses kepada origin yang tidak semestinya.

Pemeriksaan otorisasi harus dijalankan pada kode sisi server atau API tepercaya, bukan hanya melalui antarmuka pengguna. Praktik pencegahannya mencakup *deny by default*, prinsip hak akses minimum, pemeriksaan kepemilikan objek, penggunaan mekanisme kontrol akses yang konsisten, pembatasan laju, invalidasi sesi, pencatatan kegagalan, serta pengujian unit dan integrasi untuk aturan otorisasi.

### A02: Security Misconfiguration

Kesalahan konfigurasi menempati posisi kedua pada edisi 2025 dan mencakup konfigurasi aplikasi, kerangka kerja, server, layanan awan, izin, fitur, serta antarmuka yang tidak aman. Risiko ini dapat muncul melalui akun atau kata sandi bawaan, layanan yang tidak diperlukan, pesan galat yang terlalu rinci, izin penyimpanan yang terbuka, dan header keamanan yang tidak memadai.

Organisasi perlu memakai proses *hardening* yang berulang dan terotomasi, menghapus fitur yang tidak dipakai, menggunakan kredensial berbeda pada setiap lingkungan, serta memverifikasi efektivitas konfigurasi di seluruh lingkungan. Konfigurasi juga perlu diperiksa setelah pembaruan infrastruktur, dependensi, atau layanan terkelola karena fitur keamanan baru dapat dinonaktifkan atau belum dikonfigurasi dengan aman.

### A03: Software Supply Chain Failures

Kategori ini memperluas risiko komponen rentan dan usang menjadi kegagalan pada seluruh proses pembangunan, distribusi, dan pembaruan perangkat lunak. Cakupannya meliputi dependensi langsung dan transitif, sistem operasi, server, basis data, IDE, ekstensi pengembang, repositori kode, CI/CD, registri kontainer, repositori artefak, serta integrasi pihak ketiga.

Pencegahan memerlukan inventaris komponen dan *Software Bill of Materials* (SBOM), pemantauan kerentanan berkelanjutan, penggunaan sumber paket tepercaya, verifikasi tanda tangan dan asal artefak, pembaruan berbasis risiko, serta penghapusan dependensi yang tidak diperlukan. Repositori kode, server pembangunan, workstation pengembang, rahasia CI/CD, dan artefak rilis perlu dilindungi dengan kontrol akses, MFA, pemisahan tugas, log yang tahan manipulasi, serta proses perubahan yang dapat diaudit. OWASP SCVS menyediakan kerangka kegiatan dan kontrol yang dapat diterapkan bertahap untuk mengurangi risiko rantai pasok perangkat lunak.

### A04: Cryptographic Failures

Kegagalan kriptografi mencakup ketiadaan kriptografi, algoritma yang kurang kuat, kebocoran kunci, entropi yang tidak memadai, validasi rantai sertifikat yang keliru, dan penggunaan protokol yang tidak aman. Dampaknya dapat berupa paparan data sensitif atau pengambilalihan sistem, sehingga istilah lama *Sensitive Data Exposure* lebih tepat dipahami sebagai akibat dari kegagalan kontrol yang mendasarinya.

Tim perlu mengklasifikasikan data, meminimalkan retensi, mengenkripsi data sensitif saat transit dan tersimpan, serta memakai implementasi kriptografi yang tepercaya dan terpelihara. Kunci harus dibuat secara acak, disimpan dan dirotasi dengan benar, dibatasi aksesnya, serta tidak dimasukkan ke repositori kode. Kata sandi perlu disimpan dengan fungsi hash adaptif dan garam, bukan melalui enkripsi reversibel atau fungsi hash cepat.

### A05: Injection

Injeksi terjadi ketika masukan tidak tepercaya dikirim ke interpreter dan sebagian masukan tersebut diperlakukan sebagai perintah atau struktur yang dapat dieksekusi. Bentuknya mencakup injeksi SQL, NoSQL, perintah sistem operasi, LDAP, ORM, expression language, dan *cross-site scripting*.

Pertahanan utama adalah memisahkan data dari perintah dengan API aman, kueri berparameter, atau antarmuka yang tidak menyusun perintah melalui penggabungan string. Validasi sisi server dengan daftar nilai yang diizinkan dan *escaping* yang sesuai konteks dapat melengkapi pertahanan ketika pemisahan penuh tidak memungkinkan, tetapi tidak menggantikan parameterisasi. OWASP menyarankan tinjauan kode dan pengujian otomatis terhadap parameter, header, URL, kuki, JSON, SOAP, dan XML; SAST, DAST, IAST, serta *fuzzing* dapat digunakan sebagai lapisan deteksi di pipeline CI/CD.

### A06: Insecure Design

Desain tidak aman berarti kontrol yang dibutuhkan tidak dirancang atau rancangan kontrolnya tidak efektif. Masalah ini berbeda dari cacat implementasi: implementasi yang benar tidak dapat menggantikan kontrol yang sejak awal tidak dibuat untuk menghadapi skenario serangan tertentu.

Pencegahan dimulai sebelum penulisan kode melalui persyaratan keamanan, pemodelan ancaman, pola desain aman, peninjauan arsitektur, serta analisis alur normal dan alur kegagalan. Tim perlu memasukkan kontrol keamanan ke dalam cerita pengguna, menyusun kasus penggunaan dan penyalahgunaan, menguji alur kritis terhadap model ancaman, serta memisahkan lapisan dan penyewa sesuai kebutuhan perlindungan.

### A07: Authentication Failures

Kegagalan autentikasi memungkinkan sistem menganggap identitas yang salah sebagai pengguna yang sah. Contohnya mencakup *credential stuffing*, serangan brute force, kredensial bawaan atau lemah, pemulihan akun yang tidak aman, MFA yang tidak efektif, session fixation, token yang tidak dibatalkan, serta validasi `aud`, `iss`, atau cakupan token yang tidak tepat.

OWASP menganjurkan MFA, pemeriksaan terhadap kredensial yang telah bocor, penghapusan kredensial bawaan, pembatasan percobaan masuk, dan pencatatan serta peringatan untuk aktivitas otomatis. Aplikasi sebaiknya memakai pengelola sesi sisi server yang menghasilkan identifier acak baru setelah login, menyimpannya dalam kuki aman, dan membatalkannya pada logout serta ketika waktu diam atau masa absolut berakhir.

### A08: Software or Data Integrity Failures

Kategori ini berfokus pada kegagalan menjaga batas kepercayaan dan memverifikasi integritas perangkat lunak, kode, atau artefak data. Contohnya mencakup pembaruan tanpa verifikasi, dependensi dari repositori yang tidak dipercaya, pipeline CI/CD tanpa pemeriksaan integritas, dan deserialisasi data yang dapat dimodifikasi penyerang.

Kontrol yang relevan mencakup tanda tangan digital, repositori tepercaya, peninjauan perubahan kode dan konfigurasi, pemisahan tugas serta kontrol akses pada CI/CD, dan pemeriksaan integritas data terserialisasi sebelum dipakai. Pada rantai rilis, organisasi perlu menjaga provenance artefak dan mempromosikan artefak yang sama antarlingkungan agar hasil produksi tidak berasal dari pembangunan ulang yang tidak setara.

### A09: Security Logging and Alerting Failures

Aplikasi tidak dapat mendeteksi dan menangani serangan secara memadai bila kejadian keamanan tidak dicatat, dipantau, atau menghasilkan peringatan yang dapat ditindaklanjuti. Kegagalan meliputi login gagal yang tidak dicatat, log tanpa perlindungan integritas, penyimpanan hanya secara lokal, ketiadaan ambang peringatan, data sensitif di dalam log, log injection, dan *playbook* respons yang tidak lengkap.

Aplikasi perlu mencatat keberhasilan dan kegagalan kontrol keamanan dengan konteks yang cukup, memakai format yang dapat diproses sistem manajemen log, melindungi jejak audit dari perubahan, dan menetapkan peringatan untuk perilaku mencurigakan. Tim keamanan perlu menguji apakah pemindaian DAST, percobaan brute force, pelanggaran kontrol akses, dan pola serangan lain benar-benar memicu peringatan serta proses eskalasi.

### A10: Mishandling of Exceptional Conditions

Kategori baru pada 2025 ini mencakup penanganan galat yang tidak tepat, kesalahan logika, kondisi *fail open*, parameter yang hilang, nilai kembalian yang tidak diperiksa, kebocoran informasi melalui pesan galat, dan keadaan abnormal lain yang tidak ditangani dengan aman. Kondisi tersebut dapat memicu kerusakan status, kebocoran data, transaksi tidak konsisten, kehabisan sumber daya, atau pengabaian pemeriksaan autentikasi dan otorisasi.

Aplikasi perlu menangani galat dekat dengan sumbernya, mengembalikan pesan yang aman kepada pengguna, mencatat konteks internal secara terkontrol, dan mengeluarkan peringatan jika pola kegagalan menunjukkan serangan. Transaksi yang gagal harus dipulihkan secara konsisten dan kontrol keamanan harus *fail closed*; pembatasan laju, kuota sumber daya, penanganan galat terpusat, pengujian stres, analisis statis, serta pengujian penetrasi membantu menemukan jalur kegagalan sebelum produksi.

## Dari daftar risiko menjadi program keamanan

OWASP Top 10 memberi bahasa bersama untuk membahas risiko, tetapi tidak cukup sebagai daftar pemeriksaan tunggal. ASVS menyediakan persyaratan terukur untuk merancang dan menguji kontrol teknis aplikasi web, serta dapat digunakan sebagai metrik, panduan implementasi, dan dasar persyaratan pengadaan. Versi stabil ASVS 5.0.0 menyediakan identifier kebutuhan berversi agar persyaratan dalam desain, tiket, pengujian, dan laporan tetap dapat ditelusuri.

WSTG melengkapi ASVS dengan skenario pengujian keamanan aplikasi dan layanan web untuk pengembang, penguji penetrasi, dan organisasi. Referensi skenario sebaiknya menyertakan versi karena identifier dan konten pada jalur `stable` atau `latest` dapat berubah.

Program keamanan yang memadai menempatkan kontrol pada seluruh siklus pengembangan:

1. Inventarisasi aplikasi, API, data, dependensi, artefak, dan pemilik risikonya.
2. Tetapkan persyaratan keamanan dari ASVS berdasarkan eksposur dan dampak bisnis.
3. Lakukan pemodelan ancaman dan tinjauan desain sebelum implementasi.
4. Terapkan tinjauan kode, SAST, pengujian dependensi, pemindaian rahasia, serta kontrol integritas di CI/CD.
5. Jalankan DAST, *fuzzing*, pengujian akses, pengujian autentikasi, dan skenario WSTG pada lingkungan yang representatif.
6. Pantau log dan peringatan di produksi, latih respons insiden, lalu gunakan temuan operasional untuk memperbarui model ancaman dan pengujian.

Tidak satu pun alat dapat membuktikan bahwa aplikasi bebas dari seluruh risiko. Pengujian otomatis efektif untuk pola yang dapat dikenali, sedangkan cacat logika bisnis, penyalahgunaan alur, batas kepercayaan yang keliru, dan desain kontrol yang hilang memerlukan peninjauan manusia serta pengujian berbasis konteks. Hasil pemindaian perlu ditriase berdasarkan kemungkinan eksploitasi, dampak bisnis, eksposur, dan kontrol kompensasi, bukan hanya berdasarkan posisi kategori pada Top 10.

## Kesimpulan

OWASP Top 10:2025 mencakup kontrol akses, konfigurasi, rantai pasok, kriptografi, injeksi, desain, autentikasi, integritas, pencatatan dan peringatan, serta penanganan kondisi abnormal. Daftar ini paling berguna sebagai bahan kesadaran dan pemetaan risiko awal. Untuk penerapan yang dapat diverifikasi, organisasi perlu menghubungkannya dengan persyaratan ASVS, skenario WSTG, kontrol rantai pasok SCVS, model ancaman, serta pemantauan dan perbaikan berkelanjutan.

## Lihat juga

- [[References/Content Security Policy\|Content Security Policy]]
- [[References/HTTPS\|HTTPS]]
- [[References/CORS\|CORS]]
- [[References/Package Managers\|Package Managers]]
- [[GitHub\|GitHub]]

## Sumber

- [OWASP Top 10:2025](https://owasp.org/Top10/2025): daftar resmi sepuluh kategori risiko aplikasi web pada edisi 2025.
- [Introduction: OWASP Top 10:2025](https://owasp.org/Top10/2025/0x00_2025-Introduction): perubahan kategori, metodologi, struktur CWE, dan batas data OWASP Top 10:2025.
- [What are Application Security Risks?: OWASP Top 10:2025](https://owasp.org/Top10/2025/0x02_2025-What_are_Application_Security_Risks): model risiko, konteks organisasi, faktor data, dan metode pemeringkatan.
- [A01:2025 Broken Access Control](https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control): ruang lingkup, contoh, dan mitigasi kegagalan kontrol akses.
- [A03:2025 Software Supply Chain Failures](https://owasp.org/Top10/2025/A03_2025-Software_Supply_Chain_Failures): dependensi, SBOM, CI/CD, artefak, dan perlindungan rantai pasok.
- [A05:2025 Injection](https://owasp.org/Top10/2025/A05_2025-Injection): injeksi terhadap interpreter, parameterisasi, validasi, dan pengujian keamanan.
- [A07:2025 Authentication Failures](https://owasp.org/Top10/2025/A07_2025-Authentication_Failures): kredensial, MFA, serangan otomatis, token, dan manajemen sesi.
- [A09:2025 Security Logging and Alerting Failures](https://owasp.org/Top10/2025/A09_2025-Security_Logging_and_Alerting_Failures): pencatatan kejadian keamanan, integritas log, peringatan, dan respons.
- [A10:2025 Mishandling of Exceptional Conditions](https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions): penanganan galat, fail closed, transaksi, dan kondisi abnormal.
- [OWASP Application Security Verification Standard](https://owasp.org/www-project-application-security-verification-standard): persyaratan terukur untuk desain dan verifikasi kontrol keamanan aplikasi.
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide): kerangka dan skenario pengujian keamanan aplikasi serta layanan web.
- [OWASP Software Component Verification Standard](https://owasp.org/www-project-software-component-verification-standard): kontrol bertahap untuk mengurangi risiko rantai pasok perangkat lunak.
- [A02:2025 Security Misconfiguration](https://owasp.org/Top10/2025/A02_2025-Security_Misconfiguration): kesalahan konfigurasi, hardening, dan verifikasi konfigurasi lintas lingkungan.
- [A04:2025 Cryptographic Failures](https://owasp.org/Top10/2025/A04_2025-Cryptographic_Failures): klasifikasi data, algoritma, pengelolaan kunci, hash kata sandi, dan perlindungan data.
- [A06:2025 Insecure Design](https://owasp.org/Top10/2025/A06_2025-Insecure_Design): persyaratan keamanan, pemodelan ancaman, desain aman, dan siklus pengembangan.
- [A08:2025 Software or Data Integrity Failures](https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures): verifikasi integritas perangkat lunak, data, pembaruan, dan pipeline CI/CD.
