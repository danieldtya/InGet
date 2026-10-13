# ADR-0027: Jenis, Isi, dan Urutan Pesan

- **Status:** Diterima
- **Tanggal:** 2026-10-13
- **Melengkapi:** [ADR-0015](0015-tampilan-bubble-dan-agenda.md), [ADR-0024](0024-rekap-harian-otomatis-dan-default-h-x.md) (menjawab pertanyaan terbukanya)

## Konteks

Bot mengirim beberapa jenis pesan, sebagian otomatis dan sebagian sebagai balasan perintah. Tanpa pembeda yang jelas, riwayat chat sulit dibaca: pemilik tidak tahu mana pesan yang datang sendiri dan mana balasan atas permintaannya. ADR-0024 juga menyisakan tiga pertanyaan terbuka.

## Keputusan

### Jenis pesan dan labelnya

Setiap bubble diawali label tetap:

| Kelompok | Jenis | Label | Kapan |
| --- | --- | --- | --- |
| Otomatis | Rekap harian | 📋 Rekap hari ini | 00.05 |
| | H-x | 🔔 N hari lagi | 00.05 |
| | Menjelang waktu | ⏰ N menit lagi | N menit sebelum jam kegiatan |
| | Pengingat terlewat | ⚠️ Terlewat | Saat server pulih dari gangguan |
| Balasan perintah | Agenda | 📅 Agenda | Saat pemilik mengetik `/agenda` |
| | Hitung mundur | ⏳ Hitung mundur | Di akhir balasan `/agenda` |
| | Lainnya | Konfirmasi, daftar, arsip, kesalahan, bantuan | Saat pemilik mengetik perintah |

Isi bubble kegiatan di rekap harian dan di `/agenda` sama. Hanya labelnya yang berbeda.

### Isi bubble H-x

Label, judul, sisa hari, hari dan tanggal hari H, jam (jika ada), frekuensi dalam bahasa manusia, dan ID. Contoh:

```
🔔 7 hari lagi
Pajak motor
Hari H: Minggu, 25 Oktober 2026
Setiap tahun, 25 Oktober
ID 14
```

### Urutan rekap harian

1. Bubble **H-x** (jika ada).
2. Bubble **kegiatan hari ini**: kegiatan tanpa jam lebih dulu dengan keterangan **"Sepanjang hari"**, lalu kegiatan berjam secara menaik.
3. Jika tidak ada kegiatan hari ini, langkah 2 diganti satu bubble **"Tidak ada kegiatan hari ini"**. Bubble H-x tetap dikirim.

Rekap harian selalu menghasilkan minimal satu bubble kegiatan atau bubble "Tidak ada kegiatan hari ini", sehingga ketiadaan pesan sama sekali di pagi hari menjadi tanda bot bermasalah.

### Urutan balasan `/agenda`

1. Bubble **kegiatan hari itu**: tanpa jam lebih dulu ("Sepanjang hari"), lalu berjam secara menaik.
2. Satu bubble **hitung mundur** berisi **daftar** kegiatan dalam 7 hari ke depan ([ADR-0012](0012-hitung-mundur-di-agenda.md)).

Hitung mundur **hanya** ada di balasan `/agenda`, tidak di rekap harian. Fungsi peringatan sebelum hari H sudah dijalankan oleh bubble H-x.

### Mode default

Jika mode tidak disebutkan, mode ditentukan oleh frekuensi:

| Frekuensi | Mode default |
| --- | --- |
| Sekali, bulanan, bulanan-hari, tahunan, setiap N tahun | Notifikasi (H-x `7,2`) |
| Harian, mingguan | Rekap |

## Alasan

- **Label per jenis:** riwayat chat tetap terbaca beberapa hari kemudian, dan pesan otomatis dapat dibedakan dari balasan.
- **"Sepanjang hari" di atas:** kegiatan tanpa jam biasanya berlaku sepanjang hari (tagihan, ulang tahun) dan mudah terlewat jika ditaruh di bawah. Pola ini sama dengan acara "sepanjang hari" di aplikasi kalender.
- **Hitung mundur satu bubble:** balasan `/agenda` dikirim di dalam jendela layanan 24 jam, sehingga tidak terikat aturan template dan boleh berisi beberapa baris. Satu bubble per kegiatan akan membuat balasan terlalu panjang.
- **Mode default mengikuti frekuensi:** kesalahan paling berbahaya, yaitu lupa memberi mode Notifikasi pada tagihan, tidak mungkin terjadi.

## Konsekuensi

- Label menjadi bagian tetap dari setiap template WhatsApp.
- Teks label dan isi bubble lain bersifat draf sampai sintaks dan isi pesan difinalkan.
