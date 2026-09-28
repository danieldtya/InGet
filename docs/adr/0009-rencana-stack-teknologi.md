# ADR-0009: Rencana Stack Teknologi

- **Status:** Diusulkan
- **Tanggal:** 2026-09-24 (diperbarui 2026-09-28)

## Konteks

Stack perlu mendukung webhook WhatsApp, pemicu terjadwal setiap jam ([ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md)), penyimpanan data pengingat, serta dashboard web di fase 3. Proyek ditujukan untuk berjalan dengan biaya seminimal mungkin.

## Usulan

| Komponen | Usulan |
| --- | --- |
| Pengembangan | Seluruhnya di laptop terlebih dahulu, dengan tunnel HTTPS untuk menerima webhook |
| Backend dan webhook | Python, FastAPI |
| Database | PostgreSQL (Supabase) |
| Aturan pengulangan | `python-dateutil` |
| Dashboard (fase 3) | React, Vite, TypeScript |

## Hal yang Perlu Diputuskan Sebelum Diterima

- **Hosting produksi.** Dua kandidat yang sama-sama gratis:
  - **Render + Supabase:** tetap FastAPI, tetapi layanan gratis Render tidur setelah 15 menit tanpa trafik dan butuh sekitar satu menit untuk bangun.
  - **Supabase saja:** balasan selalu cepat dan hanya satu platform, tetapi backend ditulis dalam TypeScript (Edge Function), bukan FastAPI.
- **Mekanisme pemicu setiap jam**, yang bergantung pada pilihan hosting.
- **Peringatan saat bot gagal**, supaya kegagalan tidak terjadi diam-diam.
- **Penyimpanan token dan kredensial** di luar repositori.

Keputusan ini sengaja ditunda sampai MVP berjalan di laptop.
