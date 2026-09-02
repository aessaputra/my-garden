---
{"dg-publish":true,"dg-path":"Model Context Protocol.md","permalink":"/model-context-protocol/","title":"Model Context Protocol","hideInFiletree":true,"tags":["references","gpt","research","security","architecture","ai-agents"],"dg-note-properties":{"title":"Model Context Protocol","category":"references","tags":["references","gpt","research","security","architecture","ai-agents"],"sources":["_raw/articles/model-context-protocol-research-packet.md"],"created":"2026-09-02","updated":"2026-09-02","confidence":"high"}}
---

Model Context Protocol (MCP) adalah protokol terbuka untuk menghubungkan aplikasi LLM dengan sumber data, tool, dan layanan eksternal melalui antarmuka standar.

MCP bukan aturan untuk mengemas system role, permintaan pengguna, memory, riwayat, tool call, atau kode ke dalam satu prompt. Penyusunan konteks akhir tetap menjadi tanggung jawab aplikasi host.

Primitive `prompts` memang dapat membawa template pesan. Namun, ia hanya satu fitur MCP dan tidak menetapkan urutan global isi prompt model.

## Masalah yang diselesaikan

Tanpa protokol bersama, setiap aplikasi AI memerlukan integrasi khusus untuk setiap database, repository, API, atau layanan. MCP menyeragamkan batas komunikasi agar client dan server dapat dipadukan ulang.

Analogi yang prudent adalah protokol integrasi, bukan format prompt. MCP mendefinisikan pertukaran capability dan pesan, sedangkan [[References/Prompt Engineering\|Prompt Engineering]] membahas desain instruksi serta konteks bagi model.

Standardisasi mengurangi pekerjaan konektor, tetapi tidak menjamin kualitas data, kebenaran jawaban, keamanan server, atau keberhasilan tugas agent.

## Arsitektur

[MCP memakai arsitektur client-server](https://modelcontextprotocol.io/docs/learn/architecture) dengan tiga peran: host, client, dan server.

Host adalah aplikasi AI seperti IDE atau antarmuka chat. Host mengelola pengalaman pengguna, izin, lifecycle koneksi, kebijakan keamanan, dan cara context diberikan kepada model.

Host membuat satu MCP client untuk setiap MCP server. Client mempertahankan koneksi, menukar capability, dan meneruskan pesan antara host dengan server tersebut.

Server menyediakan context atau capability melalui primitive protokol. Server dapat berjalan lokal sebagai subprocess atau remote sebagai layanan jaringan.

Batas ini penting bagi [[References/AI Agents\|AI Agents]]. MCP menyediakan jalur ke tool dan data, sedangkan loop, perencanaan, approval, state, evaluasi, dan kondisi berhenti tetap ditentukan agent harness atau host.

## Dua lapisan

Data layer menggunakan JSON-RPC 2.0. Lapisan ini mendefinisikan pesan, versioning, capability, notification, dan primitive seperti resources, prompts, serta tools.

Transport layer mengatur channel, framing, koneksi, dan aspek authorization. Pemisahan ini memungkinkan semantik protokol digunakan pada transport berbeda.

[Binding standar](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports) mencakup `stdio` dan Streamable HTTP. Custom transport juga dapat dibuat bila memenuhi kebutuhan implementasi.

`stdio` memakai pesan berbatas baris melalui standard input dan output subprocess yang diluncurkan client. Bentuk ini umum bagi server lokal.

Streamable HTTP mengirim pesan ke satu endpoint MCP melalui HTTP POST. Respons dapat berupa objek JSON atau stream SSE yang terikat pada request.

## Primitive server

Server dapat menawarkan resources, prompts, dan tools. Dukungan diumumkan melalui capability, sehingga implementasi tidak wajib menyediakan semua primitive.

### Resources

[Resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources) mengekspos data atau context seperti file, schema database, dan informasi aplikasi. Setiap resource diidentifikasi dengan URI.

Resources bersifat application-driven. Host menentukan apakah resource dipilih pengguna, dicari, difilter, atau dimasukkan otomatis berdasarkan heuristik.

Resource tersedia tidak berarti isinya otomatis masuk ke context window. Host tetap mengendalikan pemilihan, izin, ukuran, dan penyajiannya kepada model.

### Prompts

[Prompts](https://modelcontextprotocol.io/specification/2026-07-28/server/prompts) adalah template pesan dan instruksi terstruktur. Client dapat menemukan prompt, mengambil isinya, dan memberikan argument untuk menyesuaikannya.

Prompts bersifat user-controlled dalam model interaksi konseptual MCP. Aplikasi biasanya menyajikannya sebagai command, pilihan, atau template eksplisit.

Protokol tidak mewajibkan satu antarmuka pengguna. Ia juga tidak menentukan bagaimana host menggabungkan prompt MCP dengan system message, memory, riwayat, atau kebijakan internal.

### Tools

[Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools) adalah fungsi yang dapat ditemukan dan dipanggil model. Contohnya mencakup query database, pemanggilan API, dan komputasi.

Definisi tool menyertakan nama, deskripsi, dan schema input. Hasil dapat memuat teks, gambar, audio, structured content, atau link ke resource.

Tool bersifat model-controlled secara konseptual, tetapi host dapat meminta approval, membatasi izin, menolak invocation, atau menyajikannya melalui pola interaksi lain.

## Fitur client

Server juga dapat meminta capability dari client. Fitur utamanya mencakup sampling, roots, dan elicitation.

[Sampling](https://modelcontextprotocol.io/specification/2026-07-28/client/sampling) memungkinkan server meminta generasi model melalui client. Client mempertahankan kontrol atas model, akses, izin, dan review pengguna.

Sampling dapat mendukung perilaku agentik yang bersarang di fitur server. Server tidak memerlukan API key model sendiri, tetapi host tetap harus mengendalikan biaya, data, izin, dan tool yang tersedia.

[Roots](https://modelcontextprotocol.io/specification/2026-07-28/client/roots) memberi tahu server tentang direktori atau file yang dianggap relevan oleh client.

Roots hanya guidance informasional, bukan sandbox atau access-control mechanism. Enforcement tetap memerlukan permission, validasi path, isolasi proses, dan kontrol filesystem.

[Elicitation](https://modelcontextprotocol.io/specification/2026-07-28/client/elicitation) memungkinkan server meminta informasi tambahan melalui client.

Mode form meminta data terstruktur. Mode URL mengarahkan pengguna ke interaksi sensitif di luar client, misalnya authorization atau pembayaran, tanpa meminta secret diketik ke server melalui form biasa.

## Versioning dan capability

[Spesifikasi 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28) mewajibkan base protocol, versioning, dan pola pesan. Fitur lain bersifat opsional berdasarkan capability.

Setiap request modern membawa versi protokol dan capability client. Server menerima atau menolak request menurut versi yang didukung.

Revisi lama memakai lifecycle dan handshake berbeda. Implementasi yang mendukung server lama memerlukan mekanisme deteksi dan fallback yang sesuai binding.

Capability negotiation mencegah asumsi bahwa semua pihak mendukung fitur sama. Ia tidak membuktikan bahwa implementasi benar, aman, atau sesuai kebutuhan aplikasi.

## Authorization

[Authorization MCP](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization) berlaku pada transport berbasis HTTP dan bersifat opsional.

Ketika digunakan, mekanismenya mengikuti standar OAuth terkait. Client meminta akses atas nama resource owner, sedangkan server hanya boleh menerima token yang ditujukan bagi resource miliknya.

Transport `stdio` tidak memakai alur HTTP tersebut. Server lokal biasanya memperoleh credential melalui environment atau mekanisme platform lain.

Halaman [[References/MCP Authentication di Supabase\|MCP Authentication di Supabase]] membahas salah satu penerapan OAuth bagi MCP server. Ia adalah implementasi authorization, bukan definisi seluruh MCP.

## Keamanan

MCP memperluas attack surface karena model dapat membaca data dan memicu fungsi eksternal. Risiko bergantung pada host, server, package, transport, izin, dan sistem yang dapat dijangkau.

Tool description dan resource content adalah input tidak tepercaya. Prompt injection dapat memengaruhi pemilihan tool atau mengarahkan model mengirim data ke tujuan yang tidak dimaksudkan.

Gunakan least privilege, scope sempit, approval untuk side effect, validasi input dan output, audit log, timeout, rate limit, serta pemisahan credential per server.

Server lokal harus diperlakukan seperti software yang dieksekusi di host. Tinjau sumber dan dependensi, pin versi, batasi filesystem serta network, dan hindari secret yang tidak diperlukan.

Untuk HTTP, validasi issuer, audience, redirect URI, dan token. [Pertimbangan keamanan authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/security-considerations) juga melarang token passthrough.

Roots tidak menggantikan sandbox. OAuth tidak menggantikan otorisasi per aksi. Consent awal tidak membenarkan seluruh tindakan lanjutan jika dampak, tujuan, atau penerima data berubah.

[OWASP](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html) menyoroti confused deputy, token theft, package berbahaya, message tampering, sandbox escape, dan serangan lintas server.

Tidak ada satu guardrail yang menghapus seluruh risiko. Kontrol perlu berlapis dan diuji dengan server gagal, input bermusuhan, izin berlebih, serta tindakan yang tidak dapat dibatalkan.

## MCP dan prompt assembly

Host dapat mengambil resource, template prompt, hasil tool, atau respons elicitation lalu memasukkannya ke proses model. Namun, MCP tidak mendikte seluruh representasi internal tersebut.

Urutan system instruction, user message, memory, history, tool schema, hasil tool, dan code snippet merupakan keputusan host, model API, framework, atau agent harness.

Tag yang jelas dan format stabil dapat membantu prompt assembly, tetapi itu praktik desain aplikasi. Menyebut praktik tersebut sebagai definisi MCP mencampur protokol integrasi dengan konstruksi prompt.

## Batas penggunaan

MCP tepat ketika beberapa aplikasi perlu memakai server yang sama, atau satu host perlu menghubungkan banyak sumber dan tool melalui interface konsisten.

Integrasi langsung dapat lebih sederhana untuk satu layanan kecil dengan kontrak stabil. MCP menambah server lifecycle, discovery, schema, permission, transport, observability, dan dependency management.

Kepatuhan MCP tidak menjamin semantic compatibility. Dua server dapat memakai nama, deskripsi, schema, error, atau side effect berbeda meski keduanya valid secara protokol.

MCP juga tidak membuat aplikasi menjadi agent. Aplikasi dapat memakai resources tanpa otonomi, prompts atas pilihan pengguna, atau tools melalui workflow deterministik.

## Sejarah dan governance

[Anthropic memperkenalkan MCP](https://www.anthropic.com/news/model-context-protocol) pada November 2024 sebagai standar terbuka untuk menghubungkan asisten AI dengan data dan tool.

Pada Desember 2025, [Anthropic mendonasikan MCP](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) ke Agentic AI Foundation di bawah Linux Foundation.

Klaim adopsi dalam pengumuman tersebut berasal dari Anthropic dan berlaku pada tanggal publikasi. Jumlah server, produk pendukung, governance, serta revisi spesifikasi dapat berubah.

## Lihat juga

- [[References/AI Agents\|AI Agents]]
- [[References/Prompt Engineering\|Prompt Engineering]]
- [[References/MCP Authentication di Supabase\|MCP Authentication di Supabase]]
- [[References/Cursor\|Cursor]]
- [[References/Cara Kerja LLM\|Cara Kerja LLM]]

## Sumber

- [MCP Architecture](https://modelcontextprotocol.io/docs/learn/architecture): host, client, server, data layer, dan transport layer.
- [MCP Specification 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28): base protocol, primitive, capability, dan utility.
- [MCP Authorization](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization): authorization untuk transport HTTP.
- [Anthropic: Introducing MCP](https://www.anthropic.com/news/model-context-protocol): peluncuran dan tujuan awal MCP.
- [OWASP MCP Security](https://cheatsheetseries.owasp.org/cheatsheets/MCP_Security_Cheat_Sheet.html): risiko dan kontrol deployment MCP.
