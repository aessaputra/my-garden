---
{"dg-publish":true,"dg-path":"Red Hat Enterprise Linux.md","permalink":"/red-hat-enterprise-linux/","title":"Red Hat Enterprise Linux","hideInFiletree":true,"tags":["references","devops","security"],"noteIcon":"","dg-note-properties":{"title":"Red Hat Enterprise Linux","aliases":["RHEL","Red Hat Enterprise Linux"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","security"],"sources":["_raw/articles/rhel-product-2026-11-26.md","_raw/articles/rhel-life-cycle-2026-11-26.md","_raw/articles/rhel-10-3-overview-2026-11-26.md","_raw/articles/rhel-10-3-major-changes-2026-11-26.md","_raw/articles/rhel-10-3-revision-history-2026-11-26.md","_raw/articles/rhel-10-3-architectures-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"medium"}}
---

Red Hat Enterprise Linux (RHEL) adalah distribusi Linux komersial dari Red Hat yang dijual lewat langganan, bukan sekadar unduhan gratis. Posisinya adalah fondasi untuk beban kerja perusahaan: dari pusat data dan mesin virtual sampai cloud publik dan edge. Halaman produknya menempatkan RHEL sebagai "stable foundation" untuk modernisasi, container, dan AI; klaim semacam itu adalah pemasaran vendor, sedangkan angka siklus hidup di bawah berasal dari kebijakan dukungan resmi Red Hat.

## Rilis dan versi

RHEL 10 adalah rilis mayor terbaru. *Revision history* release notes mencatat RHEL 10.3 dirilis 1 Oktober 2026; halaman produk yang diambil pada 26 November 2026 masih menulis RHEL 10.2 dan 9.8 sebagai rilis terkini, jadi salinan halaman produk itu tertinggal dan tidak dipakai sebagai acuan versi. Rilis mayor menyediakan varian **Server** (fisik, virtual, cloud) dan **Workstations** (grafis dan komputasi ilmiah), dengan penekanan pada kesiapan AI, *image mode*, *confidential computing*, dan *post-quantum*.

RHEL tersedia di AWS, Microsoft Azure, Google Cloud, IBM Cloud, dan Oracle Cloud Infrastructure, dengan citra khusus per penyedia. Arsitektur yang didukung RHEL 10.3: x86-64-v3, ARMv8.0-A, POWER10 atau lebih baru, serta IBM z15/LinuxONE III; kernelnya 6.12.0-264.7.1. Release notes juga menyebut dukungan sistem berbasis Arm AGI CPU dan rencana dukungan Intel Diamond Rapids pada Q2 2027.

## Rilis 10.3: perubahan utama

Release notes 10.3 merangkum perubahan di beberapa area:

- **Installer dan image**: `image-builder` CLI kini *generally available*; web console dapat membuat image `bootc` dari base image apa pun di local store; blueprint mendukung first-boot script dan registrasi otomatis ke Satellite atau Ansible Automation Platform; Kickstart dapat menambah anchor certificate untuk HTTPS. `bootc-image-builder` deprecated tetapi didukung sampai akhir hayat RHEL 10.
- **Edge**: deployment lewat FIDO Device Onboard (FDO); `greenboot` berbasis Bash diganti `greenboot-rs`, dan `fdo-rs` diganti `go-fdo`.
- **Keamanan dan PQC**: `libreswan` 5.4 mendukung ML-KEM-768/1024 di IKEv2 dan RFC 9370; `audit` 4.2.1 mendukung TLS 1.3 PQC untuk remote logging; SSH di mode FIPS memilih hybrid `mlkem768nistp256-sha256` dan `mlkem1024nistp384-sha384`; `crypto-policies` menambah subpolicy `NO-HMAC-SHA1`; `fapolicyd` 2.0.1, Keylime 7.14.3 (termasuk verifikasi AMD SEV-SNP), `rust-sequoia-sqv` 1.5.0 (RFC 9980), dan OpenSC 0.27.1 ikut diperbarui.
- **Kernel**: subsistem eBPF dan `perf` di-rebase ke upstream 6.19; `rtla` mendapat aksi ambang eBPF; IMA appraisal diperketat; dukungan IBM Z (`libzpc` 2), Intel RDT/QAT/IAA, serta driver NPU `amdxdna` sebagai Technology Preview.
- **Toolchain**: GCC Toolset 16 (GCC 16.2, Binutils 2.46, fitur C++26 Reflection dan Contracts, default `-std=gnu++20`), LLVM Toolset 22.1.8, Rust 1.97 (mangling v0), Go 1.27.3 (generic methods, `encoding/json/v2`, `crypto/mldsa`), dan GDB 17.2 (CET shadow stack, AArch64 GCS).
- **Observability**: Grafana 12.4.3 pindah ke arsitektur Scenes dan menghapus legacy alerting serta plugin AngularJS; PCP 7.1.5 menambah perbaikan arsip otomatis; plugin Grafana PCP 6.1.0 mendukung Grafana 12.
- **Identity dan desktop**: modul `ipauser`/`ipagroup` dapat `state: query`; SID Active Directory dapat dipetakan ke authentication indicator; GNOME Kiosk menambah panel aksesibilitas, magnifier, dan opsi mematikan animasi.

Daftar ini contoh, bukan seluruh isi release notes; rincian per komponen ada di halaman *New features and enhancements*.

## Siklus hidup dan dukungan

RHEL 8, 9, dan 10 memakai siklus sepuluh tahun dalam tiga fase produksi, lalu dukungan tambahan opsional:

| Fase | Durasi | Isi |
|---|---|---|
| Full Support | 5 tahun | errata, rilis minor, peningkatan fitur, image instalasi baru |
| Maintenance Support | 5 tahun | errata saja; tanpa rilis minor atau fitur baru |
| Extended Life Phase | 4 tahun (ELCP) | errata untuk minor tertentu; tanpa bug fix umum atau hardware enablement baru |

Di atas itu ada **Extended Life Cycle Premium (ELCP)**: 6 tahun dukungan dari tanggal GA untuk rilis minor genap (.2, .4, .6, .8) yang memenuhi syarat, dan 9 tahun untuk rilis minor terakhir (.10). ELCP mulai berlaku sejak RHEL 9 dan menggantikan EUS, Enhanced EUS, dan E4S; pelanggan SAP pindah dari E4S ke ELCP mulai RHEL 10.6. Untuk kebutuhan lebih panjang ada **Long-Life (LL) add-on**, perpanjangan tahunan yang bisa diperbarui, sehingga errata bisa mencapai 14 tahun atau lebih. Bila LL tidak diperpanjang, konten lama tetap tersedia di kanal arsip tanpa pembaruan baru.

Kadens rilis minor berbeda menurut langganan: RHEL Standard memberi siklus 6 bulan per rilis minor, sedangkan RHEL Premium memberi 2 tahun untuk rilis minor yang memenuhi syarat. Red Hat menyatakan tanggal "End of Full Support" dan "End of Maintenance Support" adalah perkiraan yang bisa berubah, bukan janji pasti.

## Errata dan kompatibilitas

Errata keamanan (RHSA) mengikuti kriteria tetap: CVE Critical, Important, dan Moderate dengan skor CVSS 7 atau lebih, berlaku sejak 1 April 2025. Bug fix (RHBA) mencakup Urgent dan Selected High Priority. Errata bersifat kumulatif: pembaruan minor memuat seluruh errata sebelumnya. Red Hat menargetkan kompatibilitas biner untuk runtime inti lintas rilis minor dan errata, termasuk untuk container, meski pengecualian dimungkinkan untuk masalah keamanan Critical.

Application Streams, yaitu paket versi tertentu di luar basis sistem, umumnya mengikuti siklus 10 tahun, tetapi sebagian punya masa hidup lebih pendek mengikuti upstream. Tidak ada Application Stream baru selama fase Maintenance dan Extended; satu versi per stream dipilih saat fase Maintenance dimulai.

## Manajemen, keamanan, dan ekosistem

Fitur yang ditonjolkan halaman produk: **Red Hat Lightspeed** untuk peringatan kerentanan dan panduan perbaikan, **live kernel patching** agar kernel bisa ditambal tanpa reboot, profil keamanan dan sertifikasi standar, serta rantai pasok perangkat lunak yang terverifikasi. Untuk beban kerja container, RHEL 10 menonjolkan *image mode* agar konfigurasi sistem tidak melenceng dan bisa di-rollback.

Manajemen paket memakai `yum`/`dnf`, yang di halaman ini berperan sebagai contoh manajer paket tingkat OS dalam perbandingan di [[References/Package Managers\|Package Managers]]. Portofolio yang sering dipasangkan: Red Hat OpenShift untuk platform container, Ansible Automation Platform untuk otomasi, dan Red Hat AI untuk model kecil yang disesuaikan.

## Dokumentasi

Dokumentasi resmi berada di `docs.redhat.com` dan dipisah per versi mayor (10, 9, 8, 7, 6, 5, 4). Untuk setiap rilis minor, *release notes* memakai struktur tetap: Overview, Major changes, In-place upgrade, Architectures, Distribution of content (installation, repositories, Application Streams), Important changes to external kernel parameters, Device drivers, New features and enhancements per area, Technology Preview, Developer Preview, Removed features, Deprecated features, Known issues, Fixed issues, Available BPF features, List of tickets by component, dan Revision history. Di luar *What's new*, portal yang sama memuat jalur **Get started** dan **Troubleshoot**, serta PDF referensi.

Halaman portal itu sendiri adalah indeks navigasi; isi di halaman ini diverifikasi dari empat halaman anak (overview, major changes, revision history, architectures), bukan dari daftar tautannya. Karena dokumentasi mengikuti versi, rujukan fitur harus selalu menyebut rilis minor, bukan hanya "RHEL 10".

## Migrasi dari CentOS Linux 7

CentOS Linux 7 mencapai end of life pada 30 Juni 2024. Red Hat memasarkan RHEL sebagai tujuan migrasi yang didukung penuh untuk pengguna CentOS Linux, dengan jalur migrasi langsung; ini klaim vendor yang perlu diverifikasi per kasus, bukan jaminan bahwa setiap aplikasi CentOS berjalan tanpa perubahan.

## Catatan sumber

Halaman produk bersifat pemasaran: frasa seperti "stable foundation", "ideal OS", dan "30 years of open source community leadership" adalah klaim vendor, bukan fakta terukur. Angka siklus hidup, kriteria errata, dan aturan ELCP/LL diambil dari halaman *Red Hat Enterprise Linux Life Cycle* di Customer Portal, yang menyebut dirinya dapat berubah. Halaman ini mencampur ketiganya, jadi diperlakukan `confidence: medium`. Sebagai pembanding, siklus Debian yang jauh lebih pendek (3 tahun penuh plus 2 tahun LTS) dijelaskan di [[References/Debian\|Debian]]; perbedaan ini menjelaskan mengapa RHEL dipilih untuk beban kerja yang butuh dukungan bertahun-tahun, dengan biaya langganan sebagai trade-off. Kebutuhan keamanan operasional seperti patch dan pemantauan kerentanan dibahas di [[References/Server Security\|Server Security]].

Ketidaksesuaian versi: halaman produk yang diambil 26 November 2026 masih menyebut 10.2 dan 9.8 sebagai rilis terkini, sedangkan *revision history* release notes mencatat 10.3 dirilis 1 Oktober 2026. Dua sumber ini bertabrakan; yang dipakai adalah release notes karena memuat tanggal rilis eksplisit, dan halaman produk diperlakukan sebagai salinan yang tertinggal. Rincian 10.3 adalah snapshot rilis tersebut, bukan rekomendasi untuk langsung melakukan upgrade.

## Sumber

- [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux), halaman produk, diakses 26 November 2026.
- [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata), Red Hat Customer Portal, diakses 26 November 2026.
- [Portal dokumentasi RHEL](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/), indeks navigasi; isi diambil dari halaman anak di bawah.
- [Overview of Red Hat Enterprise Linux 10.3](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/whats_new-rhel_10_3_overview), dokumentasi resmi.
- [Major changes in RHEL 10.3](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/whats_new-rhel_10_3_major_changes), dokumentasi resmi.
- [Revision history](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/whats_new-rhel_10_3_revision_history), dokumentasi resmi; mencatat rilis 10.3 pada 1 Oktober 2026.
- [Architectures for Red Hat Enterprise Linux 10.3](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/whats_new-rhel_10_3_architectures), dokumentasi resmi.
