# ADR-0004: Pengingat H-x yang Bisa Dipilih Lebih dari Satu

- **Status:** Digantikan oleh [ADR-0011](0011-default-h-x-dan-jam-kirim.md)
- **Tanggal:** 2026-09-24

## Konteks

Pengguna perlu diingatkan sebelum hari H agar sempat bersiap. Ada dua pendekatan: pola bertahap yang tetap (misalnya H-7, H-3, H-1), atau satu H-x yang dipilih pengguna.

Pengingat jauh dan pengingat dekat memiliki fungsi berbeda. Pengingat jauh (misalnya H-7) membantu **persiapan**, seperti menyisihkan uang atau membeli kado. Pengingat dekat (H-1 atau H-0) membantu **eksekusi**. Satu pengingat saja tidak dapat memenuhi keduanya.

## Keputusan

- Setiap pengingat bermode Notifikasi dapat memiliki **daftar H-x**, misalnya `7,1,0`. Default: `0`.
- Pesan **H-x dengan x ≥ 1** dikirim bersamaan dengan **jam rekap harian**.
- Pesan **H-0** dikirim **tepat di jam kegiatan**.

## Alternatif yang Dipertimbangkan

- **Pola bertahap tetap:** tidak fleksibel. Tidak semua kegiatan membutuhkan persiapan seminggu.
- **Satu H-x pilihan pengguna:** tidak bisa memenuhi fungsi persiapan dan eksekusi sekaligus.

## Konsekuensi

- Data H-x disimpan sebagai daftar bilangan bulat per pengingat.
- Pengingat H-x yang jatuh di hari yang sama dengan rekap bisa digabung ke dalam pesan rekap atau dikirim terpisah. Keputusan ini diambil saat implementasi.
