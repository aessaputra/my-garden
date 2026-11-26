---
{"dg-publish":true,"dg-path":"LXC.md","permalink":"/lxc/","title":"LXC","hideInFiletree":true,"tags":["references","devops","deployment","security"],"noteIcon":"","dg-note-properties":{"title":"LXC","aliases":["Linux Containers","liblxc","LXC containers"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","deployment","security"],"sources":["_raw/articles/lxc-introduction-2026-11-26.md","_raw/articles/lxc-getting-started-2026-11-26.md","_raw/articles/lxc-documentation-2026-11-26.md","_raw/articles/lxc-news-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

LXC adalah antarmuka userspace untuk fitur containment di kernel Linux. Lewat API dan seperangkat alat sederhana, pengguna Linux bisa membuat dan mengelola container sistem atau aplikasi tanpa hypervisor dan tanpa kernel terpisah. Posisinya sering digambarkan di antara chroot dan mesin virtual penuh: tujuannya adalah lingkungan yang sedekat mungkin dengan instalasi Linux standar, tetapi berbagi kernel host.

## Fitur kernel yang dipakai

Container LXC memakai fitur kernel berikut untuk mengurung proses:

- Kernel namespace: ipc, uts, mount, pid, network, dan user.
- Profil AppArmor dan SELinux.
- Kebijakan seccomp.
- Chroot dengan `pivot_root`.
- Kernel capabilities.
- CGroup (control group).

Isolasi berasal dari kombinasi tersebut, bukan dari virtualisasi perangkat keras. Konsekuensinya, container LXC lebih ringan daripada VM, tetapi kernel tetap bersama dan menjadi batas keamanan utama.

## Komponen dan lisensi

LXC terdiri dari library `liblxc`, binding bahasa untuk API (python3, lua, Go, ruby, Haskell), seperangkat alat standar untuk mengendalikan container, dan template distribusi. Sebagian besar kode berlisensi GNU LGPLv2.1+, beberapa bit kompatibilitas Android memakai BSD 2-clause, dan sebagian biner serta template memakai GPLv2. Lisensi default proyek adalah LGPLv2.1+.

## Versi dan dukungan

LXC 7.0 LTS dirilis 30 April 2026 dan didukung sampai Juni 2031. Rilis ini adalah LTS ketujuh, hasil dua tahun kerja sejak 6.0. Sorotan 7.0:

- **Landlock monitor.** Bila dibangun dengan `landlock-monitor`, LXC memakai Landlock untuk membatasi apa yang boleh dilakukan handler API monitor, praktis hanya berinteraksi dengan container dan filesystem-nya.
- **Pemisahan environment hook dan runtime.** Kunci baru `lxc.environment.hooks` dan `lxc.environment.runtime` memungkinkan pemberian variabel environment hanya ke container, hook, atau keduanya secara terpisah.
- **Fitur yang dihapus.** Dukungan CGroupV1, kernel tanpa PIDFD, dan kernel tanpa mount API baru dihentikan.
- **Keamanan.** CVE-2026-39402 (bypass otorisasi penghapusan port OVS di `lxc-user-nic`) diperbaiki.

Halaman Introduction masih menyebut LXC 6.0 didukung sampai 1 Juni 2029 dan 5.0 sampai 1 Juni 2027; daftar itu tertinggal dari rilis 7.0 pada halaman News, yang menjadi sumber versi terkini. Branch 6.0 sendiri tetap menerima bugfix dan patch keamanan sampai Juni 2029 menurut pengumuman 6.0.3.

## Persyaratan dan instalasi

Dependensi keras: salah satu dari glibc, musl libc, uclibc, atau bionic sebagai C library, dan kernel Linux >= 2.6.32. `lxc-attach` membutuhkan kernel >= 3.8. Container unprivileged membutuhkan `libpam-cgfs`, versi shadow yang menyediakan `newuidmap` dan `newgidmap`, serta kernel >= 3.12. Library yang disarankan: libcap, libapparmor, libselinux, libseccomp, libgnutls, liblua, dan python3-dev.

Instalasi biasanya lewat paket distribusi, langsung dari repository atau backport. Pada Ubuntu, `sudo apt-get install lxc` memasang perintah LXC, template, dan binding python3. Setelah itu `lxc-checkconfig` memeriksa apakah kernel memenuhi konfigurasi yang dibutuhkan.

## Alur dasar container privileged

Container privileged dibuat oleh root dan berjalan sebagai root. Ini cara termudah untuk belajar, tetapi bukan pilihan yang aman untuk produksi: capability dropping, profil AppArmor, konteks SELinux, atau seccomp bisa melindungi sebagian, tetapi prosesnya tetap root, dan root di dalam container tidak boleh diberikan kepada pihak yang tidak dipercaya. Secara default LXC membuat container privileged.

```bash
lxc-create --name mycontainer --template download
lxc-create --name mycontainer --template download -- --dist alpine --release 3.19 --arch amd64
lxc-start --name mycontainer
lxc-info --name mycontainer
lxc-ls --fancy
lxc-attach --name mycontainer --clear-env
lxc-stop --name mycontainer
lxc-destroy --name mycontainer
```

Template `download` memilih image dari images.linuxcontainers.org. Perubahan di dalam container bersifat persisten: setelah container dihentikan dan dijalankan lagi, perubahan tetap ada, mirip mesin virtual ringan.

## Autostart, alamat IP, dan volume

Container tidak otomatis berjalan saat host restart. Tambahkan `lxc.start.auto = 1` ke `/var/lib/lxc/<nama>/config` untuk satu container, atau ke `/etc/lxc/default.conf` agar semua container baru ikut autostart.

Alamat IP diberikan dinamis oleh dnsmasq di bridge `lxcbr0`, sehingga bisa berubah setiap container bergabung ke jaringan. `lxc-ls --fancy` menampilkan IP setelah beberapa detik; jika tidak muncul, aturan `ufw` pada `lxcbr0` perlu dibuka. Lease tercatat di `/var/lib/misc/dnsmasq.lxcbr0.leases`, dan `lxc-destroy --force` tidak membersihkan lease. Untuk alamat yang stabil, aktifkan `LXC_DHCP_CONFILE` di `/etc/default/lxc-net` dan tambahkan `dhcp-host=<nama>,<ip>` di `/etc/lxc/dnsmasq.conf`.

Filesystem container dibatasi di `/var/lib/lxc/<nama>/rootfs`, dan semuanya ikut terhapus saat container dihancurkan. Untuk data yang harus bertahan atau dibagi, buat direktori di host lalu mount dengan `lxc.mount.entry`:

```bash
echo "lxc.mount.entry = /host/path/to/volume container/mount/point none bind,create=dir 0 0" >>/var/lib/lxc/mycontainer/config
```

Target mount di sisi container bersifat relatif, tanpa garis miring di depan.

## Container unprivileged

Container unprivileged memetakan uid dan gid: root (uid 0) di dalam container menjadi uid seperti 100000 di host. Jika terjadi escape, penyerang hanya mendapat hak setara user biasa. Konsekuensinya, beberapa operasi umum tidak diizinkan, termasuk mounting sebagian besar filesystem, membuat device node, dan operasi terhadap uid/gid di luar rentang yang dipetakan; karena itu template distribusi biasa sering tidak cocok dan template `download` lebih disarankan.

Untuk container unprivileged yang dibuat root, alokasikan rentang subordinate di `/etc/subuid` dan `/etc/subgid`, lalu set `lxc.idmap` di `/etc/lxc/default.conf`. Rentang yang sama untuk semua container lebih sederhana tetapi kurang aman dibanding rentang terpisah per container, yang bisa dibuat lewat file konfigurasi khusus saat `lxc-create`.

Untuk container unprivileged sebagai user biasa, user harus punya rentang di `/etc/subuid` dan `/etc/subgid`, izin perangkat jaringan di `/etc/lxc/lxc-usernet` (misalnya `user veth lxcbr0 10`), dan konfigurasi `~/.config/lxc/default.conf` dengan `lxc.idmap` yang cocok. Pada cgroup v2, setiap pemanggilan `lxc-*` perlu dibungkus `systemd-run --scope -p "Delegate=yes" --` agar user mendapat cgroup yang didelegasikan.

## API dan binding

LXC menyediakan C API stabil dan beberapa binding. API diberi versi: penambahan boleh terjadi pada liblxc1, tetapi simbol yang ada tidak dihapus atau diubah tanpa menaikkan nama menjadi liblxc2. Versi pertama dengan API stabil adalah LXC 1.0.0, dan hanya simbol di `lxccontainer.h` yang dianggap publik; sisanya internal dan bisa berubah.

Binding Python memungkinkan menjalankan fungsi di konteks container. Contoh pola yang ditunjukkan dokumentasi: memeriksa state, menunggu `get_ips`, lalu menjalankan `apt-get update` dan `dist-upgrade` lewat `attach_wait` dengan `lxc.attach_run_command`, kemudian mematikan container dengan `shutdown(30)` atau `stop()`.

## LXC dan Docker

Keduanya memakai namespace dan cgroup, tetapi fokusnya berbeda. LXC mengelola **system container**: container menjalankan init dan lingkungan OS lengkap, perubahan persisten, dan pengalaman mirip VM ringan. [[References/Docker\|Docker]] mengelola **application container** berbasis image berlapis dengan registry dan siklus build/run yang lebih sempit. Untuk host yang menjalankan banyak layanan mirip mesin, LXC lebih dekat ke model VM; untuk mendistribusikan aplikasi, Docker lebih langsung. Keduanya masuk kategori beban kerja yang disiapkan di [[References/Provisioning\|Provisioning]].

## Catatan sumber

Halaman Documentation di linuxcontainers.org hanya membahas command line (yang diarahkan ke man page), API C, dan binding; isi operasional diambil dari Introduction, Getting started, dan News. Halaman Getting started masih menyarankan LXC 6.0 dan Ubuntu 18.04 sebagai host, jadi rekomendasi versi dan distro di sana sudah usang; paket distro tetap jalur instalasi yang dianjurkan. Halaman Introduction juga tertinggal soal dukungan LTS. LXC tidak terpasang di mesin ini, sehingga `lxc-checkconfig` dan perilaku container tidak diuji lokal; klaim versi dan fitur bersandar halaman resmi per 26 November 2026.

## Sumber

- [What's LXC?](https://linuxcontainers.org/lxc/introduction/), diakses 26 November 2026.
- [Getting started](https://linuxcontainers.org/lxc/getting-started/), diakses 26 November 2026.
- [Documentation](https://linuxcontainers.org/lxc/documentation/), diakses 26 November 2026.
- [News](https://linuxcontainers.org/lxc/news/), diakses 26 November 2026.
