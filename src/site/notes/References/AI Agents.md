---
{"dg-publish":true,"dg-path":"AI Agents.md","permalink":"/ai-agents/","title":"AI Agents","hideInFiletree":true,"tags":["references","gpt","ai-agents","research","security","workflow"],"dg-note-properties":{"title":"AI Agents","category":"references","tags":["references","gpt","ai-agents","research","security","workflow"],"sources":["_raw/articles/ai-agents-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

AI agent adalah sistem AI yang mengarahkan proses dan memakai tool secara dinamis untuk mencapai tujuan atas nama pengguna.

Agent tidak hanya menghasilkan teks. Ia memilih langkah, bertindak, mengamati hasil, menyesuaikan rencana, dan berhenti atau meminta bantuan berdasarkan kondisi yang ditetapkan.

Code completion, deteksi error, dan generasi blok kode adalah kemampuan asisten. Sistem menjadi coding agent ketika dapat mengelola rangkaian tindakan pengembangan dengan tingkat kemandirian tertentu.

## Prasyarat

Model bahasa menyediakan reasoning dan pemilihan tindakan. Mekanisme dasarnya dibahas di [[References/Cara Kerja LLM\|Cara Kerja LLM]], sedangkan penyusunan instruksi dibahas di [[References/Prompt Engineering\|Prompt Engineering]].

Agent menambahkan harness, tool, state, guardrail, dan loop eksekusi. Karena itu, kemampuan serta risikonya tidak dapat dinilai hanya dari kualitas respons model.

Istilah agent belum seragam. [Anthropic](https://www.anthropic.com/research/building-effective-agents) membedakan workflow dengan jalur kode tetap dari agent yang mengarahkan proses dan tool secara dinamis.

## Loop agent

Agent menerima tujuan, instruksi, konteks, dan batas. Model kemudian merencanakan atau memilih tindakan berikutnya berdasarkan state yang tersedia.

Tindakan dijalankan melalui tool seperti pencarian, database, browser, terminal, API, email, atau editor. Hasil tool menjadi observasi untuk keputusan berikutnya.

Loop berlanjut sampai tujuan tercapai, anggaran habis, kondisi berhenti terpenuhi, terjadi kegagalan, atau keputusan harus dieskalasikan kepada manusia.

Pola [ReAct](https://arxiv.org/abs/2210.03629) menggabungkan reasoning dan acting agar model dapat memperbarui langkah berdasarkan informasi dari environment.

Reasoning yang terlihat bukan bukti kebenaran. Keberhasilan harus ditentukan oleh state eksternal, test, validator, sumber, atau hasil yang dapat diperiksa.

## Komponen utama

Model menafsirkan tujuan dan memilih tindakan. Instruksi menentukan tugas, kebijakan, prioritas, larangan, format, serta kondisi eskalasi.

Tool menentukan apa yang dapat dilakukan agent. Schema tool perlu menjelaskan input, output, error, side effect, idempotency, dan batas otorisasi secara tepat.

State menyimpan progres, hasil antara, dan konteks sesi. Memory persisten dapat menyimpan pengetahuan atau preferensi lintas sesi, tetapi tidak wajib bagi setiap agent.

Guardrail membatasi input, output, tool, dan tindakan. Orchestrator mengatur loop, routing, retry, timeout, budget, concurrency, handoff, serta lifecycle eksekusi.

Identity menentukan siapa agent ketika mengakses sistem lain. Observability merekam trace, keputusan, tool call, hasil, biaya, latensi, error, dan intervensi manusia.

## Workflow dan agent

Workflow memakai urutan yang telah ditentukan kode. Ia lebih mudah diuji, dijelaskan, diaudit, dan diprediksi ketika langkah serta aturan bisnis sudah diketahui.

Agent memilih langkah secara dinamis. Ia lebih sesuai untuk tugas yang tidak dapat dipetakan seluruhnya lebih awal, membutuhkan interpretasi, atau menghadapi informasi yang berubah.

Arsitektur hibrida sering lebih prudent. Kode deterministik mengatur invariant dan gate, sedangkan agent menangani keputusan yang membutuhkan bahasa, reasoning, atau adaptasi.

Jangan memakai agent untuk proses yang dapat diselesaikan dengan query, rule engine, state machine, script, atau workflow biasa dengan reliabilitas lebih tinggi.

## Single-agent dan multi-agent

Single-agent memakai satu loop dengan beberapa tool. Bentuk ini biasanya lebih sederhana untuk dibangun, dievaluasi, diamati, dan diamankan.

Multi-agent membagi pekerjaan menurut peran, domain, tahap, atau paralelisme. Polanya mencakup sequential, concurrent, handoff, group chat, dan manager-worker.

[Microsoft](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns) menekankan pemilihan pola berdasarkan dependensi, koordinasi, dan kebutuhan state.

Multi-agent bukan peningkatan otomatis. Ia menambah komunikasi, context loss, konflik, duplikasi, latensi, biaya, failure mode, dan kesulitan menentukan tanggung jawab.

Mulai dari satu agent. Tambahkan agent lain hanya bila spesialisasi, isolasi, atau paralelisme memberi hasil eval yang lebih baik daripada satu agent dengan tool yang memadai.

## State dan memory

Short-term state memuat percakapan, rencana, tool result, dan progres eksekusi. Durable state memungkinkan pekerjaan dilanjutkan setelah restart atau interupsi.

Long-term memory dapat memakai database, retrieval, profil, atau knowledge base. Informasi yang disimpan perlu memiliki scope, provenance, masa berlaku, kontrol akses, dan mekanisme koreksi.

Memory dapat salah, usang, beracun, atau berasal dari pengguna lain. Jangan membiarkan model menulis fakta persisten tanpa validasi dan batas namespace.

Simpan data minimum yang diperlukan. Pisahkan memory pengguna, organisasi, dan task agar konteks tidak bocor melintasi tenant atau tujuan.

## Tool dan tindakan

Tool mengubah agent dari generator menjadi pelaku. Tool read-only memiliki risiko berbeda dari tool yang mengirim pesan, membayar, menghapus data, deploy, atau mengubah izin.

Berikan fungsi sempit dengan schema ketat. Hindari tool serbaguna jika kebutuhan dapat dipenuhi oleh operasi yang lebih terbatas dan mudah diaudit.

Terapkan least privilege pada credential, file, jaringan, API scope, database role, dan environment. Otorisasi harus diperiksa oleh sistem tujuan pada setiap tindakan.

Gunakan idempotency key, dry-run, transaction, timeout, retry terbatas, rate limit, dan rollback bila domain mendukungnya.

Tindakan berisiko tinggi memerlukan preview dan approval. Agent tidak boleh mengartikan diamnya pengguna sebagai persetujuan untuk side effect yang sulit dibatalkan.

## Identity dan accountability

[NCCoE](https://www.nccoe.nist.gov/projects/software-and-ai-agent-identity-and-authorization) menyoroti kebutuhan identitas serta otorisasi untuk tindakan software dan AI agent.

Gunakan identitas agent yang dapat dibedakan dari pengguna dan service umum. Catat principal manusia, agent, sesi, tujuan, credential, tool, serta hasil setiap tindakan.

Delegasi harus dibatasi oleh scope, waktu, resource, dan tujuan. Hak agent perlu dapat dicabut tanpa mencabut seluruh akses pengguna atau aplikasi.

Audit log harus berasal dari komponen tepercaya di luar model. Narasi yang dibuat agent tidak cukup sebagai catatan tindakan aktual.

## Keamanan

Indirect prompt injection terjadi ketika agent membaca instruksi berbahaya dari halaman, dokumen, email, issue, repository, atau output tool.

[NIST](https://www.nist.gov/news-events/news/2025/01/technical-blog-strengthening-ai-agent-hijacking-evaluations) menguji hijacking ketika data menyerang agent di tengah tugas pengguna yang sah.

Delimiter dan system prompt tidak memberi isolasi sempurna. Konten eksternal perlu diperlakukan sebagai data tidak tepercaya walau berasal dari sumber yang tampak sah.

[OWASP](https://genai.owasp.org/llmrisk/llm06-sensitive-information-disclosure) mengaitkan excessive agency dengan fungsi, izin, atau otonomi yang berlebihan.

Kurangi blast radius melalui least privilege, sandbox, allowlist, validasi input dan output, approval, egress control, secret isolation, audit, serta pengujian adversarial.

Jangan memberikan secret melalui prompt bila tool dapat memperoleh credential scoped saat diperlukan. Model tidak perlu melihat nilai secret untuk memakai layanan secara aman.

## Human oversight

Otonomi bukan nilai biner. Agent dapat bekerja mandiri untuk langkah berisiko rendah dan meminta persetujuan pada keputusan yang mahal, sensitif, ambigu, atau tidak dapat dibatalkan.

Approval harus menampilkan tindakan aktual, target, data, biaya, diff, dan konsekuensi. Pertanyaan umum seperti "lanjutkan?" tidak memberi dasar keputusan yang cukup.

Gunakan checkpoint pada perubahan scope, akses baru, publish, deployment, pembayaran, komunikasi eksternal, perubahan izin, migrasi data, dan operasi destruktif.

Manusia tetap dapat mengalami automation bias dan approval fatigue. Gate perlu jarang, jelas, dan ditempatkan pada titik yang benar-benar mengendalikan risiko.

## Evaluasi

Evaluasi agent harus mengukur hasil end-to-end, bukan hanya respons akhir. Periksa task success, constraint violation, side effect, recovery, biaya, latensi, dan kebutuhan intervensi.

Dataset perlu mencakup happy path, ambiguity, tool failure, stale memory, partial completion, prompt injection, privilege boundary, dan kondisi yang harus ditolak.

Trace membantu menemukan keputusan salah, tetapi trace panjang tidak otomatis menjelaskan sebab. Hubungkan setiap tindakan dengan state, policy, tool result, dan verifier.

[METR](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks) mengukur horizon tugas melalui durasi kerja manusia yang dapat diselesaikan agent dengan reliabilitas tertentu.

Horizon meningkat, tetapi tugas yang lebih panjang tetap lebih sulit. Hasil benchmark tidak menjamin kemampuan pada repository, data, tool, atau risiko organisasi tertentu.

## Coding agent

Coding agent adalah AI agent yang bekerja pada repository dan tool pengembangan. Ia dapat mencari kode, membuat rencana, mengubah berkas, menjalankan command, menguji, dan menyiapkan diff.

[GitHub](https://docs.github.com/en/copilot/concepts/coding-agent/about-copilot-coding-agent) menjelaskan agent cloud yang bekerja pada branch dan dapat menyiapkan pull request untuk ditinjau.

Completion dan chat tetap berguna untuk interaksi lokal. Agent menambah loop tindakan serta akses tool, sehingga produktivitas potensial dan verification tax sama-sama meningkat.

Praktik coding agent dibahas lebih rinci di [[References/AI-Assisted Coding\|AI-Assisted Coding]]. Produk seperti [[References/Google Antigravity\|Google Antigravity]], [[References/Cursor\|Cursor]], [[References/GitHub Copilot\|GitHub Copilot]], dan [[References/Claude Code\|Claude Code]] memiliki tingkat otonomi berbeda.

Coding agent perlu branch atau worktree terpisah, permission terbatas, diff review, test independen, dependency scan, secret scan, dan larangan melemahkan quality gate.

## Workflow implementasi

1. Pilih tugas yang bernilai, tidak sepenuhnya deterministik, dan memiliki hasil terverifikasi.
2. Definisikan tujuan, non-goal, constraint, budget, kondisi berhenti, serta eskalasi.
3. Mulai dari satu agent dengan tool read-only atau berisiko rendah.
4. Tambahkan state, memory, dan tool hanya setelah kebutuhan dibuktikan.
5. Tegakkan identity, least privilege, approval, sandbox, dan audit di luar model.
6. Bangun eval end-to-end, adversarial test, tracing, dan observability sebelum produksi.
7. Uji kegagalan tool, retry, timeout, partial state, rollback, dan recovery.
8. Ukur task success, kualitas, biaya, latensi, intervensi, insiden, dan dampak pengguna.

## Batas dan antipola

Agent tidak memahami tujuan organisasi secara otomatis. Instruksi yang ambigu dapat diubah menjadi rangkaian tindakan yang tampak koheren tetapi salah sasaran.

Lebih banyak tool memperluas capability sekaligus attack surface. Lebih banyak memory memperluas konteks sekaligus risiko privasi dan kontaminasi.

Lebih banyak agent tidak menjamin reasoning lebih baik. Konsensus antarmodel dapat tetap salah, sedangkan koordinasi dapat menyembunyikan sumber kegagalan.

Demo satu kali bukan bukti reliabilitas. Agent perlu diuji pada distribusi tugas, perubahan environment, serangan, dan kegagalan yang realistis.

Tujuan yang prudent bukan otonomi maksimum. Tujuannya adalah tingkat delegasi terkecil yang menghasilkan manfaat terukur dengan risiko yang dapat dikendalikan.

## Lihat juga

- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Skills pada AI Coding Assistants\|Skills pada AI Coding Assistants]]
- [[References/Prompt Engineering\|Prompt Engineering]]
- [[References/Model Context Protocol\|Model Context Protocol]]
- [[References/AI-Assisted Coding\|AI-Assisted Coding]]
- [[References/Google Antigravity\|Google Antigravity]]
- [[References/Cursor\|Cursor]]
- [[References/GitHub Copilot\|GitHub Copilot]]
- [[References/Claude Code\|Claude Code]]

## Sumber

- [Anthropic: Building Effective AI Agents](https://www.anthropic.com/research/building-effective-agents): workflow, agent, tool, memory, dan pola orchestration.
- [Anthropic: Measuring AI Agent Autonomy](https://www.anthropic.com/research/measuring-agent-autonomy): otonomi nyata, intervensi manusia, dan risiko tindakan.
- [Anthropic: Trustworthy Agents](https://www.anthropic.com/research/trustworthy-agents): loop agent, harness, tool, oversight, dan prompt injection.
- [OpenAI: Practical Guide to Building Agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents): definisi, use case, single-agent, multi-agent, dan guardrail.
- [OpenAI: Building Agents](https://developers.openai.com/tracks/building-agents): model, tool, state, orchestration, guardrail, dan evaluasi.
- [OpenAI: New Tools for Building Agents](https://openai.com/index/new-tools-for-building-agents): handoff, tracing, observability, dan komponen agent.
- [Google Cloud: Core Concepts of AI Agents](https://cloud.google.com/resources/core-concepts-ai-agents): model, tool, data architecture, memory, dan enterprise operation.
- [Microsoft: Agent Orchestration Patterns](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns): sequential, concurrent, handoff, group chat, dan magentic patterns.
- [Microsoft: Shared Responsibility for AI Agents](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility-ai-agent): autonomy, identity, memory, tool, dan security boundary.
- [Yao dkk.: ReAct](https://arxiv.org/abs/2210.03629): integrasi reasoning dan acting dengan observasi environment.
- [NIST: Agent Hijacking Evaluations](https://www.nist.gov/news-events/news/2025/01/technical-blog-strengthening-ai-agent-hijacking-evaluations): indirect prompt injection dan skenario evaluasi agent.
- [NCCoE: Agent Identity and Authorization](https://www.nccoe.nist.gov/projects/software-and-ai-agent-identity-and-authorization): identitas, otoritas, dan audit tindakan agent.
- [OWASP: Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection): serangan langsung dan tidak langsung serta batas mitigasi.
- [OWASP: Excessive Agency](https://genai.owasp.org/llmrisk/llm06-sensitive-information-disclosure): fungsi, izin, otonomi, dan human approval.
- [GitHub: Copilot Cloud Agent](https://docs.github.com/en/copilot/concepts/coding-agent/about-copilot-coding-agent): repository research, planning, branch, perubahan kode, dan pull request.
- [METR: Long Software Tasks](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks): task-completion time horizon dan batas reliabilitas.
