---
{"dg-publish":true,"dg-path":"SOAP.md","permalink":"/soap/","title":"SOAP","hideInFiletree":true,"tags":["references","programming","backend","http","xml"],"noteIcon":"","dg-note-properties":{"title":"SOAP","categories":["APIs"],"type":"reference","status":"evergreen","source_type":"standards-and-official-docs","tags":["references","programming","backend","http","xml"],"sources":["_raw/articles/soap-user-summary-2026-09-10.md","https://www.w3.org/TR/soap12-part1/","https://www.w3.org/TR/soap12-part0/","https://www.w3.org/TR/wsdl20/"],"created":"2026-09-10","updated":"2026-09-10"}}
---

SOAP adalah protokol pesan XML untuk pertukaran data antarsistem, distandarkan W3C pada SOAP 1.2 (Recommendation). Nama historisnya "Simple Object Access Protocol"; sejak SOAP 1.2 W3C tidak lagi memperlakukan SOAP sebagai akronim yang diperluas.

## Struktur pesan

Pesan SOAP adalah envelope XML dengan dua bagian: Header opsional berisi blok fitur seperti routing atau metadata transaksi, dan Body wajib berisi payload atau elemen `Fault` yang membakukan laporan kesalahan. SOAP 1.2 memindahkan aturan encoding data keluar dari spesifikasi inti, sehingga serialisasi payload tidak lagi diwajibkan oleh inti.

## Transport dan kontrak

SOAP tidak terikat satu protokol transport. Binding HTTP yang paling umum mengirim pesan lewat POST, dan binding lain seperti SMTP juga terdefinisi. Kontrak layanan dideskripsikan dengan WSDL (W3C Recommendation versi 2.0, praktik umum masih WSDL 1.1) yang memuat operasi, tipe pesan berbasis XML Schema, dan binding. Ekstensi WS-* seperti WS-Security menambahkan keamanan pesan di luar transport. [[HTTPS protects transit, not application logic\|HTTPS protects transit, not application logic]] tetap relevan karena WS-Security bekerja pada lapisan pesan.

## Posisi dibanding gaya lain

Kontrak formal dan tooling stub generator membedakan SOAP dari [[References/REST\|REST]] yang mengandalkan semantik [[References/HTTP\|HTTP]] dan representasi ringan. Sebagai gambaran, Richardson Maturity Model menempatkan web service SOAP pada Level 0: satu URI dengan POST untuk semua operasi, sebagaimana dicatat pada [[_raw/articles/backend-development-evidence-addendum-2026-09-07\|evidence addendum Backend Development]]. SOAP tetap banyak dipakai pada integrasi enterprise dan sistem legacy. [[References/OWASP Security Risks\|OWASP Security Risks]] mencatat SOAP dan XML termasuk permukaan uji keamanan API.

[[References/APIs\|APIs]] menempatkan SOAP sebagai salah satu bentuk HTTP API berformat XML, bersebelahan dengan [[References/JSON APIs\|JSON APIs]] pada lapisan representasi.

## Sumber

- [SOAP Version 1.2 Part 1: Messaging Framework](https://www.w3.org/TR/soap12-part1/): envelope, Header, Body, Fault, binding.
- [SOAP Version 1.2 Part 0: Primer](https://www.w3.org/TR/soap12-part0/): penjelasan pengantar dan status nama SOAP.
- [WSDL 2.0 Core Language](https://www.w3.org/TR/wsdl20/): deskripsi kontrak layanan.
