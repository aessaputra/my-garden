---
{"dg-publish":true,"dg-path":"AI-Assisted Coding.md","permalink":"/ai-assisted-coding/","title":"AI-Assisted Coding","hideInFiletree":true,"tags":["programming","coding","gpt","research","testing","security","workflow"],"noteIcon":"","dg-note-properties":{"title":"AI-Assisted Coding","category":"references","tags":["programming","coding","gpt","research","testing","security","workflow"],"sources":["_raw/articles/ai-assisted-coding-research-packet.md"],"created":"2026-09-01","updated":"2026-09-02","confidence":"medium"}}
---

AI-assisted coding memakai model AI untuk membantu pengembang memahami, menulis, mengubah, menguji, dan meninjau kode.

Bentuknya mencakup autocomplete, percakapan tentang kode, penyuntingan lintas berkas, serta agen yang dapat menjalankan alat dan memeriksa hasil.

AI mempercepat produksi kandidat solusi. Pengembang tetap menentukan maksud, batas perubahan, standar mutu, dan keputusan apakah hasil layak masuk ke produk.

Mekanisme model dasarnya dibahas di [[References/Cara Kerja LLM\|Cara Kerja LLM]]. Perbedaannya dari pengembangan deterministik dibahas di [[References/AI dan Pengembangan Perangkat Lunak Tradisional\|AI dan Pengembangan Perangkat Lunak Tradisional]].

## Cara kerja

Asisten membaca prompt dan konteks yang tersedia, seperti kode di sekitar kursor, berkas aktif, dokumentasi, atau indeks repositori.

Model lalu memprediksi teks atau perubahan yang sesuai. Hasilnya dapat berupa satu baris, fungsi, test, penjelasan, refactor, atau diff lintas berkas.

[Dokumentasi GitHub](https://docs.github.com/en/copilot/responsible-use/copilot-code-completion) menyatakan bahwa saran dapat melengkapi kode, membuat blok baru, serta mengubah atau menghapus kode.

Kualitas hasil dibatasi oleh kualitas konteks. Model tidak otomatis mengetahui kebutuhan bisnis, aturan internal, versi dependensi, atau batas keamanan yang tidak diberikan.

Agen menambah kemampuan bertindak. Ia dapat mencari kode, mengedit berkas, menjalankan perintah, membaca kegagalan, lalu mencoba perbaikan.

Otonomi tidak menghapus kebutuhan review. Analisis [Anthropic](https://www.anthropic.com/research/impact-software-development) menemukan bahwa feedback loop dengan validasi manusia tetap umum pada coding agent.

## Pekerjaan yang cocok

AI paling berguna ketika tugas dapat dijelaskan, dibatasi, dan diverifikasi dengan biaya rendah.

Contohnya meliputi boilerplate, dokumentasi, penjelasan kode, pembuatan test awal, migrasi pola berulang, pencarian sumber bug, dan prototipe.

AI juga dapat mengusulkan edge case, menerjemahkan contoh antarbahasa, atau menyiapkan alternatif implementasi untuk dibandingkan.

Untuk perubahan lintas berkas, nilai utamanya adalah kecepatan eksplorasi. Risikonya juga meluas karena satu asumsi salah dapat tersebar ke banyak lokasi.

Tugas berisiko tinggi memerlukan kendali lebih ketat. Contohnya adalah autentikasi, otorisasi, pembayaran, migrasi data, kriptografi, infrastruktur, dan kode keselamatan.

## Alur kerja

### 1. Tetapkan kontrak tugas

Nyatakan masalah, perilaku yang diharapkan, batas perubahan, versi teknologi, larangan, dan kriteria penerimaan.

Pisahkan pekerjaan besar menjadi unit kecil. Diff kecil lebih mudah dipahami, diuji, dibatalkan, dan ditinjau daripada perubahan massal.

### 2. Berikan konteks terpilih

Sertakan antarmuka, dokumentasi, test terkait, konvensi repositori, dan contoh pola yang benar.

Jangan mengirim secret, data pribadi, atau kode terlarang oleh kebijakan organisasi. Periksa kebijakan retensi data dan penggunaan konten penyedia.

### 3. Minta rencana sebelum perubahan

Untuk tugas nontrivial, minta AI menjelaskan berkas yang akan disentuh, asumsi, risiko, dan cara verifikasi.

Rencana bukan bukti kebenaran. Gunanya adalah membuat asumsi terlihat sebelum kode berubah.

### 4. Inspeksi diff

Periksa apakah perubahan memenuhi kebutuhan tanpa memperluas ruang lingkup. Pastikan kode dapat dipahami oleh tim tanpa riwayat percakapan AI.

Tinjau logika, error handling, konkurensi, kompatibilitas, dependensi, performa, aksesibilitas, dan dampak operasional yang relevan.

### 5. Jalankan pemeriksaan deterministik

Kompilasi dan jalankan formatter, linter, type checker, unit test, integration test, serta pemeriksaan keamanan yang sesuai.

[GitHub](https://docs.github.com/en/copilot/tutorials/review-ai-generated-code) menyarankan automated test dan static analysis sebagai pemeriksaan awal, bukan pengganti human review.

### 6. Uji perilaku nyata

Uji happy path, input salah, batas data, kegagalan jaringan, izin pengguna, rollback, dan observability.

Test buatan AI juga harus ditinjau. Assertion yang hanya mengulang implementasi dapat lulus tanpa membuktikan kebutuhan pengguna.

### 7. Catat keputusan

Simpan alasan arsitektur, asumsi, dan batas solusi di kode atau dokumentasi proyek.

Kode produksi harus tetap dapat dipelihara ketika model, alat, atau percakapan asal tidak tersedia.

## Bukti produktivitas

Bukti saat ini bercampur. Hasil bergantung pada jenis tugas, pengalaman pengembang, kedekatan dengan repositori, mutu konteks, dan biaya verifikasi.

Eksperimen [GitHub](https://github.blog/2022-09-07-research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness) melibatkan 95 pengembang profesional.

Kelompok Copilot menyelesaikan pembuatan HTTP server JavaScript 55 persen lebih cepat. Temuan ini mendukung manfaat pada tugas singkat dan terdefinisi jelas.

RCT [METR](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study) menguji 16 pengembang berpengalaman pada 246 issue nyata di repositori besar yang mereka kenal.

Dengan alat AI awal 2025, peserta memerlukan waktu 19 persen lebih lama. Mereka sebelumnya memperkirakan akan lebih cepat dan tetap merasa terbantu setelah eksperimen.

Kedua hasil tidak harus saling membatalkan. Mereka mengukur populasi, alat, repositori, durasi, dan standar keberhasilan yang berbeda.

Kesimpulan yang aman: AI dapat mempercepat tugas tertentu, tetapi keuntungan harus diukur setelah review, pengujian, koreksi, dan integrasi.

Jangan memakai jumlah baris kode atau acceptance rate sebagai ukuran tunggal. Ukur lead time, rework, defect, incident, review time, dan hasil pengguna.

[DORA](https://dora.dev/insights/balancing-ai-tensions) menemukan adopsi AI berkaitan dengan throughput lebih tinggi sekaligus delivery instability lebih tinggi.

DORA menyebut AI sebagai amplifier. Platform internal, API, workflow, dan test yang kuat membantu AI, sedangkan sistem rapuh dapat menghasilkan technical debt lebih cepat.

## Risiko

### Kode meyakinkan tetapi salah

Model dapat membuat API yang tidak ada, mengabaikan edge case, salah membaca versi, atau memberi implementasi yang lolos kompilasi tetapi tidak memenuhi kebutuhan.

Kefasihan bukan confidence yang terkalibrasi. Setiap klaim tentang API, konfigurasi, dan perilaku library perlu diperiksa pada dokumentasi atau eksekusi nyata.

### Keamanan dan supply chain

Kode dapat berfungsi sambil memakai default tidak aman, validasi lemah, hak akses berlebihan, secret tertanam, atau autentikasi yang tidak lengkap.

[OWASP](https://owasp.org/www-project-citizen-development-top10-security-risks/content/2022/en/CD-SEC-07-Security-Misconfiguration) memperingatkan bahwa solusi fungsional dapat melewatkan kontrol yang tidak diminta.

Model juga dapat menyarankan package yang tidak ada atau tidak aman. Nama package, versi, maintainer, checksum, dan riwayat keamanan harus diverifikasi sebelum instalasi.

[OWASP GenAI](https://genai.owasp.org/llmrisk/llm09-overreliance) mencatat risiko penyerang menerbitkan package berbahaya dengan nama yang sering dihalusinasikan model.

### Privasi dan kekayaan intelektual

Prompt dapat membocorkan source code, data pelanggan, credential, atau detail arsitektur kepada layanan eksternal.

Tim perlu menetapkan data yang boleh dikirim, mode penyimpanan yang diizinkan, kontrol akses, audit, serta aturan untuk keluaran yang mirip kode publik.

### Penurunan pemahaman

Delegasi penuh dapat membuat pengembang menerima kode tanpa membangun model mental yang diperlukan untuk debugging dan pemeliharaan.

Dalam RCT kecil [Anthropic](https://www.anthropic.com/research/AI-assistance-coding-skills), kelompok AI mendapat skor penguasaan 17 poin persentase lebih rendah.

Keuntungan waktu pada studi itu kecil dan tidak signifikan secara statistik. Kesenjangan terbesar muncul pada pertanyaan debugging.

Temuan tersebut masih terbatas pada 52 peserta, satu library Python yang belum dikenal, dan intervensi singkat. Ia menunjukkan risiko, bukan hukum universal.

### Verification tax

Generasi cepat memindahkan pekerjaan ke audit, koreksi, dan integrasi. DORA menyebut biaya tersembunyi ini sebagai verification tax.

Prototype dapat selesai cepat, tetapi production readiness menuntut edge case, keamanan, observability, dokumentasi, dan integrasi dengan sistem internal.

## Kontrol tim

Tetapkan kebijakan yang membedakan saran inline, chat, dan agen. Semakin luas izin alat, semakin ketat sandbox, logging, approval, dan review gate yang diperlukan.

Gunakan least privilege. Agen tidak perlu akses production, secret, atau perintah destruktif untuk mengerjakan perubahan lokal biasa.

Wajibkan branch terpisah, diff review, status check, secret scanning, dependency scanning, SAST, serta persetujuan manusia untuk area sensitif.

Pertahankan test yang independen dari implementasi. Jangan izinkan agen menghapus, melemahkan, atau melewati test hanya untuk memperoleh status hijau.

Untuk pembelajaran, minta penjelasan, prediksi hasil, dan alasan trade-off. Sesekali kerjakan bagian inti tanpa generasi agar kemampuan membaca dan debugging tetap terlatih.

Evaluasi alat memakai tugas repositori nyata. Catat waktu total, kualitas, rework, insiden, biaya, serta pengalaman pengembang sebelum memperluas adopsi.

## Batas kesimpulan

Fitur dan kemampuan model berubah cepat. Angka produktivitas terikat pada tanggal, alat, tugas, populasi, dan metode penelitian.

Sebagian bukti berasal dari vendor. Gunakan temuan itu bersama studi independen dan pengukuran internal, bukan sebagai jaminan hasil.

Confidence halaman ini medium. Definisi, risiko, dan kontrol didukung kuat, tetapi besar dampak produktivitas tidak dapat digeneralisasi.

## Lihat juga

- [[References/AI Agents\|AI Agents]]
- [[References/Skills pada AI Coding Assistants\|Skills pada AI Coding Assistants]]
- [[References/Prompt Engineering\|Prompt Engineering]]
- [[References/Google Antigravity\|Google Antigravity]]
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/AI dan Pengembangan Perangkat Lunak Tradisional\|AI dan Pengembangan Perangkat Lunak Tradisional]]
- [[References/Generative AI for Frontend Development\|Generative AI for Frontend Development]]
- [[References/Claude Code\|Claude Code]]
- [[References/GitHub\|GitHub]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [GitHub Docs: Application card untuk inline suggestions](https://docs.github.com/en/copilot/responsible-use/copilot-code-completion), kemampuan, evaluasi, serta batas saran kode.
- [GitHub Docs: Review AI-generated code](https://docs.github.com/en/copilot/tutorials/review-ai-generated-code), pemeriksaan fungsional, mutu, keamanan, dan integrasi.
- [GitHub: Riset produktivitas Copilot](https://github.blog/2022-09-07-research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness), eksperimen terkontrol pada tugas JavaScript.
- [METR: Dampak AI awal 2025 pada pengembang berpengalaman](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study), RCT issue nyata di repositori besar.
- [DORA: Balancing AI tensions](https://dora.dev/insights/balancing-ai-tensions), throughput, instability, verification tax, skill, dan workflow gap.
- [Anthropic: Dampak AI pada software development](https://www.anthropic.com/research/impact-software-development), analisis 500.000 interaksi coding.
- [Anthropic: Dampak bantuan AI pada pembentukan skill coding](https://www.anthropic.com/research/AI-assistance-coding-skills), RCT pemahaman dan debugging.
- [OWASP: Security Misconfiguration](https://owasp.org/www-project-citizen-development-top10-security-risks/content/2022/en/CD-SEC-07-Security-Misconfiguration), risiko default tidak aman.
- [OWASP GenAI: LLM09 Misinformation](https://genai.owasp.org/llmrisk/llm09-overreliance), halusinasi, package palsu, verifikasi, dan human oversight.
