# ADR-0025: Balasan Bot dalam Bahasa Indonesia, Disusun dengan Template Teks

- **Status:** Diterima
- **Tanggal:** 2026-10-08

## Konteks

[ADR-0023](0023-bahasa-perintah-dan-dokumentasi.md) menetapkan perintah dalam bahasa Inggris, tetapi menyisakan pertanyaan terbuka tentang bahasa isi balasan bot.

## Keputusan

- Isi balasan bot (bubble, notifikasi, rekap, pesan kesalahan, bantuan) memakai **bahasa Indonesia**.
- Balasan disusun dengan **template teks berisi isian**, misalnya `f'Pengingat "{judul}" tersimpan untuk {tanggal}'`. Nama hari dan bulan dalam bahasa Indonesia disimpan sebagai daftar sederhana di kode.
- Perintah dibaca dengan **parser berbasis aturan**: teks dipecah berdasarkan tanda `|`, lalu setiap bagian divalidasi, dengan bantuan regular expression untuk pola seperti tanggal dan daftar H-x.
- **Tidak ada NLP atau model AI** di MVP.

## Alasan

Format perintah sudah ditentukan, sehingga tidak ada bahasa bebas yang perlu dipahami. Template teks memberi hasil yang selalu sama dan mudah diuji.

## Konsekuensi

- Semua teks balasan sebaiknya dikumpulkan di satu modul, supaya mudah diubah atau diterjemahkan di masa depan.
- Input bahasa alami tetap berada di fase 4.
