---
{"dg-publish":true,"dg-path":"SSH.md","permalink":"/ssh/","title":"SSH","hideInFiletree":true,"tags":["references","security","networking"],"noteIcon":"","dg-note-properties":{"title":"SSH","aliases":["Secure Shell","SSH Protocol"],"categories":["Security"],"type":"reference","tags":["references","security","networking"],"sources":["_raw/articles/ssh-com-ssh-protocol-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

SSH (*Secure Shell*) adalah protokol client-server untuk login remote, eksekusi perintah, transfer file, dan tunneling melalui jaringan yang tidak aman. Ia menggantikan protokol cleartext seperti telnet, rlogin, dan FTP. SSH berjalan di atas TCP dengan port default 22; lihat [[References/TCP IP\|TCP/IP]] untuk lapisan transport di bawahnya.

Spesifikasi SSH-2 distandarkan IETF pada Januari 2006 melalui RFC 4250 sampai 4254. OpenSSH 9.6p1 di mesin ini hanya mendukung protokol versi 2 (`ssh -Q protocol-version` menghasilkan `2`), dan `ssh -1` ditolak dengan pesan "SSH protocol v.1 is no longer supported".

## Arsitektur tiga lapis

RFC 4251 membagi SSH-2 menjadi tiga protokol yang berlapis:

| Lapisan | RFC | Fungsi |
|---|---|---|
| Transport (SSH-TRANS) | 4253 | Key exchange, autentikasi server, enkripsi, integritas, kompresi |
| User Authentication (SSH-USERAUTH) | 4252 | Autentikasi client: publickey, password, keyboard-interactive, hostbased, GSSAPI |
| Connection (SSH-CONN) | 4254 | Multiplexing channel, shell/exec, port forwarding, X11, agent, subsistem SFTP |

Transport berjalan di atas TCP. Setelah kanal transport aman terbentuk, lapisan autentikasi memverifikasi client, lalu lapisan connection membuka satu atau banyak channel logis di dalam satu koneksi.

## Cara kerja koneksi

1. **TCP connect** ke port 22 (atau port lain yang dikonfigurasi).
2. **Version exchange.** Kedua pihak mengirim string `SSH-2.0-...`; versi yang tidak kompatibel diputus.
3. **Key exchange (KEX).** Client dan server menyepakati algoritma KEX, membuat secret bersama, dan menurunkan kunci simetris. Server menandatangani hash handshake dengan host key untuk membuktikan identitasnya.
4. **Verifikasi host key.** Client membandingkan host key dengan `known_hosts`. Pada koneksi pertama, client memakai trust on first use (TOFU) dan menampilkan fingerprint untuk dikonfirmasi.
5. **Autentikasi user.** Client membuktikan identitasnya dengan publickey, password, keyboard-interactive, atau metode lain yang disepakati.
6. **Connection.** Shell interaktif, perintah tunggal, port forwarding, atau subsistem seperti SFTP dibuka sebagai channel.
7. **Rekey dan disconnect.** Kunci dapat diperbarui setelah batas byte/waktu; sesi ditutup dengan pesan disconnect.

## Autentikasi

- **Public key.** Client memiliki private key; server menyimpan public key di `authorized_keys`. Client menandatangani data handshake, dan private key tidak pernah dikirim. Private key sebaiknya dilindungi passphrase dan permission file `600`.
- **Password.** Sederhana, tetapi rentan brute force dan credential stuffing; sebaiknya dimatikan setelah key berfungsi.
- **Keyboard-interactive.** Dipakai untuk PAM, OTP, dan MFA.
- **Hostbased dan GSSAPI/Kerberos.** Dipakai di lingkungan tertentu, misalnya domain Windows atau kluster dengan tiket Kerberos.
- **Sertifikat OpenSSH.** CA internal dapat menandatangani user/host certificate sehingga `authorized_keys` tidak perlu diisi satu per satu.

Key adalah kredensial: siapa pun yang memegang private key tanpa passphrase dapat bertindak sebagai pemilik akun. Karena itu key perlu diprovision, diaudit, dirotasi, dan dicabut seperti akun.

## Enkripsi dan integritas

Parameter yang dinegosiasikan menentukan keamanan koneksi. Default OpenSSH 9.6 di mesin ini mencakup:

- **KEX:** `sntrup761x25519-sha512`, `curve25519-sha256`, ECDH nistp256/384/521, dan DH group14/16/18-sha256.
- **Cipher:** `chacha20-poly1305`, AES-CTR, AES-GCM.
- **MAC:** HMAC-SHA2-256/512, UMAC, dan varian encrypt-then-MAC.
- **Host key:** Ed25519, ECDSA nistp256/384/521, RSA-SHA2-256/512.

Cipher AEAD seperti GCM dan ChaCha20-Poly1305 menggabungkan enkripsi dan integritas; cipher non-AEAD memerlukan MAC terpisah. SHA-2 adalah *Secure Hash Algorithm 2*, bukan "Standard Hashing Algorithm".

## Versi dan standar

- **SSH-1** dirancang Tatu Ylonen pada 1995. Versi ini sudah usang, memiliki kelemahan serius (antara lain serangan kompensasi CRC32), dan tidak boleh diaktifkan.
- **SSH-2** adalah standar IETF: RFC 4250 (overview), 4251 (arsitektur), 4252 (autentikasi), 4253 (transport), 4254 (connection), serta 4256 (GSSAPI). Ekstensi penting mencakup RFC 4419 (DH group exchange), 5656 (ECC), 8308 (extensions), 8332 (RSA-SHA2), dan 9142 (algoritma KEX).
- **SFTP** adalah subsistem di atas SSH, bukan protokol jaringan terpisah. Versi 3 (`draft-ietf-secsh-filexfer-02`) adalah versi yang paling luas diimplementasikan; draft versi 4 sampai 6 tidak banyak dipakai. Lihat [[References/scp\|scp]] untuk transfer file.
- **Format public key.** RFC 4716 (Informational) mendefinisikan format `---- BEGIN SSH2 PUBLIC KEY ----`. OpenSSH memakai format satu baris `authorized_keys` yang berbeda dan bersifat de facto, bukan RFC 4716.

## Keamanan operasional

- **Verifikasi host key.** Jangan mematikan `StrictHostKeyChecking` secara permanen; TOFU pada koneksi pertama tetap titik lemah terhadap MITM.
- **Utamakan key, matikan password.** Setelah key berfungsi, set `PasswordAuthentication no` dan `PermitRootLogin prohibit-password` (atau `no`).
- **Batasi permukaan.** Port 22 yang terbuka di [[References/Firewall\|Firewall]] adalah sasaran pemindaian dan brute force; gunakan bastion, allowlist, rate limit, atau Fail2Ban. Mengganti port bukan kontrol keamanan yang berarti.
- **Audit key.** Hapus key yang tidak dipakai, batasi dengan opsi `authorized_keys` seperti `from=`, `command=`, `restrict`, dan `no-pty`.
- **Lindungi private key.** Passphrase, permission `600`, dan agent yang tidak diekspos. Agent forwarding memberi server remote kemampuan memakai key Anda; aktifkan hanya bila perlu.
- **Matikan algoritma lemah.** Tolak SSH-1, cipher/MAC/KEX usang, dan pantau kegagalan autentikasi.

## Koreksi terhadap artikel SSH.com

1. **"Melindungi keamanan dan integritas komunikasi dengan enkripsi kuat."** Enkripsi memberi kerahasiaan; integritas berasal dari MAC atau AEAD, bukan dari enkripsi itu sendiri. Artikel menyebut hashing di bagian berikutnya, tetapi kalimat pembuka mencampur keduanya.
2. **"Client memakai public key cryptography untuk memverifikasi identitas server."** Protokol menyediakan mekanismenya, tetapi verifikasi bergantung pada kebijakan client. Pada koneksi pertama (TOFU) atau dengan `StrictHostKeyChecking=no`, MITM masih mungkin terjadi.
3. **"Autentikasi yang paling umum adalah password dan public key."** Publickey dan password memang umum, tetapi keyboard-interactive (PAM/OTP/MFA) juga banyak dipakai, begitu pula hostbased dan GSSAPI di lingkungan tertentu.
4. **"Public key authentication memberi akses kepada siapa pun yang memiliki salinan private key."** Benar hanya jika private key tidak dilindungi passphrase. Key berpassphrase memerlukan passphrase tersebut, dan server dapat membatasi key dengan opsi `authorized_keys`.
5. **"Kami menemukan beberapa juta SSH key ... 90% tidak dipakai."** Klaim vendor SSH Communications Security tanpa metodologi atau sitasi. Perlakukan sebagai pemasaran, bukan statistik umum.
6. **"Proyek identity management melewatkan 90% kredensial karena mengabaikan SSH key."** Klaim vendor yang sama; tidak ada bukti independen di artikel.
7. **"SHA-2 (Standard Hashing Algorithm)."** SHA-2 adalah *Secure Hash Algorithm 2* (FIPS 180-4), bukan "Standard Hashing Algorithm".
8. **"SSH kini standar Internet yang dijelaskan dalam dokumen berikut."** Daftarnya tidak lengkap: RFC 4250 (overview) dan 4256 (GSSAPI) tidak disebut, begitu pula ekstensi 4419, 5656, 8308, 8332, dan 9142.
9. **"SFTP saat ini didokumentasikan dalam draft-ietf-secsh-filexfer-02."** Itu Internet-Draft lama untuk SFTP versi 3, bukan standar yang berlaku. Versi 3 memang baseline interoperabel, tetapi menyebutnya "currently documented" menyesatkan.
10. **"Format public key bukan standar formal (dokumen informational)."** RFC 4716 memang Informational, tetapi format `authorized_keys` OpenSSH berbeda dari RFC 4716 dan tidak dijelaskan artikel.
11. **"SSH adalah metode untuk secure remote login."** Definisi terlalu sempit. SSH juga melakukan eksekusi perintah, transfer file, port forwarding, dan tunneling; artikel sendiri membahasnya kemudian.
12. **"Alternatif aman untuk telnet, rlogin, dan FTP."** Benar, tetapi pengganti FTP adalah SFTP/SCP, bukan "FTP di atas SSH". Menyalurkan FTP melalui tunnel SSH tidak sama dengan SFTP.
13. **Port dan transport tidak disebut.** SSH berjalan di atas TCP port 22 secara default; `ssh -G localhost` di mesin ini mengonfirmasi `port 22`.
14. **Versi protokol tidak disebut.** Artikel hanya menyebut "internet standard" tanpa menjelaskan bahwa SSH-1 usang dan SSH-2 yang dipakai. OpenSSH 9.6 menolak SSH-1 secara eksplisit.
15. **"Kunci enkripsi dihasilkan saat negosiasi."** Penyederhanaan. Pada SSH-2, kunci diturunkan dari hasil key exchange, dan rekey dapat terjadi setelah batas byte atau waktu tertentu.

## Verifikasi

Fakta protokol dicek ke dokumen primer: RFC 4251 dan RFC 4253 (Januari 2006, Standards Track) serta RFC 4716 (November 2006, Informational). Pemeriksaan lokal: OpenSSH 9.6p1; `ssh -Q protocol-version` menghasilkan `2`; `ssh -1` ditolak; `ssh -G localhost` menunjukkan `port 22` beserta daftar cipher, MAC, KEX, dan host key default. Halaman SSH.com adalah artikel vendor tanpa tanggal terbit. Tidak ada perubahan konfigurasi atau layanan.

## Sumber

- [SSH Protocol – Secure Remote Login and File Transfer oleh SSH.com Academy](https://www.ssh.com/academy/ssh/protocol): definisi, model client-server, autentikasi key, enkripsi, dan daftar RFC. Disimpan sebagai `_raw/articles/ssh-com-ssh-protocol-2026-11-26.md`.
- [RFC 4251: The Secure Shell (SSH) Protocol Architecture](https://www.rfc-editor.org/rfc/rfc4251), Januari 2006.
- [RFC 4253: The Secure Shell (SSH) Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253), Januari 2006.
- [RFC 4716: The Secure Shell (SSH) Public Key File Format](https://www.rfc-editor.org/rfc/rfc4716), November 2006.
