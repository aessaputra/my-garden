---
{"dg-publish":true,"dg-path":"Web Servers.md","permalink":"/web-servers/","title":"Web Servers","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"Web Servers","categories":["Web Technologies"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/web-servers-user-summary-2026-09-11.md"],"created":"2026-09-11","updated":"2026-09-12","confidence":"medium"}}
---

Web server adalah perangkat lunak yang menerima request HTTP dari client dan mengembalikan response HTTP. Client umumnya browser atau aplikasi lintas jaringan, sedangkan server yang dibahas di sini adalah server perangkat lunak yang dijalankan pada host, bukan mesin fisiknya.

## Peran utama

Web server dapat menyajikan file statis secara langsung, misalnya HTML, CSS, JavaScript, gambar, dan font. Ia juga dapat meneruskan request dinamis ke runtime atau layanan aplikasi di belakangnya dan mengembalikan hasilnya.

Istilah origin server berarti layanan yang menjadi sumber awal response untuk request tertentu. Layanan ini dapat berupa web server, aplikasi, fungsi serverless, atau object storage dengan endpoint HTTP.

Web server pada umumnya mengikuti pola request/response HTTP. [[References/HTTP\|HTTP]] menjelaskan protokolnya, sedangkan [[References/HTTPS\|HTTPS]] menjelaskan perlindungan transport setelah handshake TLS selesai.

## Fungsi deployment

Web server dapat menjadi bagian dari arsitektur yang lebih besar, bukan satu-satunya komponen. Fungsi terkait dapat meliputi:

- **Hosting dan routing**: memilih virtual host atau handler berdasarkan host, path, header, atau aturan lain.
- **Reverse proxying**: meneruskan request ke layanan backend berdasarkan konfigurasi.
- **Load distribution**: mengirim request ke beberapa instance backend menurut kebijakan yang ditetapkan.
- **TLS termination**: menjadi endpoint TLS lalu mempertahankan koneksi aman yang diperlukan ke backend.
- **Caching dan kompresi**: menyimpan response yang dapat digunakan kembali dan menyesuaikan representasi.
- **Proteksi dasar**: pembatasan akses, rate limiting, logging, dan pemeriksaan header.

[[References/Cache-Control\|Cache-Control]] mengatur perilaku cache yang mematuhi aturan HTTP. [[References/Caching\|Caching]] menjelaskan reuse di luar HTTP, [[References/Client Side Caching\|Client Side Caching]] untuk perangkat pengguna, dan [[References/Deployment\|Deployment]] untuk peran server dalam release.

[[References/Apache\|Apache HTTP Server]], [[References/Nginx\|Nginx]], [[References/Caddy\|Caddy]], dan IIS adalah contoh perangkat lunak web server. Daftar tersebut bukan rekomendasi universal atau jaminan kesesuaian untuk setiap workload. Pilihan bergantung pada kebutuhan statis dan dinamis, traffic, operasional, dan ekosistem tim.

## Batas arsitektur

Interaksi dengan database tidak dilakukan langsung oleh setiap permintaan HTTP pada web server semata. Query umumnya dijalankan oleh aplikasi atau layanan backend yang kemudian diminta melalui proxy, FastCGI, modul, atau koneksi internal. Karakteristik koneksi dan transaksi tetap ditentukan oleh aplikasi dan database.

Keamanan koneksi juga bergantung pada konfigurasi, bukan sekadar kehadiran web server. Menghentikan TLS di reverse proxy berarti proxy menjadi endpoint TLS tersendiri, sehingga koneksi lanjutan ke backend perlu diamankan dan dipercaya sesuai kebutuhan.

[[References/Web Security Knowledge\|Web Security Knowledge]] memetakan kontrol keamanan aplikasi dan infrastruktur. TLS tidak memperbaiki kerentanan aplikasi, dan web server tidak menggantikan proteksi pada lapisan aplikasi.

## Sumber

Paste pengguna sebagai konteks awal. Dokumentasi berikut menjadi rujukan pemeriksaan implementasi, bukan snapshot yang diverifikasi pada ingest ini:

- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/): server HTTP Apache.
- [NGINX Documentation](https://nginx.org/en/docs/): server dan proxy NGINX.
- [IIS documentation](https://learn.microsoft.com/en-us/iis/): layanan web IIS.
