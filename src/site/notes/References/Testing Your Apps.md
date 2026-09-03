---
{"dg-publish":true,"dg-path":"Testing Your Apps.md","permalink":"/testing-your-apps/","title":"Testing Your Apps","hideInFiletree":true,"tags":["references","programming","testing","ci-cd","devops"],"noteIcon":"","dg-note-properties":{"title":"Testing Your Apps","category":"references","tags":["references","programming","testing","ci-cd","devops"],"sources":["_raw/articles/testing-your-apps-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04","confidence":"high"}}
---


Pengujian memverifikasi perilaku aplikasi pada level berbeda sebelum perubahan sampai ke pengguna.

Unit menguji fungsi kecil secara terisolasi. Integrasi menguji sambungan modul, database, dan API. Sistem menguji aplikasi utuh.

Acceptance menguji kebutuhan bisnis dari sudut pengguna pada environment mirip produksi. Lihat [jenis pengujian Atlassian](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing).

## Level pengujian

Unit memanggil kode langsung dan menilai output tanpa UI, service, atau database. Test ini murah, cepat, dan cocok dijalankan setiap perubahan.

Integrasi menguji pertukaran data antar komponen, misalnya model dengan database atau method dengan API. Lihat [porsi integration pada piramida](https://semaphore.io/blog/testing-pyramid).

System test menilai aplikasi lengkap yang terintegrasi. Acceptance test menilai kesiapan rilis terhadap kriteria bisnis.

Batas antar level tidak selalu tegas. Satu test tergolong integrasi bila menyentuh modul lain, sistem eksternal, network, atau I/O file dan database.

## Piramida pengujian

Piramida dari buku Mike Cohn menumpuk banyak unit test di dasar, sedikit integration di tengah, dan sedikit E2E di puncak.

Acuan umum adalah 70 persen unit, 20 persen integrasi, dan 10 persen E2E. Rasio tepat bergantung pada konteks proyek. Lihat [strategi piramida CircleCI](https://circleci.com/blog/testing-pyramid).

E2E meniru klik, ketik, dan alur pengguna pada browser atau perangkat nyata. Keyakinannya tertinggi, tetapi paling lambat dan paling flaky.

Cadangkan E2E untuk alur vital seperti login, checkout, dan pembayaran. Uji sisanya dengan test yang lebih cepat.

## Test-driven development

TDD menulis satu test gagal, menulis kode minimal sampai hijau, lalu refactor. Siklus ini dikenal sebagai Red Green Refactor.

Proses selalu berangkat dari daftar test eksplisit. Urutan test dipilih agar desain penting lebih dulu terbentuk.

Berpikir tentang test lebih dulu memaksa interface dirancang sebelum implementasi. Lihat [bliki TDD Fowler](https://martinfowler.com/bliki/TestDrivenDevelopment.html).

Langkah refactor wajib dijaga. Tanpa itu, kode hanya menjadi tumpukan fragmen yang kebetulan lolos test.

## Behavior-driven development

BDD menutup jarak bisnis dan teknis lewat kolaborasi lintas peran, iterasi kecil, dan contoh konkret perilaku sistem.

Contoh yang disepakati menjadi dokumentasi hidup yang dicek otomatis terhadap perilaku aplikasi. Lihat [definisi BDD Cucumber](https://cucumber.io/docs/bdd).

BDD tidak menggantikan proses agile yang ada. Ia berperan sebagai plugin yang memperkuat delivery bertahap.

Gunakan BDD untuk fitur lintas tim dengan aturan bisnis yang mudah disalahpahami. Lihat [siklus TDD IBM](https://www.ibm.com/think/topics/test-driven-development) sebagai pembanding ritme kerja.

## Pengujian di CI

Pengujian CI mengotomatisasi build dan pemeriksaan pada setiap perubahan. Setiap tahap pipeline menjawab aman tidaknya perubahan untuk lanjut.

CI memvalidasi merge ke repo bersama. Delivery menjaga kode teruji selalu siap dirilis secara aman dan terprediksi. Lihat [panduan CI Harness](https://www.harness.io/blog/testing-methodologies-for-cd-pipelines).

Strategi berlapis memberi keyakinan bertahap: unit, integrasi, regresi, E2E, performa, dan keamanan sebelum promosi ke produksi.

Test pipeline harus cepat, deterministik, dan terisolasi. Test yang berbagi state global, port, atau database menghasilkan gagal acak.

## Memilih strategi

Mulai dari piramida untuk proporsi, TDD untuk urutan penulisan unit, BDD untuk kesepakatan contoh, dan CI untuk irama eksekusi.

Jalankan unit pada setiap commit, integrasi setelah merge besar, dan E2E pada milestone rilis. Proporsi dan frekuensi menyesuaikan biaya tiap lapisan.

Coverage menunjukkan kode yang tereksekusi, bukan kualitas assertion. Angka tinggi tetap dapat menyembunyikan test yang lemah.

## Batasan

Halaman ASTQB tentang test level tidak dapat diambil karena consent wall, sehingga definisi formal ISTQB belum terverifikasi di sini.

Klaim produktivitas TDD dan BDD pada sumber ini bersifat laporan praktisi dan vendor, bukan uji terkontrol. Generalisasi lintas konteks perlu bukti tambahan.

## Terkait

- [[Small unit tests give fast feedback\|Small unit tests give fast feedback]]
- [[Integration tests catch contract mismatch\|Integration tests catch contract mismatch]]
- [[E2E tests stay small and rare\|E2E tests stay small and rare]]
- [[TDD starts from a failing test\|TDD starts from a failing test]]
- [[BDD agrees on examples before code\|BDD agrees on examples before code]]
- [[CI gates every change early\|CI gates every change early]]
- [[References/Jest\|Jest]]
- [[References/Vitest\|Vitest]]
- [[References/Cypress\|Cypress]]
- [[References/Playwright\|Playwright]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]
- [[References/Deployment\|Deployment]]
- [[References/Linters dan Formatters\|Linters dan Formatters]]

## Sumber

- [Jenis pengujian perangkat lunak](https://www.atlassian.com/continuous-delivery/software-testing/types-of-software-testing): Atlassian, level unit, integrasi, sistem, dan acceptance.
- [Piramida pengujian Semaphore](https://semaphore.io/blog/testing-pyramid): Semaphore, asal Cohn, lapisan, dan alternatif bentuk.
- [Piramida pengujian CircleCI](https://circleci.com/blog/testing-pyramid): CircleCI, rasio 70/20/10 dan irama tiap lapisan.
- [Piramida praktis Fowler](https://martinfowler.com/articles/practical-test-pyramid.html): Fowler, portofolio responsif dan maintainable.
- [Bliki TDD Fowler](https://martinfowler.com/bliki/TestDrivenDevelopment.html): Fowler, Red Green Refactor dan atribusi Kent Beck.
- [TDD menurut IBM](https://www.ibm.com/think/topics/test-driven-development): IBM, alur kerja dan efek biaya debugging.
- [BDD menurut Cucumber](https://cucumber.io/docs/bdd): Cucumber, kolaborasi, iterasi, dan dokumentasi tereksekusi.
- [Pengujian CI Harness](https://www.harness.io/blog/testing-methodologies-for-cd-pipelines): Harness, gerbang kualitas pipeline dan strategi berlapis.
