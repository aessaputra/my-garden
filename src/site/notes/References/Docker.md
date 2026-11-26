---
{"dg-publish":true,"dg-path":"Docker.md","permalink":"/docker/","title":"Docker","hideInFiletree":true,"tags":["references","docker","devops","deployment"],"noteIcon":"","dg-note-properties":{"title":"Docker","aliases":["Docker Engine","Docker platform","Docker Desktop"],"categories":["Software Systems"],"type":"reference","tags":["references","docker","devops","deployment"],"sources":["_raw/articles/docker-docs-home-2026-11-26.md","_raw/articles/docker-overview-2026-11-26.md","_raw/articles/docker-install-ubuntu-2026-11-26.md","_raw/articles/mooc-devops-with-docker-2026-11-26.md","_raw/articles/mooc-devops-docker-spring-2026-2026-11-26.md","_raw/articles/prepare-docker-course-2026-11-26.md","_raw/articles/prepare-docker-ch1-intro-2026-11-26.md","_raw/articles/prepare-docker-ch3-layers-2026-11-26.md","_raw/articles/prepare-docker-ch8-networking-2026-11-26.md","_raw/articles/prepare-docker-ch9-security-2026-11-26.md","_raw/articles/google-cloud-what-are-containers-2026-11-26.md"],"created":"2026-11-26","updated":"2026-11-26","confidence":"high"}}
---

Docker adalah platform terbuka untuk mengembangkan, mengirim, dan menjalankan aplikasi. Ia memisahkan aplikasi dari infrastruktur sehingga perangkat lunak bisa dikirim cepat, dan infrastruktur dikelola dengan cara yang sama seperti aplikasi. Intinya adalah container: lingkungan terisolasi ringan yang membawa aplikasi beserta dependensinya, sehingga host tidak perlu menyediakan versi Python, library, atau konfigurasi yang sama.

## Container versus mesin virtual

Docker menyebut container sebagai alternatif yang ringan dan hemat biaya dibanding mesin virtual berbasis hypervisor. Container berjalan di atas kernel host dan berbagi kernel itu, sementara VM membawa sistem operasi sendiri. Karena itu Docker bisa menjalankan lebih banyak beban kerja pada perangkat keras yang sama, dan cocok untuk lingkungan padat maupun deployment kecil dan menengah. Workload yang sama bisa berjalan di laptop, mesin fisik atau virtual di data center, atau cloud, lalu dideploy sebagai container atau layanan terorkestrasi. Posisi ini melengkapi pembahasan alur rilis di [[References/Deployment\|Deployment]], tempat container menjadi artifact yang dipromosikan antar-environment.

## Konsep container dan penggunaannya

Google Cloud mendefinisikan container sebagai paket kode aplikasi beserta dependensi seperti versi runtime dan library. Yang dipaketkan adalah image; container adalah instance yang berjalan. Manfaat yang disebut: pemisahan tanggung jawab (developer fokus logika, operasi fokus deployment), portabilitas beban kerja, isolasi aplikasi, batch processing, dan hybrid cloud.

Penggunaannya: pengembangan agile, efisiensi operasi, menjalankan di mana saja, microservices (satu container per service), modernisasi aplikasi lama, dan pipeline CI/CD yang konsisten. Untuk banyak container, orkestrasi seperti [[Kubernetes\|Kubernetes]] mengambil alih penjadwalan dan replikasi; [[References/LXC\|LXC]] menunjukkan container sistem yang lebih mirip VM ringan.

## Arsitektur client-server

Docker memakai arsitektur client-server. Client `docker` mengirim perintah ke daemon `dockerd`, yang membangun, menjalankan, dan mendistribusikan container. Keduanya bisa berada di mesin yang sama atau terpisah, dan berkomunikasi lewat REST API melalui UNIX socket atau antarmuka jaringan. Docker Compose adalah client lain yang bekerja dengan aplikasi berisi beberapa container.

- **Docker daemon** (`dockerd`) mendengarkan permintaan API dan mengelola objek seperti image, container, network, dan volume; daemon juga bisa berkomunikasi dengan daemon lain untuk layanan Docker.
- **Docker client** (`docker`) adalah antarmuka utama; perintah seperti `docker run` dikirim ke daemon. Satu client bisa berbicara dengan lebih dari satu daemon.
- **Docker Desktop** membungkus daemon, client, Compose, Content Trust, Kubernetes, dan Credential Helper untuk Mac, Windows, atau Linux.
- **Registry** menyimpan image. Docker Hub adalah registry publik default; registry privat juga bisa dijalankan. `docker pull` dan `docker run` menarik image, `docker push` mengirim image.

## Objek: image dan container

Image adalah template read-only berisi instruksi untuk membuat container, sering diturunkan dari image lain. Dockerfile mendefinisikan langkah pembuatannya, dan setiap instruksi menghasilkan satu layer; saat Dockerfile diubah, hanya layer yang berubah yang dibangun ulang. Itu sebabnya image Docker ringan dibanding teknologi virtualisasi lain.

Container adalah instance image yang bisa dijalankan. Container dapat dibuat, dijalankan, dihentikan, dipindah, atau dihapus lewat API atau CLI, dihubungkan ke satu atau lebih network, dan diberi storage. Secara default container cukup terisolasi dari container lain dan host, dan tingkat isolasi network serta storage dapat diatur. Perubahan state container yang tidak disimpan di storage persisten akan hilang saat container dihapus.

Contoh `docker run -i -t ubuntu /bin/bash` menjalankan urutan: image `ubuntu` ditarik bila belum ada, container dibuat, filesystem read-write dialokasikan sebagai layer terakhir, network interface dibuat dan IP diberikan, `/bin/bash` dijalankan secara interaktif, lalu `exit` menghentikan container tanpa menghapusnya.

## Build, layer, dan port

Kursus prepare.sh menjelaskan layer sebagai selisih antara state sebelum dan sesudah satu instruksi. Docker memakai layer untuk cache: jika tidak ada perubahan, langkah dilewati dan versi cache dipakai, sehingga urutan instruksi menentukan berapa lama rebuild. `CMD` adalah perintah default yang mudah ditimpa argumen `docker run`, sedangkan `ENTRYPOINT` menetapkan binary yang selalu dijalankan dan argumen tambahan hanya ditempel di belakang. `ENTRYPOINT` bisa ditimpa dengan flag `--entrypoint`. Perbedaan praktisnya: override `CMD` gagal dengan jelas, sedangkan `ENTRYPOINT` bisa gagal diam-diam karena argumen asing diabaikan aplikasi.

Port di dalam container terisolasi dari port host. Port mapping membuat "pintu" dari host ke container, misalnya `-p 8080:5000`; nomor di kedua sisi tidak harus sama. Untuk data yang harus bertahan, kursus menempatkan volume dan bind mount sebagai bab tersendiri: volume bernama bertahan setelah container dihapus, sedangkan bind mount memetakan direktori host ke container.

## Jaringan

Secara default container terhubung ke bridge network privat. Container mendapat IP di bridge itu dan bisa saling mengirim trafik, tetapi IP tidak stabil dan berubah saat container dijalankan ulang, sehingga alamat IP tidak boleh dijadikan alamat tetap. Bridge default juga tidak menyediakan DNS antar-container: `getent hosts db` dari container lain tidak menemukan apa pun.

Perbaikannya adalah user-defined network. Setelah `docker network create app-net`, Docker memberi DNS otomatis sehingga container bisa dihubungi dengan namanya, misalnya `db`, tanpa peduli IP. Container dijalankan dengan `--network app-net`. Port yang tidak dipublish tidak bisa diakses dari host, tetapi tetap bisa diakses container lain di network yang sama; publish port hanya untuk membuka akses dari luar.

## Keamanan image

Kursus prepare.sh menyoroti tiga masalah yang tidak muncul sebagai error saat build atau run: container berjalan sebagai root, secret ikut tercopy ke dalam layer, dan base image tidak dipin versinya. Perbaikannya: buat user low-privilege setelah `COPY`/`RUN` terakhir dan sebelum aplikasi dijalankan, jangan copy file `.env` ke image, dan pin versi base image. Pemindaian CVE dilakukan dari luar dengan Trivy yang memeriksa image per layer dan melaporkan paket yang outdated atau exploitable. Pemindaian perlu diulang setiap build karena CVE baru bisa muncul, dan sebagian CVE belum punya perbaikan.

## Instalasi Docker Engine di Ubuntu

Prasyarat resmi: Ubuntu 64-bit 26.10, 26.04 LTS, 24.04 LTS, atau 22.04 LTS, dengan arsitektur x86_64, armhf, arm64, s390x, atau ppc64le. Distribusi turunan seperti Linux Mint tidak didukung resmi meski mungkin berjalan.

Ada peringatan firewall yang penting: port container yang dipublish melewati aturan `ufw` atau `firewalld`. Docker hanya kompatibel dengan `iptables-nft` dan `iptables-legacy`; aturan yang dibuat dengan `nft` tidak didukung, dan aturan firewall sebaiknya ditambahkan ke chain `DOCKER-USER`.

Sebelum memasang paket resmi, hapus paket tidak resmi yang berpotensi konflik: `docker.io`, `docker-compose`, `docker-compose-v2`, `docker-doc`, `docker-buildx`, `podman-docker`, serta `containerd` dan `runc` bila sudah ada. Metode instalasi yang tersedia: Docker Desktop, repository apt resmi, instalasi manual, dan convenience script yang hanya disarankan untuk pengujian.

Jalur apt resmi: pasang `ca-certificates` dan `curl`, tambahkan GPG key ke `/etc/apt/keyrings/docker.asc`, tulis repository ke `/etc/apt/sources.list.d/docker.sources`, lalu:

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Di mesin ini (Pop!_OS 24.04, basis noble) terpasang Docker 29.10.1 build c48360f dan Docker Compose v5.8.0; versi itu cocok dengan kandidat `docker-ce` 29.10.1 di repository resmi. Pemasangan pada host baru masuk lingkup [[References/Provisioning\|Provisioning]], karena langkah repository, key, dan paket dasar adalah bagian dari bootstrap mesin.

## Kursus

- **DevOps with Docker (University of Helsinki, MOOC.fi).** Empat bab: Getting started, Docker basics, Docker compose, serta Security and optimization. Kursus menekankan containerisasi, orkestrasi Compose untuk deployment satu host, dan praktik produksi seperti optimasi container serta pipeline deployment. Latihan harus dikerjakan di komputer sendiri karena Docker tidak bisa dipasang di komputer departemen.
- **Docker Course (prepare.sh).** Sembilan bab dengan video dan mesin nyata di browser, tanpa setup lokal. Babnya: intro container dan image, Dockerfile untuk Flask, layer/directive/port mapping, multi-stage build dan optimasi, tag/push/pull registry, flag `docker run`, volume versus bind mount, networking by name, dan keamanan image dengan pemindaian CVE.

## Koreksi terhadap artikel Google Cloud

1. **"Container memvirtualisasi sistem operasi."** Container bukan OS virtual; ia proses terisolasi yang berbagi kernel host. Yang di-virtualisasi adalah tampilan OS (namespace) dan batas resource (cgroup), bukan kernel atau perangkat keras.
2. **"Container adalah paket software."** Yang dipaketkan adalah image; container adalah proses yang berjalan dari image itu. Artikel mencampur keduanya.
3. **"Run anywhere."** Portabilitas berlaku selama kernel dan arsitektur cocok. Image Linux butuh kernel Linux; di macOS dan Windows, Docker Desktop menjalankan VM Linux lebih dulu.
4. **"Everything at Google runs in containers."** Klaim pemasaran. Google memang memakai container secara luas sejak Borg, tetapi "everything" tidak bisa diverifikasi dari artikel.
5. **Isolasi lebih lemah dari VM.** Kernel dibagi, jadi bug kernel atau konfigurasi longgar bisa menembus batas; seccomp, capabilities, dan rootless membantu, bukan menghilangkan risiko. Klaim "fraction of memory" juga bergantung beban kerja, bukan jaminan.

## Catatan sumber

Beranda docs.docker.com adalah portal produk (AI sandboxes, Desktop, Engine, Hub, dan seterusnya), bukan tutorial; isi teknis diambil dari halaman "What is Docker?" dan "Install Docker Engine on Ubuntu". Halaman kursus MOOC.fi dirender JavaScript sehingga teksnya diambil lewat browser, bukan `web_extract`. Edisi pada URL yang diminta sudah berakhir dan menautkan edisi spring-2026; halaman edisi itu sendiri menyebut kursus berakhir 16 Juni 2026, jadi keduanya sudah lewat pada 26 November 2026, dan daftar kursus organisasi tidak lagi bisa ditemukan. Halaman prepare.sh adalah indeks kursus; empat dari sembilan bab (1, 3, 8, 9) diambil penuh, lima lainnya hanya tercatat dari ringkasan. Klaim platform dan instalasi bersandar dokumentasi resmi; klaim operasional build, network, dan keamanan berasal dari kursus sekunder dan tidak diuji dengan container baru di mesin ini. Artikel Google Cloud adalah penjelas vendor tanpa tanggal terbit; URL dikirim dua kali dan di-ingest sekali, dan blok promosi "Solve your business challenges" di tengah artikel dipangkas.

## Sumber

- [Docker Docs](https://docs.docker.com/), portal, diakses 26 November 2026.
- [What is Docker?](https://docs.docker.com/get-started/docker-overview/), diakses 26 November 2026.
- [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/), diakses 26 November 2026.
- [DevOps with Docker](https://courses.mooc.fi/org/uh-cs/courses/devops-with-docker), edisi lama, diakses 26 November 2026.
- [DevOps with Docker, spring 2026](https://courses.mooc.fi/org/uh-cs/courses/devops-with-docker-spring-2026), diakses 26 November 2026.
- [Docker Course](https://prepare.sh/course/docker-course), indeks, diakses 26 November 2026.
- [Docker Intro: Containers, Images & Your First Container](https://prepare.sh/tutorial/docker-intro-containers-images-your-first-container?c=docker-course), diakses 26 November 2026.
- [Docker Fundamentals: Layers, Directives & Port Mapping](https://prepare.sh/tutorial/docker-fundamentals-layers-directives-port-mapping?c=docker-course), diakses 26 November 2026.
- [Docker Networking Explained](https://prepare.sh/tutorial/docker-networking-explained-connect-containers-by-name?c=docker-course), diakses 26 November 2026.
- [Docker Image Security: Best Practices & CVE scanning](https://prepare.sh/tutorial/docker-image-security-best-practices-cve-scanning?c=docker-course), diakses 26 November 2026.
- [What are Containers?](https://cloud.google.com/learn/what-are-containers), Google Cloud, diakses 26 November 2026.
- Verifikasi lokal: `docker --version` 29.10.1, `docker compose version` v5.8.0, dan `docker info` kernel 7.2.15-76070215-generic, cgroup v2, runtime runc pada Pop!_OS 24.04.
