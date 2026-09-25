---
{"dg-publish":true,"dg-path":"IIS.md","permalink":"/iis/","title":"IIS","hideInFiletree":true,"tags":["references","programming","performance","security"],"noteIcon":"","dg-note-properties":{"title":"IIS","aliases":["MS IIS","Microsoft IIS","Internet Information Services"],"categories":["Software Systems"],"type":"reference","tags":["references","programming","performance","security"],"sources":["_raw/articles/iis-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

IIS (Internet Information Services) adalah web server Microsoft untuk Windows Server. Berbeda dengan [[References/Nginx\|Nginx]] atau [[References/Apache\|Apache]] yang lintas platform, IIS hanya berjalan di Windows dan terintegrasi dengan ekosistem Microsoft seperti Active Directory dan Windows authentication.

Dalam keluarga [[References/Web Servers\|Web Servers]], IIS menyajikan konten statis, menjalankan aplikasi dinamis, dan mengakhiri koneksi TLS. Peran ini menempatkannya pada jalur yang sama dengan [[References/Web Hosting\|Web Hosting]] dan proses [[References/Deployment\|Deployment]] aplikasi Windows.

## Workload yang didukung

- **Konten statis**: menyajikan file HTML, gambar, dan aset langsung.
- **ASP.NET**: integrasi native dengan runtime .NET; aplikasi berjalan dalam application pool yang mengisolasi proses antar situs.
- **PHP**: dijalankan lewat FastCGI, prosesnya dikelola terpisah dari worker IIS sehingga kapasitas pool dan backend PHP perlu diselaraskan.

## Fitur: autentikasi, TLS, URL rewriting

IIS mendukung beberapa skema autentikasi (anonymous, basic, Windows, client certificate) yang dipilih per situs sesuai kebutuhan. Fleksibilitas ini bukan keamanan otomatis: skema yang lemah tanpa [[References/HTTPS\|HTTPS]] yang benar tetap membocorkan kredensial.

TLS memerlukan sertifikat, private key, nama host, dan versi protokol yang dikonfigurasi secara eksplisit. Modul URL Rewrite mengatur routing dan redirect berbasis aturan, tetapi aturan yang longgar dapat mengekspos path internal atau menciptakan loop redirect.

## Manajemen dan operasi

Administrasi lewat IIS Manager (GUI) atau command line (`appcmd`, cmdlet PowerShell WebAdministration). GUI mempermudah setup awal, sedangkan skrip PowerShell membuat konfigurasi deployment dapat direproduksi dan diaudit.

Perbarui Windows Server, runtime .NET/PHP, dan modul IIS secara teratur. Batasi hak tulis konfigurasi, berikan akses filesystem hanya pada direktori yang diperlukan, dan uji setiap situs setelah perubahan seperti praktik pada [[References/Apache\|Apache]]: periksa binding, sertifikat, dan log sebelum menyimpulkan konfigurasi sudah benar.

## Sumber

- [IIS Documentation](https://learn.microsoft.com/en-us/iis/)
