# ADR-0021: Idempotensi Pesan Masuk dan Pesan Keluar

- **Status:** Diterima
- **Tanggal:** 2026-10-07

## Konteks

Ada dua sumber pemrosesan ganda:

1. **Pesan masuk:** Meta dapat mengirim webhook yang sama lebih dari sekali, misalnya jika server terlambat membalas. Tanpa pengaman, satu perintah tambah bisa membuat dua pengingat yang identik.
2. **Pesan keluar:** pemicu berjalan setiap jam ([ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md)), dan dua pemicu bisa saja berjalan bersamaan, sehingga satu jadwal kirim terkirim dua kali.

## Keputusan

**Pesan masuk:**

- ID pesan WhatsApp setiap pesan masuk dicatat di tabel tersendiri dengan ID sebagai kunci unik, **sebelum** perintahnya diproses.
- Jika ID sudah pernah tercatat, pesan diabaikan.

**Pesan keluar:**

- Gabungan pengingat, tanggal kemunculan, dan nilai H-x bersifat **unik** di tabel jadwal kirim, sehingga baris kembar ditolak oleh database.
- Sebelum mengirim, pemicu **mengklaim** jadwal dengan mengubah statusnya dari `pending` menjadi `sending` dalam satu operasi. Pemicu lain tidak bisa mengklaim jadwal yang sama.
- Status menjadi `sent` setelah WhatsApp Cloud API mengonfirmasi pengiriman.

## Pilihan antara Pesan Ganda dan Pesan Hilang

Jika server mati tepat setelah pesan terkirim tetapi sebelum status menjadi `sent`, sistem tidak bisa tahu apakah pesan sudah sampai. Jadwal yang tertahan di `sending` terlalu lama dikembalikan ke `pending` dan dicoba lagi.

Akibatnya, dalam kondisi yang sangat jarang, pemilik bisa menerima **pesan ganda**. Ini dipilih secara sadar: untuk aplikasi pengingat, menerima pesan dua kali jauh lebih baik daripada tidak menerimanya sama sekali.

## Konsekuensi

- Perlu tabel pencatat pesan masuk dan constraint unik di tabel jadwal kirim.
- Perlu batas waktu untuk status `sending` yang dianggap macet.
