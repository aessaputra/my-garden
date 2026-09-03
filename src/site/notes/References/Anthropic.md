---
{"dg-publish":true,"dg-path":"Anthropic.md","permalink":"/anthropic/","title":"Anthropic","hideInFiletree":true,"tags":["references","companies","gpt","research","programming","coding","security","governance","sdk"],"noteIcon":"","dg-note-properties":{"title":"Anthropic","category":"references","tags":["references","companies","gpt","research","programming","coding","security","governance","sdk"],"sources":["_raw/articles/anthropic-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04","confidence":"medium"}}
---

Anthropic adalah perusahaan safety dan riset AI. [Halaman perusahaannya](https://www.anthropic.com/company) menyebut sistem yang reliabel dan steerable.

Fokusnya adalah sistem yang dapat diandalkan dan dipahami. Klaim misi ini menjelaskan arah, bukan bukti hasil safety aktual.

Safety diperlakukan sebagai sains sistematis. Riset diterapkan ke produk, lalu wawasan produk diberi umpan balik ke riset.

Pendekatan ini bersifat multidisiplin dan kolaboratif. Pemerintah, akademisi, nonprofit, dan industri dilibatkan untuk safety luas.

Anthropic termasuk pembuat [[References/Cara Kerja LLM\|LLM]] generatif. Keluaran yang lancar tetap dapat salah, bias, atau tidak sesuai kebutuhan aplikasi.

Konteks penting karena nama model, API, harga, dan kebijakan berubah cepat. Catat model, tanggal akses, dan konfigurasi saat membahas perilaku.

## Pandangan safety

Anthropic menilai dampak AI dapat sebesar revolusi industri dan sains. [Pandangan intinya](https://www.anthropic.com/news/core-views-on-ai-safety) memakai ekstrapolasi scaling compute.

Pandangan itu mengakui ketidakpastian dan skeptisisme yang wajar. Persiapan tetap dianggap serius karena bukti kemajuan yang cepat.

Masalah intinya adalah perilaku robust. Belum ada yang tahu cara melatih sistem sangat kuat agar helpful, honest, dan harmless.

Risiko mencakup tujuan berbahaya dan kesalahan high-stakes. Kompetisi deployment yang terburu-buru dapat memperbesar kedua risiko tersebut.

Responsnya adalah riset empiris multi-arah. [Riset alignment](https://alignment.anthropic.com) dan transparansi menjadi bagian pendekatan tersebut.

## Konstitusi Claude

[Konstitusi Claude](https://www.anthropic.com/constitution) memandu nilai, pengetahuan, dan kebijaksanaan model lintas situasi.

Urutan prioritas saat konflik adalah safe, etis, patuh pedoman, lalu helpful. Prioritas ini holistik, bukan tie-breaker kaku.

Oversight manusia ditempatkan di atas etika luas selama fase kini. Tujuannya mencegah kesalahan tak terkoreksi sebelum menyebar luas.

Pendekatan ini mengutamakan judgment atas aturan kaku. Aturan dipakai untuk kasus berat yang butuh prediktabilitas dan evaluasi.

Konstitusi dapat diperbarui, terakhir Januari 2026. Verifikasi versi terbaru sebelum mengutip prinsip tertentu.

Motivasi dan prinsip penuh dijelaskan pada [pengumuman konstitusi](https://www.anthropic.com/news/claudes-constitution). Prinsip rinci ada pada dokumen induk.

## Constitutional AI

[Constitutional AI](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback) melatih harmlessness lewat feedback AI.

Fase supervised memakai self-critique dan revisi. Fase RL memakai preference model dari penilaian AI sebagai reward.

Hasilnya asisten yang menolak dengan penjelasan, bukan menghindar. Chain-of-thought membantu transparansi keputusan.

Metode ini mengurangi label manusia dan membuat kontrol lebih presisi. Paper penuh tersedia di [arXiv](https://arxiv.org/abs/2212.08073).

CAI menjawab keterbatasan feedback implisit. Skala, paparan output mengganggu, dan biaya review menjadi alasan pendekatan eksplisit.

Teknik prompt umum dibahas di [[References/Prompt Engineering\|Prompt Engineering]]. Untuk evaluasi perilaku, lihat pola pada [[References/AI-Powered Code Review\|AI-Powered Code Review]].

## Keluarga Claude dan pemilihan model

Claude adalah keluarga model bahasa frontier Anthropic. [Gambaran model](https://platform.claude.com/docs/en/models/overview) memuat lineup kini.

Empat model aktif adalah Fable 5.1, Opus 5, Sonnet 5, dan Haiku 4.5. Semuanya mendukung teks dan gambar, vision, serta tool use.

Fable untuk reasoning menuntut dan agen long-horizon. Opus untuk coding agen kompleks dan enterprise dengan thinking adaptif.

Sonnet adalah default serbaguna harian untuk coding dan analisis. Haiku tercepat untuk jawaban ringan dan ekstraksi sederhana.

[Panduan Academy](https://academy.claude.com/tutorials/choosing-the-right-claude-model) memetakan beban rate limit tiap model secara praktis.

Jangan memilih dari nama atau benchmark umum. Ukur task success, latency, biaya, limit, dan kualitas pada kasus nyata.

Gunakan model ID eksplisit pada production. Alias dapat berubah ketika provider memperbarui targetnya.

Rencanakan migrasi sebelum model dihentikan. Simpan eval set, prompt, schema, dan hasil pembanding agar upgrade teruji ulang.

## Akses developer

[Messages API](https://platform.claude.com/docs/en/build-with-claude/working-with-messages) bersifat stateless untuk kontrol penuh.

Aplikasi selalu mengirim riwayat percakapan penuh. Synthetic assistant messages dapat dipakai untuk membangun pola.

Respons memakai `stop_reason` eksplisit seperti end, tool, dan refusal. Refusal menyertakan kategori policy pada `stop_details`.

Model 4.7 ke atas tidak mendukung sampling manual. Hilangkan `temperature`, `top_p`, dan `top_k` lalu arahkan perilaku via prompt.

Pola integrasi frontend dibahas di [[References/Generative AI for Frontend Development\|AI dalam Pengembangan Frontend]]. Secret tetap berada pada server.

Untuk agen dan orchestration, lihat [[References/AI Agents\|AI Agents]]. Setiap tool perlu izin, validasi, approval, dan audit oleh kode.

## Tool use

[Tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) juga disebut function calling pada dokumen lain.

Client tools dieksekusi aplikasi lewat loop `tool_use` dan `tool_result`. Contohnya tool buatan user, bash, dan text editor.

Server tools dieksekusi infrastruktur Anthropic tanpa handler. Contohnya web search, web fetch, dan code execution.

Claude memutuskan pemanggilan dari deskripsi tool tiap turn. Prompt dapat mengarahkan, sedangkan `tool_choice` memaksa bila perlu.

Pisahkan pemilihan tool dari otorisasi. Identitas, permission, precondition, rate limit, dan audit diperiksa oleh kode.

Tool call adalah input tidak tepercaya. Argumen buatan model dapat salah atau disuntik lewat data eksternal.

## Structured output

[Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) memakai constrained decoding berbasis grammar.

Dua fitur tersedia, yaitu JSON outputs dan strict tool use. Keduanya dapat dipakai terpisah atau bersamaan dalam satu request.

Schema valid mengurangi parsing error dan retry. SDK mendukung Pydantic, Zod, Java, Ruby, PHP, C#, Go, dan CLI.

Schema disederhanakan sebelum dikirim ke model. Validasi penuh tetap memakai schema asli pada kode aplikasi.

Validasi makna tetap dua tingkat, yaitu bentuk dan domain. Aturan bisnis, izin, dan database diperiksa kode deterministik.

Batasi kompleksitas schema agar kompilasi cepat. Strict tools maks 20, optional params 24, dan timeout kompilasi 180 detik.

## Prompt caching dan konteks

[Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) memakai `cache_control` untuk prefiks prompt.

Mode otomatis cocok untuk percakapan multi-turn. Breakpoint eksplisit cocok untuk kontrol presisi atas konten statis.

TTL default lima menit dan opsi satu jam. Tulis cache berbiaya lebih, sedangkan hit cache jauh lebih murah.

Letakkan konten statis di awal prompt sesuai hierarki tools. Jangan pasang breakpoint pada blok yang berubah tiap request.

Konteks Fable, Opus, dan Sonnet kini satu juta token. Haiku 4.5 hanya dua ratus ribu dengan output maks lebih kecil.

Konteks besar bukan memori permanen dan tidak menjamin temuan. Retrieval, urutan sumber, dan eval tetap diperlukan.

## MCP dan Agent Skills

[Pengumuman MCP](https://www.anthropic.com/news/model-context-protocol) membuka standar penghubung AI dan data.

MCP memakai server yang mengekspos data dan klien yang terhubung. Server awal mencakup Drive, Slack, GitHub, Git, dan Postgres.

Spesifikasi kini dibahas pada [[References/Model Context Protocol\|Model Context Protocol]]. Detail versi terbaru dicek pada dokumen spesifikasi tersebut.

[Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) mengemas keahlian prosedural agen.

Skill berisi SKILL.md, script, dan resource yang dimuat dinamis. Metadata preload sebagai disclosure pertama yang hemat konteks.

Repo publik tersedia di [anthropics/skills](https://github.com/anthropics/skills). Sebagian skill dokumen bersifat source-available.

Pola desain skill dibahas di [[References/Skills pada AI Coding Assistants\|Skills pada AI Coding Assistants]]. Alat terkait ada pada [[References/Claude Code\|Claude Code]].

## Governance dan safety

[Responsible Scaling Policy](https://www.anthropic.com/responsible-scaling-policy) versi 3.4 mengatur scaling risiko proporsional.

Kebijakan bersifat iteratif dan exportable dengan versi serta redline. Risk report publik diterbitkan berkala sejak versi 3.

Safeguard ASL-3 memakai defense in depth empat lapis. Fokusnya mencegah penyalahgunaan CBRN dan risiko katastropik lain.

[Pusat bantuan safety](https://support.claude.com/en/articles/8106465-our-approach-to-user-safety) menjadi rujukan operasional user.

Filter dan refusal bukan policy engine lengkap. Aplikasi tetap perlu authorization, validasi, encoding, dan incident response.

Pola review dan quality gate ada pada [[References/Refactoring dengan AI\|Refactoring dengan AI]]. Perbandingan provider ada pada [[References/OpenAI\|OpenAI]] dan [[References/Gemini\|Gemini]].

## Evaluasi dan batas kesimpulan

Confidence halaman ini medium. Mekanisme API didukung primer, tetapi lineup dan angka berubah cepat.

Benchmark vendor berguna untuk orientasi, bukan vonis aplikasi. Prompt, tool, konteks, sampling, dan data dapat mengubah hasil.

Buat eval dari pekerjaan nyata. Ukur correctness, groundedness, task completion, format validity, dan tool success.

Sertakan latency, biaya, refusal, safety, dan kualitas lintas pengguna. Bandingkan dengan baseline non-AI pada kondisi sama.

Belum ada perbandingan independen lintasmodel pada workload identik dalam scope ini. Keputusan akhir memakai data aplikasi sendiri.

Model, harga, limit, region, policy, dan SDK berubah cepat. Verifikasi ulang sebelum implementasi dan setelah upgrade besar.

## Lihat juga

- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Prompt Engineering\|Prompt Engineering]]
- [[References/AI Agents\|AI Agents]]
- [[References/Generative AI for Frontend Development\|AI dalam Pengembangan Frontend]]
- [[References/Claude Code\|Claude Code]]
- [[References/Skills pada AI Coding Assistants\|Skills pada AI Coding Assistants]]
- [[References/Model Context Protocol\|Model Context Protocol]]
- [[References/OpenAI\|OpenAI]]
- [[References/Gemini\|Gemini]]

## Sumber

- [Company](https://www.anthropic.com/company), identitas safety dan riset, misi, dan sains safety.
- [Core views](https://www.anthropic.com/news/core-views-on-ai-safety), scaling, perilaku robust, dan pendekatan empiris.
- [Constitution](https://www.anthropic.com/constitution), nilai inti, prioritas, dan oversight manusia.
- [CAI research](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback), metode SL dan RLAIF.
- [CAI paper](https://arxiv.org/abs/2212.08073), paper Constitutional AI lengkap.
- [Constitution news](https://www.anthropic.com/news/claudes-constitution), motivasi CAI dan prinsip penuh.
- [Models](https://platform.claude.com/docs/en/models/overview), lineup, harga, konteks, output, dan cutoff.
- [Choosing](https://academy.claude.com/tutorials/choosing-the-right-claude-model), panduan Haiku Sonnet Opus Fable.
- [Messages](https://platform.claude.com/docs/en/build-with-claude/working-with-messages), stateless, stop reason, dan refusal.
- [Tool use](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview), client versus server tools.
- [Structured](https://platform.claude.com/docs/en/build-with-claude/structured-outputs), JSON outputs dan strict tool use.
- [Caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching), otomatis, breakpoint, TTL, dan harga.
- [MCP](https://www.anthropic.com/news/model-context-protocol), standar terbuka, server, dan klien.
- [Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills), SKILL.md dan disclosure.
- [Skills repo](https://github.com/anthropics/skills), implementasi dan lisensi campuran.
- [RSP](https://www.anthropic.com/responsible-scaling-policy), versi 3.4, redline, dan risk report.
- [Safety](https://support.claude.com/en/articles/8106465-our-approach-to-user-safety), pendekatan user safety.
- [Alignment](https://alignment.anthropic.com), riset alignment Anthropic.
