# ADR-0026: Pengingat Menjelang Waktu, Pemicu Setiap 5 Menit, dan Pengingat Terlewat

- **Status:** Diterima
- **Tanggal:** 2026-10-13
- **Menggantikan:** [ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md)

## Konteks

Sejak [ADR-0024](0024-rekap-harian-otomatis-dan-default-h-x.md), semua pesan dikirim pukul 00.05. Akibatnya, pengingat yang dibuat untuk **hari yang sama** tidak menghasilkan satu pesan otomatis pun. Contoh: pukul 11.00 pemilik mencatat "Ke dokter, pukul 18.00". Rekap harian hari itu sudah terkirim, dan H-x `7,2` sudah lewat sehingga dilewati. Tidak ada yang mengingatkan menjelang pukul 18.00.

Jam kirim 00.05 dipilih karena pemilik membaca pesan setelah bangun tidur. Alasan itu berlaku untuk kegiatan yang diketahui sebelumnya, tetapi tidak untuk kegiatan siang atau sore saat pemilik sedang terjaga.

## Keputusan

**Pengingat menjelang waktu:**

- Atribut **opsional** yang dapat dipasang pada pengingat apa pun yang **memiliki jam**, apa pun mode dan frekuensinya.
- Pilihan: **5, 10, 15, 30, atau 60 menit** sebelum jam kegiatan.
- Dikirim sebagai bubble tersendiri dengan label "⏰ N menit lagi" ([ADR-0027](0027-jenis-isi-dan-urutan-pesan.md)).
- Atribut ini **bukan mode**. Mode menjawab "perlu persiapan atau tidak", sedangkan atribut ini menjawab "perlu diingatkan tepat sebelum jamnya atau tidak". Keduanya dapat dipakai bersamaan.

**Pemicu:**

- Pemicu terjadwal membangunkan bot **setiap 5 menit** (sebelumnya setiap jam). Pesan menjelang waktu tiba dalam rentang 0 sampai 5 menit dari targetnya.
- Rekap harian dan pesan H-x tetap diproses pada pemicu pertama setelah 00.05 waktu lokal pemilik, dengan penanda "hari terakhir yang sudah diproses".
- Pemrosesan bersifat idempoten ([ADR-0021](0021-idempotensi-pesan-masuk-dan-keluar.md)).

**Pengingat terlewat** (tetap dari ADR-0014, ditambah aturan untuk pesan menjelang waktu):

| Jenis pesan | Masih dikirim jika | Jika tidak, masuk ke |
| --- | --- | --- |
| Rekap harian dan H-x | Hari kemunculannya belum lewat | Pesan "Pengingat yang terlewat" |
| Menjelang waktu | Jam kegiatannya belum lewat | Pesan "Pengingat yang terlewat" |

- Pesan "Pengingat yang terlewat" mengumpulkan semua pesan yang terlewat dalam **satu** bubble, bukan dibuang.

## Alternatif yang Dipertimbangkan

- **Mode ketiga khusus kegiatan sekali di hari yang sama:** ditolak. Kebutuhan yang sama muncul pada kegiatan berjam di hari lain, dan mencampur dua pertanyaan dalam satu klasifikasi mengulang masalah sistem kuadran ([ADR-0003](0003-dua-mode-notifikasi.md)).
- **Status 0 dan 1 di tabel pengingat:** ditolak. Satu pengingat menghasilkan banyak pesan dan banyak kemunculan, sehingga status harus dicatat per pesan di tabel jadwal kirim ([ADR-0020](0020-jadwal-kirim-dibuat-ulang.md), [ADR-0021](0021-idempotensi-pesan-masuk-dan-keluar.md)).

## Konsekuensi

- Pesan menjelang waktu tetap notifikasi WhatsApp biasa yang tunduk pada mode senyap, sehingga tidak bertentangan dengan [ADR-0002](0002-alarm-tidak-masuk-mvp.md).
- Pemicu setiap 5 menit membuat layanan gratis yang bisa tidur (misalnya Render) praktis selalu menyala. Pemakaian satu layanan sepanjang bulan (sekitar 744 jam) masih di bawah kuota gratis Render sebesar 750 jam per bulan.
- Jadwal kirim untuk pesan menjelang waktu menyimpan **tanggal dan jam** kirim, sedangkan rekap dan H-x cukup tanggal.
