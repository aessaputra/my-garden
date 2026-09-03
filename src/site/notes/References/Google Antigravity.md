---
{"dg-publish":true,"dg-path":"Google Antigravity.md","permalink":"/google-antigravity/","title":"Google Antigravity","hideInFiletree":true,"tags":["programming","coding","gpt","research","security","workflow"],"noteIcon":"","dg-note-properties":{"title":"Google Antigravity","category":"references","tags":["programming","coding","gpt","research","security","workflow"],"sources":["_raw/articles/google-antigravity-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Google Antigravity adalah platform pengembangan agentik untuk merencanakan, menjalankan, dan memverifikasi pekerjaan perangkat lunak melalui agent AI.

Antigravity memiliki Tab completion dan inline command. Namun, menyebutnya hanya sebagai alat code completion terlalu sempit karena model utamanya mencakup agent, terminal, browser, artifact, dan orkestrasi paralel.

Google memperkenalkan produk ini pada 2025. Dokumentasi yang ditinjau juga mencakup Antigravity 2.0, sehingga beberapa konsep peluncuran awal telah berkembang menjadi ekosistem dengan aplikasi, IDE, CLI, dan SDK.

## Bentuk produk

[Antigravity 2.0](https://codelabs.developers.google.com/getting-started-google-antigravity) menyediakan aplikasi mandiri untuk mengelola banyak agent lokal, project, dan scheduled task.

Antigravity IDE tetap tersedia untuk pengembangan visual yang terintegrasi dengan editor. CLI menyediakan alur terminal, sedangkan SDK memberi kontrol programatik untuk membangun agent khusus.

Permukaan tersebut memakai kemampuan agent yang saling berkaitan, tetapi tidak identik. Pilihan sebaiknya mengikuti kebutuhan visual, terminal, headless execution, otomasi, atau integrasi programatik.

## Editor dan completion

[Editor View](https://antigravity.google/blog/introducing-google-antigravity) menyediakan Tab completion, inline command, dan agent pada side panel.

Completion membantu perubahan sinkron di sekitar kode. Agent menangani tugas yang lebih luas dengan membaca project, mengubah banyak berkas, menjalankan command, dan memeriksa hasil.

Kedua mode saling melengkapi. Completion tidak otomatis memahami seluruh requirement, sedangkan agent tetap dapat membuat asumsi salah meski memperoleh konteks lebih luas.

## Agent dan orkestrasi

[Antigravity IDE](https://antigravity.google/docs/ide/overview) memberi agent akses terkontrol ke editor, terminal, dan browser untuk tugas end-to-end.

Agent dapat membangun fitur, memperbaiki bug, melakukan riset, menguji UI, dan menghasilkan laporan. Kemampuan aktual tetap bergantung pada model, tool, permission, context, serta kualitas verifikasi.

Antigravity mendukung subagent paralel. Project dapat memakai Git worktree agar pekerjaan background berlangsung di folder terisolasi tanpa langsung bercampur dengan working tree utama.

Worktree mengisolasi perubahan Git, bukan seluruh side effect. Command, credential, jaringan, database, browser session, dan layanan eksternal masih memerlukan batas tersendiri.

## Planning dan artifact

[Artifact](https://antigravity.google/docs/artifacts) adalah deliverable terstruktur untuk menjelaskan rencana, progres, perubahan, dan hasil agent.

Bentuknya mencakup implementation plan, task list, code diff, diagram arsitektur, gambar, screenshot, walkthrough, dan browser recording.

Artifact memungkinkan review pada milestone tanpa memantau setiap tool call. Pengguna dapat memberi komentar langsung agar agent menyesuaikan rencana atau implementasi.

Artifact meningkatkan keterlihatan, tetapi bukan bukti kebenaran. Rencana dapat salah, screenshot dapat hanya mencakup happy path, dan walkthrough dapat melewatkan regresi yang tidak diuji.

## Browser dan verifikasi UI

[Browser integration](https://antigravity.google/docs/ide/browser) memungkinkan agent membuka aplikasi, berinteraksi dengan halaman, mengambil screenshot, dan merekam sesi.

Kemampuan ini membantu iterasi UI dan pengujian perilaku nyata. Agent dapat menghubungkan perubahan kode dengan hasil visual tanpa hanya mengandalkan test atau static analysis.

Browser menghadapi state, timing, autentikasi, popup, data, dan halaman tidak tepercaya. Prompt injection dari konten web juga dapat memengaruhi agent bila permission serta instruksi tidak dibatasi.

Screenshot dan recording perlu dilengkapi pemeriksaan aksesibilitas, responsive layout, keyboard, error state, loading, network failure, serta assertion yang dapat diulang.

## Permission dan sandbox

[Permission engine](https://antigravity.google/docs/permissions) mengatur akses baca dan tulis berkas, command, URL, browser action, MCP tool, serta eksekusi tanpa sandbox.

Operasi dapat diatur menjadi ask, allow, atau deny. Workspace mendapat beberapa akses bawaan, sedangkan path di luar project dan tindakan sensitif dapat memerlukan persetujuan.

Project setting dapat membatasi folder, terminal execution, akses luar folder, sandbox mode, dan permission khusus. Konfigurasi berbeda dapat diterapkan pada project tepercaya dan tidak tepercaya.

Wildcard seperti akses seluruh berkas, command, URL, atau MCP memperluas blast radius. Least privilege lebih prudent daripada menghilangkan approval demi sedikit kenyamanan.

Sandbox juga bukan batas sempurna bagi semua side effect. Token cloud, repository remote, package registry, browser profile, dan database perlu diamankan secara terpisah.

## Ekstensibilitas

Antigravity mendukung Open VSX extension, MCP server, skill, rule, workflow, plugin, hook, dan integrasi Google.

MCP dan skill menambah kemampuan khusus. Hook dapat menjalankan script pada tahap tertentu dalam lifecycle agent untuk menerapkan policy, logging, atau kontrol tambahan.

Ekstensibilitas juga memperluas supply chain. Extension, MCP server, package, script, dan skill harus diperiksa sumber, permission, update, serta perilaku jaringannya.

Instruksi dalam repository, issue, halaman web, output command, dan respons tool perlu dianggap sebagai data tidak tepercaya, bukan arahan yang otomatis boleh dijalankan.

## Model, paket, dan kuota

[Plans](https://antigravity.google/docs/plans) menyatakan bahwa akses model dan rate limit berbeda menurut paket Google AI.

Dokumentasi saat penelitian mencantumkan beberapa model Gemini, unlimited Tab completion, serta akses fitur dasar. Paket Pro dan Ultra memperoleh kuota atau pilihan model yang lebih tinggi.

Model, entitlement, rate limit, nama paket, dan harga cepat berubah. Detail tersebut harus diperiksa kembali sebelum pembelian atau standardisasi alat di dalam tim.

Hasil juga dapat berubah menurut model dan reasoning mode. Perbandingan alat harus memakai tugas repository nyata, quality gate yang sama, serta biaya verifikasi penuh.

## Data dan telemetry

[Settings](https://antigravity.google/docs/settings) menyediakan toggle telemetry. Saat aktif, interaksi dapat dikumpulkan untuk evaluasi, pengembangan, dan peningkatan Antigravity serta model pendukung.

Pengguna dapat menonaktifkan telemetry menurut dokumentasi. Ketentuan rinci tetap tunduk pada Terms of Service dan kebijakan yang berlaku pada akun serta paket.

Sebelum memakai repository sensitif, periksa data yang dikirim, retensi, training, subprocessor, lokasi pemrosesan, kontrol admin, audit, dan kewajiban organisasi.

Telemetry off tidak mengubah secret menjadi aman untuk prompt. Credential, data pelanggan, private key, dan informasi produksi tetap harus dipisahkan dari workspace agent.

## Risiko dan keterbatasan

Agent dapat mengarang API, salah memahami requirement, memilih dependency berisiko, mengubah scope, atau menghasilkan test yang hanya membenarkan implementasinya sendiri.

Akses terminal dan browser membuat kesalahan dapat memiliki side effect nyata. Command destruktif, publish, deployment, migrasi data, dan perubahan layanan eksternal memerlukan review serta konfirmasi yang sesuai.

Parallel agent meningkatkan throughput sekaligus risiko konflik, duplikasi, dan asumsi tidak konsisten. Pisahkan task, worktree, contract, dan ownership sebelum menjalankan pekerjaan bersamaan.

Klaim produktivitas dalam sumber resmi bersifat vendor-authored. Halaman ini tidak menemukan studi independen yang cukup untuk mengukur dampak Antigravity secara umum.

## Dibanding alat terkait

[[References/Cursor\|Cursor]] juga memadukan completion, editor, agent, terminal, MCP, dan pekerjaan paralel. Perbedaan antarmuka, model, cloud execution, permission, data policy, serta harga perlu diuji langsung.

[[References/GitHub Copilot\|GitHub Copilot]] tersedia di beberapa editor dan permukaan GitHub, dari inline suggestion sampai coding agent. Antigravity lebih menonjolkan command center agent dan verifikasi visual lintas browser.

Google menyarankan Antigravity untuk agent manager dan pengembangan visual. Gemini CLI lebih sesuai untuk pengguna terminal, headless execution, piping, CI/CD, atau automation script.

Semua pilihan berada dalam spektrum [[References/AI-Assisted Coding\|AI-Assisted Coding]]. Nilainya tidak ditentukan oleh jumlah kode yang dihasilkan, tetapi oleh waktu sampai perubahan benar-benar dipahami dan terverifikasi.

## Workflow yang aman

- Gunakan project dan worktree terpisah untuk setiap perubahan nontrivial.
- Tetapkan requirement, scope, larangan, dan quality gate sebelum agent bekerja.
- Review implementation plan, artifact, tool access, dan diff pada milestone penting.
- Batasi file, command, URL, browser action, MCP, extension, dan credential sesuai tugas.
- Jalankan formatter, linter, type checker, test, build, dan security scan secara deterministik.
- Uji UI, aksesibilitas, error state, data, rollback, serta efek eksternal secara nyata.
- Periksa telemetry, Terms of Service, kebijakan data, model, kuota, dan biaya sebelum adopsi.
- Ukur lead time terverifikasi, defect, rework, review time, insiden, dan hasil pengguna.

## Lihat juga

- [[References/AI Agents\|AI Agents]]
- [[References/AI-Assisted Coding\|AI-Assisted Coding]]
- [[References/Cursor\|Cursor]]
- [[References/GitHub Copilot\|GitHub Copilot]]
- [[References/Claude Code\|Claude Code]]
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Git\|Git]]
- [[References/Chrome DevTools\|Chrome DevTools]]

## Sumber

- [Google Developers Blog](https://developers.googleblog.com/build-with-google-antigravity-our-new-agentic-development-platform): platform agentik, Editor View, Agent Manager, artifact, dan model.
- [Introducing Google Antigravity](https://antigravity.google/blog/introducing-google-antigravity): visi produk, completion, agent, browser, artifact, dan verifikasi.
- [Antigravity Home](https://antigravity.google/docs/home): aplikasi, IDE, CLI, SDK, subagent, integrasi, dan permission.
- [IDE Overview](https://antigravity.google/docs/ide/overview): editor, terminal, browser, agent paralel, serta artifact.
- [Feature Overview](https://antigravity.google/docs/features): project, worktree, review, hook, browser, scheduled task, dan keamanan bawaan.
- [Artifacts](https://antigravity.google/docs/artifacts): plan, diff, diagram, gambar, recording, review, dan feedback.
- [Browser](https://antigravity.google/docs/ide/browser): kontrol browser, screenshot, recording, dan verifikasi UI.
- [Getting Started](https://antigravity.google/docs/getting-started): instalasi, project, command, dan penggunaan awal.
- [Plans](https://antigravity.google/docs/plans): model, Tab completion, paket, kuota, dan rate limit.
- [Permissions](https://antigravity.google/docs/permissions): resource, rule, file, command, URL, browser, MCP, dan sandbox.
- [Settings](https://antigravity.google/docs/settings): global setting, project boundary, telemetry, model, dan customizations.
- [FAQ](https://antigravity.google/docs/faq): akun, geografi, data collection, rate limit, dan dukungan.
- [Models](https://antigravity.google/docs/models): reasoning model, pilihan model, boost, dan model pendukung.
- [Google Codelab](https://codelabs.developers.google.com/getting-started-google-antigravity): arsitektur 2.0, aplikasi, IDE, permission, MCP, skill, dan artifact.
- [Antigravity or Gemini CLI](https://cloud.google.com/blog/topics/developers-practitioners/choosing-antigravity-or-gemini-cli): perbandingan UI, headless execution, browser, extensibility, dan use case.
