---
{"dg-publish":true,"dg-path":"Deployment.md","permalink":"/deployment/","title":"Deployment","hideInFiletree":true,"tags":["references","deployment","devops","performance"],"dg-note-properties":{"title":"Deployment","category":"references","tags":["references","deployment","devops","performance"],"sources":["_raw/articles/deployment-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Deployment adalah proses memindahkan versi aplikasi yang telah dibangun ke environment tempat pengguna atau sistem lain dapat mengaksesnya.

Untuk website sederhana, proses ini dapat berarti menaruh HTML, CSS, JavaScript, gambar, dan font pada hosting, lalu menghubungkan domain melalui DNS.

Aplikasi dinamis juga memerlukan runtime, konfigurasi, secret, jaringan, database, migrasi, health check, observabilitas, scaling, serta prosedur pemulihan.

Deployment bukan sekadar upload file. Ia menghubungkan source, build, artifact, environment, rollout, verifikasi, operasi, dan rollback dalam satu lifecycle perubahan.

## Source, build, dan artifact

Source code, asset, dependency, dan konfigurasi menjadi input build. Build dapat melakukan transpilation, bundling, minification, optimasi asset, pengujian, serta packaging.

Hasilnya adalah artifact yang dapat diterapkan, seperti direktori statis, archive, binary, function bundle, atau container image.

Artifact idealnya immutable. Artifact yang sama dipromosikan antar-environment agar staging dan produksi tidak menjalankan hasil build yang berbeda tanpa disengaja.

[Release Engineering](https://sre.google/sre-book/release-engineering/) menekankan build yang reproducible, otomatis, dan repeatable. Input serta toolchain perlu dipin agar release tidak menjadi hasil unik yang sulit dibuat ulang.

[Multi-stage build](https://docs.docker.com/build/building/best-practices/) memisahkan tahap kompilasi dari image runtime. Hasil akhir dapat lebih kecil karena hanya memuat berkas yang diperlukan aplikasi.

Image kecil tetap memerlukan patching, scanning, dependency pinning, provenance, serta konfigurasi runtime yang aman.

## Environment dan konfigurasi

Development, preview, staging, dan production memiliki tujuan berbeda. Pemisahan ini membatasi blast radius dan memberi tempat untuk verifikasi sebelum trafik produksi dialihkan.

Environment sebaiknya memiliki konfigurasi, secret, data, domain, izin, dan dependency yang disengaja. Kesamaan dengan produksi perlu cukup tinggi untuk menghasilkan pengujian bermakna.

[GitHub Environments](https://docs.github.com/actions/deployment/targeting-different-environments/using-environments-for-deployment) dapat menahan job dan secret sampai protection rule terpenuhi.

Approval, branch restriction, concurrency, dan policy eksternal dapat menjadi gate. Gate yang terlalu lemah membuka bypass, sedangkan gate berlebihan memperlambat pemulihan dan perubahan aman.

Secret tidak boleh masuk ke repository, artifact publik, bundle browser, atau log. Akses pipeline sebaiknya minimal, berjangka pendek, dapat diaudit, dan dibatasi per-environment.

## Continuous delivery dan deployment

Continuous integration mengotomatisasi integrasi, build, dan pengujian. Continuous delivery menjaga perubahan siap dirilis, tetapi keputusan produksi masih dapat memerlukan persetujuan.

Continuous deployment menerapkan perubahan yang lolos pipeline tanpa persetujuan manual. Istilah ini kadang dipakai tidak konsisten, sehingga definisi tim perlu dinyatakan eksplisit.

[GitHub Actions](https://docs.github.com/actions/deployment/about-deployments/deploying-with-github-actions) mendukung trigger, environment, concurrency group, protection rule, review, log, dan deployment record.

Otomasi mengurangi variasi manual, tetapi memperbesar dampak credential atau workflow yang disusupi. Pipeline harus diperlakukan sebagai infrastruktur produksi.

## Strategi rollout

All-at-once mengganti seluruh versi aktif sekaligus. Strategi ini sederhana dan cepat, tetapi dapat menyebabkan downtime atau blast radius penuh bila versi baru bermasalah.

Rolling update mengganti instance secara bertahap. [Kubernetes Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) mengontrol laju penggantian melalui replica, surge, dan unavailability.

Blue-green menjalankan versi lama dan baru pada environment terpisah. Trafik dialihkan setelah versi baru diuji, sedangkan versi lama dipertahankan untuk pemulihan cepat.

Canary mengirim sebagian kecil trafik ke versi baru. [Google SRE](https://sre.google/workbook/canarying-releases/) menjelaskan bahwa pembatasan paparan dapat menurunkan biaya defect sambil menghasilkan bukti produksi.

Strategi rollout tidak selalu eksklusif. Tim dapat memakai rolling update untuk instance, canary untuk trafik, dan feature flag untuk mengendalikan perilaku aplikasi.

Pemilihan strategi bergantung pada biaya kapasitas, state, toleransi downtime, volume trafik, kualitas sinyal, kecepatan rollback, dan kompatibilitas versi.

## Health check dan verifikasi

Build yang berhasil hanya membuktikan pipeline menghasilkan artifact. Deployment baru dinilai sehat setelah startup, dependency, route, data, security, performa, dan perilaku pengguna diverifikasi.

Readiness check menentukan kapan instance menerima trafik. Liveness check membantu mendeteksi proses yang macet. Keduanya harus menguji kondisi yang relevan tanpa menciptakan beban atau restart loop.

Smoke test memeriksa jalur kritis segera setelah rollout. Pengujian sintetis, transaction check, serta pemeriksaan error dan latency membantu mendeteksi kegagalan yang tidak terlihat dari status proses.

Verifikasi harus membandingkan versi baru dengan baseline. Alarm yang hanya melihat nilai absolut dapat melewatkan regresi kecil atau bereaksi pada variasi normal.

## Database dan state

Rollback aplikasi tidak otomatis membalikkan schema, data, queue message, file, cache, atau efek samping pada layanan eksternal.

Migrasi database sebaiknya kompatibel dengan versi lama dan baru selama rollout. Pola expand and contract mengurangi kebutuhan perubahan schema yang harus terjadi serentak dengan kode.

Backup perlu disertai restore test. Snapshot tanpa prosedur pemulihan, target waktu, dan validasi integritas belum menjadi strategi rollback data yang memadai.

Aplikasi stateful memerlukan perhatian pada session, file lokal, lock, scheduler, idempotency, replica, dan koneksi panjang sebelum memakai rollout bertahap atau horizontal scaling.

## Rollback, roll forward, dan feature flag

Rollback memulihkan artifact atau konfigurasi sebelumnya. Roll forward menerapkan perbaikan baru. Pilihan ditentukan oleh kecepatan, kompatibilitas data, tingkat kerusakan, dan kepastian perbaikan.

[Kubernetes](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) menyimpan revision untuk rollback workload. Mekanisme platform ini tidak menjamin dependency dan data juga kembali aman.

Feature flag memisahkan deployment kode dari aktivasi fitur. Flag dapat membatasi pengguna yang terpapar, tetapi menambah kombinasi state dan utang konfigurasi bila tidak memiliki owner serta tanggal penghapusan.

Prosedur pemulihan harus dilatih. Rollback yang belum diuji sering gagal saat paling dibutuhkan, sebuah bentuk konsistensi yang agak tidak membantu.

## Keamanan supply chain

[OWASP CI/CD Security](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html) menyoroti credential hygiene, access control, dependency, artifact integrity, konfigurasi, logging, dan visibility.

Pipeline dapat menjalankan kode dengan akses luas. Pull request, action pihak ketiga, build script, package lifecycle, runner, registry, dan deployment token merupakan bagian dari attack surface.

Artifact perlu diidentifikasi dengan digest dan diverifikasi sebelum deployment. Tag yang dapat berubah tidak cukup untuk membuktikan image yang diuji sama dengan image yang dijalankan.

[SLSA](https://slsa.dev/spec/v1.2/faq) memakai provenance untuk menjelaskan bagaimana artifact dibangun dan pihak yang mengendalikan proses tersebut. Provenance tetap harus diverifikasi terhadap policy.

## Domain, TLS, cache, dan performa

Deployment web juga mencakup DNS, domain, TLS, redirect, compression, cache header, CDN, routing, serta invalidasi asset.

DNS dan certificate issuance dapat bersifat asynchronous. Perubahan perlu memperhitungkan TTL, propagasi, validasi kepemilikan, serta jalur pemulihan bila domain salah diarahkan.

[Web performance guidance](https://web.dev/articles/web-performance-made-easy) merekomendasikan text compression dan cache policy efisien agar resource tidak dikirim ulang tanpa kebutuhan.

Asset dengan content hash dapat dicache lama karena URL berubah bersama isi. HTML dan response dinamis memerlukan policy berbeda agar pengguna tidak menerima shell lama atau data privat dari shared cache.

## Observabilitas

Deployment perlu dikorelasikan dengan versi, commit, artifact digest, konfigurasi, waktu rollout, dan environment.

[OpenTelemetry](https://opentelemetry.io/docs/concepts/observability-primer/) menjelaskan metric, log, dan trace sebagai sinyal observabilitas. Ketiganya membantu melihat gejala, konteks, serta alur request.

Pantau error rate, latency, saturation, throughput, availability, business metric, dan dependency. Sinyal teknis yang sehat belum tentu berarti checkout, login, atau alur penting pengguna berhasil.

Retensi dan cardinality perlu dirancang. Telemetry berlebihan dapat mahal, sedangkan telemetry tanpa dimensi versi deployment menyulitkan diagnosis regresi.

## Metrik delivery

[DORA metrics](https://dora.dev/guides/dora-metrics/) mencakup change lead time, deployment frequency, failed deployment recovery time, change fail rate, dan deployment rework rate.

Metrik tersebut menilai throughput serta instability secara bersama. Deployment yang sering bukan keberhasilan bila banyak perubahan memerlukan rollback, hotfix, atau pemulihan panjang.

Gunakan tren untuk memperbaiki sistem delivery, bukan menilai individu. Target tunggal dapat mendorong perubahan kecil tanpa nilai atau menyembunyikan insiden agar angka terlihat baik.

## Workflow deployment yang aman

- Build artifact sekali, uji artifact tersebut, lalu promosikan identitas yang sama.
- Pisahkan environment, secret, data, domain, serta izin dengan jelas.
- Terapkan test, review, protection rule, dan policy sesuai risiko perubahan.
- Pilih strategi rollout berdasarkan state, blast radius, sinyal, dan kemampuan rollback.
- Jalankan health check, smoke test, serta verifikasi metric sebelum menyelesaikan rollout.
- Rencanakan migrasi data, backup, restore, rollback, dan roll forward secara terpisah.
- Catat commit, artifact digest, konfigurasi, approver, waktu, dan hasil deployment.
- Pantau keamanan, reliability, performa, pengalaman pengguna, serta biaya setelah rilis.

## Lihat juga

- [[References/Web Hosting\|Web Hosting]]
- [[GitHub\|GitHub]]
- [[References/Vercel\|Vercel]]
- [[References/Netlify\|Netlify]]
- [[References/Railway\|Railway]]
- [[References/Render\|Render]]
- [[References/Cloudflare\|Cloudflare]]
- [[References/Cache-Control\|Cache-Control]]
- [[References/HTTPS\|HTTPS]]

## Sumber

- [MDN: Your first website](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Your_first_website): publikasi kode dan asset agar website tersedia online.
- [GitHub: Deploying with GitHub Actions](https://docs.github.com/actions/deployment/about-deployments/deploying-with-github-actions): trigger, environment, concurrency, protection, review, log, dan record.
- [GitHub: Managing environments](https://docs.github.com/actions/deployment/targeting-different-environments/using-environments-for-deployment): environment secret dan protection rule.
- [Kubernetes Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment): declarative update, rolling rollout, revision, dan rollback.
- [AWS Deployment Strategies](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/deployment-strategies.html): in-place, rolling, blue-green, canary, linear, dan all-at-once.
- [Google SRE: Canarying Releases](https://sre.google/workbook/canarying-releases/): canary, error budget, automation, dan evaluasi produksi.
- [Google SRE: Release Engineering](https://sre.google/sre-book/release-engineering/): reproducible build, automation, artifact, dan release velocity.
- [DORA Metrics](https://dora.dev/guides/dora-metrics/): throughput, instability, recovery, failure, dan rework.
- [OWASP CI/CD Security](https://cheatsheetseries.owasp.org/cheatsheets/CI_CD_Security_Cheat_Sheet.html): credential, pipeline, artifact, access, logging, dan monitoring.
- [SLSA FAQ](https://slsa.dev/spec/v1.2/faq): provenance, reproducible build, runner, dan tingkat kepercayaan artifact.
- [Docker Build Best Practices](https://docs.docker.com/build/building/best-practices/): multi-stage build, image, cache, pinning, dan rebuild.
- [OpenTelemetry Observability Primer](https://opentelemetry.io/docs/concepts/observability-primer/): metric, log, trace, dan pemahaman sistem.
- [Web Performance Made Easy](https://web.dev/articles/web-performance-made-easy): optimasi asset, compression, cache, dan validasi regresi.
