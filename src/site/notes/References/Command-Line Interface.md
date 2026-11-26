---
{"dg-publish":true,"dg-path":"Command-Line Interface.md","permalink":"/command-line-interface/","title":"Command-Line Interface","hideInFiletree":true,"tags":["references","programming"],"noteIcon":"","dg-note-properties":{"title":"Command-Line Interface","aliases":["CLI","Command Line Interface","command-line shell"],"categories":["Software Systems"],"type":"reference","tags":["references","programming"],"sources":["_raw/articles/cli-wikipedia-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

Command-line interface (CLI) adalah cara berinteraksi dengan software lewat perintah, masing-masing ditulis sebagai satu baris teks. Artikel Wikipedia menyebut sinonim "command-line shell", tetapi ketiga istilah yang sering tertukar sebenarnya menunjuk lapisan berbeda: terminal emulator adalah jendela yang menampilkan teks, shell adalah program yang membaca dan menafsirkan baris perintah, dan CLI adalah bentuk antarmukanya. Memisahkan ketiganya penting karena mengganti shell tidak menghapus terminalnya, dan masalah render tidak sama dengan masalah parsing.

## Anatomi satu baris perintah

Pola umumnya adalah `prompt command param1 param2 … paramN`. Prompt menandakan kesiapan menerima input dan sering memuat direktori kerja serta hostname. Perintah terbagi dua: internal atau built-in yang diproses interpreter, dan eksternal yang menjalankan executable yang dicari lewat `PATH`. Parameter bisa berupa argumen biasa atau option, dan pemisahnya adalah whitespace.

Artikel membedakan syntax (aturan tata bahasa perintah) dari semantics (operasi dan data yang mungkin). Dua CLI disebut cukup mirip untuk dipakai tanpa belajar ulang hanya jika keduanya sepakat pada syntax dan semantics sekaligus; itulah syarat agar skrip bisa dipindahkan. Konsekuensinya, skrip yang jalan di satu shell belum tentu jalan di shell lain meskipun perintahnya terlihat sama.

Varian bentuk perintah yang lazim adalah `cmd how files`, `cmd how src dst`, redirection `< input > output`, dan pipeline `cmd | cmd > output`. Artikel menyebut `>` menimpa file, `>>` menambahkan, dan `|` menyambungkan output satu perintah sebagai input berikutnya. Mekanisme redirection dan pipeline ini yang membuat CLI bisa dirangkai untuk otomasi, bukan hanya diketik satu per satu.

## Kekuatan dan biayanya

Dibanding GUI, CLI membutuhkan resource sistem lebih sedikit dan, bagi pengguna berpengalaman, lebih cepat ketika jumlah perintah dan opsi besar. History dan line editing membuat pengulangan mudah, dan skrip membuat rangkaian perintah bisa disimpan sebagai satu program. Artikel juga menyebut CLI dipakai pada sistem yang tidak mampu menjalankan GUI dan populer di kalangan pengguna dengan disabilitas visual lewat refreshable braille display.

Biayanya adalah hafalan: pengguna baru harus mengetahui nama perintah, parameter, dan syntax, dengan bantuan manual atau opsi `help`. Lingkungan CLI juga tidak memberi font atau jendela edit seperti GUI. Klaim bahwa command line bersifat "self-documenting" ditandai sebagai disputed di artikel, jadi tidak dipakai sebagai dasar di sini.

## Sejarah singkat

Sebelum CLI, interaksi memakai switch fisik atau punched card dalam batch processing. Teletype dan teleprinter menggeser komunikasi ke teks; CTSS (1961) memperkenalkan interpreter yang menerima perintah seperti LOGIN atau PRINT. Louis Pouzin mencetuskan istilah shell di Multics (1964) lewat RUNCOM, lalu Thompson shell (1971), Bourne shell (1977), dan Bash melanjutkan garis yang sama. GUI mulai menantang posisi CLI pada Apple Lisa (1983) dan Macintosh (1984); pengguna PC DOS baru benar-benar berpindah pada Windows 95 (1995).

Jadi "CLI" bukan satu era, melainkan lapisan yang bertahan karena skrip dan pipeline tetap lebih ringkas daripada klik untuk pekerjaan berulang. Garis waktu ini menjelaskan mengapa tiap keluarga sistem punya aturan syntax sendiri: DOS/Windows (`cmd.exe`, PowerShell), Unix (`sh`, `bash`, `zsh`, `ksh`), OpenVMS (DCL), dan router seperti Cisco IOS.

## Jenis dan batas

Ada CLI milik sistem operasi (shell) dan CLI milik aplikasi. Aplikasi bisa menawarkan tiga mekanisme sekaligus: parameter saat peluncuran, sesi interaktif dengan prompt sendiri, dan inter-process communication lewat standard stream atau named pipe. Beberapa aplikasi menyediakan GUI sebagai wrapper atas CLI, sehingga CLI bisa mengekspos fitur yang tidak ada di GUI; MATLAB disebut sebagai contoh yang semua fiturnya tersedia di CLI.

Untuk praktik, Bash sebagai shell Linux adalah pintu masuk paling umum, dan tutorial interaktif seperti [[References/Learn Shell\|Learn Shell]] memberi latihan bertahap sebelum skrip dipakai di [[References/Provisioning\|Provisioning]] atau pipeline [[References/Deployment\|Deployment]]. Artikel juga menyinggung Python dan bahasa lain yang punya mode interaktif berisi REPL, sejalan dengan peran [[References/Python\|Python]] sebagai bahasa tool di [[References/Programming Languages for DevOps\|Programming Languages for DevOps]].

Batas bukti: ini sumber tersier yang pada Agustus 2025 ditandai butuh lebih banyak sitasi, dan beberapa klaimnya membawa tag citation needed. Definisi dan anatomi dipakai karena sesuai dengan dokumentasi shell, tetapi tanggal sejarah dan detail vendor sebaiknya diperiksa ke sumber primer bila keputusannya bergantung pada versi. Untuk perilaku perintah tertentu, manual resmi tetap acuan, bukan ensiklopedia.

## Sumber

- [Command-line interface](https://en.wikipedia.org/wiki/Command-line_interface), Wikipedia bahasa Inggris, diakses 26 November 2026.
