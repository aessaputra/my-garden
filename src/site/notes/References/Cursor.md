---
{"dg-publish":true,"dg-path":"Cursor.md","permalink":"/cursor/","title":"Cursor","hideInFiletree":true,"tags":["programming","coding","gpt","ai-agents","workflow","security","development"],"noteIcon":"","dg-note-properties":{"title":"Cursor","category":"references","tags":["programming","coding","gpt","ai-agents","workflow","security","development"],"sources":["_raw/articles/cursor-research-packet.md"],"created":"2026-09-01","updated":"2026-09-02","confidence":"high"}}
---

*Cursor* adalah code editor dan coding agent berbasis codebase VS Code. Ia menggabungkan editing biasa dengan autocomplete, perubahan berbasis instruksi, pencarian repositori, terminal, dan agen lintas berkas.

Tujuannya adalah mengurangi pekerjaan repetitif dan mempercepat eksplorasi. Hasil AI tetap berupa usulan yang perlu dibatasi, ditinjau, diuji, dan dipertanggungjawabkan oleh pengembang.

Cursor merupakan penerapan khusus dari [[References/AI-Assisted Coding\|AI-Assisted Coding]]. Dasar model yang menghasilkan saran dijelaskan di [[References/Cara Kerja LLM\|Cara Kerja LLM]].

## Fondasi editor

[Dokumentasi migrasi Cursor](https://cursor.com/docs/configuration/migrations/vscode) menyatakan bahwa editor ini berbasis codebase VS Code.

Pengguna dapat mengimpor profil, pengaturan, tema, dan keybinding. Cursor melakukan rebase terhadap VS Code, tetapi dapat memakai versi yang sedikit lebih lama demi stabilitas.

Kemiripan antarmuka memudahkan migrasi, tetapi kompatibilitas tidak identik. Cursor memakai Open VSX dan proxy marketplace sendiri untuk extension pihak ketiga.

Tidak semua extension Microsoft Marketplace tersedia. [Panduan extension](https://cursor.com/docs/configuration/extensions) juga memperingatkan bahwa ID yang sama dapat menunjuk publisher berbeda di registry lain.

## Mode kerja utama

### Tab

[Cursor Tab](https://cursor.com/docs/tab/overview) adalah autocomplete yang memakai edit terbaru, kode sekitar, dan error linter.

Sarannya dapat mencakup beberapa baris, import, perpindahan ke lokasi edit berikutnya, serta perubahan terkait di berkas lain.

Tab cocok untuk kelanjutan pola yang jelas. Pengguna tetap menerima dengan Tab atau menolak dengan Escape dan melanjutkan pengetikan.

### Inline Edit

[Inline Edit](https://cursor.com/docs/inline-edit/overview) mengubah kode terpilih berdasarkan instruksi singkat tanpa membuka panel percakapan.

Mode ini cocok untuk perubahan lokal. Pekerjaan lintas berkas atau perubahan kompleks lebih tepat dipindahkan ke Agent.

### Ask

[Ask mode](https://cursor.com/help/ai-features/ask-mode) bersifat read-only. Agent dapat mencari dan menjelaskan codebase tanpa mengedit berkas.

Gunakan Ask untuk orientasi, pelacakan alur, atau pemeriksaan asumsi sebelum memberikan izin perubahan.

### Plan

[Plan mode](https://cursor.com/help/ai-features/plan-mode) mengeksplorasi codebase, dapat mengajukan pertanyaan, lalu menghasilkan rencana yang bisa ditinjau dan diedit sebelum implementasi.

Mode ini berguna ketika perubahan menyentuh banyak komponen atau memiliki keputusan arsitektur. Rencana dapat disimpan ke workspace sebagai dokumentasi tim.

### Agent

[Agent](https://cursor.com/docs/agent/overview) dapat mencari codebase dan web, mengedit beberapa berkas, menjalankan terminal, membaca hasil, lalu mengulangi pekerjaan.

Cursor menjelaskan harness Agent melalui tiga unsur: instruksi, alat, dan model. Kinerja bergantung pada ketiganya, bukan hanya kemampuan model.

## Cara Cursor memperoleh konteks

Cursor tidak memahami seluruh repositori secara sempurna. Ia memilih konteks dari kode sekitar, berkas yang dibuka, pencarian, instruksi, dan hasil alat.

Untuk simbol atau error yang pasti, Agent memakai pencarian exact dan regex. [Search](https://cursor.com/docs/agent/tools/search) juga dapat memakai Explore subagent untuk penelusuran luas.

Subagent menjaga context window utama tetap fokus dengan mengembalikan temuan terpilih, bukan seluruh hasil pencarian.

Kelemahannya tetap ada. Retrieval dapat melewatkan requirement implisit, dokumentasi lama, konfigurasi eksternal, atau hubungan yang tidak terlihat dari teks kode.

## Rules dan instruksi proyek

[Cursor Rules](https://cursor.com/docs/context/rules) memasukkan konteks persisten ke prompt. Project rules disimpan sebagai berkas `.mdc` dalam `.cursor/rules` dan dapat masuk version control.

Rules dapat selalu aktif, dipilih berdasarkan relevansi, dibatasi dengan glob, atau dipanggil manual. `AGENTS.md` dapat dipakai untuk instruksi plain Markdown.

Rules cocok untuk arsitektur, style, perintah verifikasi, dan larangan proyek. Isinya sebaiknya ringkas, konkret, serta menunjuk berkas contoh yang masih berlaku.

Rules bukan policy engine deterministik. Model tetap dapat salah menerapkan instruksi, sehingga pembatasan keamanan harus ditegakkan oleh izin, sandbox, CI, dan review.

## Alur kerja yang disarankan

### 1. Orientasi tanpa edit

Mulai dengan Ask untuk memahami struktur, dependency, test, dan titik perubahan. Minta Agent menyertakan berkas serta simbol yang mendukung kesimpulannya.

### 2. Bekukan ruang lingkup

Nyatakan hasil yang diinginkan, berkas yang boleh disentuh, larangan, kompatibilitas, kriteria penerimaan, dan perintah verifikasi.

Perubahan kecil lebih mudah ditinjau dan dibatalkan. Untuk tugas besar, gunakan Plan lalu koreksi rencana sebelum memilih Build.

### 3. Siapkan konteks

Tambahkan rules yang relevan, dokumentasi, test, contoh pola yang benar, dan batas keamanan.

Jangan menyalin seluruh repositori ke prompt. Berikan petunjuk yang membantu Agent mencari sumber kebenaran sendiri.

### 4. Kerjakan pada branch

Gunakan Git sebelum memberi Agent izin mengubah kode. Workspace edit dapat disimpan langsung tanpa persetujuan per berkas.

Checkpoints menyimpan snapshot sebelum perubahan besar. Fitur ini membantu rollback file, tetapi tidak menggantikan commit, branch, reflog, atau backup.

### 5. Inspeksi diff

Periksa scope, logika, error handling, dependensi, API, konkurensi, performa, keamanan, dan maintainability.

Jangan menilai kualitas dari status kompilasi saja. Kode dapat valid secara sintaks tetapi salah terhadap requirement atau operasi produksi.

### 6. Jalankan verifikasi

Jalankan formatter, linter, type checker, unit test, integration test, build, dan pemeriksaan keamanan yang sesuai.

Pastikan test tidak dihapus, dilemahkan, atau diubah hanya untuk membuat implementasi lulus.

### 7. Uji perilaku nyata

Periksa input ekstrem, kegagalan jaringan, izin, rollback, migrasi, observability, dan efek pada data.

Untuk perubahan UI, uji aksesibilitas, ukuran layar, keyboard, loading, error state, dan kompatibilitas browser atau perangkat.

## Checkpoints dan steering

Agent membuat checkpoint sebelum perubahan signifikan. Pengguna dapat melihat keadaan sebelumnya dan memulihkan berkas yang telah dimodifikasi.

Restore hanya mengembalikan file. Ia tidak membatalkan pesan percakapan, command eksternal, request jaringan, migrasi data, atau side effect di layanan lain.

Instruksi dapat dimasukkan ke antrean atau dikirim untuk mengarahkan Agent yang sedang bekerja. Steering berguna ketika asumsi salah terlihat sebelum tugas selesai.

## Cloud Agents

[Cloud Agents](https://cursor.com/docs/cloud-agent) menjalankan agen pada VM cloud terisolasi dengan clone repositori, dependency, startup command, secret, dan akses jaringan yang dikonfigurasi.

Mereka dapat bekerja paralel tanpa laptop lokal tetap online. Hasil dikerjakan pada branch terpisah dan dapat diserahkan sebagai pull request.

Lingkungan yang lengkap memungkinkan Agent menjalankan build dan test. Lingkungan yang tidak representatif dapat memberi hasil hijau yang gagal di sistem nyata.

Cloud Agent memperluas attack surface. Secret, domain keluar, private network, MCP, akses source control, dan izin membuka pull request harus dibatasi sesuai tugas.

## Keamanan izin

[Agent Security](https://cursor.com/docs/agent/security) menyatakan bahwa pembacaan dan pencarian kode tidak membutuhkan approval. Agent juga dapat mengubah workspace tanpa approval per berkas.

Terminal memerlukan approval secara default. Run Modes dapat mengizinkan command tertentu, tetapi Cursor menyebut mekanisme ini best-effort guardrail, bukan batas keamanan keras.

Koneksi MCP memerlukan approval. Tool tertentu dapat dimasukkan ke allowlist, yang mengurangi friksi sekaligus meningkatkan dampak jika konteks atau instruksi disusupi.

Prompt injection dan halusinasi tetap mungkin terjadi. Perlakukan repository asing, issue, dokumentasi, output terminal, halaman web, dan respons MCP sebagai input tidak tepercaya.

Auto-reload dapat mengeksekusi perubahan sebelum review. Matikan perilaku ini untuk proyek sensitif atau gunakan sandbox dengan data dan credential nonproduksi.

## Melindungi berkas sensitif

[`.cursorignore`](https://cursor.com/docs/reference/ignore-file) membatasi akses Agent, Tab, Inline Edit, dan referensi berbasis mention pada path tertentu.

Gunakan pola ini untuk secret, credential, dump data, private key, artefak besar, dan direktori yang tidak relevan.

Cursor memperingatkan bahwa perlindungan penuh tidak dijamin karena perilaku LLM tidak selalu dapat diprediksi. Secret tetap harus disimpan di secret manager dan dicabut bila terpapar.

## Privasi data

[Privacy Mode](https://cursor.com/help/security-and-privacy/privacy) memastikan kode tidak dipakai untuk training oleh Cursor atau penyedia model menurut kebijakan yang dinyatakan.

Privacy Mode tidak berarti semua pemrosesan terjadi lokal. Ketika fitur AI dipakai, prompt dan context kode dapat dikirim ke Cursor serta penyedia seperti OpenAI, Anthropic, atau Google.

Teams mengaktifkannya secara default dan admin dapat mewajibkannya. Penggunaan API key sendiri mengikuti kebijakan penyedia terkait, bukan pengaturan zero data retention Cursor.

Model tertentu dapat memiliki ketentuan retensi berbeda dan perlu persetujuan admin. Evaluasi DPA, subprocessor, lokasi pemrosesan, retensi, serta kebutuhan regulasi sebelum adopsi.

Cursor menyatakan memiliki SOC 2 Type II, ISO/IEC 27001:2022, ISO/IEC 42001:2023, dan AIUC-1. Bukti dapat diminta melalui [trust portal Cursor](https://trust.cursor.com/).

Sertifikasi menunjukkan kontrol organisasi yang dinilai. Ia tidak membuktikan bahwa setiap output Agent aman atau benar.

## Model dan biaya

Cursor mendukung model miliknya dan model pihak ketiga. Model berbeda memiliki karakter, context window, latensi, serta biaya yang berbeda.

[Models & Pricing](https://cursor.com/docs/models-and-pricing) membagi penggunaan ke Cursor Models dan Other Models. Pilihan model menentukan seberapa cepat kuota bulanan terpakai.

Harga, nama model, pool, dan batas plan berubah cepat. Periksa dashboard dan dokumentasi saat memilih langganan, bukan mengandalkan angka lama.

Nilai alat sebaiknya dihitung dari waktu total sampai perubahan terverifikasi, bukan hanya kecepatan generasi atau jumlah kode yang diterima.

## Risiko dan keterbatasan

### Konteks yang salah

Agent dapat memilih berkas yang tidak relevan, memakai dokumentasi usang, atau tidak menemukan requirement yang hanya diketahui tim.

Kurangi risiko dengan rules terpelihara, test representatif, referensi eksplisit, dan perubahan berukuran kecil.

### Kode yang meyakinkan tetapi keliru

Model dapat memakai API palsu, versi salah, dependency berisiko, validasi lemah, atau implementasi yang hanya memenuhi happy path.

Verifikasi dokumentasi, dependency, lisensi, dan keamanan. Gunakan hasil eksekusi sebagai bukti, bukan penjelasan Agent semata.

### Perubahan terlalu luas

Agent lintas berkas dapat menyebarkan asumsi yang salah dengan cepat. Checkpoint membantu pemulihan lokal, tetapi review diff tetap wajib.

### Ketergantungan alat

Penerimaan kode tanpa pemahaman melemahkan kemampuan debugging dan pemeliharaan. Minta alasan, trade-off, dan prediksi perilaku sebelum menerima perubahan inti.

### Extension supply chain

Extension berjalan dengan akses besar ke workspace dan credential pengembang. Gunakan publisher tepercaya, allowlist, cooldown instalasi, dan signature verification bila tersedia.

## Kapan Cursor tepat digunakan

Cursor sesuai untuk tim yang menginginkan editor familier dengan AI terintegrasi, pencarian codebase, aturan proyek, workflow agen, serta pilihan model.

Manfaatnya paling jelas pada tugas yang terdefinisi, memiliki test, dan dapat ditinjau. Risiko meningkat ketika tugas ambigu, akses luas, atau lingkungan tidak memiliki quality gate.

Untuk kode sensitif, keputusan harus mempertimbangkan kebijakan data, kontrol endpoint, extension, model, jaringan, serta kesiapan tim melakukan review.

Confidence halaman ini high untuk kemampuan dan kebijakan yang terdokumentasi pada 2026-09-01. Detail model, harga, antarmuka, dan plan bersifat cepat berubah.

## Lihat juga

- [[References/Google Antigravity\|Google Antigravity]]
- [[References/AI-Assisted Coding\|AI-Assisted Coding]]
- [[References/Model Context Protocol\|Model Context Protocol]]
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Claude Code\|Claude Code]]
- [[References/Git\|Git]]
- [[References/GitHub\|GitHub]]
- [[References/Pengujian di React Native\|Pengujian di React Native]]

## Sumber

- [Cursor Docs](https://cursor.com/docs), peta kemampuan editor, Agent, kustomisasi, Cloud Agents, integrasi, dan CLI.
- [VS Code Migration](https://cursor.com/docs/configuration/migrations/vscode), fondasi editor, impor profil, dan strategi rebase.
- [Tab completion](https://cursor.com/docs/tab/overview), autocomplete, multi-line edit, import, dan cross-file suggestion.
- [Inline Edit](https://cursor.com/docs/inline-edit/overview), perubahan lokal berbasis instruksi.
- [Agent overview](https://cursor.com/docs/agent/overview), harness, tools, checkpoints, antrean, dan steering.
- [Search](https://cursor.com/docs/agent/tools/search), Instant Grep, Explore subagent, serta konteks pencarian.
- [Rules](https://cursor.com/docs/context/rules), project rules, glob, mode penerapan, dan `AGENTS.md`.
- [Ask mode](https://cursor.com/help/ai-features/ask-mode), eksplorasi read-only.
- [Plan mode](https://cursor.com/help/ai-features/plan-mode), rencana yang dapat ditinjau sebelum implementasi.
- [Agent Security](https://cursor.com/docs/agent/security), approval, Run Modes, MCP, prompt injection, dan batas guardrail.
- [Ignore File](https://cursor.com/docs/reference/ignore-file), `.cursorignore`, security, dan batas perlindungan.
- [Privacy and data](https://cursor.com/help/security-and-privacy/privacy), Privacy Mode, provider, training, dan pengecualian retensi.
- [Cursor Security](https://cursor.com/security), sertifikasi, subprocessor, least privilege, dan disclosure.
- [Cloud Agents](https://cursor.com/docs/cloud-agent), VM, branch, parallel work, environment, network, dan handoff.
- [Models & Pricing](https://cursor.com/docs/models-and-pricing), model, usage pool, dan plan yang berlaku saat riset.
- [Extensions](https://cursor.com/docs/configuration/extensions), Open VSX, marketplace proxy, scanning, allowlist, dan signature.
- [Best practices for coding with agents](https://cursor.com/blog/agent-best-practices), planning, context, rollback, dan verifikasi.
