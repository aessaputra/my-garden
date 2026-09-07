---
{"dg-publish":true,"dg-path":"Android.md","permalink":"/android/","title":"Android","hideInFiletree":true,"tags":["references","programming","architecture","security"],"noteIcon":"","dg-note-properties":{"title":"Android","categories":["Frameworks"],"tags":["references","programming","architecture","security"],"sources":["_raw/articles/android-research-packet.md"],"created":"2026-09-07","updated":"2026-09-07","confidence":"high"}}
---

Android adalah sistem operasi seluler berbasis Linux untuk ponsel, tablet,
foldable, dan perangkat sejenis.
[Fundamentals](https://developer.android.com/guide/components/fundamentals)
menyatakan aplikasi ditulis dengan Kotlin, Java, atau C++.

Toolchain SDK mengompilasi kode, data, dan resource menjadi paket instalasi
atau publikasi. Rincian runtime ada di
[[Android ART executes DEX bytecode, not Java SE class files\|Android ART executes DEX bytecode, not Java SE class files]], konteks lintas
platform ada di [[References/Mobile Apps\|Mobile Apps]].

## Komponen aplikasi

Sistem mengenal empat komponen: Activity, Service, Broadcast receiver, dan
Content provider. Masing-masing adalah pintu masuk dengan lifecycle berbeda.

Activity menampilkan satu layar dan memfasilitasi interaksi pengguna.
[Fundamentals](https://developer.android.com/guide/components/fundamentals)
mencatat aplikasi lain dapat memulai activity yang diizinkan, misalnya berbagi
gambar.

Service berjalan tanpa UI untuk operasi lama atau kerja proses remote.
Contohnya musik latar atau pengambilan data jaringan tanpa memblokir activity.

Aplikasi modern memakai arsitektur single-activity. [Panduan arsitektur](https://developer.android.com/topic/architecture) menempatkan satu
Activity sebagai wadah layar atau destinasi Compose.

## Sandbox dan keamanan

Setiap aplikasi hidup di sandbox sendiri sebagai user Linux berbeda. Sistem
memberi UID unik, membatasi file, dan menjalankan tiap proses di VM sendiri.

Model ini menerapkan least privilege: aplikasi hanya mengakses komponen yang
dibutuhkan. Berbagi UID hanya mungkin bila sertifikat tanda tangan sama.

## Izin

Izin melindungi data dan aksi terbatas seperti lokasi, kamera, dan audio.
[Gambaran izin](https://developer.android.com/guide/topics/permissions/overview)
membedakan install-time yang otomatis dan runtime yang diminta saat dibutuhkan.

Aturan praktisnya: minta dalam konteks, jangan blokir pengguna, dan sediakan
degradasi mulus saat izin ditolak. [Panduan
runtime](https://developer.android.com/training/permissions/requesting)
mewajibkan pemeriksaan ulang setiap operasi yang dilindungi.

Pola ini sejalan dengan [[Mobile permissions are revocable feature
dependencies]]: grant lama bukan akses permanen.

## Arsitektur aplikasi

Arsitektur yang dianjurkan memakai dua lapis: UI untuk tampilan dan data untuk
logika bisnis. [Panduan arsitektur](https://developer.android.com/topic/architecture) membolehkan lapis domain opsional di antaranya.

Aliran data satu arah menjaga konsistensi dari sumber ke UI. State holder
seperti ViewModel memegang data selama elemen UI membutuhkannya.

Repository menengahi sumber data berkas, jaringan, dan database lokal.
Foldable dan perubahan konfigurasi memaksa UI menyusun ulang state adaptif.

## Build dan distribusi

APK adalah arsip instalasi perangkat. AAB adalah format publikasi yang tidak
dapat diinstal langsung.

Server Play menurunkan APK teroptimasi per perangkat dari satu AAB. Pemisahan
ini melengkapi [[Store distribution does not define mobile architecture\|Store distribution does not define mobile architecture]]:
kanal bukan arsitektur.

Build memakai Gradle dan Android Gradle Plugin. [Konfigurasi build](https://developer.android.com/build) mendefinisikan tipe debug dan
rilis, flavor produk, serta varian sebagai kombinasinya.

Rilis wajib ditandatangani eksplisit, sedangkan debug memakai kunci bawaan. R8
menyusutkan kode dan resource per varian untuk memperkecil ukuran.

## Bahasa

Kotlin adalah bahasa utama untuk aplikasi baru. Java tetap didukung dan C++
dipakai untuk kebutuhan native performa tinggi.

Pilihan bahasa tidak mengubah pipeline runtime. Kode menuju format DEX lalu
dieksekusi ART di perangkat.

## Lanjutan

- [[Android components are separate system entry points with distinct lifecycles\|Android components are separate system entry points with distinct lifecycles]]
- [[Installable Android packages differ from publishable bundles\|Installable Android packages differ from publishable bundles]]
- [[Android build variants multiply release targets from types and flavors\|Android build variants multiply release targets from types and flavors]]
