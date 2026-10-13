# InGet

Bot WhatsApp pengingat pribadi untuk kegiatan sekali, harian, mingguan, bulanan, tahunan, hingga beberapa tahun sekali.

> **Status:** tahap desain. Kode belum ditulis. Semua keputusan desain dicatat di [`docs/adr/`](docs/adr/).

---

## Latar Belakang

Aplikasi kalender dan to-do list bekerja dengan pola **pull**: pengguna harus membuka aplikasinya untuk melihat apa yang perlu dilakukan. Bagi saya, pola ini gagal. Jadwal yang sudah tercatat di kalender tetap terlewat, karena saya jarang membuka aplikasinya dan notifikasinya mudah terabaikan.

Satu-satunya pengingat yang terbukti berhasil adalah **alarm**, karena alarm bekerja dengan pola **push**: ia datang sendiri tanpa perlu membuka aplikasi apa pun.

InGet menerapkan pola push melalui satu-satunya aplikasi yang notifikasinya selalu saya perhatikan: **WhatsApp**. Rekap kegiatan dan pengingat penting dikirim sebagai pesan chat, sehingga agenda bisa diketahui cukup dari notifikasi.

## Tujuan

- Mengirim pengingat ke WhatsApp tanpa pengguna perlu membuka aplikasi lain.
- Mendukung kegiatan berulang dengan berbagai frekuensi, termasuk kegiatan yang jarang terjadi (pajak kendaraan, perpanjangan SIM, ulang tahun).
- Memberi waktu persiapan sebelum hari H melalui pengingat H-x.
- Mengirim rekap kegiatan setiap hari, dan mengingatkan beberapa menit sebelum kegiatan berjam.
- Menyajikan agenda hari ini dan besok sesuai permintaan, sebagai alat untuk mengatur waktu.

## Bukan Tujuan (untuk saat ini)

- Alarm yang berbunyi seperti alarm HP ([ADR-0002](docs/adr/0002-alarm-tidak-masuk-mvp.md)).
- Input bahasa alami berbasis LLM ([ADR-0025](docs/adr/0025-balasan-bot-bahasa-indonesia-tanpa-nlp.md)).
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

### Pesan yang Dikirim Bot

| Kelompok | Jenis | Label | Kapan |
| --- | --- | --- | --- |
| Otomatis | Rekap harian | 📋 Rekap hari ini | 00.05 |
| | H-x | 🔔 N hari lagi | 00.05 |
| | Menjelang waktu | ⏰ N menit lagi | N menit sebelum jam kegiatan |
| | Pengingat terlewat | ⚠️ Terlewat | Saat server pulih dari gangguan |
| Balasan perintah | Agenda | 📅 Agenda | Saat mengetik `/agenda` |
| | Hitung mundur | ⏳ Hitung mundur | Di akhir balasan `/agenda` |

Setiap bubble diawali labelnya, sehingga pesan otomatis dapat dibedakan dari balasan perintah ([ADR-0027](docs/adr/0027-jenis-isi-dan-urutan-pesan.md)).

### Rekap Harian dan Mode

Setiap hari pukul **00.05**, bot mengirim **rekap harian** ([ADR-0024](docs/adr/0024-rekap-harian-otomatis-dan-default-h-x.md), [ADR-0027](docs/adr/0027-jenis-isi-dan-urutan-pesan.md)):

1. Bubble H-x (jika ada).
2. Satu bubble untuk setiap kegiatan hari itu: kegiatan tanpa jam lebih dulu dengan keterangan **"Sepanjang hari"**, lalu kegiatan berjam secara menaik.
3. Jika tidak ada kegiatan, satu bubble **"Tidak ada kegiatan hari ini"**. Jadi, kalau suatu pagi tidak ada pesan sama sekali, bot sedang bermasalah.

| Mode | Rekap harian di hari H | Pesan H-x sebelum hari H |
| --- | --- | --- |
| **Notifikasi** | Ya | Ya |
| **Rekap** | Ya | Tidak |

Mode Notifikasi untuk kegiatan yang butuh persiapan (tagihan, pajak, ulang tahun). Mode Rekap untuk kegiatan yang tidak butuh persiapan. Jika mode tidak dipilih, mode default mengikuti frekuensi: **Notifikasi** untuk sekali, bulanan, bulanan-hari, tahunan, dan setiap N tahun, serta **Rekap** untuk harian dan mingguan.

### Pengingat H-x

Pengingat bermode Notifikasi bisa memiliki hingga lima H-x, misalnya `7,2` untuk 7 dan 2 hari sebelum hari H.

- Nilai H-x antara 1 dan 365. Hari H sendiri sudah tercakup oleh rekap harian.
- **Default:** `7,2`.
- Bubble H-x memuat sisa hari, hari dan tanggal hari H, jam (jika ada), frekuensi, dan ID.
- H-x yang tanggalnya sudah lewat saat pengingat dibuat akan dilewati.

### Pengingat Menjelang Waktu

Atribut opsional untuk pengingat yang **punya jam**, apa pun mode dan frekuensinya: bot mengirim pesan **5, 10, 15, 30, atau 60 menit** sebelum jam kegiatan ([ADR-0026](docs/adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md)). Berguna untuk kegiatan yang baru dicatat di hari yang sama, misalnya "Ke dokter pukul 18.00" yang dicatat pukul 11.00.

### Pesan yang Gagal Terkirim

Status setiap pesan dicatat terpisah. Jika server bermasalah, pesan yang tertunda tetap dikirim selama hari H (atau jam kegiatan, untuk pengingat menjelang waktu) belum lewat. Selebihnya dikumpulkan dalam satu bubble **"Terlewat"**, bukan dibuang ([ADR-0026](docs/adr/0026-pengingat-menjelang-waktu-dan-pemicu-5-menit.md)).

### Agenda dan Hitung Mundur

`/agenda` menampilkan rencana hari ini, sebagai alat untuk mengatur waktu ([ADR-0015](docs/adr/0015-tampilan-bubble-dan-agenda.md), [ADR-0027](docs/adr/0027-jenis-isi-dan-urutan-pesan.md)). `/agenda tomorrow` menampilkan rencana besok.

1. Satu bubble untuk setiap kegiatan: "Sepanjang hari" lebih dulu, lalu kegiatan berjam secara menaik.
2. Satu bubble **hitung mundur** berisi daftar kegiatan dalam 7 hari ke depan, misalnya *"Pajak motor: 5 hari lagi, Minggu 25 Oktober"*. Berlaku untuk semua mode dan semua frekuensi kecuali harian dan mingguan ([ADR-0012](docs/adr/0012-hitung-mundur-di-agenda.md)).

Hitung mundur hanya muncul di `/agenda`, tidak pernah dikirim otomatis.

### Pengecualian dan Arsip

- **Abaikan** satu kemunculan pengingat berulang, atau **ubah** satu kemunculan saja tanpa memengaruhi yang lain ([ADR-0016](docs/adr/0016-abaikan-dan-ubah-satu-kemunculan.md)).
- Pengingat yang selesai masuk **arsip**, dapat dilihat lewat `/archive` dan diaktifkan kembali dengan mengubah tanggalnya. Arsip dihapus otomatis setelah 30 hari ([ADR-0017](docs/adr/0017-siklus-hidup-pengingat.md)).

### Zona Waktu

Default WIB, dapat diubah ke WITA atau WIT lewat `/set zone` ([ADR-0013](docs/adr/0013-waktu-lokal-dan-zona-waktu.md)).

---

## Cara Memakai Bot (draf)

> Nama perintah dan kata kunci memakai bahasa Inggris, balasan bot memakai bahasa Indonesia ([ADR-0023](docs/adr/0023-bahasa-perintah-dan-dokumentasi.md), [ADR-0025](docs/adr/0025-balasan-bot-bahasa-indonesia-tanpa-nlp.md)). Teks di bawah masih draf.

### Cara utama: percakapan terpandu

Ketik perintahnya saja, lalu jawab pertanyaan bot satu per satu. Pilihan tetap disajikan sebagai tombol atau daftar ([ADR-0028](docs/adr/0028-percakapan-terpandu-dan-perintah-satu-baris.md)).

> **Kamu:** /add
> **Bot:** Apa nama kegiatannya?
> **Kamu:** Bayar kartu kredit
> **Bot:** Tanggal berapa? (DD/MM/YYYY)
> **Kamu:** 25/10/2026
> **Bot:** Jam berapa? `[Tanpa jam]`
> **Kamu:** 08:00
> **Bot:** Seberapa sering? *(daftar: Sekali, Harian, Mingguan, Bulanan, ...)*
> **Bot:** Mode? `[Notifikasi]` `[Rekap]`
> **Bot:** Diingatkan berapa hari sebelumnya? `[Pakai default 7,2]`
> **Bot:** Sampai kapan? `[Tanpa tanggal akhir]`
> **Bot:** Ingatkan menjelang jamnya? *(daftar: Tidak perlu, 5, 10, 15, 30, 60 menit)*
> **Bot:** *(ringkasan)* `[Simpan]` `[Batal]`

Ketik `/cancel` kapan saja untuk membatalkan percakapan.

| Perintah | Fungsi |
| --- | --- |
| `/add` | Menambah pengingat |
| `/list` | Melihat semua pengingat aktif beserta ID-nya |
| `/agenda`, `/agenda tomorrow` | Melihat rencana hari ini atau besok, beserta hitung mundur |
| `/edit` | Mengubah pengingat (semua kemunculan, atau satu kemunculan saja) |
| `/skip` | Melewati satu kemunculan |
| `/delete` | Menghapus pengingat |
| `/archive` | Melihat pengingat yang selesai (30 hari terakhir) |
| `/set zone` | Mengubah zona waktu (WIB, WITA, WIT) |
| `/cancel` | Membatalkan percakapan yang sedang berjalan |
| `/help` | Menampilkan daftar perintah |

### Jalan pintas: perintah satu baris

Untuk yang sudah hafal, semua isian bisa ditulis dalam satu baris tanpa tanya jawab.

| Perintah | Contoh |
| --- | --- |
| `/add` | `/add 25/10/2026 08:00 \| Bayar kartu kredit \| monthly \| notify \| 7,2 \| until 25/09/2027 \| before 15` |
| `/edit` (semua kemunculan) | `/edit 12 time=09:00` |
| `/edit` (satu kemunculan) | `/edit 12 25/11/2026 date=26/11/2026` |
| `/skip` | `/skip 12 25/11/2026` |
| `/delete` | `/delete 12` |
| `/set zone` | `/set zone WITA` |

Urutan isian `/add`: `tanggal [jam] | deskripsi | frekuensi | [mode] | [H-x] | [until tanggal] | [before menit]`. Isian dalam kurung siku bersifat opsional.

| Kata kunci frekuensi | Arti |
| --- | --- |
| `once` | Sekali |
| `daily` | Harian |
| `weekly` | Mingguan |
| `monthly` | Bulanan |
| `monthly-weekday` | Bulanan berdasarkan urutan hari |
| `yearly` | Tahunan |
| `yearly:N` | Setiap N tahun, misalnya `yearly:5` |

Kata kunci mode: `notify` (Notifikasi) dan `recap` (Rekap). Pilihan `before`: `5`, `10`, `15`, `30`, `60`.

---

## Ruang Lingkup dan Tahapan

| Fase | Isi |
| --- | --- |
| **1. MVP** | Percakapan terpandu dan perintah satu baris, tujuh jenis frekuensi, dua mode, rekap harian, H-x, pengingat menjelang waktu, hitung mundur, arsip, pengecualian kemunculan, zona waktu |
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

**Sebelum menulis kode:** format tanggal final, teks setiap pertanyaan dan bubble, template WhatsApp, isi `/help`, pesan kesalahan, batas waktu percakapan terpandu, dan konfirmasi sebelum menghapus.

**Sebelum deploy:** hosting, mekanisme pemicu setiap 5 menit, peringatan saat bot gagal, backup data, penyimpanan token, dan setup akun Meta.

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
