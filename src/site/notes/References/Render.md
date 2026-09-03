---
{"dg-publish":true,"dg-path":"Render.md","permalink":"/render/","title":"Render","hideInFiletree":true,"tags":["references","deployment","devops","performance"],"noteIcon":"","dg-note-properties":{"title":"Render","category":"references","tags":["references","deployment","devops","performance"],"sources":["_raw/articles/render-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Render adalah platform cloud terkelola untuk menjalankan situs statis, aplikasi web, service privat, worker, cron job, workflow, dan datastore.

Platform ini mengabstraksikan build, deployment, networking, scaling, penyimpanan, TLS, observabilitas, dan sebagian pengelolaan infrastruktur.

Render dapat melayani frontend dan backend. Model operasionalnya lebih luas daripada static hosting, tetapi tetap memiliki batas runtime, paket, region, dan konfigurasi khusus platform.

## Model layanan

[Service Types](https://render.com/docs/service-types) membedakan web service, static site, private service, background worker, cron job, dan workflow.

Render juga menyediakan Postgres dan Key Value terkelola. Pemilihan tipe service menentukan akses jaringan, lifecycle proses, scaling, dan penyimpanan yang tersedia.

Web service menerima trafik HTTP publik. Private service hanya tersedia melalui private network. Background worker menjalankan proses berkelanjutan tanpa endpoint publik.

Cron job menjalankan perintah terjadwal. Static site menyajikan hasil build tanpa proses server aplikasi yang terus hidup.

## Source dan deployment

[Web service](https://render.com/docs/web-services) dapat dibangun dan diterapkan pada setiap push ke branch Git yang terhubung. Repository publik juga dapat digunakan tanpa koneksi akun penuh.

Build command menghasilkan artefak aplikasi. Start command menjalankan proses. Aplikasi web harus bind ke `0.0.0.0` dan port yang diharapkan agar router Render dapat menjangkaunya.

[Deployment](https://render.com/docs/deploys) memakai instance baru sebelum menggantikan versi aktif untuk service yang didukung. Persistent disk menonaktifkan pola zero-downtime ini.

Rollback aplikasi tidak otomatis membalikkan migrasi database, perubahan data, atau efek samping pada service eksternal.

## Static site dan CDN

[Static Sites](https://render.com/docs/static-sites) membangun frontend dari repository dan menyajikannya melalui CDN global. Push ke branch yang dipilih dapat memicu deployment otomatis.

Setiap site mendapat subdomain `onrender.com` dan dapat memakai custom domain. Situs dinamis yang memerlukan proses server harus memakai web service.

Static site tetap menggunakan kuota bandwidth dan pipeline minute menurut paket workspace. Klaim gratis perlu dibaca bersama limit dan kebijakan harga terbaru.

## Blueprint dan preview

[Blueprint](https://render.com/docs/blueprint-spec) memakai `render.yaml` untuk mendeskripsikan service, datastore, runtime, build, environment, disk, dan scaling sebagai infrastructure as code.

Konfigurasi berbasis file mempermudah review dan reproduksi. Nilai secret tidak boleh ditulis langsung ke repository; gunakan placeholder dan isi nilainya melalui dashboard.

[Preview Environment](https://render.com/docs/preview-environments) dapat membuat instance service dan datastore dari Blueprint untuk pull request.

Preview tidak menyalin data dari resource yang sudah ada. Ia juga memakai resource baru, sehingga isolasi secret, biaya, seed data, dan lifecycle penghapusan perlu diperiksa.

## Konfigurasi dan secret

[Environment Variables and Secrets](https://render.com/docs/configure-environment-variables) mendukung environment variable, secret file, serta environment group untuk berbagi konfigurasi.

Secret yang tersimpan aman tetap dapat bocor melalui log, response aplikasi, dependency, atau bundle browser. Nilai frontend yang dikirim ke browser harus dianggap publik.

Perubahan konfigurasi dapat memicu deployment baru. Pisahkan nilai produksi, staging, dan preview agar pengujian tidak mengakses data atau layanan produksi tanpa sengaja.

## Networking, domain, dan TLS

Service dalam region yang sama dapat berkomunikasi melalui private network. Public web service juga mendapat subdomain bawaan dan dapat memakai custom domain.

Render menyediakan [sertifikat TLS terkelola](https://render.com/docs/tls) untuk domain yang memenuhi konfigurasi DNS. Sertifikat diterbitkan dan diperbarui otomatis.

DNS, verifikasi domain, port, health check, dan redirect HTTPS tetap perlu diuji. TLS terkelola tidak memperbaiki aplikasi yang salah bind, salah route, atau tidak sehat.

## Scaling dan region

[Scaling](https://render.com/docs/scaling) mendukung jumlah instance tetap dan autoscaling untuk service tertentu. Trafik masuk dibagi ke instance yang sedang berjalan.

Autoscaling tersedia pada paket tertentu. Setiap instance memakai compute plan dan ditagihkan tersendiri. Persistent disk mencegah service diskalakan ke beberapa instance.

Aplikasi horizontal harus menghindari session dan state penting di filesystem lokal. Job, scheduler, WebSocket, cache lokal, serta migrasi harus aman ketika instance bertambah.

Region sebaiknya dipilih berdasarkan pengguna, database, dependency eksternal, data locality, dan kepatuhan. Resource yang saling berkomunikasi idealnya ditempatkan dengan sengaja.

## Penyimpanan

[Persistent Disks](https://render.com/docs/disks) mempertahankan perubahan filesystem melewati deploy dan restart untuk web service, private service, serta worker berbayar tertentu.

Filesystem service bersifat ephemeral tanpa disk. Persistent disk hanya tersedia bagi satu instance, menonaktifkan zero-downtime deploy, dan tidak dapat dipakai oleh cron job.

Disk bukan pengganti backup. Database dan data produksi tetap memerlukan backup, restore test, pemantauan kapasitas, serta prosedur migrasi dan pemulihan.

## Observabilitas

Render menyediakan [log](https://render.com/docs/logging) untuk service dan deployment. Retensi log bergantung pada paket workspace, sedangkan static site tidak menghasilkan runtime log.

[Service Metrics](https://render.com/docs/service-metrics) mencakup CPU, memory, disk, HTTP request, bandwidth, dan aktivitas database sesuai tipe resource.

Sebagian metrik dan detail memerlukan paket tertentu. Untuk retensi atau analisis lebih lama, log dan metrik dapat diteruskan ke sistem observabilitas eksternal yang sesuai.

## Biaya dan trade-off

Paket workspace, compute plan, instance, bandwidth, pipeline minute, disk, database, dan custom domain dapat memengaruhi biaya.

[Workspace Plans](https://render.com/docs/new-workspace-plans) berubah pada 23 April 2026. Harga, nama paket, kuota, retensi, dan fitur dapat berubah lagi, sehingga dokumentasi terbaru tetap menjadi acuan.

Render mengurangi pekerjaan provisioning, deployment, TLS, private networking, scaling, dan observabilitas dasar. Sebagai gantinya, aplikasi bergantung pada lifecycle, limit, region, dan harga platform.

[[References/Netlify\|Netlify]] dan [[References/Vercel\|Vercel]] berfokus kuat pada workflow web dan frontend. [[References/Railway\|Railway]] dan Render lebih menyerupai platform aplikasi untuk frontend, backend, worker, dan datastore.

Pilihan perlu mempertimbangkan runtime, state, private networking, preview, scaling, persistence, observabilitas, biaya, dan kontrol operasional, bukan dukungan framework saja.

## Workflow yang aman

- Pilih tipe service sesuai lifecycle, trafik, dan kebutuhan jaringan.
- Verifikasi build command, start command, runtime, host, port, dan health check.
- Pisahkan secret dan data untuk preview, staging, dan produksi.
- Simpan konfigurasi nonsecret yang sesuai dalam `render.yaml` dan review melalui Git.
- Uji deploy, rollback, scaling, disk, backup, restore, domain, dan TLS sebelum rilis.
- Pantau log, metrik, penggunaan resource, kuota, dan biaya setelah deployment.

## Lihat juga

- [[References/Deployment\|Deployment]]
- [[References/Railway\|Railway]]
- [[References/Netlify\|Netlify]]
- [[References/Vercel\|Vercel]]
- [[References/Cloudflare\|Cloudflare]]
- [[References/Web Hosting\|Web Hosting]]
- [[GitHub\|GitHub]]

## Sumber

- [Services and Service Types](https://render.com/docs/service-types): tipe service, datastore, lifecycle, dan akses jaringan.
- [Web Services](https://render.com/docs/web-services): Git deployment, runtime, port binding, domain, dan private network.
- [Static Sites](https://render.com/docs/static-sites): build statis, CDN, Git, domain, TLS, dan penggunaan.
- [Deploying on Render](https://render.com/docs/deploys): build, pre-deploy, start, zero-downtime deploy, dan persistent disk.
- [Scaling Render Services](https://render.com/docs/scaling): manual scaling, autoscaling, instance, billing, dan batas disk.
- [Environment Variables and Secrets](https://render.com/docs/configure-environment-variables): variable, secret file, environment group, dan Blueprint.
- [Persistent Disks](https://render.com/docs/disks): persistence, filesystem ephemeral, batas instance, dan deployment.
- [Regions](https://render.com/docs/regions): lokasi resource dan komunikasi antarlayanan.
- [Fully Managed TLS Certificates](https://render.com/docs/tls): penerbitan serta pembaruan sertifikat.
- [Custom Domains](https://render.com/docs/custom-domains): domain, DNS, verifikasi, dan batas paket.
- [Preview Environments](https://render.com/docs/preview-environments): resource per pull request, data, instance, dan biaya.
- [Logs in the Render Dashboard](https://render.com/docs/logging): log service, deploy, request, pencarian, dan retensi.
- [Service Metrics](https://render.com/docs/service-metrics): CPU, memory, disk, HTTP, bandwidth, dan database.
- [New Workspace Plans](https://render.com/docs/new-workspace-plans): paket, fitur, bandwidth, domain, dan perubahan harga.
- [Blueprint YAML Reference](https://render.com/docs/blueprint-spec): service, runtime, build, environment, scaling, disk, dan database.
