# InGet

Bot WhatsApp pengingat pribadi untuk kegiatan sekali, harian, mingguan, bulanan, tahunan, hingga beberapa tahun sekali.

> **Status:** tahap desain. Kode belum ditulis. Semua keputusan desain dicatat di [`docs/adr/`](docs/adr/).

---

## Latar Belakang

Aplikasi kalender dan to-do list bekerja dengan pola **pull**: pengguna harus membuka aplikasinya untuk melihat apa yang perlu dilakukan. Bagi saya, pola ini gagal. Jadwal yang sudah tercatat di kalender tetap terlewat, karena saya jarang membuka aplikasinya dan notifikasinya mudah terabaikan.

Satu-satunya pengingat yang terbukti berhasil adalah **alarm**, karena alarm bekerja dengan pola **push**: ia datang sendiri tanpa perlu membuka aplikasi apa pun.

InGet menerapkan pola push melalui satu-satunya aplikasi yang notifikasinya selalu saya perhatikan: **WhatsApp**. Pengingat penting dikirim sebagai pesan chat, dan agenda lengkap bisa dilihat cukup dengan membuka satu chat.

## Tujuan

- Mengirim pengingat ke WhatsApp tanpa pengguna perlu membuka aplikasi lain.
- Mendukung kegiatan berulang dengan berbagai frekuensi, termasuk kegiatan yang jarang terjadi (pajak kendaraan, perpanjangan SIM, ulang tahun).
- Memberi waktu persiapan sebelum hari H melalui pengingat H-x.
- Menyajikan agenda hari ini dan besok dalam format yang mudah dibaca di chat.

## Bukan Tujuan (untuk saat ini)

- Alarm yang berbunyi seperti alarm HP ([ADR-0002](docs/adr/0002-alarm-tidak-masuk-mvp.md)).
- Input bahasa alami berbasis LLM ([ADR-0006](docs/adr/0006-input-mvp-perintah-teks.md)).
- Tombol konfirmasi "Sudah" dan pengiriman ulang.
- Dashboard web desktop (direncanakan di fase berikutnya).
- Dukungan banyak pengguna. Bot hanya melayani satu nomor pemilik ([ADR-0018](docs/adr/0018-hanya-melayani-nomor-pemilik.md)).

---

## Konsep Inti

### Frekuensi

> **Catatan:** contoh di bawah hanyalah ilustrasi sederhana. Setiap pengingat dapat memiliki jam yang ditetapkan pengguna (opsional). Dengan begitu, agenda satu hari tersusun menurut jam dan dapat dipakai untuk mengatur waktu, termasuk pengingat dari frekuensi lain yang kebetulan jatuh di hari yang sama.

| Frekuensi | Contoh |
| --- | --- |
| Sekali | Janji dokter |
| Harian | Minum vitamin jam 07.00, belajar jam 19.00 |
| Mingguan | Olahraga setiap Selasa |
| Bulanan | Bayar tagihan kartu kredit setiap tanggal 25 |
| Bulanan-hari | Rapat komunitas setiap Jumat pertama ([ADR-0019](docs/adr/0019-pengulangan-bulanan-berdasarkan-hari.md)) |
| Tahunan | Ulang tahun, pajak kendaraan tahunan |
| Setiap N tahun | Perpanjangan SIM (5 tahun) |

Pengingat berulang boleh memiliki **tanggal akhir**, misalnya cicilan yang berakhir bulan tertentu ([ADR-0017](docs/adr/0017-siklus-hidup-pengingat.md)).

### Mode

| Mode | Terkirim otomatis? | Muncul di mana |
| --- | --- | --- |
| **Notifikasi** | Ya, di setiap H-x | Pesan tersendiri, dan juga di `/agenda` |
| **Rekap** | Tidak pernah | Hanya saat meminta `/agenda` |

Mode Rekap cocok untuk kegiatan yang penting tetapi tidak mendesak ([ADR-0010](docs/adr/0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md)).

### Pengingat H-x

Pengingat bermode Notifikasi bisa memiliki lebih dari satu H-x, misalnya `7,2,0` untuk H-7, H-2, dan hari H ([ADR-0011](docs/adr/0011-default-h-x-dan-jam-kirim.md)).

- **Semua pesan dikirim jam 00.05** waktu lokal, termasuk H-0.
- **Default:** `7,2,0` untuk sekali, bulanan, bulanan-hari, tahunan, dan setiap N tahun. `0` untuk harian dan mingguan.
- H-x yang tanggalnya sudah lewat saat pengingat dibuat akan dilewati.
- Pengingat yang gagal terkirim karena gangguan server tetap dikirim, atau dikumpulkan dalam satu pesan "Pengingat yang terlewat" jika hari H-nya sudah lewat ([ADR-0014](docs/adr/0014-pemicu-per-jam-dan-pengingat-terlewat.md)).

### Agenda dan Hitung Mundur

`/agenda` menampilkan kegiatan hari ini, **satu bubble per kegiatan**, diurutkan menurut jam ([ADR-0015](docs/adr/0015-tampilan-bubble-dan-agenda.md)). `/agenda besok` menampilkan kegiatan besok.

Di bawahnya terdapat bagian **hitung mundur**: kegiatan dalam 7 hari ke depan, misalnya *"Pajak motor: 5 hari lagi (25 Oktober)"*. Bagian ini berlaku untuk semua mode dan semua frekuensi kecuali harian dan mingguan, sehingga pengingat bermode Rekap tetap terlihat sebelum hari H ([ADR-0012](docs/adr/0012-hitung-mundur-di-agenda.md)).

### Pengecualian dan Arsip

- **Abaikan** satu kemunculan pengingat berulang, atau **ubah** satu kemunculan saja tanpa memengaruhi yang lain ([ADR-0016](docs/adr/0016-abaikan-dan-ubah-satu-kemunculan.md)).
- Pengingat yang selesai masuk **arsip**, dapat dilihat lewat `/arsip` dan diaktifkan kembali dengan mengubah tanggalnya. Arsip dihapus otomatis setelah 30 hari ([ADR-0017](docs/adr/0017-siklus-hidup-pengingat.md)).

### Zona Waktu

Default WIB, dapat diubah ke WITA atau WIT lewat `/atur zona` ([ADR-0013](docs/adr/0013-waktu-lokal-dan-zona-waktu.md)).

---

## Perintah Bot (draf)

> Sintaks di bawah masih draf dan akan difinalkan sebelum implementasi.

| Perintah | Fungsi | Contoh |
| --- | --- | --- |
| `/tambah` | Menambah pengingat | `/tambah 25/10/2026 08:00 \| Bayar kartu kredit \| bulanan \| notif \| 7,2,0 \| sampai 25/09/2027` |
| `/daftar` | Melihat semua pengingat aktif beserta ID-nya | `/daftar` |
| `/agenda` | Melihat kegiatan hari ini dan hitung mundur | `/agenda`, `/agenda besok` |
| `/ubah` | Mengubah pengingat (semua kemunculan) | `/ubah 12 jam=09:00` |
| `/ubah` + tanggal | Mengubah satu kemunculan saja | `/ubah 12 25/11/2026 tanggal=26/11/2026` |
| `/abaikan` | Melewati satu kemunculan | `/abaikan 12 25/11/2026` |
| `/hapus` | Menghapus pengingat | `/hapus 12` |
| `/arsip` | Melihat pengingat yang selesai (30 hari terakhir) | `/arsip` |
| `/atur` | Mengubah zona waktu | `/atur zona WITA` |
| `/bantuan` | Menampilkan daftar perintah | `/bantuan` |

Urutan parameter `/tambah`: `tanggal [jam] | deskripsi | frekuensi | [mode] | [H-x] | [sampai tanggal]`. Parameter dalam kurung siku bersifat opsional. Mode default: `rekap`.

---

## Ruang Lingkup dan Tahapan

| Fase | Isi |
| --- | --- |
| **1. MVP** | Semua perintah di atas, tujuh jenis frekuensi, dua mode, H-x, hitung mundur, arsip, pengecualian kemunculan, zona waktu |
| **2. Input formulir** | WhatsApp Flows untuk menambah dan mengubah pengingat |
| **3. Dashboard** | Aplikasi web desktop untuk melihat dan mengelola pengingat |
| **4. Pengembangan lanjutan** | Mingguan di beberapa hari sekaligus, `/agenda minggu`, balas bubble untuk mengubah atau menghapus, input bahasa alami, eskalasi alarm |

## Rencana Teknologi

Masih berstatus usulan ([ADR-0009](docs/adr/0009-rencana-stack-teknologi.md)). Pengembangan dilakukan di laptop terlebih dahulu. Hosting produksi diputuskan setelah MVP berjalan.

| Komponen | Usulan |
| --- | --- |
| Saluran | WhatsApp Cloud API (resmi) |
| Backend dan webhook | Python, FastAPI |
| Database | PostgreSQL (Supabase) |
| Aturan pengulangan | Format RRULE (RFC 5545), library `python-dateutil` |
| Dashboard (fase 3) | React, Vite, TypeScript |

## Pertanyaan yang Masih Terbuka

**Sebelum menulis kode:** sintaks perintah final (termasuk format tanggal), isi bubble notifikasi dan template WhatsApp, bentuk `/agenda` saat kosong, pesan kesalahan dan isi `/bantuan`, konfirmasi sebelum menghapus, serta cara mengirim beberapa notifikasi sekaligus.

**Sebelum deploy:** hosting, mekanisme pemicu setiap jam, peringatan saat bot gagal, backup data, penyimpanan token, dan setup akun Meta.

**Manajemen proyek:** definisi "MVP selesai", kapasitas waktu per minggu, dan bentuk hasil akhir portofolio.

## Struktur Repositori

```text
InGet/
├── README.md
└── docs/
    ├── kebutuhan.md  # Dokumen kebutuhan (functional dan non-functional requirements)
    ├── adr/          # Catatan keputusan desain (Architecture Decision Record)
    └── diagrams/     # Use case, activity, class diagram, ERD (menyusul)
```

## Dokumentasi Keputusan

Setiap keputusan desain penting dicatat sebagai ADR di [`docs/adr/`](docs/adr/), lengkap dengan konteks, alasan, dan konsekuensinya. Mulailah dari [indeks ADR](docs/adr/README.md).

Ringkasan semua kebutuhan sistem untuk fase MVP ada di [dokumen kebutuhan](docs/kebutuhan.md).
