# ADR-0028: Percakapan Terpandu sebagai Input Utama, Perintah Satu Baris sebagai Jalan Pintas

- **Status:** Diterima
- **Tanggal:** 2026-10-13
- **Menggantikan:** [ADR-0006](0006-input-mvp-perintah-teks.md)

## Konteks

ADR-0006 memilih perintah teks satu baris untuk MVP, misalnya `/add 25/10/2026 08:00 | Bayar kartu kredit | monthly | notify | 7,2`. Format seperti ini terasa seperti terminal dan mudah terlupa, padahal pemilik menjalankan beberapa proyek sekaligus dan sering lupa detail teknis.

ADR-0006 menunda **WhatsApp Flows** karena membutuhkan endpoint terenkripsi dan persetujuan Meta. Ada cara interaktif ketiga yang belum dipertimbangkan saat itu: **percakapan terpandu**.

## Keputusan

- **Cara utama di MVP:** percakapan terpandu. Bot menanyakan setiap isian satu per satu, dan pilihan tetap disajikan sebagai **tombol balasan** (paling banyak 3) atau **daftar pilihan** (paling banyak 10 baris) WhatsApp.
- **Jalan pintas:** perintah satu baris tetap tersedia dan didokumentasikan di README.
- **WhatsApp Flows** tetap di fase 2. **LLM** tetap tidak dipakai ([ADR-0025](0025-balasan-bot-bahasa-indonesia-tanpa-nlp.md)).
- Perintah **`/cancel`** membatalkan percakapan yang sedang berjalan.

### Urutan pertanyaan `/add` (draf)

| No | Pertanyaan | Bentuk jawaban |
| --- | --- | --- |
| 1 | Nama kegiatan | Teks |
| 2 | Tanggal (kemunculan pertama) | Teks `DD/MM/YYYY` |
| 3 | Jam | Teks `HH:MM`, atau tombol **Tanpa jam** |
| 4 | Frekuensi | Daftar pilihan (7 frekuensi). Untuk "Setiap N tahun", bot menanyakan N |
| 5 | Mode | Tombol **Notifikasi** dan **Rekap**, dengan default sesuai frekuensi ([ADR-0027](0027-jenis-isi-dan-urutan-pesan.md)) |
| 6 | H-x (hanya mode Notifikasi) | Tombol **Pakai default**, atau teks seperti `14,7` |
| 7 | Tanggal akhir (hanya pengingat berulang) | Teks `DD/MM/YYYY`, atau tombol **Tanpa tanggal akhir** |
| 8 | Pengingat menjelang waktu (hanya jika ada jam) | Daftar pilihan: Tidak perlu, 5, 10, 15, 30, 60 menit |
| 9 | Konfirmasi | Bubble ringkasan dengan tombol **Simpan** dan **Batal** |

`/edit` mengikuti pola yang sama: bot menanyakan isian mana yang ingin diubah, lalu meminta nilai barunya.

## Alasan

- Pemilik tidak perlu mengingat urutan parameter dan kata kunci.
- Tombol dan daftar pilihan dapat dikirim tanpa template, karena pemilik yang memulai percakapan (di dalam jendela layanan 24 jam). Tidak perlu endpoint terenkripsi maupun persetujuan Meta.
- Alur tanya jawab tetap dapat diuji tanpa WhatsApp, sebagai mesin status (state machine).
- Perintah satu baris tetap berguna untuk menambah pengingat dengan cepat.

## Konsekuensi

- Bot harus menyimpan **status percakapan** (sedang di langkah ke berapa dan jawaban sementara), sehingga dibutuhkan satu tabel tambahan.
- Perlu batas waktu: percakapan yang tidak dijawab dalam jangka tertentu dibatalkan otomatis.
- Tanggal tetap diketik. Pemilih tanggal hanya tersedia di WhatsApp Flows.
- Detail berikut difinalkan bersama sintaks dan isi pesan: batas waktu percakapan, teks setiap pertanyaan, dan pesan kesalahan per langkah.
