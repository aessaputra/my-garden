---
{"dg-publish":true,"dg-path":"Skills pada AI Coding Assistants.md","permalink":"/skills-pada-ai-coding-assistants/","title":"Skills pada AI Coding Assistants","hideInFiletree":true,"tags":["references","ai-agents","coding","programming","workflow","security"],"noteIcon":"","dg-note-properties":{"title":"Skills pada AI Coding Assistants","category":"references","tags":["references","ai-agents","coding","programming","workflow","security"],"sources":["_raw/articles/skills-ai-coding-assistants-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---


Skill pada AI coding assistant adalah paket instruksi, pengetahuan prosedural, script, dan resource yang dapat ditemukan agent untuk menangani jenis tugas tertentu.

Skill tidak selalu sama dengan tool. Tool menyediakan operasi terhadap environment, sedangkan skill menjelaskan kapan dan bagaimana operasi tersebut dipakai dalam workflow yang dapat diulang.

Perbedaan ini penting. Skill dapat mengarahkan agent untuk membaca dokumentasi, menjalankan test, memanggil tool database, memeriksa hasil, memperbaiki kegagalan, lalu melaporkan bukti.

## Struktur Agent Skill

Dalam standar Agent Skills, sebuah skill adalah direktori yang minimal memiliki `SKILL.md`. Berkas ini memakai YAML frontmatter diikuti instruksi Markdown.

Field `name` dan `description` wajib. Deskripsi menyatakan kemampuan skill dan kondisi penggunaannya, sehingga agent dapat menilai relevansi sebelum memuat seluruh instruksi.

Direktori dapat menyertakan `scripts/`, `references/`, dan `assets/`. Script memberi operasi yang dapat dijalankan, reference menyimpan detail teknis, sedangkan asset menyediakan template atau resource statis.

Skill yang baik menyimpan prosedur utama dalam `SKILL.md`, lalu memindahkan detail panjang ke reference yang dapat dibaca saat diperlukan. Struktur ini menjaga instruksi inti tetap mudah ditemukan.

## Progressive disclosure

Agent Skills memakai *progressive disclosure*. Pada tahap discovery, agent hanya menerima metadata seperti nama dan deskripsi setiap skill.

Ketika tugas cocok, agent memuat isi `SKILL.md`. Reference, script, template, atau file lain baru dibaca dan dijalankan ketika langkah aktif membutuhkannya.

Mekanisme ini mengurangi konteks awal karena seluruh manual tidak dimasukkan ke setiap request. Agent dapat mengetahui banyak skill tanpa membawa semua detailnya sekaligus.

Penghematan tersebut tidak tanpa batas. Skill yang aktif, hasil tool, file yang dibaca, dan riwayat tindakan tetap memakai context window. Deskripsi buruk juga dapat menyebabkan skill tidak dipilih atau dipilih secara keliru.

## Skill, tool, dan function calling

Function calling atau tool calling memungkinkan model meminta operasi eksternal melalui kontrak yang ditentukan aplikasi. Function tool biasanya memiliki nama, deskripsi, dan schema argumen.

Model menghasilkan tool call, tetapi aplikasi atau agent harness yang mengeksekusi fungsi. Hasil eksekusi dikirim kembali agar model dapat menjawab, memilih tindakan lain, atau memulihkan kegagalan.

Tool dapat berasal dari fungsi aplikasi, fasilitas bawaan platform, terminal, browser, database, atau server [[References/Model Context Protocol\|MCP]]. Setiap tool menentukan tindakan yang benar-benar tersedia.

Skill berada satu lapis di atas operasi tersebut. Ia dapat mengajarkan urutan penggunaan tool, validasi hasil, fallback, kebijakan keamanan, format keluaran, dan kondisi berhenti.

Sebagai contoh, tool test hanya menjalankan perintah. Skill debugging dapat mewajibkan reproduksi, diagnosis akar masalah, test yang gagal, perbaikan minimal, dan verifikasi regresi dengan tool yang sesuai.

## Hubungan dengan coding agent

[[References/AI Agents\|Coding agent]] memakai model, tool, state, policy, dan loop eksekusi. Skills menambahkan prosedur khusus tanpa harus membuat agent baru untuk setiap domain.

Satu agent dapat memuat skill untuk review kode, deployment, migrasi database, dokumentasi, atau pengujian UI. Agent memilih skill berdasarkan tugas, lalu bekerja dengan tool yang memang diberikan harness.

Skill tidak menambah izin dengan sendirinya. Instruksi deployment tidak dapat melakukan deployment bila agent tidak memiliki tool, credential, jaringan, atau approval yang diperlukan.

Sebaliknya, tool tanpa skill masih dapat dipakai langsung. Namun, agent mungkin kehilangan konvensi proyek, urutan pemeriksaan, fallback, atau standar bukti yang membuat penggunaan tool konsisten.

## Perbedaan dari instruksi tetap

Instruksi proyek seperti `AGENTS.md` atau `.github/copilot-instructions.md` biasanya berlaku pada setiap request dalam scope tertentu. Isinya cocok untuk konvensi kode, arsitektur, dan aturan yang selalu relevan.

Skill dimuat menurut kebutuhan. Ia cocok untuk workflow khusus yang hanya relevan pada tugas tertentu dan terlalu panjang bila selalu ditempatkan dalam konteks.

Custom agent mendefinisikan persona, model, tool, atau scope kerja khusus. Plugin membungkus beberapa bentuk ekstensi. Istilah dan precedence tepatnya berbeda pada setiap produk.

## Perbedaan dari hooks

Hook berjalan pada event lifecycle yang ditentukan. Claude Code, misalnya, memakai hook untuk menjalankan shell command secara deterministik sebelum atau sesudah tindakan tertentu.

Gunakan hook ketika sesuatu harus selalu terjadi, seperti formatter setelah perubahan. Gunakan skill ketika agent perlu menilai situasi dan mengikuti prosedur yang memerlukan judgment.

Keduanya dapat dipadukan. Skill meminta test dan review, sementara hook menegakkan pemeriksaan wajib walaupun model lupa atau memilih jalur lain.

## Portabilitas dan batasnya

Agent Skills adalah standar terbuka. Format yang sama didukung oleh beberapa agent, termasuk Claude dan GitHub Copilot pada lingkungan yang kompatibel.

Portabilitas format tidak menjamin perilaku identik. Agent dapat memiliki model, tool, lokasi skill, sandbox, aturan aktivasi, dependency, dan permission yang berbeda.

Skill perlu menyatakan prasyarat environment dan menghindari asumsi tersembunyi. Script juga harus mendokumentasikan dependency, input, output, error, dan platform yang didukung.

## Akurasi dan efisiensi

Skill dapat meningkatkan konsistensi dengan menyediakan prosedur teruji, sumber resmi, template, dan validator. Ia juga mengurangi kebutuhan menjelaskan workflow yang sama pada setiap percakapan.

Namun, skill bukan sumber akurasi otomatis. Instruksi dapat usang, salah, ambigu, atau tidak cocok dengan environment. Script dapat gagal dan tool result dapat disalahtafsirkan.

Efisiensi perlu diukur secara menyeluruh. Context awal dapat berkurang, tetapi skill yang terlalu besar, pemilihan yang salah, atau eksekusi berulang dapat menambah token, latensi, dan biaya.

Nilai skill dibuktikan melalui task success, waktu total, jumlah koreksi, kegagalan tool, pelanggaran constraint, kebutuhan intervensi, dan mutu artifact akhir.

## Keamanan

Skill harus diperlakukan sebagai instruksi dan code yang tidak tepercaya sampai diaudit. Skill berbahaya dapat meminta akses file, menjalankan script, menghubungi jaringan, atau mengekstraksi data.

Tinjau seluruh `SKILL.md`, dependency, script, reference, asset, dan URL eksternal. Periksa apakah tindakannya sesuai deskripsi serta apakah source dan versi dapat dipercaya.

Jalankan dengan least privilege. Batasi filesystem, jaringan, credential, tool, dan command. Minta approval untuk deployment, publikasi, pembayaran, penghapusan, perubahan izin, dan operasi sulit dibatalkan.

Isi yang diambil skill dari web, issue, repository, atau dokumen tetap merupakan data tidak tepercaya. Prompt injection dari sumber tersebut tidak memperoleh otoritas hanya karena dibaca melalui skill.

Sandbox dan approval mengurangi dampak, tetapi tidak membuktikan keamanan. Log eksekusi, diff, test, dan hasil eksternal harus menjadi bukti yang dapat diperiksa di luar narasi model.

## Perancangan skill

Mulai dari satu tugas yang berulang dan memiliki hasil terverifikasi. Nyatakan trigger, tujuan, non-goal, prasyarat, urutan kerja, kondisi gagal, serta bentuk bukti penyelesaian.

Gunakan tool sempit dengan schema jelas. Pisahkan operasi baca dari side effect, sediakan dry-run bila mungkin, dan tentukan kapan agent harus berhenti meminta keputusan pengguna.

Letakkan ringkasan pemilihan pada deskripsi. Simpan prosedur aktif dalam `SKILL.md`, detail teknis dalam references, code deterministik dalam scripts, dan template dalam assets.

Uji happy path, input ambigu, tool gagal, dependency hilang, permission ditolak, data berbahaya, partial completion, retry, dan rollback. Perbaiki skill berdasarkan trace kegagalan nyata.

Versikan skill bersama source yang relevan. Catat kompatibilitas dan perubahan kontrak tool agar prosedur tidak diam-diam tertinggal dari environment.

## Contoh alur

Untuk skill deployment, agent dapat membaca konfigurasi proyek, menjalankan test dan build, meninjau diff, meminta approval, memanggil tool deployment, lalu memeriksa health endpoint.

Yang membuatnya skill bukan perintah deploy semata. Nilainya berasal dari prosedur, batas, verifikasi, fallback, dan bukti yang membentuk penggunaan tool secara aman dan konsisten.

## Lihat juga

- [[References/AI Agents\|AI Agents]]: model, tool, state, guardrail, dan loop tindakan.
- [[References/AI-Assisted Coding\|AI-Assisted Coding]]: penggunaan AI sepanjang workflow pengembangan.
- [[References/Model Context Protocol\|Model Context Protocol]]: protokol untuk menemukan dan memanggil tool eksternal.
- [[References/Claude Code\|Claude Code]]: coding agent yang mendukung skill, hooks, dan instruksi proyek.
- [[References/GitHub Copilot\|GitHub Copilot]]: asisten coding yang mendukung Agent Skills pada beberapa lingkungan.
- [[References/Prompt Engineering\|Prompt Engineering]]: penyusunan instruksi dan konteks bagi model.

## Sumber

- [Agent Skills Overview](https://agentskills.io/home): definisi, struktur, progressive disclosure, dan tujuan format.
- [Agent Skills Specification](https://agentskills.io/specification): aturan `SKILL.md`, metadata, scripts, references, assets, dan validasi.
- [Claude Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview): arsitektur, context loading, custom skills, dan keamanan.
- [Anthropic: Equipping agents with skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills): desain, progressive disclosure, dan risiko skill.
- [VS Code: Use Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills): dukungan Copilot, aktivasi, portabilitas, dan script.
- [VS Code: Custom instructions](https://code.visualstudio.com/docs/agent-customization/custom-instructions): instruksi tetap dan instruksi berbasis pola file.
- [OpenAI: Function calling](https://platform.openai.com/docs/guides/function-calling): schema, tool call, eksekusi aplikasi, dan tool output.
- [OpenAI: Using tools](https://platform.openai.com/docs/guides/tools): built-in tools, function calling, tool search, dan remote MCP.
- [MCP Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools): discovery, invocation, hasil, error, dan kontrol keamanan tool.
- [Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks-guide): otomasi lifecycle yang deterministik.
