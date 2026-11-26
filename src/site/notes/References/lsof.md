---
{"dg-publish":true,"dg-path":"lsof.md","permalink":"/lsof/","title":"lsof","hideInFiletree":true,"tags":["references","devops","security"],"noteIcon":"","dg-note-properties":{"title":"lsof","aliases":["list open files","lsof(8)"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","security"],"sources":["_raw/articles/lsof-man-page-2026-11-26.md","_raw/articles/lsof-cheatsheet-neverendingsecurity-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

`lsof` mencetak daftar file yang sedang dibuka oleh proses. "File" di sini luas: regular file, direktori, block dan character device, program yang dieksekusi, shared library, stream, NFS, serta socket jaringan dan UNIX domain socket. Tanpa opsi apa pun, `lsof` mendaftar semua file terbuka milik semua proses aktif, sehingga perintah ini sering dipakai sebagai titik awal investigasi port, file yang terkunci, atau proses yang menahan mount point.

## Cara memilih apa yang didaftar

Aturan yang paling mudah salah: opsi pemilihan digabung dengan OR secara default. Menulis `-i` bersama `-u foo` mendaftar file jaringan ATAU file milik user foo, bukan irisan keduanya. `-a` mengubah semua opsi pemilihan menjadi AND, dan penempatannya tidak mengubah arti — di mana pun `-a` berada, ia meng-AND seluruh opsi. Negasi dengan `^` (user, PID, PGID, nama perintah, state protokol) diterapkan lebih dulu sebagai pengecualian, tanpa OR atau AND.

Beberapa pemilih yang sering dipakai:

| Pemilih | Arti |
|---|---|
| `-p 1234` | proses dengan PID tersebut |
| `-u abe,1234` | login atau UID |
| `-c ssh` | nama perintah berawalan `ssh`; `/regex/` untuk regex |
| `-d cwd,1,3` | file descriptor tertentu; `^6,^2` untuk eksklusi |
| `-i` | file Internet; `-i 4`, `-i 6`, `-i :8080`, `-i @host:513-515` |
| `-U` | UNIX domain socket |
| `-N` | file NFS |
| `path` | file atau direktori tertentu |
| `+d` / `+D` | direktori satu level / seluruh kedalaman |

`+d` tidak menelusuri subdirektori dan `+D` menelusuri penuh dengan biaya besar: man page memperingatkan `+D` lambat dan memakan memori karena memanggil `stat` untuk setiap file. Keduanya juga tunduk pada izin user.

## Kolom dan arti FD

Kolom utama adalah COMMAND, PID, TID, USER, FD, TYPE, DEVICE, SIZE/OFF, NODE, dan NAME. Kolom FD lebih informatif daripada angka: `cwd` (direktori kerja), `rtd` (root directory), `txt` (program text), `mem` (memory-mapped file), `mmap` (device ter-map), `DEL` (file terhapus), dan `NOFD` ketika direktori `/proc/<PID>/fd` tidak bisa dibuka. Setelah nama FD ada karakter mode (`r`, `w`, `u`) dan karakter lock jika ada. FD di atas 9999 disingkat menjadi `*` plus tiga digit terakhir.

TYPE membedakan `REG`, `DIR`, `CHR`, `FIFO`, `IPv4`, `IPv6`, `unix`, `netlink`, dan lainnya. Pasangan FD `DEL` dengan TYPE `DEL` menandakan file yang sudah dihapus tetapi masih terbuka, kondisi yang menjelaskan mengapa ruang disk belum kembali.

## Pola pakai yang aman

Untuk mencari pemilik port, batasi ke socket TCP yang listen dan matikan resolusi nama agar cepat dan tidak bergantung DNS: `lsof -nP -iTCP:8080 -sTCP:LISTEN`. Untuk mematikan proses yang menahan sebuah file, `-t` mencetak PID saja sehingga bisa dipakai `kill -HUP $(lsof -t /path/file)`. Untuk skrip, `-F` menghasilkan output field yang stabil, sedangkan `-Q` membuat exit code tetap 0 ketika hasil pencarian kosong.

Exit code mengikuti aturan ketat: `lsof` mengembalikan 1 jika ada error, termasuk gagal menemukan item yang diminta; 0 hanya jika tidak ada error dan semua argumen berhasil didaftar. Karena itu `lsof` tanpa `-Q` mudah membuat skrip gagal saat prosesnya memang tidak ada.

## Resep umum dan koreksinya

Cheatsheet 2015 dari Never Ending Security merangkum pola yang sering dipakai: `lsof -u cyberpunk` untuk satu user, `lsof -i TCP:22` untuk satu port, `lsof -i 4` dan `lsof -i 6` untuk IPv4/IPv6, `lsof -i TCP:1-1024` untuk rentang port, `lsof -p 1` untuk satu PID, dan `lsof -i -u^root` untuk mengecualikan root. Resep-resep ini masih berlaku karena memakai opsi yang sama dengan man page, tetapi contoh outputnya berasal dari distro 2015 sehingga nama library dan service-nya sudah usang.

Dua koreksi penting. Pertama, cheatsheet memberi judul "Find Out who's Looking What Files" pada `lsof -i -u cyberpunk`, padahal tanpa `-a` opsi itu berarti file jaringan ATAU semua file milik cyberpunk, bukan irisan keduanya; untuk irisan perlu `lsof -a -i -u cyberpunk`. Kedua, resep mematikan semua proses satu user lewat `lsof -t -u` lalu `kill -9` (cheatsheet menulisnya dengan backtick) berisiko: ia membunuh seluruh proses user dan tidak memeriksa apakah PID masih milik proses yang sama saat perintah dijalankan; untuk file tertentu, `kill -HUP $(lsof -t /path/file)` dari man page lebih sempit dan lebih aman.

## Biaya, izin, dan jebakan

`-n` mematikan konversi alamat ke hostname dan `-P` mematikan konversi port ke nama service; keduanya mempercepat eksekusi dan berguna saat DNS atau `/etc/services` bermasalah. `-b` menghindari panggilan kernel yang bisa memblokir (`stat`, `lstat`, `readlink`), relevan pada NFS yang menggantung. `-O` melewati strategi fork dan mempercepat startup tetapi bisa menggantung bila kernel tidak merespons; man page menyebutnya perlu hati-hati.

Izin tidak seragam antar build. Man page menjelaskan bahwa mode kompilasi default mengizinkan siapa pun mendaftar semua file terbuka, sedangkan build dengan `HASSECURITY` membatasi non-root ke proses miliknya sendiri; status ini terlihat di output `lsof -h`. `lsof` juga membuat device cache file di home directory user, dan `-Di` bisa mematikannya sementara.

Dua batas praktis: hasil `lsof` adalah snapshot, sehingga proses bisa menutup file tepat setelah baris dicetak; dan untuk pertanyaan khusus socket, `ss` biasanya lebih langsung karena membaca `/proc/net` tanpa memindai seluruh file terbuka. Dalam pekerjaan operasional, `lsof` lebih tepat untuk pertanyaan "siapa yang membuka file ini", bukan untuk memantau koneksi berkecepatan tinggi.

Perintah ini adalah contoh nyata dari [[References/Command-Line Interface\|Command-Line Interface]] yang dirangkai di skrip shell seperti yang dilatih [[References/Learn Shell\|Learn Shell]], dan hasilnya sering dipakai saat audit host di [[References/Server Security\|Server Security]].

## Sumber

- [lsof(8) - Linux manual page](https://man7.org/linux/man-pages/man8/lsof.8.html), man7.org, diakses 26 November 2026.
- [Lsof Commands Cheatsheet](https://neverendingsecurity.wordpress.com/2015/04/13/lsof-commands-cheatsheet/), Never Ending Security, 13 April 2015.
