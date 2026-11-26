---
{"dg-publish":true,"dg-path":"Learn Shell.md","permalink":"/learn-shell/","title":"Learn Shell","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"Learn Shell","aliases":["Learn Shell Tutorial","learnshell.org"],"categories":["Programming Languages"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/learnshell-tutorial-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

Learn Shell adalah tutorial shell interaktif gratis dari learnshell.org untuk interpreter Unix/Linux, dengan Bash sebagai acuan utama. Formatnya bab pendek ditambah editor latihan di dalam browser, bukan dokumentasi referensi. Nilainya sebagai bahan belajar bertahap, bukan sebagai sumber otoritatif perilaku shell.

## Cakupan

Daftar bab di halaman Welcome mencakup Hello World, special variables, `trap`, file testing, input parameter parsing, pipelines, process substitution, regular expressions, special commands, dan user management. Empat bab yang berhasil dibaca memberi gambaran level materinya: shebang dan izin eksekusi, pengujian file, penyambungan perintah, dan process substitution.

Materi yang tersedia konsisten dengan klaim di [[References/Programming Languages for DevOps\|Programming Languages for DevOps]] bahwa Bash adalah shell yang dihadapi di host Linux. Untuk pekerjaan nyata, tutorial ini melengkapi pengetahuan operating-system layer yang dipakai saat bootstrap di [[References/Provisioning\|Provisioning]], misalnya skrip yang memeriksa keberadaan file sebelum menulis konfigurasi.

## Materi yang tercatat

Bab Hello World menjelaskan bahwa skrip disimpan sebagai file, diberi izin eksekusi dengan `chmod +x`, lalu dijalankan dengan namanya. Baris pertama `#!` diikuti path interpreter menentukan program yang mengeksekusi file tersebut, dan path yang salah menghasilkan error seperti "Command not found". Penjelasan ini benar untuk mekanisme dasar, tetapi tidak membahas pemilihan interpreter portabel (`/usr/bin/env bash`).

Bab File Testing mendokumentasikan operator `-e`, `-d`, `-r`, `-w`, `-x`, `-f`, dan `-s` dalam bentuk `[ -e "$file" ]`. Contohnya memakai `if` dan `touch`, sehingga pembaca melihat hubungan antara tes dan aksi, bukan hanya daftar flag.

Bab Pipelines menjelaskan `|` sebagai penyambung output satu perintah ke input perintah berikutnya, dengan `grep` sebagai contoh. Bab Process Substitution menjelaskan `<(...)` dan `>(...)` sebagai cara memperlakukan output proses sebagai nama file, dengan contoh `tee >(tr ...)` yang menulis versi lowercase ke file sambil tetap mencetak output asli.

## Batas

Halaman Welcome hanya berisi daftar bab; isi sebenarnya ada di halaman per bab. Beberapa paragraf tidak terambil saat fetch karena dirender lewat JavaScript, dan latihan hanya berjalan di editor tertanam situs. Situs juga memuat tautan afiliasi ke produk terminal, yang tidak berkaitan dengan materi.

Tutorial ini berhenti di level pengenalan. Ia tidak membahas `set -euo pipefail`, quoting yang aman, portabilitas POSIX versus Bash, atau perilaku berbeda antar shell seperti `sh`, `csh`, dan `tcsh` di luar satu kalimat pembuka. Untuk skrip operasional, manual resmi Bash dan panduan shellcheck lebih tepat. Confidence `medium`: isi empat bab terverifikasi, sisanya hanya diketahui dari daftar.

## Sumber

- [Learn Shell - Free Interactive Shell Tutorial](https://www.learnshell.org/en/Welcome), halaman Welcome dan empat bab pertama, diakses 26 November 2026.
