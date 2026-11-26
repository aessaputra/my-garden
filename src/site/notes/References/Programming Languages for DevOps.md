---
{"dg-publish":true,"dg-path":"Programming Languages for DevOps.md","permalink":"/programming-languages-for-dev-ops/","title":"Programming Languages for DevOps","hideInFiletree":true,"tags":["references","devops","programming"],"noteIcon":"","dg-note-properties":{"title":"Programming Languages for DevOps","aliases":["Bahasa Pemrograman untuk DevOps"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","programming"],"sources":["_raw/articles/programming-language-for-devops-csfyi-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"low"}}
---

Panduan ini membagi bahasa yang dibutuhkan praktisi DevOps menjadi dua kategori terpisah: bahasa pemrograman untuk menulis tool dan otomasi, serta shell language untuk menjalankan perintah dan skrip di host. Rekomendasinya adalah Python atau Go untuk kategori pertama, dan Bash atau PowerShell untuk kategori kedua. Pemilihan akhir ditentukan oleh lingkungan yang dioperasikan, bukan oleh peringkat bahasa secara umum.

## Dua kategori, bukan satu daftar

Artikel menempatkan bahasa pemrograman dan shell sebagai kebutuhan yang berbeda. Shell dipakai untuk merangkai perintah dan skrip pendek di mesin yang dioperasikan; bahasa pemrograman dipakai ketika skrip perlu menjadi tool yang lebih besar, misalnya CLI, agent, atau integrasi API. Memisahkan keduanya mencegah kesalahan umum berupa memaksakan satu bahasa untuk semua pekerjaan.

## Python

Artikel menyebut Python sebagai pilihan termudah untuk pemula karena sintaks dan dukungan lintas platform. Bukti yang dipakai adalah pemakaiannya di tool mapan: Ansible dan SaltStack untuk configuration management, serta AWS CLI dan Azure CLI. Itu bukti adopsi ekosistem, bukan keunggulan teknis, karena artikel tidak membandingkan performa atau keandalan. Detail kemampuan dan batas eksekusinya ada di [[References/Python\|Python]].

Catatan versinya usang: kalimat "Python 2 retired akhir Januari 2020" ditulis seolah masih akan terjadi, padahal artikel terbit Maret 2023. Python 2 memang sudah tidak didukung, jadi pilih Python 3 dan jangan jadikan artikel ini rujukan versi.

## Go

Go digambarkan sebagai bahasa yang lahir untuk jaringan dan sistem terdistribusi, lalu menyebar ke DevOps karena container. Bukti konkretnya kuat dan mudah diperiksa: Docker, Kubernetes, dan banyak tool container lain ditulis dalam Go. Bagi praktisi yang harus membaca, menambal, atau membuat extension untuk tool tersebut, kemampuan membaca Go bukan opsional.

Alasan kedua adalah konkurensi dan garbage collection, yang disebut cocok untuk sistem yang dapat diskalakan. Artikel tidak memberikan angka, jadi ini tetap klaim kualitatif. Mekanisme goroutine dan batasnya dibahas di [[References/Go\|Go]] dan pada catatan tersendiri [[References/Golang untuk DevOps\|Golang untuk DevOps]] yang menekankan penggunaan Go untuk membangun tool internal.

## Shell: Bash dan PowerShell

Bash direkomendasikan untuk lingkungan Linux, PowerShell Core untuk Windows, dengan catatan PowerShell Core tersedia lintas platform dan dapat dipasang di Linux. Pembagian ini mengikuti sistem operasi host, bukan preferensi pribadi, dan itulah bagian artikel yang paling tahan waktu.

Implikasinya untuk pekerjaan provisioning dan deployment: perintah yang dijalankan pada host target tetap ditulis dalam shell milik host tersebut, sementara tool yang mengorkestrasi keduanya bisa ditulis dalam Python atau Go. Hubungan ini terlihat pada [[References/Provisioning\|Provisioning]], di mana bootstrap dan konfigurasi awal biasanya berupa skrip shell, dan pada [[References/Deployment\|Deployment]], di mana pipeline memanggil perintah shell di runner.

## Batas bukti

Panduan ini opini satu penulis, Maret 2023, tanpa benchmark dan tanpa versi tool. Yang layak dipertahankan kerangkanya: pisahkan bahasa tool dari shell host, lalu ikuti lingkungan kerja; klaim "Go lebih baik" atau "Python lebih mudah" bukan hasil pengukuran.

Dokumentasi resmi Python dan Go, serta dokumentasi Bash dan PowerShell, adalah rujukan yang lebih tepat untuk keputusan versi dan fitur.

## Sumber

- [Guide to Picking a Language for DevOps](https://cs.fyi/guide/programming-language-for-devops), cs.fyi, 15 Maret 2023.
