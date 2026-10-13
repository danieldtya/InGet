# Dokumen Kebutuhan InGet

- **Status:** draf
- **Cakupan:** fase 1 (MVP)
- **Tanggal:** 2026-10-06 (diperbarui 2026-10-13)

Dokumen ini merangkum apa yang harus bisa dilakukan InGet di fase MVP. Setiap butir diberi nomor agar bisa dirujuk dari diagram, issue, dan kode, serta ditautkan ke ADR yang menjadi dasar keputusannya.

Dokumen ini menjawab **apa** yang dilakukan sistem. Teks pertanyaan, isi bubble, dan sintaks final diputuskan terpisah (lihat [Pertanyaan yang Masih Terbuka](../README.md#pertanyaan-yang-masih-terbuka) di README).

---

## 1. Istilah

| Istilah | Arti |
| --- | --- |
| **Pemilik** | Satu-satunya pengguna bot, diidentifikasi dari nomor WhatsApp utamanya |
| **Pengingat** | Satu kegiatan yang dicatat, beserta tanggal, jam (opsional), deskripsi, frekuensi, mode, H-x, tanggal akhir (opsional), dan pengingat menjelang waktu (opsional) |
| **Kemunculan** | Satu tanggal ketika sebuah pengingat jatuh. Pengingat berulang punya banyak kemunculan |
| **H-x** | Pesan yang dikirim x hari sebelum sebuah kemunculan, dengan x antara 1 dan 365 |
| **Rekap harian** | Pesan otomatis setiap hari pukul 00.05, berisi satu bubble untuk setiap kegiatan hari itu |
| **Mode Notifikasi** | Pengingat yang muncul di rekap harian **dan** mendapat pesan tambahan di setiap H-x |
| **Mode Rekap** | Pengingat yang hanya muncul di rekap harian, tanpa pesan H-x |
| **Hitung mundur** | Satu bubble berisi daftar kemunculan dalam 7 hari ke depan, hanya di akhir balasan `/agenda` |
| **Pengingat menjelang waktu** | Pesan yang dikirim 5, 10, 15, 30, atau 60 menit sebelum jam kegiatan |
| **Sepanjang hari** | Keterangan untuk kegiatan tanpa jam |
| **Percakapan terpandu** | Cara memasukkan data dengan menjawab pertanyaan bot satu per satu, dengan tombol dan daftar pilihan |
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
| FR-01 | Pemilik dapat menambah pengingat dengan tanggal, jam (opsional), deskripsi, frekuensi, mode (opsional), H-x (opsional), tanggal akhir (opsional, hanya untuk pengingat berulang), dan pengingat menjelang waktu (opsional, hanya jika ada jam) | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md), [0017](adr/0017-siklus-hidup-pengingat.md), [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| FR-01a | Pemilik dapat menambah dan mengubah pengingat lewat percakapan terpandu, maupun lewat perintah satu baris sebagai jalan pintas | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |
| FR-01b | Pemilik dapat membatalkan percakapan terpandu kapan saja, dan percakapan yang tidak dijawab dibatalkan otomatis setelah batas waktu tertentu | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |
| FR-02 | Setelah menambah atau mengubah pengingat, sistem membalas dengan bubble konfirmasi yang memuat tafsiran frekuensi dalam bahasa manusia, misalnya "Setiap bulan, Jumat pertama" | [0015](adr/0015-tampilan-bubble-dan-agenda.md), [0019](adr/0019-pengulangan-bulanan-berdasarkan-hari.md) |
| FR-03 | Pemilik dapat melihat daftar semua pengingat aktif beserta ID-nya | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |
| FR-04 | Pemilik dapat mengubah pengingat, dan perubahan berlaku untuk semua kemunculan berikutnya | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-05 | Pemilik dapat mengubah satu kemunculan saja dari pengingat berulang | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-06 | Pemilik dapat mengabaikan satu kemunculan dari pengingat berulang | [0016](adr/0016-abaikan-dan-ubah-satu-kemunculan.md) |
| FR-07 | Pemilik dapat menghapus pengingat, termasuk semua jadwal kirimnya yang belum terkirim | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |

### 3.2 Melihat agenda

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-08 | Pemilik dapat melihat agenda hari ini. Setiap kegiatan ditampilkan dalam satu bubble: kegiatan tanpa jam lebih dulu dengan keterangan "Sepanjang hari", lalu kegiatan berjam secara menaik | [0015](adr/0015-tampilan-bubble-dan-agenda.md), [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |
| FR-09 | Pemilik dapat melihat agenda besok dengan aturan tampilan yang sama | [0015](adr/0015-tampilan-bubble-dan-agenda.md) |
| FR-10 | Balasan agenda diakhiri satu bubble hitung mundur berisi daftar kemunculan dalam 7 hari ke depan, beserta sisa harinya | [0012](adr/0012-hitung-mundur-di-agenda.md), [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |

### 3.3 Mengirim notifikasi

| ID | Kebutuhan | Sumber |
| --- | --- | --- |
| FR-11 | Sistem mengirim rekap harian otomatis setiap hari: bubble H-x lebih dulu, lalu satu bubble untuk setiap kegiatan hari itu dengan urutan yang sama seperti FR-08. Jika tidak ada kegiatan, sistem mengirim "Tidak ada kegiatan hari ini" | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md), [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |
| FR-11a | Sistem mengirim bubble H-x untuk setiap pengingat bermode Notifikasi pada setiap H-x-nya, memuat sisa hari dan hari serta tanggal hari H | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md), [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |
| FR-11b | Sistem mengirim pengingat menjelang waktu sesuai jumlah menit yang dipilih | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| FR-11c | Setiap bubble diawali label sesuai jenisnya, sehingga pesan otomatis dapat dibedakan dari balasan perintah | [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |
| FR-12 | Pesan yang tertunda karena gangguan tetap dikirim jika hari kemunculannya belum lewat, atau untuk pengingat menjelang waktu, jika jam kegiatannya belum lewat | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| FR-13 | Pesan yang tidak memenuhi FR-12 dikumpulkan dan dikirim sebagai satu bubble "Terlewat" | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |

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
| FR-19 | Pemilik dapat melihat daftar perintah beserta contohnya, termasuk contoh perintah satu baris | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |
| FR-20 | Sistem membalas dengan pesan kesalahan yang menjelaskan letak kesalahan jika jawaban atau perintah tidak valid | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md) |
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
| BR-07 | Default H-x untuk mode Notifikasi adalah `7,2` untuk sekali, bulanan, bulanan-hari, tahunan, dan setiap N tahun, serta kosong untuk harian dan mingguan. Pengingat bermode Rekap tidak boleh memiliki H-x | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md) |
| BR-08 | Rekap harian dan semua pesan H-x dikirim pukul 00.05 waktu lokal pemilik. Pengingat menjelang waktu dikirim N menit sebelum jam kegiatan | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md), [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| BR-08a | Jika mode tidak dipilih, mode default adalah Notifikasi untuk sekali, bulanan, bulanan-hari, tahunan, dan setiap N tahun, serta Rekap untuk harian dan mingguan | [0027](adr/0027-jenis-isi-dan-urutan-pesan.md) |
| BR-08b | Pengingat menjelang waktu hanya dapat dipasang pada pengingat yang memiliki jam, dengan pilihan 5, 10, 15, 30, atau 60 menit | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| BR-09 | H-x yang tanggalnya sudah lewat saat pengingat dibuat atau diubah dilewati | [0024](adr/0024-rekap-harian-otomatis-dan-default-h-x.md) |
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
| NFR-01 | Keandalan | Tidak ada pesan otomatis yang hilang tanpa pemberitahuan. Pemicu berjalan setiap 5 menit, sehingga jika satu pemicu gagal, pemicu berikutnya mencoba lagi | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md) |
| NFR-02 | Keandalan | Pemrosesan bersifat idempoten: pemicu yang berjalan dua kali dan webhook yang dikirim ulang oleh Meta tidak menghasilkan pesan atau pengingat ganda | [0026](adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md), [0021](adr/0021-idempotensi-pesan-masuk-dan-keluar.md) |
| NFR-03 | Biaya | Sistem dapat berjalan tanpa biaya server. Satu-satunya biaya rutin adalah pesan template WhatsApp | [0009](adr/0009-rencana-stack-teknologi.md) |
| NFR-04 | Kepatuhan | Hanya memakai WhatsApp Cloud API resmi. Pesan di luar jendela layanan 24 jam memakai template yang disetujui Meta | [0001](adr/0001-whatsapp-cloud-api-resmi.md) |
| NFR-05 | Keamanan | Token WhatsApp, kredensial database, dan nomor pemilik tidak pernah disimpan di repositori | [0009](adr/0009-rencana-stack-teknologi.md) |
| NFR-06 | Privasi | Data pengingat hanya dapat diakses oleh pemilik | [0018](adr/0018-hanya-melayani-nomor-pemilik.md) |
| NFR-07 | Kemudahan pengembangan | Logika inti terpisah dari saluran, sehingga saluran baru dapat ditambahkan tanpa mengubah logika inti | [0008](adr/0008-arsitektur-tidak-terikat-saluran.md) |
| NFR-08 | Dapat diuji | Parser perintah, alur percakapan terpandu, dan logika inti dapat diuji tanpa WhatsApp. Kasus tepi tanggal (BR-02, BR-03, BR-04) wajib memiliki unit test | [0028](adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md), [0013](adr/0013-waktu-lokal-dan-zona-waktu.md) |
| NFR-09 | Dokumentasi | Setiap keputusan desain penting dicatat sebagai ADR | [Indeks ADR](adr/README.md) |
| NFR-10 | Keamanan | Webhook yang tanda tangannya tidak valid ditolak sebelum diproses, sehingga pesan palsu yang mengaku dari nomor pemilik tidak dapat masuk | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) |
| NFR-11 | Keamanan | Semua akses database memakai query berparameter | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) |
| NFR-12 | Keandalan | Input dibatasi: deskripsi maksimal 200 karakter, maksimal 200 pengingat aktif, dan maksimal 5 nilai H-x antara 1 dan 365 | [0022](adr/0022-keamanan-webhook-dan-batas-input.md) |
| NFR-13 | Bahasa | Nama perintah dan kata kunci memakai bahasa Inggris. Balasan bot dan dokumentasi memakai bahasa Indonesia. Balasan disusun dengan template teks, tanpa NLP | [0023](adr/0023-bahasa-perintah-dan-dokumentasi.md), [0025](adr/0025-balasan-bot-bahasa-indonesia-tanpa-nlp.md) |

---

## 6. Di Luar Lingkup MVP

- Alarm yang berbunyi seperti alarm HP ([0002](adr/0002-alarm-tidak-masuk-mvp.md)).
- Input bahasa alami berbasis LLM ([0025](adr/0025-balasan-bot-bahasa-indonesia-tanpa-nlp.md)).
- Tombol konfirmasi "Sudah" dan pengiriman ulang.
- Formulir WhatsApp Flows (fase 2).
- Dashboard web desktop (fase 3).
- Mingguan di beberapa hari sekaligus, `/agenda minggu`, dan membalas bubble untuk mengubah atau menghapus (fase 4).
- Dukungan banyak pengguna.

## 7. Hal yang Perlu Dikonfirmasi

Tidak ada untuk saat ini. Pertanyaan tentang teks pesan dan sintaks final tercatat di README.

## 8. Riwayat Dokumen

| Tanggal | Perubahan |
| --- | --- |
| 2026-10-06 | Draf pertama, disusun dari ADR 0001 sampai 0019 |
| 2026-10-07 | Sumber BR-18 dan NFR-02 dilengkapi (ADR 0020, 0021). Ditambah NFR-10 sampai NFR-13. Bagian 7 diperbarui |
| 2026-10-08 | Rekap harian otomatis dan H-0 dihapus (ADR 0024). FR-11 dipecah menjadi FR-11 dan FR-11a. Istilah, BR-07 sampai BR-09, NFR-10 sampai NFR-13, dan bagian 7 diperbarui |
| 2026-10-13 | Pengingat menjelang waktu dan pemicu 5 menit (ADR 0026), jenis dan urutan pesan serta mode default (ADR 0027), percakapan terpandu (ADR 0028). Ditambah FR-01a, FR-01b, FR-11b, FR-11c, BR-08a, BR-08b. Semua hal di bagian 7 terjawab |
