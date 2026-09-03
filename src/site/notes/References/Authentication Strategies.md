---
{"dg-publish":true,"dg-path":"Authentication Strategies.md","permalink":"/authentication-strategies/","title":"Authentication Strategies","hideInFiletree":true,"tags":["references","security","auth","architecture"],"noteIcon":"","dg-note-properties":{"title":"Authentication Strategies","category":"references","tags":["references","security","auth","architecture"],"sources":["_raw/articles/authentication-strategies-research-packet.md"],"created":"2026-09-03","updated":"2026-09-03","confidence":"high"}}
---


Authentication memverifikasi bahwa pihak yang meminta akses menguasai autentikator yang terikat ke suatu identitas. Authorization kemudian menentukan resource dan tindakan yang diizinkan.

Identity proofing berbeda lagi. Proses itu menilai apakah identitas digital benar-benar terkait dengan orang atau organisasi yang diklaim.

Strategi autentikasi bukan sekadar format token. Ia mencakup cara login, penyimpanan state, pengiriman bukti, validasi, pencabutan, pemulihan akun, dan peningkatan assurance.

## Model ancaman dan kebutuhan

Pilihan dimulai dari client, trust boundary, data, dan dampak kompromi. Browser first-party, API internal, aplikasi native, integrasi pihak ketiga, dan tenaga kerja enterprise memiliki kebutuhan berbeda.

Nilai juga phishing, credential stuffing, pencurian token, session fixation, CSRF, XSS, perangkat hilang, insider risk, serta serangan pada recovery dan fallback.

Kebutuhan operasional meliputi logout, pencabutan segera, skala horizontal, audit, interoperabilitas, masa hidup akses, dan pengalaman pengguna.

TLS wajib untuk semua strategi yang mengirim kredensial atau token. TLS melindungi jalur transport, tetapi tidak memperbaiki penyimpanan buruk, XSS, phishing, atau validasi token yang salah.

## HTTP Basic Authentication

HTTP Basic mengirim user ID dan password yang dikodekan dengan Base64 pada setiap request. Base64 bukan enkripsi, sehingga Basic tidak aman tanpa TLS.

Basic sederhana dan luas didukung. Ia dapat memadai untuk alat internal terbatas, integrasi legacy, atau akses mesin yang dilindungi TLS dan secret management ketat.

Kelemahannya ialah kredensial reusable terus dikirim. Basic tidak memiliki delegation, scope, rotasi, logout, atau MFA sebagai bagian native protokol.

Jangan memakai Basic untuk login konsumen modern bila session, OIDC, atau kredensial tahan phishing tersedia. Jangan menaruh kredensial Basic di URL, log, source code, atau penyimpanan client publik.

## Session berbasis server

Dalam session-based authentication, server memvalidasi login lalu menyimpan state autentikasi. Client menerima session ID opaque yang biasanya dikirim melalui cookie.

Session ID menjadi setara sementara dengan hasil login. ID harus acak, tidak bermakna, diperbarui setelah autentikasi atau perubahan privilege, dan dimatikan saat logout atau kedaluwarsa.

Cookie web sebaiknya memakai `Secure`, `HttpOnly`, dan kebijakan `SameSite` yang sesuai. Proteksi CSRF tetap diperlukan ketika browser mengirim cookie secara otomatis dalam konteks yang berisiko.

State server memudahkan pencabutan, logout, dan kontrol perangkat. Biayanya ialah session store, replikasi, cleanup, dan ketergantungan availability pada state tersebut.

Session cocok untuk aplikasi web first-party ketika backend mengendalikan browser flow dan membutuhkan pencabutan cepat. Cookie hanyalah mekanisme transport, bukan definisi session.

## Token-based authentication

Token-based authentication memakai token sebagai bukti akses setelah login atau grant lain. Token dapat opaque atau self-contained, dan dapat dibawa melalui header, cookie, atau kanal lain.

Bearer token dapat digunakan siapa pun yang memilikinya tanpa bukti kunci tambahan. Token harus dilindungi saat transit dan tersimpan, dibatasi scope dan audience, serta berumur sesingkat yang praktis.

Opaque token memerlukan lookup atau introspection agar resource server mengetahui status dan hak akses. Model ini memberi kontrol pusat dan pencabutan cepat dengan biaya ketergantungan runtime.

Self-contained token membawa data yang dapat divalidasi lokal. Model ini mengurangi lookup, tetapi pencabutan segera biasanya memerlukan denylist, introspection, rotasi, atau masa hidup pendek.

Penyimpanan token di browser mengubah profil ancaman. JavaScript-readable storage rentan terhadap XSS, sedangkan cookie otomatis membawa risiko CSRF dan tetap dapat disalahgunakan melalui request yang dipicu penyerang.

## JWT

JSON Web Token adalah format claims yang ringkas dan URL-safe, bukan strategi autentikasi. JWT dapat dipakai sebagai access token, ID Token, session artifact, atau objek lain menurut profilnya.

JWT bertanda tangan memberi integritas dan autentisitas penerbit bila diverifikasi benar. Payload JWT bertanda tangan biasa dapat dibaca dan tidak boleh dianggap terenkripsi.

Validator harus membatasi algoritme, memverifikasi signature, issuer, audience, expiry, dan klaim yang diwajibkan profil. Key selection dan rotasi juga harus mengikuti sumber kunci tepercaya.

JWT bukan otomatis lebih aman atau lebih scalable daripada session. Keputusan bergantung pada kebutuhan validasi lokal, pencabutan, ukuran token, distribusi kunci, dan batas kepercayaan.

## OAuth 2.0

OAuth adalah framework authorization untuk memberi client akses terbatas ke resource. Ia memisahkan resource owner, client, authorization server, dan resource server.

OAuth tidak dengan sendirinya membuktikan identitas pengguna kepada client. Istilah "Login with Google" biasanya berarti OpenID Connect atau profil identitas lain di atas OAuth.

Untuk browser dan aplikasi native, Authorization Code dengan PKCE adalah baseline yang prudent. Redirect URI harus tervalidasi ketat, dan flow memerlukan proteksi CSRF serta validasi issuer bila relevan.

Scope membatasi permintaan akses, tetapi bukan pengganti authorization di resource server. Access token juga harus dibatasi audience dan privilege agar kebocoran tidak membuka resource lain.

Grant lama seperti Resource Owner Password Credentials sebaiknya tidak dipakai. Ia membuat client menangani password pengguna dan menghilangkan banyak manfaat pemisahan peran OAuth.

## OpenID Connect

OpenID Connect menambahkan identity layer di atas OAuth 2.0. Client menerima ID Token untuk memverifikasi hasil autentikasi dan dapat mengambil claims profil melalui UserInfo.

ID Token ditujukan kepada client dan tidak otomatis menjadi access token untuk API. Client harus memverifikasi issuer, audience, signature, expiry, nonce, dan aturan flow yang dipakai.

OIDC cocok untuk login federatif aplikasi konsumen, SaaS, mobile, dan web. Ia mengurangi kebutuhan aplikasi menyimpan password pihak ketiga, tetapi menambah ketergantungan pada identity provider.

## Single Sign-On

Single Sign-On adalah pengalaman satu login untuk mengakses beberapa aplikasi. SSO adalah hasil arsitektur, bukan satu protokol.

Enterprise SSO umum memakai SAML 2.0 atau OpenID Connect. Identity provider mengautentikasi pengguna, lalu service provider mempercayai assertion atau token sesuai konfigurasi federation.

SSO memusatkan kebijakan, onboarding, offboarding, dan audit. Ia juga memperbesar blast radius gangguan atau kompromi identity provider, sehingga MFA, monitoring, dan recovery harus dirancang serius.

Single Logout tidak selalu konsisten antarprotokol dan produk. Aplikasi tetap perlu mengelola session lokal, expiry, back-channel event bila tersedia, dan pencabutan akses.

## MFA dan step-up authentication

Multi-factor authentication memakai faktor independen, misalnya sesuatu yang diketahui, dimiliki, atau melekat pada pengguna. Dua langkah dari kategori faktor yang sama bukan otomatis MFA.

MFA mengurangi risiko kompromi satu faktor. Namun SMS, OTP, push, dan faktor tahan phishing memiliki sifat keamanan berbeda.

Step-up authentication meminta assurance lebih tinggi saat risiko meningkat atau tindakan sensitif dilakukan. Contohnya perubahan email, pembayaran, ekspor data, atau administrasi hak akses.

Reauthentication juga membatasi dampak session yang ditinggalkan atau dibajak. Kebijakan harus menyeimbangkan risiko, masa session, sinyal perangkat, dan friction.

## Passkeys dan WebAuthn

Passkeys memakai public-key credentials yang terikat ke relying party. Server menyimpan public key, sedangkan authenticator menggunakan private key setelah verifikasi pengguna.

Binding ke relying-party origin membuat WebAuthn tahan terhadap phishing dibanding password dan OTP yang dapat dimasukkan ke situs palsu.

Passkeys dapat tersinkronisasi atau terikat perangkat. Arsitektur harus menilai pemulihan akun, perangkat baru, shared device, attestation, dan fallback karena jalur tersebut dapat melemahkan sistem.

## Perbandingan

| Strategi | State utama | Kekuatan | Risiko dan biaya utama |
|---|---|---|---|
| Basic | Password reusable | Sederhana dan interoperabel | Kredensial dikirim berulang, tanpa delegation atau scope |
| Server session | State server dan ID opaque | Revocation serta logout terpusat | Session store, cookie security, CSRF |
| Opaque token | Authorization server | Kontrol pusat dan introspection | Lookup runtime dan availability |
| JWT access token | Claims bertanda tangan | Validasi lokal dan distribusi | Revocation, key rotation, claim validation |
| OAuth | Grant dan access token | Delegation serta limited access | Flow kompleks dan salah konfigurasi redirect |
| OIDC | OAuth ditambah ID Token | Login federatif interoperabel | Ketergantungan IdP dan validasi protocol |
| SSO | Federation dan session lokal | Satu login serta lifecycle terpusat | Blast radius IdP dan logout lintas aplikasi |
| Passkey | Public-key credential | Tahan phishing dan tanpa shared secret | Recovery, perangkat, dan fallback |

## Pedoman pemilihan

Untuk web first-party, session server dengan cookie terproteksi sering menjadi pilihan sederhana. Gunakan token bila API, client native, atau boundary layanan membutuhkan delegation atau validasi terdistribusi.

Untuk login federatif, gunakan OIDC. Untuk akses API pihak ketiga, gunakan OAuth dengan scope sempit. Untuk workforce enterprise, dukung OIDC atau SAML sesuai identity provider pelanggan.

Untuk assurance tinggi, tawarkan MFA tahan phishing atau passkey. Jangan mengandalkan JWT, SSO, atau MFA sebagai label keamanan tanpa memeriksa validasi, recovery, revocation, dan fallback.

Gunakan umur akses pendek, rotasi aman, rate limiting, deteksi anomali, audit, dan reauthentication untuk tindakan sensitif. Hindari membangun kriptografi atau parser token sendiri.

## Batasan

Tidak ada strategi yang unggul untuk semua sistem. Pilihan akhir membutuhkan threat model, klasifikasi data, client yang didukung, kebutuhan regulasi, serta uji pada implementasi aktual.

Standar dan rekomendasi keamanan berkembang. Default produk, grant yang didukung, perilaku browser, dan kebijakan identity provider harus diperiksa kembali pada versi yang digunakan.

## Terkait

- [[References/Auth di Supabase\|Auth di Supabase]]
- [[References/JWTs di Supabase\|JWTs di Supabase]]
- [[References/Enterprise SSO di Supabase\|Enterprise SSO di Supabase]]
- [[References/OAuth 2.1 Server di Supabase\|OAuth 2.1 Server di Supabase]]
- [[References/Sessions di Supabase\|Sessions di Supabase]]
- [[References/MFA di Supabase\|MFA di Supabase]]
- [[References/Passkeys di Supabase\|Passkeys di Supabase]]