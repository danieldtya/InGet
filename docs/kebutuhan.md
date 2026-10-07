# Dokumen Kebutuhan InGet

- **Status:** draf
- **Cakupan:** fase 1 (MVP)
- **Tanggal:** 2026-10-06 (diperbarui 2026-10-07)

Dokumen ini merangkum apa yang harus bisa dilakukan InGet di fase MVP. Setiap butir diberi nomor agar bisa dirujuk dari diagram, issue, dan kode, serta ditautkan ke ADR yang menjadi dasar keputusannya.

Dokumen ini menjawab **apa** yang dilakukan sistem, bukan **bagaimana** bentuk perintah atau isi pesannya. Sintaks perintah dan format pesan diputuskan terpisah (lihat [Pertanyaan yang Masih Terbuka](../README.md#pertanyaan-yang-masih-terbuka) di README).

---

## 1. Istilah

| Istilah | Arti |
| --- | --- |
| **Pemilik** | Satu-satunya pengguna bot, diidentifikasi dari nomor WhatsApp utamanya |
| **Pengingat** | Satu kegiatan yang dicatat, beserta tanggal, jam (opsional), deskripsi, frekuensi, mode, dan H-x |
| **Kemunculan** | Satu tanggal ketika sebuah pengingat jatuh. Pengingat berulang punya banyak kemunculan |
| **H-x** | Pesan yang dikirim x hari sebelum sebuah kemunculan. H-0 adalah hari kemunculan itu sendiri |
| **Mode Notifikasi** | Pengingat yang dikirim otomatis di setiap H-x |
| **Mode Rekap** | Pengingat yang tidak pernah dikirim otomatis, hanya terlihat lewat `/agenda` |
| **Hitung mundur** | Bagian di bawah hasil `/agenda` yang menampilkan kemunculan dalam 7 hari ke depan |
| **Pengecualian** | Satu kemunculan pengingat berulang yang diabaikan |
| **Arsip** | Tempat pengingat yang sudah selesai, disimpan 30 hari |

## 2. Aktor

| Aktor | Peran |
| --- | --- |
| **Pemilik** | Mengirim perintah lewat WhatsApp dan menerima pengingat |
| **Pemicu terjadwal** | Membangunkan sistem setiap jam untuk mengirim notifikasi dan menjalankan pekerjaan harian |
| **WhatsApp Cloud API** | Sistem eksternal yang meneruskan pesan masuk ke sistem dan mengirim pesan keluar ke pemilik |

---

## 3. Kebutuhan Fungsional

### 3.1 Mengelola pengingat

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-01 | Pemilik dapat menambah pengingat dengan tanggal, jam (opsional), deskripsi, frekuensi, mode (opsional), H-x (opsional), dan tanggal akhir (opsional, hanya untuk pengingat berulang) | [0010](adr/0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md), [0011](adr/0011-default-h-x-dan-jam-kirim.md), [0017](adr/0017-siklus-hidup-pengingat.md) |
| FR-02 | Setelah menambah atau mengubah pengingat, sistem membalas dengan bubble konfirmasi yang memuat tafsiran frekuensi dalam bahasa manusia, misalnya "Setiap bulan, Jumat pertama" | [0015](adr/0015-tampilan-bubble-dan-agenda.md), [0019](adr/0019-pengulangan-bulanan-berdasarkan-hari.md) |
| FR-03 | Pemilik dapat melihat daftar semua pengingat aktif beserta ID-nya | [0006](adr/0006-input-mvp-perintah-teks.md) |
| FR-04 | Pemilik dapat mengubah pengingat, dan perubahan berlaku untuk semua kemunculan berikutnya | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-05 | Pemilik dapat mengubah satu kemunculan saja dari pengingat berulang | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-06 | Pemilik dapat mengabaikan satu kemunculan dari pengingat berulang | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-07 | Pemilik dapat menghapus pengingat, termasuk semua jadwal kirimnya yang belum terkirim | [0006](adr/0006-input-mvp-perintah-teks.md) |

### 3.2 Melihat agenda

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-08 | Pemilik dapat melihat agenda hari ini. Setiap kegiatan ditampilkan dalam satu bubble, diurutkan menurut jam, dan kegiatan tanpa jam ditempatkan di atas | [0015](adr/0015-tampilan-bubble-dan-agenda.md) |
| FR-09 | Pemilik dapat melihat agenda besok dengan aturan tampilan yang sama | [0015](adr/0015-tampilan-bubble-dan-agenda.md) |
| FR-10 | Hasil agenda memuat bagian hitung mundur untuk kemunculan dalam 7 hari ke depan, beserta sisa harinya | [0012](adr/0012-hitung-mundur-di-agenda.md) |

### 3.3 Mengirim notifikasi

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-11 | Sistem mengirim pesan otomatis untuk setiap pengingat bermode Notifikasi pada setiap H-x-nya | [0010](adr/0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md), [0011](adr/0011-default-h-x-dan-jam-kirim.md) |
| FR-12 | Jadwal kirim yang tertunda karena gangguan tetap dikirim jika hari kemunculannya belum lewat | [0014](adr/0014-pemicu-per-jam-dan-pengingat-terlewat.md) |
| FR-13 | Jadwal kirim yang hari kemunculannya sudah lewat dikumpulkan dan dikirim sebagai satu pesan "Pengingat yang terlewat" | [0014](adr/0014-pemicu-per-jam-dan-pengingat-terlewat.md) |

### 3.4 Arsip

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-14 | Pengingat yang selesai ditandai selesai dan tidak lagi muncul di daftar maupun agenda | [0017](adr/0017-siklus-hidup-pengingat.md) |
| FR-15 | Pemilik dapat melihat pengingat yang selesai dalam 30 hari terakhir | [0017](adr/0017-siklus-hidup-pengingat.md) |
| FR-16 | Pemilik dapat mengaktifkan kembali pengingat dari arsip dengan mengubah tanggalnya ke masa depan | [0017](adr/0017-siklus-hidup-pengingat.md) |
| FR-17 | Sistem menghapus otomatis pengingat yang selesai lebih dari 30 hari | [0017](adr/0017-siklus-hidup-pengingat.md) |

### 3.5 Pengaturan, bantuan, dan keamanan

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-18 | Pemilik dapat mengganti zona waktu ke WIB, WITA, atau WIT | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| FR-19 | Pemilik dapat melihat daftar perintah beserta contohnya | [0006](adr/0006-input-mvp-perintah-teks.md) |
| FR-20 | Sistem membalas dengan pesan kesalahan yang menjelaskan letak kesalahan jika format perintah tidak valid | [0006](adr/0006-input-mvp-perintah-teks.md) |
| FR-21 | Sistem mengabaikan pesan dari nomor selain nomor pemilik tanpa membalas apa pun | [0018](adr/0018-hanya-melayani-nomor-pemilik.md) |

---

## 4. Aturan Bisnis

Aturan yang menentukan perilaku kebutuhan fungsional di atas.

### 4.1 Frekuensi dan tanggal

| ID | Aturan | Sumber |
| --- | --- | --- |
| BR-01 | Frekuensi yang didukung: sekali, harian, mingguan, bulanan, bulanan-hari, tahunan, dan setiap N tahun | [0019](adr/0019-pengulangan-bulanan-berdasarkan-hari.md) |
| BR-02 | Tanggal yang tidak ada di suatu bulan (29, 30, 31) dijadwalkan ke hari terakhir bulan tersebut | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| BR-03 | Pengingat 29 Februari dijadwalkan ke 28 Februari pada tahun bukan kabisat | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| BR-04 | Untuk `bulanan-hari`, urutan hari disimpulkan dari tanggal kemunculan pertama: 1 sampai 7 pertama, 8 sampai 14 kedua, 15 sampai 21 ketiga, 22 sampai 28 keempat, 29 sampai 31 terakhir | [0019](adr/0019-pengulangan-bulanan-berdasarkan-hari.md) |
| BR-05 | Tanggal akhir hanya dapat berupa tanggal, bukan jumlah kemunculan | [0017](adr/0017-siklus-hidup-pengingat.md) |
| BR-06 | Jam kegiatan bersifat opsional dan hanya dipakai untuk tampilan serta urutan agenda | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |

### 4.2 Pengiriman notifikasi

| ID | Aturan | Sumber |
| --- | --- | --- |
| BR-07 | Default H-x adalah `7,2,0` untuk sekali, bulanan, bulanan-hari, tahunan, dan setiap N tahun, serta `0` untuk harian dan mingguan | [0011](adr/0011-default-h-x-dan-jam-kirim.md) |
| BR-08 | Semua pesan notifikasi, termasuk H-0, dikirim pukul 00.05 waktu lokal pemilik | [0011](adr/0011-default-h-x-dan-jam-kirim.md) |
| BR-09 | H-x yang tanggalnya sudah lewat saat pengingat dibuat atau diubah dilewati | [0011](adr/0011-default-h-x-dan-jam-kirim.md) |
| BR-10 | "Hari ini" selalu dihitung berdasarkan zona waktu pemilik, bukan jam server | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| BR-11 | Setelah zona waktu diganti, jam kegiatan tetap tertulis sama (jam dinding), dan notifikasi dikirim pukul 00.05 waktu setempat yang baru | [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |

### 4.3 Agenda dan hitung mundur

| ID | Aturan | Sumber |
| --- | --- | --- |
| BR-12 | Hitung mundur berlaku untuk semua mode, dan untuk semua frekuensi kecuali harian dan mingguan | [0012](adr/0012-hitung-mundur-di-agenda.md) |
| BR-13 | Jendela hitung mundur tetap 7 hari dan tidak dapat diubah lewat perintah | [0012](adr/0012-hitung-mundur-di-agenda.md) |
| BR-14 | Untuk pengingat berulang, tanggal yang ditampilkan adalah kemunculan berikutnya | [0015](adr/0015-tampilan-bubble-dan-agenda.md) |

### 4.4 Perubahan dan siklus hidup

| ID | Aturan | Sumber |
| --- | --- | --- |
| BR-15 | Mengubah satu kemunculan dilakukan dengan mengabaikan kemunculan asli, lalu membuat pengingat sekali yang baru dengan data yang diubah | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| BR-16 | Mengabaikan dan mengubah satu kemunculan hanya berlaku untuk kemunculan yang belum terjadi | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| BR-17 | Pengingat sekali selesai setelah hari kemunculannya lewat. Pengingat berulang selesai setelah melewati tanggal akhirnya | [0017](adr/0017-siklus-hidup-pengingat.md) |
| BR-18 | Setiap perubahan pada pengingat menghapus jadwal kirim yang belum terkirim, lalu membuatnya ulang berdasarkan data terbaru | [0020](adr/0020-jadwal-kirim-dibuat-ulang.md) |

---

## 5. Kebutuhan Non-fungsional

| ID | Kategori | Kebutuhan | Sumber |
| --- | --- | --- | --- |
| NFR-01 | Keandalan | Tidak ada pengingat bermode Notifikasi yang hilang tanpa pemberitahuan. Jika satu pemicu gagal, pemicu berikutnya mencoba lagi | [0014](adr/0014-pemicu-per-jam-dan-pengingat-terlewat.md) |
| NFR-02 | Keandalan | Pemrosesan bersifat idempoten: pemicu yang berjalan dua kali dan webhook yang dikirim ulang oleh Meta tidak menghasilkan pesan atau pengingat ganda | [0014](adr/0014-pemicu-per-jam-dan-pengingat-terlewat.md), [0021](adr/0021-idempotensi-pesan-masuk-dan-keluar.md) |
| NFR-03 | Biaya | Sistem dapat berjalan tanpa biaya server. Satu-satunya biaya rutin adalah pesan template WhatsApp | [0009](adr/0009-rencana-stack-teknologi.md) |
| NFR-04 | Kepatuhan | Hanya memakai WhatsApp Cloud API resmi. Pesan di luar jendela layanan 24 jam memakai template yang disetujui Meta | [0001](adr/0001-whatsapp-cloud-api-resmi.md) |
| NFR-05 | Keamanan | Token WhatsApp, kredensial database, dan nomor pemilik tidak pernah disimpan di repositori | [0009](adr/0009-rencana-stack-teknologi.md) |
| NFR-06 | Privasi | Data pengingat hanya dapat diakses oleh pemilik | [0018](adr/0018-hanya-melayani-nomor-pemilik.md) |
| NFR-07 | Kemudahan pengembangan | Logika inti terpisah dari saluran, sehingga saluran baru dapat ditambahkan tanpa mengubah logika inti | [0008](adr/0008-arsitektur-tidak-terikat-saluran.md) |
| NFR-08 | Dapat diuji | Parser perintah dan logika inti dapat diuji tanpa WhatsApp. Kasus tepi tanggal (BR-02, BR-03, BR-04) wajib memiliki unit test | [0006](adr/0006-input-mvp-perintah-teks.md), [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| NFR-09 | Dokumentasi | Setiap keputusan desain penting dicatat sebagai ADR | [Indeks ADR](adr/README.md) |
| NFR-10 | Keamanan | Webhook yang tanda tangannya tidak valid ditolak sebelum diproses, sehingga pesan palsu yang mengaku dari nomor pemilik tidak dapat masuk | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) (diusulkan) |
| NFR-11 | Keamanan | Semua akses database memakai query berparameter | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) (diusulkan) |
| NFR-12 | Keandalan | Input dibatasi: deskripsi maksimal 100 karakter, maksimal 100 pengingat aktif, maksimal 5 nilai H-x, dan H-x terbesar 365 | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) (diusulkan) |
| NFR-13 | Bahasa | Nama perintah dan kata kunci memakai bahasa Inggris. Dokumentasi memakai bahasa Indonesia | [0023](adr/0023-bahasa-perintah-dan-dokumentasi.md) |

---

## 6. Di Luar Lingkup MVP

- Alarm yang berbunyi seperti alarm HP ([0002](adr/0002-alarm-tidak-masuk-mvp.md)).
- Input bahasa alami berbasis LLM ([0006](adr/0006-input-mvp-perintah-teks.md)).
- Tombol konfirmasi "Sudah" dan pengiriman ulang.
- Formulir WhatsApp Flows (fase 2).
- Dashboard web desktop (fase 3).
- Mingguan di beberapa hari sekaligus, `/agenda minggu`, dan membalas bubble untuk mengubah atau menghapus (fase 4).
- Dukungan banyak pengguna.

## 7. Hal yang Perlu Dikonfirmasi

| No | Hal | Status |
| --- | --- | --- |
| A-01 | Mode yang dipakai saat pemilik tidak menyebutkan mode di perintah tambah | Belum diputuskan |
| A-02 | Bahasa isi balasan bot (bubble, notifikasi, pesan kesalahan) | Belum diputuskan. Bahasa perintah sudah diputuskan di NFR-13 |
| A-03 | Angka batas input di NFR-12 | Menunggu konfirmasi |

## 8. Riwayat Dokumen

| Tanggal | Perubahan |
| --- | --- |
| 2026-10-06 | Draf pertama, disusun dari ADR 0001 sampai 0019 |
| 2026-10-07 | Sumber BR-18 dan NFR-02 dilengkapi (ADR 0020, 0021). Ditambah NFR-10 sampai NFR-13. Bagian 7 diperbarui |
