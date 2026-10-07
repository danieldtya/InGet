# ADR-0023: Perintah dalam Bahasa Inggris, Dokumentasi dalam Bahasa Indonesia

- **Status:** Diterima
- **Tanggal:** 2026-10-07

## Konteks

Nama perintah dan kata kunci (frekuensi, mode) akan diketik setiap hari. Proyek juga dirancang agar bisa berkembang di luar Indonesia.

## Keputusan

- **Nama perintah dan kata kunci** memakai bahasa Inggris, misalnya `/add`, `/agenda`, `monthly`, `notify`.
- **Dokumentasi** (README, ADR, dokumen kebutuhan) memakai bahasa Indonesia.
- Nama tabel, kolom, dan kode memakai bahasa Inggris, mengikuti konvensi umum.

## Alasan

Bahasa Inggris lebih universal dan sudah lazim untuk perintah aplikasi. Dokumentasi berbahasa Indonesia lebih mudah dirawat oleh pemilik proyek.

## Pertanyaan Terbuka

Bahasa **isi balasan bot** (bubble, pesan notifikasi, pesan kesalahan) belum diputuskan.

## Konsekuensi

- Daftar nama perintah dan kata kunci di README bersifat draf sampai sintaks final diputuskan.
