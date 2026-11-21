---
{"dg-publish":true,"dg-path":"Provisioning.md","permalink":"/provisioning/","title":"Provisioning","hideInFiletree":true,"tags":["references","devops","deployment","configuration","security"],"noteIcon":"","dg-note-properties":{"title":"Provisioning","aliases":["Provisioning Infrastruktur"],"categories":["Software Systems"],"type":"reference","tags":["references","devops","deployment","configuration","security"],"sources":["_raw/articles/provisioning-research-packet-2026-11-14.md"],"created":"2026-11-14","updated":"2026-11-14","confidence":"high"}}
---

Provisioning adalah proses menyiapkan resource komputasi beserta identitas, akses, jaringan, dan penyimpanan sampai resource itu siap dipakai — dan karena resource juga berakhir, pelepasan serta penghapusannya adalah bagian dari siklus yang sama. Istilah ini bukan nama satu produk atau satu proses tunggal: ia mencakup penyediaan mesin, akun, perangkat jaringan, tenant layanan, sampai sertifikat, dan batasnya dengan configuration management tidak tajam.

Pada praktik self-hosted, provisioning VPS berarti menyiapkan mesin, akses awal, jaringan, penyimpanan, lalu menyerahkan hasilnya ke tahap [[References/Deployment\|Deployment]]. Pada cloud, penekanannya bergeser ke sifat swalayan: kapabilitas dapat diprovision dan dilepas dengan usaha manajemen minimal, dan konsumen dapat memprovision sebagian kemampuan tanpa interaksi manusia dengan provider.

## Lingkup yang berbeda-beda

- Komputasi: mesin fisik, mesin virtual, container, dan node klaster.
- Jaringan: alamat, subnet, firewall, dan konfigurasi perangkat jaringan. Untuk perangkat jaringan, standar seperti NETCONF menyediakan mekanisme memasang, memanipulasi, dan menghapus konfigurasi melalui RPC.
- Identitas dan akses: pembuatan akun, keanggotaan grup, peran, serta pencabutannya.
- Penyimpanan dan layanan pendukung: volume, bucket, database, DNS, dan tenant aplikasi.
- Sertifikat dan kunci: penerbitan serta pembaruan, seperti tahap provisioning pada [[References/Caddy\|Caddy]] ketika konfigurasi dimuat dan sertifikat TLS diurus.

Model layanan cloud menegaskan pembagian tanggung jawab: pada IaaS, penyedia memberi kemampuan memprovision processing, storage, dan jaringan, sedangkan konsumen mengelola sistem operasi, penyimpanan, dan aplikasi di atasnya. Provisioning karena itu tidak hanya soal membuat mesin, tetapi juga soal siapa yang berwenang membuat, mengubah, dan menghapus apa.

## Deklaratif, state, dan drift

Pendekatan modern menuliskan konfigurasi sebagai end state, bukan rangkaian langkah imperatif. Alat seperti Terraform dan OpenTofu bekerja melalui API platform dengan alur write, plan, apply, serta menyusun resource graph untuk menentukan urutan dan paralelisme. Pendekatan ini muncul sebagai koreksi terhadap praktik lama berbasis script dan runbook manual yang membuat environment baru tidak selalu repeatable dan konsisten.

Alat deklaratif memerlukan state: pemetaan antara objek nyata dan konfigurasi yang menjadi sumber kebenaran, dan alat tersebut tidak dapat bekerja tanpanya. Sebelum operasi, state diselaraskan dengan infrastruktur nyata, sehingga perbedaan antara keduanya dapat terlihat. Konsekuensinya, deklaratif bukan jaminan bebas drift; ia hanya membuat drift terdeteksi bila proses penyelarasan dan verifikasi benar-benar dijalankan. State juga dapat memuat data sensitif, sehingga tidak layak disimpan pada media tanpa locking dan kontrol akses.

Provisioner yang menjalankan perintah setelah resource dibuat (menyalin file, menjalankan skrip lokal atau remote) diperlakukan sebagai opsi terakhir: perilakunya tidak dapat dimodelkan secara andal, umumnya membutuhkan akses jaringan dan kredensial, dan validasi host key SSH pada koneksi awal dimatikan secara default. Untuk pekerjaan setelah resource hidup, alat configuration management lebih tepat.

## Image, bootstrap, dan pendaftaran node

Pola yang lazim memisahkan pembuatan image dari konfigurasi runtime. Machine image adalah unit statis berisi sistem operasi dan software yang sudah terpasang, dipakai untuk membuat mesin baru dengan cepat; alat pembuat image tidak menggantikan configuration management. Di sisi runtime, mekanisme inisialisasi instance membaca metadata dan user data saat boot, lalu mengatur jaringan, penyimpanan, dan akses awal seperti kunci SSH.

Configuration management melengkapi bagian itu dengan menyatakan desired state dan bersifat idempoten: bila sistem sudah sesuai, tidak ada perubahan meskipun dijalankan berulang. Ansible, misalnya, bekerja tanpa agen melalui SSH dengan komponen control node, inventory, dan managed node. Idempotensi adalah sifat yang harus dirancang dan diuji, bukan efek otomatis dari memakai alat tertentu.

Pada klaster, pendaftaran node pun bagian dari provisioning. Node dapat mendaftar sendiri melalui kubelet atau ditambahkan manual; objek node yang tidak valid tetap disimpan dan kesehatannya terus diperiksa sampai objek itu dihapus eksplisit. Nama node harus unik, dan sistem mengasumsikan nama yang sama menandakan state yang sama, sehingga instance yang diubah tanpa mengganti nama dapat menimbulkan inkonsistensi. Setelah itu, controller bekerja sebagai control loop yang menggerakkan state saat ini menuju desired state melalui API server; controller tidak menjalankan beban sendiri. Alat pembentuk klaster seperti kubeadm dapat dijadikan bagian dari sistem provisioning yang lebih besar, misalnya Ansible atau Terraform.

## Provisioning identitas dan deprovisioning

Pada identitas, provisioning berarti pembuatan identitas dan peran secara otomatis, termasuk pemeliharaan dan penghapusan saat status atau peran berubah. Standar SCIM menyediakan skema user umum dan protokol berbasis HTTP untuk mengelola identitas lintas domain, dengan tujuan menurunkan biaya dan kompleksitas user management. Layanan direktori menjalankan siklus awal lalu siklus inkremental berulang, dan siklus itu berlanjut sampai dihentikan secara manual.

Jalurnya tidak selalu SCIM. Sebagian aplikasi hanya mendukung just-in-time provisioning melalui [[References/SAML\|SAML]], yaitu akun terbentuk saat pengguna pertama kali masuk — berbeda dari sinkronisasi akun terjadwal. Perbedaan ini penting ketika [[References/Authentication\|Authentication]] dan otorisasi dibahas: masuk berhasil tidak otomatis berarti akun, grup, dan peran sudah selaras di semua sistem.

Deprovisioning bukan pelengkap administratif. Layanan provisioning umumnya menonaktifkan atau menghapus lunak akun yang keluar dari cakupan secara default, dan pengembang aplikasi disarankan mendukung penghapusan lunak sekaligus permanen. Kontrol akun menuntut akun dibuat, diaktifkan, diubah, dinonaktifkan, dan dihapus sesuai proses, penggunaannya dipantau, dan prosesnya diselaraskan dengan terminasi serta transfer personel. Akun yatim yang tertinggal karena penghapusan lunak adalah risiko keamanan, bukan sekadar ketidakrapian data; pembahasannya bersinggungan dengan [[References/Server Security\|Server Security]].

## Tata kelola konfigurasi

Provisioning yang baik adalah bagian dari configuration management yang berorientasi keamanan. Baseline configuration harus dikembangkan, didokumentasikan, dipelihara di bawah configuration control, dan ditinjau ketika komponen dipasang atau di-upgrade. Perubahan yang dikendalikan konfigurasi ditinjau dan disetujui dengan pertimbangan dampak keamanan, didokumentasikan, dan dicatat. Configuration settings ditetapkan serta diimplementasikan, setiap deviasi didokumentasikan dan disetujui, dan perubahannya dipantau serta dikendalikan.

Terjemahan praktisnya: kredensial otomasi (token API provider, kunci SSH, token SCIM), sumber image, dan parameter jaringan adalah bagian dari baseline. Membatasi hak akses otomasi dan menyimpan state atau rahasia pada penyimpanan yang tepat sama pentingnya dengan skrip provisioning itu sendiri. Prinsip dan kontrol host yang lebih luas ada pada [[References/Web Security Knowledge\|Web Security Knowledge]] dan [[References/Server Security\|Server Security]].

## Batas klaim dan jebakan

- Deklaratif tidak menghapus drift; tanpa penyelarasan state, rencana perubahan, dan verifikasi, perbedaan tetap tersembunyi.
- Container dan immutable infrastructure tidak menghapus provisioning: host atau node, jaringan, volume, image, kredensial, dan identitas tetap disiapkan. Container hanya memindahkan unit distribusi ke image, dan registry menyimpan image yang ditarik saat dijalankan.
- SCIM menstandarkan skema dan protokol identitas, bukan model izin aplikasi target; otorisasi tetap ditentukan aplikasi.
- Penghapusan lunak sebagai default berarti akun bisa tetap ada; kontrol akun menuntut pemantauan dan penghapusan yang benar-benar terjadi.
- Provisioning menyiapkan resource, bukan menjamin keamanan sesudahnya: patching, hardening, dan pemantauan tetap pekerjaan terpisah.
- Tidak ada klaim waktu provisioning, throughput, atau kapasitas dalam catatan ini karena tidak ada benchmark yang dijalankan. Aktivasi layanan telekomunikasi tidak diverifikasi dalam paket riset ini dan tidak dijadikan klaim.

## Sumber

- [Terraform — What is Terraform?](https://developer.hashicorp.com/terraform/intro)
- [Terraform — State](https://developer.hashicorp.com/terraform/language/state)
- [Terraform — Provisioners](https://developer.hashicorp.com/terraform/language/provisioners)
- [OpenTofu — Getting started](https://opentofu.org/docs/intro/)
- [Packer — Introduction](https://developer.hashicorp.com/packer/docs/intro)
- [cloud-init Documentation](https://cloud-init.readthedocs.io/en/latest/)
- [Ansible — Introduction to Ansible](https://docs.ansible.com/ansible/latest/getting_started/introduction.html)
- [Ansible — Getting started](https://docs.ansible.com/ansible/latest/getting_started/index.html)
- [Kubernetes — Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/)
- [Kubernetes — Controllers](https://kubernetes.io/docs/concepts/architecture/controller/)
- [Kubernetes — Creating a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)
- [NIST SP 800-145 — The NIST Definition of Cloud Computing](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-145.pdf)
- [NIST SP 800-128 — Guide for Security-Focused Configuration Management](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-128.pdf)
- [NIST SP 800-53 Rev. 5 — OSCAL catalog](https://raw.githubusercontent.com/usnistgov/oscal-content/main/nist.gov/SP800-53/rev5/json/NIST_SP-800-53_rev5_catalog.json)
- [RFC 7643 — SCIM Core Schema](https://www.rfc-editor.org/rfc/rfc7643.html)
- [RFC 7644 — SCIM Protocol](https://www.rfc-editor.org/rfc/rfc7644.html)
- [Microsoft Entra ID — What is app provisioning?](https://learn.microsoft.com/en-us/entra/identity/app-provisioning/user-provisioning)
- [Microsoft Entra ID — How Application Provisioning works](https://learn.microsoft.com/en-us/entra/identity/app-provisioning/how-provisioning-works)
- [RFC 6241 — NETCONF](https://datatracker.ietf.org/doc/html/rfc6241)
- [AWS — Infrastructure as code](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html)
- [Docker — What is Docker?](https://docs.docker.com/get-started/docker-overview/)
