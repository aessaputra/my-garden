---
{"dg-publish":true,"dg-path":"OpenAI.md","permalink":"/open-ai/","title":"OpenAI","hideInFiletree":true,"tags":["references","companies","gpt","research","programming","coding","security","governance","sdk"],"noteIcon":"","dg-note-properties":{"title":"OpenAI","category":"references","tags":["references","companies","gpt","research","programming","coding","security","governance","sdk"],"sources":["_raw/articles/openai-research-packet.md"],"created":"2026-09-04","updated":"2026-09-04","confidence":"medium"}}
---

OpenAI adalah perusahaan riset dan deployment AI. [Misi resminya](https://openai.com/about) memastikan AGI bermanfaat bagi seluruh umat manusia.

AGI di sini berarti sistem AI yang secara umum lebih pintar dari manusia. Klaim misi ini menjelaskan arah, bukan bukti hasil atau tata kelola aktual.

OpenAI terdiri dari nonprofit dan grup for-profit. [Struktur resminya](https://openai.com/about) menempatkan Foundation sebagai pengatur Group public benefit corporation.

Detail hukum struktur tersebut dapat berubah. Untuk keputusan kontrak atau kepatuhan, verifikasi dokumen hukum terbaru sebelum memakai ringkasan ini.

OpenAI termasuk pembuat [[References/Cara Kerja LLM\|LLM]] generatif. Keluaran yang lancar tetap dapat salah, bias, atau tidak sesuai kebutuhan aplikasi.

Konteks penting karena nama model, produk, dan API berubah cepat. Selalu catat model, API, tanggal akses, dan konfigurasi saat membahas perilaku.

## Keluarga model dan rujukan aktif

Portofolio memakai seri GPT dengan trade-off berbeda. [Panduan GPT-5.6](https://developers.openai.com/api/docs/guides/latest-model) menjadi baseline production saat akses.

Alias `gpt-5.6` mengarah ke `gpt-5.6-sol` untuk flagship. Varian `terra` menekan harga, sedangkan `luna` untuk workload bervolume tinggi.

Panduan reasoning menyarankan mulai dari `gpt-5.6` untuk reasoning umum. [Panduan reasoning](https://developers.openai.com/api/docs/guides/reasoning) memakai Responses API sebagai jalur utama.

Jangan memilih model dari nama atau benchmark umum. Ukur task success, latency, biaya, stabilitas format, limit, dan kualitas pada kasus nyata.

Gunakan model ID eksplisit pada production. Alias terbaru memudahkan eksperimen, tetapi dapat berubah ketika provider memperbarui targetnya.

Rencanakan migrasi sebelum model dihentikan. Simpan eval set, prompt, schema, konfigurasi, dan hasil pembanding agar upgrade dapat diuji ulang.

## Responses API

Responses API adalah jalur utama untuk reasoning dan tool calling. [Panduan migrasi](https://developers.openai.com/api/docs/guides/migrate-to-responses) mengarahkan workload baru ke API tersebut.

Chat Completions masih didukung untuk kompatibilitas. Assistants API diperlakukan sebagai legacy dengan panduan migrasi terpisah.

Pola integrasi yang lebih luas dibahas di [[References/Generative AI for Frontend Development\|AI dalam Pengembangan Frontend]]. Secret jangka panjang tetap berada pada server.

Untuk agen dan tool, pola otorisasi dibahas di [[References/AI Agents\|AI Agents]]. Setiap tool call perlu izin, validasi, approval, dan audit oleh kode aplikasi.

## Reasoning

Model reasoning memakai token reasoning internal sebelum menjawab. Cara ini membantu perencanaan, pemakaian tool, dan tugas multi-langkah yang kompleks.

Kontrol utama adalah `reasoning.effort` dengan nilai `none` sampai `max` menurut model. [Panduan effort](https://developers.openai.com/api/docs/guides/reasoning) memetakan tiap nilai ke kebutuhan latency dan kualitas.

GPT-5.6 menambah mode `standard` dan `pro`. Mode `pro` menambah kerja model untuk tugas sulit dengan toleransi latency dan biaya lebih besar.

Token reasoning tidak terlihat via API, tetapi menempati context window. Token tersebut ditagih sebagai token output sehingga perlu dianggarkan.

Persisted reasoning dapat dipakai ulang lintas turn lewat `reasoning.context`. Prompt caching eksplisit membantu, tetapi tulis cache juga berbiaya.

Mulai dari `medium` untuk workload umum dan `low` untuk latensi. Naikkan effort hanya bila eval menunjukkan kenaikan kualitas yang sepadan.

## Generasi teks dan multimodal

API mendukung generasi teks, kode, gambar, audio, dan video pada model yang sesuai. [Navigasi panduan teks](https://developers.openai.com/api/docs/guides/text) memisahkan teks, kode, prompting, dan reasoning.

Dukungan modalitas, limit, dan biaya berbeda menurut model. Aplikasi perlu memilih model dari capability yang benar-benar tersedia.

Riwayat percakapan mengonsumsi konteks. Tentukan pesan yang dipertahankan, diringkas, atau dibuang, serta cara menyimpan dan menghapusnya.

Teknik prompt umum dibahas di [[References/Prompt Engineering\|Prompt Engineering]]. Untuk konteks eksternal, lihat pola retrieval pada [[References/Model Context Protocol\|Model Context Protocol]].

## Structured output

[Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs) memaksa respons mengikuti JSON Schema yang diberikan aplikasi.

Schema mengurangi kesalahan parsing, tetapi tidak membuktikan nilai benar. Tanggal dapat valid namun salah, dan ID dapat tidak ada di database.

Validasi output pada dua tingkat, yaitu bentuk terhadap schema dan makna terhadap domain. Aturan bisnis, izin, dan state aplikasi tetap diperiksa kode.

Untuk keputusan berisiko, perlakukan output sebagai usulan. Kode deterministik dan manusia berwenang menetapkan tindakan akhir.

Dua bentuk tersedia, yaitu function calling untuk memakai tool dan `text.format` untuk respons ke user. JSON mode hanya menjamin JSON valid.

SDK mendukung schema via Zod dan Pydantic dengan `responses.parse`. Pola ini cocok untuk ekstraksi, klasifikasi, dan UI yang membutuhkan field pasti.

## Function calling dan tool

[Function calling](https://developers.openai.com/api/docs/guides/function-calling) memakai alur lima langkah milik aplikasi, bukan eksekusi oleh model.

Model mengirim tool call, aplikasi menjalankan kode yang diizinkan, lalu hasil dikirim kembali dengan `call_id`. Setelah itu model menyusun jawaban akhir.

Bedakan function tools ber-JSON Schema, custom tools teks bebas, dan built-in tools. Pilih bentuk tersempit yang memenuhi kebutuhan tugas.

Pisahkan pemilihan tool dari otorisasi. Identitas, permission, precondition, rate limit, dan audit harus diperiksa oleh kode.

Gunakan tool sempit, bertipe, dan berefek jelas. Tindakan menghapus, membeli, mengirim, atau membuka akses perlu konfirmasi yang sesuai.

Tool call adalah input tidak tepercaya. Argumen buatan model dapat salah, jahat, atau disuntik lewat prompt injection dan data eksternal.

## Streaming

[Streaming](https://developers.openai.com/api/docs/guides/streaming-responses) memakai SSE dengan event semantik seperti delta teks dan status respons.

Streaming mempercepat token pertama terlihat. Ia tidak otomatis mengurangi total waktu inferensi, token, atau biaya.

UI harus menangani state submitting, streaming, completed, cancelled, dan failed. Parser perlu mengikuti framing protokol secara ketat.

Detail transport dibahas di [[References/Streamed Responses\|Streamed Responses]]. Jangan menganggap setiap chunk sebagai dokumen lengkap yang dapat diparse sendiri.

Moderasi output parsial lebih sulit karena skor penuh tiba setelah output selesai. Untuk konten sensitif, tunda render atau gunakan filter tambahan.

## Realtime dan audio

[Realtime API GA](https://developers.openai.com/api/docs/guides/realtime) memakai WebRTC untuk agen suara dan audio interaktif.

Browser atau mobile memakai kredensial ephemeral dari server. Secret jangka panjang tidak boleh ditanam pada klien.

Integrasi beta lama memakai header `realtime=v1` dan perlu dimigrasi. Bentuk session dan event GA harus dicek pada referensi terbaru.

Gunakan panduan voice agents untuk speech-to-speech dan panduan transkripsi untuk stream transcript. Lacak biaya audio per menit secara terpisah.

Kirim identifier safety yang stabil dan privacy-preserving untuk user akhir. Tanpa itu, kontrol penyalahgunaan dan audit menjadi lebih lemah.

## Rate limit dan production

[Rate limit](https://developers.openai.com/api/docs/guides/rate-limits) memakai RPM, TPM, RPD, TPD, IPM, dan menit audio menurut model.

Limit ditetapkan pada level organization dan project, bukan user. Keluarga model tertentu memakai shared limit yang dihitung bersama.

Pantau header `x-ratelimit` untuk sisa kuota dan waktu reset. Tangani `429` dan `503` dengan backoff acak, bukan retry agresif.

Batch request atau Batch API membantu workload tidak interaktif. Untuk sinkron, gabungkan tugas kecil agar throughput token lebih efisien.

[Panduan production](https://developers.openai.com/api/docs/guides/production-best-practices) menekankan peran reader dan owner pada organization.

Simpan key pada environment atau secret manager. Terapkan spend alert, hard spend limit, tier usage, eval, dan observabilitas sebelum scale.

## Model terbuka gpt-oss

[Pengumuman gpt-oss](https://openai.com/index/introducing-gpt-oss) merilis `gpt-oss-120b` dan `gpt-oss-20b` sebagai open-weight reasoning.

Lisensi Apache 2.0 memungkinkan eksperimen dan deployment komersial. [Halaman open models](https://openai.com/open-models) menekankan agen, kustomisasi, dan CoT penuh.

Arsitektur memakai Transformer mixture-of-experts dengan konteks native 128k. Model 120b cocok untuk GPU 80 GB, 20b untuk edge 16 GB.

Model mendukung effort low, medium, dan high, plus Structured Outputs dan Responses API. Klaim benchmark vendor perlu eval independen.

CoT tidak disupervisi langsung agar dapat dimonitor. Jangan tampilkan CoT mentah ke user karena dapat berisi halusinasi atau konten sensitif.

Keterbukaan bobot memperbesar risiko fine-tuning jahat. OpenAI menguji versi adversarial, tetapi hasil itu tidak menjamin deployment tertentu aman.

## Usage policy dan safety

[Usage Policies](https://openai.com/policies/usage-policies) melarang kekerasan, senjata, CBRNE, siber jahat, dan eksploitasi anak.

Kebijakan juga membatasi privasi, pengenalan wajah tanpa izin, manipulasi pemilu, penipuan, dan keputusan high-stakes tanpa review manusia.

Pelanggaran dapat menyebabkan kehilangan akses atau penalti lain. Kebijakan diperbarui berkala, terakhir pada 2025-10-29 dalam changelog.

[Pendekatan safety](https://openai.com/safety) memakai teach, test, dan share dengan red teaming dan evaluasi kesiapan.

Filter dan refusal bukan policy engine lengkap. Aplikasi tetap perlu authorization, validasi, output encoding, dan incident response.

Pola review dan quality gate dibahas di [[References/AI-Powered Code Review\|AI-Powered Code Review]] dan [[References/Refactoring dengan AI\|Refactoring dengan AI]]. Pola asisten coding ada pada [[References/GitHub Copilot\|GitHub Copilot]].

## Evaluasi dan batas kesimpulan

Confidence halaman ini medium. Mekanisme API didukung dokumentasi primer, tetapi lineup model dan angka berubah cepat.

[Pengumuman GPT-5](https://openai.com/index/introducing-gpt-5) berisi klaim router, thinking, dan benchmark vendor yang belum diuji independen di sini.

Benchmark vendor memakai subset dan setting tertentu. Prompt, tool, konteks, sampling, latency, biaya, dan data dapat mengubah hasil.

Buat eval dari pekerjaan nyata. Ukur correctness, groundedness, task completion, format validity, tool success, latency, dan biaya.

Bandingkan dengan baseline non-AI dan model lain pada kondisi sama. Sertakan biaya review, retry, fallback, dan insiden, bukan hanya harga token.

Belum ada perbandingan independen lintasmodel pada workload identik dalam scope ini. Keputusan akhir memakai data dan failure mode aplikasi sendiri.

## Lihat juga

- [[References/Cara Kerja LLM\|Cara Kerja LLM]]
- [[References/Prompt Engineering\|Prompt Engineering]]
- [[References/AI Agents\|AI Agents]]
- [[References/Generative AI for Frontend Development\|AI dalam Pengembangan Frontend]]
- [[References/GitHub Copilot\|GitHub Copilot]]
- [[References/Gemini\|Gemini]]
- [[References/Streamed Responses\|Streamed Responses]]
- [[References/Model Context Protocol\|Model Context Protocol]]

## Sumber

- [About OpenAI](https://openai.com/about), misi AGI, visi, dan struktur nonprofit serta public benefit corporation.
- [Models](https://developers.openai.com/api/docs/models), katalog model sebagai rujukan operasional saat implementasi.
- [Using GPT-5.6](https://developers.openai.com/api/docs/guides/latest-model), alias sol terra luna, effort, pro mode, caching, dan tool calling.
- [Reasoning](https://developers.openai.com/api/docs/guides/reasoning), effort, mode, token reasoning, konteks, biaya, dan contoh Responses API.
- [Migrate to Responses](https://developers.openai.com/api/docs/guides/migrate-to-responses), arah Responses API sebagai jalur utama.
- [Text generation](https://developers.openai.com/api/docs/guides/text), navigasi generasi teks, kode, prompting, dan reasoning.
- [Function calling](https://developers.openai.com/api/docs/guides/function-calling), alur lima langkah, jenis tool, dan contoh horoscope.
- [Structured outputs](https://developers.openai.com/api/docs/guides/structured-outputs), JSON Schema strict, Zod, Pydantic, dan versus JSON mode.
- [Streaming](https://developers.openai.com/api/docs/guides/streaming-responses), SSE, event semantik, dan risiko moderasi parsial.
- [Realtime](https://developers.openai.com/api/docs/guides/realtime), WebRTC GA, kredensial ephemeral, dan migrasi beta.
- [Audio](https://developers.openai.com/api/docs/guides/audio), konsep input output audio, streaming, latensi, dan transkrip.
- [Rate limits](https://developers.openai.com/api/docs/guides/rate-limits), RPM TPM tier header backoff batching dan Batch API.
- [Production](https://developers.openai.com/api/docs/guides/production-best-practices), organization, billing, key, spend limit, dan arsitektur.
- [Usage policies](https://openai.com/policies/usage-policies), larangan, privasi, anak, high-stakes, dan changelog.
- [Safety](https://openai.com/safety), teach test share, red teaming, system cards, dan area risiko.
- [Introducing GPT-5](https://openai.com/index/introducing-gpt-5), router, thinking, benchmark vendor, dan safe completions.
- [Introducing gpt-oss](https://openai.com/index/introducing-gpt-oss), MoE, konteks 128k, eval, CoT, dan safety worst-case.
- [Open models](https://openai.com/open-models), lisensi Apache 2.0, agen, kustomisasi, dan CoT penuh.
