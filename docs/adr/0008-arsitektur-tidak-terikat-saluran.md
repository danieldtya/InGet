# ADR-0008: Arsitektur Tidak Terikat pada Satu Saluran

- **Status:** Diterima
- **Tanggal:** 2026-09-24

## Konteks

WhatsApp adalah saluran utama, tetapi ada rencana dashboard web desktop (fase 3), dan kemungkinan saluran lain di masa depan (misalnya Telegram sebagai cadangan, atau aplikasi Android kecil untuk alarm).

## Keputusan

Logika inti (data pengingat, aturan pengulangan, penjadwalan, penyusunan rekap) dipisahkan dari saluran. WhatsApp, WhatsApp Flows, dan dashboard web masing-masing hanyalah **adapter** yang membaca dan mengubah data melalui logika inti yang sama.

## Alternatif yang Dipertimbangkan

- **Logika ditulis langsung di handler webhook WhatsApp:** lebih cepat di awal, tetapi menambah dashboard atau saluran baru berarti menulis ulang logika yang sama.

## Konsekuensi

- Struktur kode sedikit lebih banyak lapisan di awal.
- Menambah saluran baru cukup dengan menulis adapter baru.
- Logika inti bisa diuji tanpa bergantung pada WhatsApp.
