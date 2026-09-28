# ADR-0006: Input MVP Memakai Perintah Teks, Tanpa LLM

- **Status:** Diterima
- **Tanggal:** 2026-09-24

## Konteks

Ada tiga cara pengguna memasukkan pengingat:

1. Bahasa alami yang diproses LLM (misalnya *"ingetin bayar kartu kredit tiap tanggal 25"*).
2. Perintah teks dengan format tetap.
3. Formulir di dalam WhatsApp melalui **WhatsApp Flows** (mendukung isian teks, dropdown, dan pemilih tanggal).

## Keputusan

- **MVP memakai perintah teks** dengan format tetap.
- **WhatsApp Flows** ditambahkan di fase 2 untuk menambah dan mengubah pengingat.
- **LLM tidak dipakai** untuk saat ini.

## Alasan

- Bagian tersulit proyek ada di backend (penjadwalan, aturan pengulangan, kasus tepi tanggal). Perintah teks memungkinkan fokus ke sana lebih dulu.
- Parser perintah teks mudah diuji otomatis tanpa membuka WhatsApp. Flows membutuhkan endpoint HTTPS yang mendekripsi data (AES-GCM) dan persetujuan Meta.
- Flows hanya mengganti lapisan input. Jika arsitekturnya benar ([ADR-0008](0008-arsitektur-tidak-terikat-saluran.md)), perpindahan ke Flows tidak menyentuh logika inti.
- Perintah teks tetap berguna setelah Flows ada, sebagai jalan pintas.

## Konsekuensi

- Pengguna perlu mengingat format perintah. Perintah `/bantuan` wajib tersedia.
- Parser harus memberi pesan kesalahan yang jelas jika format salah.
