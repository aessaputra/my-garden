---
{"dg-publish":true,"dg-path":"Prompt Engineering.md","permalink":"/prompt-engineering/","title":"Prompt Engineering","hideInFiletree":true,"tags":["references","gpt","prompt","research","testing","security"],"dg-note-properties":{"title":"Prompt Engineering","category":"references","tags":["references","gpt","prompt","research","testing","security"],"sources":["_raw/articles/prompt-engineering-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Prompt engineering adalah proses merancang, menguji, dan memperbaiki instruksi serta konteks agar model menghasilkan keluaran yang memenuhi tujuan terukur.

Ia bekerja saat inferensi tanpa mengubah parameter model. Prompt mengondisikan perilaku yang sudah dapat dilakukan model melalui instruksi, data, contoh, batasan, dan bentuk keluaran.

Karena keluaran model probabilistik, prompt yang tampak baik belum tentu stabil. Rekayasa yang dapat diandalkan memerlukan kriteria keberhasilan, data uji, evaluasi, versioning, dan pemantauan regresi.

## Prasyarat

Mekanisme token, konteks, dan prediksi keluaran dibahas di [[References/Cara Kerja LLM\|Cara Kerja LLM]]. Prompt hanya dapat memengaruhi informasi yang tersedia dan kemampuan yang telah dipelajari model.

Tetapkan tujuan sebelum mengubah kata. Nyatakan siapa pengguna hasilnya, keputusan yang didukung, batas risiko, biaya, latensi, serta kondisi yang dianggap berhasil atau gagal.

[Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) menyarankan kriteria keberhasilan, evaluasi empiris, dan draft awal sebelum optimasi prompt.

## Anatomi prompt

Prompt yang kuat biasanya memuat tujuan, konteks, data masukan, batasan, dan kontrak keluaran. Hanya sertakan informasi yang relevan serta jelaskan istilah khusus yang tidak dapat diasumsikan model.

Pisahkan instruksi dari data dengan heading, delimiter, tag, atau field terstruktur. Struktur membantu model membaca bagian prompt, tetapi bukan batas keamanan terhadap input tidak tepercaya.

Nyatakan format keluaran secara eksplisit. Untuk integrasi perangkat lunak, gunakan schema, tipe, field wajib, aturan nilai, contoh valid, dan penanganan ketika informasi tidak cukup.

Instruksi perlu menyebut prioritas dan konflik. Aturan yang tersebar, ambigu, atau saling bertentangan meningkatkan kemungkinan sebagian instruksi diabaikan.

## Zero-shot dan few-shot

Zero-shot prompting memberi instruksi tanpa contoh. Pendekatan ini murah dan cukup untuk tugas yang sudah dipahami model serta mudah dinilai.

Few-shot prompting menyertakan beberapa pasangan input dan keluaran. Contoh mengondisikan pola saat inferensi, bukan melatih atau mengubah parameter model secara permanen.

[OpenAI](https://platform.openai.com/docs/guides/prompt-engineering) dan [Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices) menyarankan contoh yang relevan serta beragam.

Contoh sebaiknya mencakup perilaku umum, edge case, dan format yang benar. Contoh yang sempit, salah, atau tidak konsisten dapat mengajarkan pola sampingan yang tidak diinginkan.

Tambah contoh hanya bila eval menunjukkan manfaat. Setiap contoh memakai jendela konteks, token, biaya, dan perhatian model yang juga dibutuhkan oleh data tugas.

## Reasoning dan pemecahan tugas

Chain-of-thought prompting meminta atau mendemonstrasikan langkah penalaran. Paper [Wei dkk.](https://arxiv.org/abs/2201.11903) menemukan peningkatan pada beberapa benchmark reasoning dengan model besar.

[Kojima dkk.](https://arxiv.org/abs/2205.11916) menunjukkan bahwa instruksi langkah demi langkah juga meningkatkan sejumlah benchmark tanpa contoh khusus.

Hasil tersebut tidak menjamin peningkatan pada semua model atau tugas. Model modern dapat memiliki mekanisme reasoning sendiri, dan penjelasan yang fasih tetap dapat mendukung jawaban salah.

Self-consistency mengambil beberapa jalur reasoning lalu memilih jawaban yang paling konsisten. [Wang dkk.](https://arxiv.org/abs/2203.11171) melaporkan peningkatan pada benchmark tertentu.

Teknik itu menambah biaya dan latensi. Kesepakatan antarsampel juga bukan bukti kebenaran bila model berbagi asumsi yang salah.

Prompt chaining memecah pekerjaan menjadi tahap seperti ekstraksi, analisis, verifikasi, dan penyusunan hasil. Setiap tahap dapat memakai kontrak keluaran serta eval tersendiri.

Chaining memudahkan diagnosis dan kontrol, tetapi menambah titik kegagalan. Informasi dapat hilang, berubah, atau terkontaminasi ketika berpindah tahap.

## Template dan versioning

Prompt template memisahkan pola tetap dari variabel. [Google Cloud](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/prompts/prompt-templates) menggunakannya untuk menguji format pada data berbeda.

Simpan prompt seperti kode: beri ID versi, catat model dan parameter, tinjau perubahan, lalu jalankan regression eval sebelum rilis.

Model snapshot, decoding, urutan konteks, tool, data retrieval, dan kebijakan penyedia dapat mengubah hasil. Prompt yang berhasil pada satu model harus diuji ulang setelah migrasi.

## Evaluasi dan iterasi

Buat dataset yang mencerminkan input produksi, termasuk kasus umum, edge case, input tidak lengkap, variasi bahasa, data berbahaya, dan kondisi yang harus ditolak.

Gunakan metrik sesuai tugas. Contohnya meliputi exact match, validitas schema, groundedness, recall, factuality, kepatuhan aturan, biaya, latensi, serta penilaian manusia.

[OpenAI](https://platform.openai.com/docs/guides/evaluation-best-practices) menekankan bahwa sistem generatif bervariasi dan memerlukan eval yang mewakili distribusi input nyata.

Ubah satu faktor pada satu waktu bila memungkinkan. Bandingkan baseline dan kandidat pada dataset tetap, lalu periksa kegagalan secara kualitatif sebelum menggeneralisasi hasil.

LLM-as-judge dapat memperluas evaluasi, tetapi rubrik, posisi jawaban, panjang, dan model penilai dapat menimbulkan bias. Kalibrasikan terhadap label manusia atau pemeriksaan deterministik.

Evaluasi bukan tahap akhir. Pantau kualitas setelah perubahan model, prompt, retrieval, tool, data, atau perilaku pengguna.

## Grounding dan verifikasi

Prompt dapat meminta model memakai sumber tertentu dan menyatakan ketidakpastian. Namun, instruksi tidak menambahkan fakta yang tidak tersedia dalam konteks atau parameter model.

Untuk pengetahuan dinamis, gunakan retrieval atau tool yang mengambil sumber aktual. Klaim material tetap perlu ditautkan ke bukti dan diverifikasi, bukan hanya dibuat lebih meyakinkan.

Untuk tugas deterministik, gunakan parser, schema validator, compiler, test, kalkulator, database constraint, atau pemeriksaan programatik. Bahasa alami sebaiknya tidak menggantikan invariant yang dapat ditegakkan oleh kode.

Human review perlu mengikuti risiko. Ringkasan internal tidak membutuhkan gate yang sama dengan keputusan medis, finansial, hukum, keamanan, atau tindakan yang mengubah sistem eksternal.

## Prompt injection

Prompt injection terjadi ketika input mengubah perilaku model secara tidak dimaksudkan. Serangan dapat datang langsung dari pengguna atau tidak langsung melalui dokumen, halaman web, email, tool, dan retrieval.

[OWASP](https://genai.owasp.org/llmrisk/llm01-prompt-injection) menyatakan tidak ada metode pencegahan yang sempurna. RAG dan fine-tuning juga tidak menghapus kerentanan tersebut.

Pisahkan data dari instruksi, tetapi jangan menganggap delimiter sebagai sandbox. Terapkan least privilege, allowlist tool, pembatasan jaringan, validasi output, approval manusia, logging, dan pengujian adversarial.

System prompt tidak boleh menjadi tempat secret. Instruksi tersembunyi dapat bocor, disimpulkan, atau diabaikan, sedangkan secret yang masuk konteks dapat muncul melalui keluaran atau tool.

Perlakukan model sebagai komponen tidak tepercaya. Otorisasi harus ditegakkan oleh aplikasi pada setiap aksi, bukan diserahkan kepada kepatuhan model terhadap kalimat larangan.

## Kapan prompt tidak cukup

Prompt engineering cocok ketika kemampuan dasar tersedia dan kegagalan dapat diperbaiki melalui instruksi, konteks, contoh, struktur, atau pemecahan tugas.

Pilih model lain bila batas utama adalah kemampuan, modalitas, latensi, biaya, atau jendela konteks. Gunakan retrieval bila informasi harus aktual dan dapat ditelusuri.

Gunakan fine-tuning bila pola perilaku perlu konsisten pada skala besar dan tersedia dataset berkualitas. Gunakan kode biasa bila aturan dapat dinyatakan secara deterministik.

Redesign tugas bila keberhasilan tidak dapat didefinisikan atau diverifikasi. Menambah prompt pada proses yang kabur hanya membuat kegagalan lebih sulit terlihat.

## Workflow praktis

1. Definisikan pengguna, tujuan, risiko, dan kriteria keberhasilan.
2. Siapkan dataset eval sebelum mengoptimalkan prompt.
3. Tulis baseline sederhana dengan tujuan, konteks, batasan, dan kontrak keluaran.
4. Jalankan baseline, kelompokkan kegagalan, dan ubah satu penyebab utama.
5. Tambahkan contoh, retrieval, chaining, atau tool hanya bila hasil eval membenarkannya.
6. Uji edge case, prompt injection, data tidak lengkap, serta keluaran tidak valid.
7. Versioning prompt, model, parameter, schema, dan dataset eval.
8. Terapkan validator, least privilege, approval, logging, dan pemantauan produksi.

## Pola yang perlu dihindari

Prompt panjang bukan otomatis prompt baik. Redundansi dan aturan yang bertentangan dapat menutupi tujuan utama serta memakai konteks tanpa manfaat.

Persona dapat membantu nada atau sudut pandang, tetapi tidak menciptakan kompetensi, akses data, otorisasi, atau jaminan kebenaran.

Kata seperti "selalu", "pasti", atau "jangan pernah" bukan kontrol teknis. Gunakan constraint aplikasi dan verifikasi untuk konsekuensi yang tidak boleh salah.

Mengoptimalkan satu contoh menghasilkan prompt rapuh. Perubahan harus dinilai pada distribusi kasus, bukan pada demo yang dipilih karena berhasil.

Prompt engineering bukan pengganti desain produk, data yang benar, model yang memadai, keamanan sistem, atau evaluasi.

## Lihat juga

- [[References/AI Agents\|AI Agents]]
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/AI-Assisted Coding\|AI-Assisted Coding]]
- [[References/Google Antigravity\|Google Antigravity]]
- [[References/Claude Code\|Claude Code]]
- [[References/Cursor\|Cursor]]
- [[References/GitHub Copilot\|GitHub Copilot]]
- [[Categories/Prompt\|Prompt]]

## Sumber

- [OpenAI Prompt Engineering](https://platform.openai.com/docs/guides/prompt-engineering): instruksi, hierarki pesan, contoh, model snapshot, dan teknik prompting.
- [OpenAI Evaluation Best Practices](https://platform.openai.com/docs/guides/evaluation-best-practices): dataset eval, rubrik, human evaluation, dan LLM-as-judge.
- [Anthropic Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview): prasyarat, kriteria keberhasilan, eval, dan batas prompting.
- [Anthropic Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices): kejelasan, contoh, struktur, reasoning, dan agent.
- [Google Cloud Prompt Design Strategies](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/prompts/prompt-design-strategies): tujuan, konteks, struktur, evaluasi, dan risiko injection.
- [Google Cloud Prompt Iteration](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/prompts/prompt-iteration): iterasi konten, struktur, dan urutan prompt.
- [Google Cloud Prompt Templates](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/prompts/prompt-templates): variabel, reuse, dan pengujian template.
- [Microsoft Prompt Engineering Techniques](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/prompt-engineering): system message, cue, zero-shot, few-shot, grounding, dan urutan.
- [Google ML Crash Course](https://developers.google.com/machine-learning/crash-course/llm/tuning): hubungan prompt engineering, fine-tuning, zero-shot, dan few-shot.
- [Brown dkk.](https://arxiv.org/abs/2005.14165): kemampuan few-shot GPT-3 dan batas generalisasi.
- [Wei dkk.](https://arxiv.org/abs/2201.11903): chain-of-thought prompting pada benchmark reasoning.
- [Wang dkk.](https://arxiv.org/abs/2203.11171): self-consistency dan hasil benchmark reasoning.
- [Kojima dkk.](https://arxiv.org/abs/2205.11916): zero-shot chain-of-thought.
- [White dkk.](https://arxiv.org/abs/2302.11382): katalog pola prompt yang dapat digunakan ulang.
- [OWASP Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection): serangan langsung dan tidak langsung, batas mitigasi, serta kontrol dampak.
