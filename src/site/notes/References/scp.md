---
{"dg-publish":true,"dg-path":"scp.md","permalink":"/scp/","title":"scp","hideInFiletree":true,"tags":["references","devops","security"],"noteIcon":"","dg-note-properties":{"title":"scp","aliases":["secure copy","scp(1)"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","security"],"sources":["_raw/articles/scp-tecmint-2026-11-26.md","_raw/articles/scp-phoenixnap-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

`scp` menyalin file antar host melalui koneksi SSH, memakai autentikasi dan keamanan yang sama dengan sesi login. Sejak OpenSSH 9.0, `scp` memakai protokol SFTP untuk transfer; protokol SCP lama hanya dipakai bila diminta eksplisit dengan `-O`. Ini penting karena banyak tutorial lama, termasuk artikel TecMint ini, masih mengasumsikan perilaku protokol SCP klasik. Versi lokal yang dipakai untuk verifikasi: OpenSSH 9.6p1.

## Sintaks

```text
scp [opsi] sumber ... tujuan
```

Sisi remote ditulis `[user@]host:path`, atau sebagai URI `scp://[user@]host[:port][/path]`. Tanpa `user@`, `scp` memakai user yang sedang login. Path relatif dihitung dari home user remote, bukan dari direktori kerja shell interaktif.

## Opsi yang sering dipakai

| Opsi | Arti |
|---|---|
| `-r` | salin direktori rekursif; symlink diikuti |
| `-p` | pertahankan mtime, atime, dan mode bit file sumber |
| `-P port` | port SSH tujuan (kapital, karena `-p` sudah dipakai) |
| `-C` | aktifkan kompresi, diteruskan ke `ssh` |
| `-c cipher` | pilih cipher, diteruskan ke `ssh` |
| `-l limit` | batas bandwidth dalam Kbit/s, bukan KB/s |
| `-q` | matikan progress meter dan pesan diagnostik |
| `-v` | debug koneksi, autentikasi, dan konfigurasi |
| `-F file` | pakai `ssh_config` alternatif |
| `-i file` | identity file untuk autentikasi public key |
| `-J host` | lompat lewat jump host (ProxyJump) |
| `-3` | dua host remote lewat host lokal; ini default |
| `-R` | dua host remote: jalankan `scp` di host asal |
| `-O` | paksa protokol SCP lama, bukan SFTP |
| `-T` | matikan pemeriksaan nama file dari sisi remote |

Batas `-l` sering salah dibaca. Nilai `400` berarti 400 Kbit/s, yaitu sekitar 50 KB/s; untuk membatasi 50 KB/s tulis `-l 400`. Angka itu juga batas agregat per proses, bukan jaminan throughput, karena kecepatan sebenarnya tetap dibatasi jaringan dan CPU saat kompresi.

## Protokol dan perilaku

Pemakaian SFTP sejak OpenSSH 9.0 mengubah beberapa hal: wildcard dan ekspansi `~` dikerjakan oleh klien SFTP, bukan shell remote, sehingga quoting karakter shell tidak lagi menjadi masalah utama. Sebaliknya, `-O` (protokol lama) tetap menjalankan shell user remote untuk globbing, jadi karakter seperti kutip harus di-quote dengan benar.

Saat menyalin antara dua host remote, `scp` default menghubungkan keduanya secara langsung (`-3`), bukan mengalirkan data lewat mesin lokal. `-R` membalik arah: `scp` dijalankan di host asal dan harus bisa login ke tujuan tanpa password. Untuk topologi berlapis, `-J` lebih jelas daripada merangkai `ProxyCommand` manual.

`scp` tidak memeriksa isi direktori tujuan sebelum menulis: file dengan nama yang sama ditimpa tanpa konfirmasi. Untuk beberapa file sekaligus, sumber bisa disebut berurutan (`scp a.txt b.txt user@host:dir/`) dengan tujuan berupa direktori; bila tujuan bukan direktori dan sumber lebih dari satu, perintah gagal.

## Koreksi terhadap artikel TecMint

Artikel "10 SCP Commands" merangkum pola yang berguna, tetapi beberapa klaimnya perlu dikoreksi terhadap `man scp` OpenSSH 9.6p1:

- "Tanpa parameter, SCP menyalin di background" salah. Perintah berjalan di foreground dan hanya tidak menampilkan apa pun sampai selesai; untuk background perlu job control shell (`&`), dan `-q` hanya mematikan progress meter.
- "Default SCP memakai AES-128" tidak akurat. Cipher mengikuti `Ciphers` di `ssh_config`; pada OpenSSH 9.6 urutan default dimulai `chacha20-poly1305@openssh.com`, lalu `aes128-ctr`, dan seterusnya (`ssh -G localhost` untuk memeriksa). `-c` hanya meneruskan pilihan ke `ssh`.
- Klaim `-C` "10 kali lebih cepat" adalah anekdot satu file log 93 MB di jaringan lambat 2012, bukan aturan umum. Kompresi dilewati untuk file yang sudah terkompresi, dan `ssh` modern memakai `Compression no` secara default.
- `-p` mempertahankan mtime, atime, dan mode bit, bukan hanya waktu.
- Contoh perintah di halaman aslinya menampilkan `user@host` sebagai `[email protected]` karena obfuscation email; anggap bagian itu placeholder, bukan alamat literal.
- Artikel tidak menyebut `-T` (pemeriksaan nama file remote) dan perilaku symlink pada `-r`, dua hal yang relevan saat menyalin direktori dari server yang tidak dipercaya.

## Koreksi terhadap artikel phoenixNAP

Artikel 16 November 2023 (Milica Dancuk) memuat tabel opsi yang sebagian tidak ada di OpenSSH `scp`; beberapa tampaknya tertukar dengan opsi `rcp` atau `sftp` lama. Yang tidak valid: `-1`/`-2` (pemilihan protokol; SSH protocol 1 sudah dihapus dari OpenSSH 7.6), `-b buffer_size`, `-D debug_level` (`-D` justru path program SFTP server, debug memakai `-v` atau `-o LogLevel`), `-d`, `-h`, `-Q`, dan `-u` (menghapus sumber bukan perilaku `scp`). Klaim default cipher `AnyStdCipher` juga salah; cipher mengikuti `Ciphers` di `ssh_config`. Sebaliknya tabel itu melewatkan opsi yang benar-benar ada: `-3`, `-R`, `-T`, `-O`, `-X`, dan `-J`.

Klaim "default AES-128" berulang di sini, sama seperti artikel TecMint. Penyebutan "SCP (Secure Copy Protocol)" sebagai nama protokol juga menyesatkan sejak OpenSSH 9.0, karena perintah `scp` kini memakai protokol SFTP; SCP hanyalah nama perintah dan protokol lama yang dipakai lewat `-O`.

Dua contohnya tidak konsisten: perintah pertama memakai `136.183.142.28` tetapi penjelasannya menulis `136.183.142.27`, dan contoh remote-to-local menghilangkan `user@` padahal penjelasannya menyebut `root@`. Bagian yang benar dan berguna: konversi `-l` (100 KB/s = 800), `-p` yang mempertahankan times, modes, dan permissions, `-P` untuk port non-22, `-r` rekursif, `-4`/`-6`, `-B`, `-F`, `-i`, `-o`, `-S`, `-q`, `-C`, serta `-v`; catatan bahwa tujuan ditimpa tanpa konfirmasi juga akurat.

## Kapan bukan scp

`scp` tidak punya resume, transfer delta, atau exclude pattern. Untuk direktori besar yang sering berubah, `rsync` lebih murah karena hanya mengirim perbedaan; untuk menjelajah isi server, `sftp` lebih nyaman. `scp` tetap pilihan tepat untuk satu file, skrip sederhana, dan host yang hanya punya `scp`/SFTP.

Perintah ini adalah contoh konkret [[References/Command-Line Interface\|Command-Line Interface]] yang biasa dirangkai di skrip seperti yang dilatih [[References/Learn Shell\|Learn Shell]], dan pemakaiannya bergantung pada kebijakan akses yang diatur di [[sshd_config\|sshd_config]] serta jalur koneksi seperti [[SSHIntoLinuxVM\|SSHIntoLinuxVM]]. Protokol dan lapisan yang mendasarinya dijelaskan di [[References/SSH\|SSH]].

## Sumber

- [10 SCP Commands to Transfer Files/Folders in Linux](https://www.tecmint.com/scp-commands-examples/), TecMint, diakses 26 November 2026.
- [SCP Command in Linux {13 Examples}](https://phoenixnap.com/kb/linux-scp-command), phoenixNAP KB, 16 November 2023.
- `man scp` OpenSSH 9.6p1 (Ubuntu 3ubuntu13.20), diperiksa lokal 26 November 2026.
