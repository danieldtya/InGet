# ADR-0024: Rekap Harian Otomatis, Mode Notifikasi sebagai Tambahan H-x, dan Default H-x `7,2`

- **Status:** Diterima
- **Tanggal:** 2026-10-08
- **Menggantikan:** [ADR-0010](0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md), [ADR-0011](0011-default-h-x-dan-jam-kirim.md)

## Konteks

ADR-0010 menetapkan bahwa rekap **tidak pernah** dikirim otomatis dan hanya muncul saat pemilik membuka `/agenda`. Dalam pemakaian yang dibayangkan, pemilik justru ingin setiap kegiatan hari itu datang sendiri ke WhatsApp tanpa perlu meminta, sesuai pola push yang menjadi alasan proyek ini dibuat ([README](../../README.md#latar-belakang)).

Begitu rekap harian dikirim otomatis, pesan H-0 dari ADR-0011 menjadi duplikat, karena kegiatan di hari H sudah tercantum di rekap hari itu.

## Keputusan

**Rekap harian:**

- Setiap hari pukul **00.05** waktu lokal pemilik, sistem mengirim **rekap harian**: **satu bubble untuk setiap kegiatan** yang jatuh pada hari itu, apa pun modenya.
- Urutan bubble mengikuti jam kegiatan. Kegiatan tanpa jam dikirim lebih dulu.

**Mode** (tetap dua, sebagai klasifikasi eksplisit):

| Mode | Rekap harian di hari H | Notifikasi H-x sebelum hari H |
| --- | --- | --- |
| **Notifikasi** | Ya | Ya, satu bubble di setiap H-x |
| **Rekap** | Ya | Tidak |

**H-x:**

- H-0 dihapus. Nilai H-x yang valid adalah **1 sampai 365**, paling banyak **5 nilai** per pengingat ([ADR-0022](0022-keamanan-webhook-dan-batas-input.md)).
- **Default H-x** untuk mode Notifikasi:

| Frekuensi | Default |
| --- | --- |
| Sekali, bulanan, bulanan-hari, tahunan, setiap N tahun | `7,2` |
| Harian, mingguan | Kosong |

- Pengingat bermode Rekap tidak boleh memiliki H-x. Perintah yang menyertakan H-x untuk mode Rekap ditolak dengan pesan kesalahan.
- H-x yang tanggalnya sudah lewat saat pengingat dibuat atau diubah **dilewati** (tetap dari ADR-0011).
- Semua pesan, baik rekap maupun H-x, dikirim pukul **00.05**.

## Alasan

- **Rekap otomatis:** pengguna tidak perlu mengingat untuk membuka bot. Ini inti masalah yang ingin diselesaikan.
- **Satu bubble per kegiatan:** konsisten dengan [ADR-0015](0015-tampilan-bubble-dan-agenda.md), dan menghindari kendala template WhatsApp yang tidak mengizinkan baris baru di dalam isian variabel. Setiap bubble cukup memakai satu template dengan isian satu baris.
- **Mode tetap dipertahankan:** klasifikasi eksplisit memudahkan pemilik memahami perlakuan setiap pengingat, walaupun secara teknis mode dapat disimpulkan dari ada atau tidaknya H-x.
- **00.05, bukan 00.00:** pemicu terjadwal tidak selalu tepat sampai hitungan detik. Jika berjalan di 23.59.59, sistem menganggap hari masih kemarin dan pesan terlambat satu hari. Pemilik baru membaca pesan setelah bangun, sehingga jam kirim yang tepat tidak penting.

## Konsekuensi

- **Biaya:** setiap bubble adalah satu pesan template. Dengan rata-rata 5 kegiatan per hari ditambah beberapa notifikasi H-x, perkiraannya sekitar 150 sampai 200 pesan per bulan, atau kurang dari US$1 dengan tarif utility Indonesia.
- **Notifikasi beruntun:** pada hari yang padat, beberapa notifikasi WhatsApp masuk hampir bersamaan pukul 00.05. Mode Jangan Ganggu di malam hari disarankan.
- Bubble rekap harian diperlakukan sebagai baris **jadwal kirim** dengan jenis "rekap", sehingga mekanisme pengiriman ulang ([ADR-0014](0014-pemicu-per-jam-dan-pengingat-terlewat.md)) dan idempotensi ([ADR-0021](0021-idempotensi-pesan-masuk-dan-keluar.md)) berlaku sama.
- Untuk frekuensi harian dan mingguan, mode Notifikasi tanpa H-x berperilaku sama dengan mode Rekap.
- Alasan di [ADR-0012](0012-hitung-mundur-di-agenda.md) yang menyebut mode Rekap "tidak pernah terkirim otomatis" tidak lagi berlaku. Keputusannya (hitung mundur di `/agenda`) tetap berlaku.
- `/agenda` tetap ada untuk melihat agenda kapan saja, termasuk agenda besok.

## Pertanyaan Terbuka

- Mode yang dipakai saat pemilik tidak menyebutkan mode di perintah tambah.
- Apakah bagian hitung mundur juga ikut dikirim dalam rekap harian.
- Apa yang dikirim pada hari tanpa kegiatan sama sekali.
