# ADR-0010: Mode Notifikasi Terkirim Otomatis, Mode Rekap Hanya Sesuai Permintaan

- **Status:** Diterima
- **Tanggal:** 2026-09-28
- **Menggantikan:** [ADR-0003](0003-dua-mode-notifikasi.md)

## Konteks

ADR-0003 mengasumsikan ada **rekap harian yang terkirim otomatis** setiap pagi. Asumsi itu keliru: rekap yang dimaksud sejak awal adalah daftar yang dilihat dengan cara **membuka chat bot**, bukan pesan yang dikirim bot dengan sendirinya.

## Keputusan

| Mode | Terkirim otomatis? | Muncul di mana |
| --- | --- | --- |
| **Notifikasi** | Ya, di setiap H-x yang dipilih | Pesan tersendiri, dan juga di `/agenda` |
| **Rekap** | Tidak pernah | Hanya saat pengguna meminta `/agenda` |

Satu-satunya hal yang membuat HP berbunyi adalah pengingat bermode Notifikasi. Tidak ada rekap harian otomatis.

## Alasan

Pengingat bermode Rekap umumnya **penting tetapi tidak mendesak**. Jika terlewat karena pengguna tidak membuka `/agenda`, kegiatannya masih bisa dijadwalkan ulang.

## Konsekuensi

- Pengaturan "jam rekap harian" dan perintah `/rekap` dihapus dari rancangan.
- Pengingat bermode Rekap bisa terlewat jika `/agenda` tidak dibuka. Risiko ini diterima secara sadar.
- Hitung mundur di `/agenda` ([ADR-0012](0012-hitung-mundur-di-agenda.md)) menjadi satu-satunya "peringatan dini" untuk pengingat bermode Rekap.
