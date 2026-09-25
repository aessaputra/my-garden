---
{"dg-publish":true,"dg-path":"Server Security.md","permalink":"/server-security/","title":"Server Security","hideInFiletree":true,"tags":["references","security","monitoring","backup","network"],"noteIcon":"","dg-note-properties":{"title":"Server Security","aliases":["server security"],"categories":["Security"],"type":"reference","tags":["references","security","monitoring","backup","network"],"sources":["_raw/articles/server-security-user-summary-2026-09-25.md"],"created":"2026-09-25","updated":"2026-09-25","confidence":"medium"}}
---

Server security melindungi server dari ancaman agar data dan layanan tetap confidential, integral, dan available. Ia melengkapi [[References/Web Security Knowledge\|Web Security Knowledge]] yang fokus pada lapisan aplikasi: aplikasi yang aman tetap bocor bila host-nya tidak di-patch, terekspos, atau tanpa backup.

## Kontrol utama

- **Patch management**: perbarui OS, runtime, dan modul (mis. Windows Server dan modul [[References/IIS\|IIS]]) secara teratur; uji setelah perubahan seperti praktik pada [[References/Web Servers\|Web Servers]].
- **Access control**: hak minimal — batasi akses filesystem, konfigurasi, dan kredensial hanya pada yang memerlukan.
- **Firewall & hardening**: tutup port/layanan tak terpakai, nonaktifkan fitur bawaan yang tidak dibutuhkan, verifikasi konfigurasi lintas environment.
- **Encryption**: TLS untuk transit (lihat [[References/HTTPS\|HTTPS]]) dan enkripsi untuk data sensitif saat at-rest sesuai klasifikasi data.
- **Backup**: cadangkan data dan konfigurasi berkala; backup yang tidak pernah diuji restore-nya bukan backup.
- **Monitoring & response**: deteksi dan respons ancaman berkelanjutan — logging, alerting, dan tindak lanjut temuan, bukan sekadar mengumpulkan log.

## Batas

Kontrol di atas menurunkan risiko, bukan menjamin kebal. Urutan remediasi mengikuti inventaris aset, klasifikasi data, threat model, dan konteks bisnis — bukan sekadar daftar generik.

## Sumber

- Ringkasan pengguna di `_raw/articles/server-security-user-summary-2026-09-25.md`; tanpa fetch eksternal baru.
