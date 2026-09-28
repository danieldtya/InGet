# ADR-0003: Dua Mode Notifikasi Menggantikan Tiga Kuadran Prioritas

- **Status:** Digantikan oleh [ADR-0010](0010-mode-notifikasi-dan-rekap-sesuai-permintaan.md)
- **Tanggal:** 2026-09-24

## Konteks

Rancangan awal memakai tiga kuadran prioritas:

1. Kuadran 1: diingatkan dengan alarm.
2. Kuadran 2: diingatkan beberapa hari sebelum tenggat.
3. Kuadran 3: cukup direkap harian.

Setelah alarm dihapus ([ADR-0002](0002-alarm-tidak-masuk-mvp.md)), satu-satunya perilaku khas Kuadran 1 yang tersisa adalah pesan tersendiri di waktu tertentu. Perilaku ini ternyata juga dibutuhkan Kuadran 2, dan sudah tercakup jika Kuadran 2 boleh memilih pengingat tepat di hari H (H-0).

## Keputusan

Memakai **dua mode notifikasi**:

| Mode | Perilaku |
| --- | --- |
| **Rekap** | Hanya muncul di rekap harian. Perilaku dasar semua pengingat. |
| **Notifikasi** | Muncul di rekap harian, ditambah pesan tersendiri di setiap H-x yang dipilih. |

Istilah "kuadran" tidak dipakai karena berasal dari matriks 2x2 (seperti Eisenhower Matrix) dan menyesatkan untuk dua kategori.

## Alternatif yang Dipertimbangkan

- **Tetap tiga kuadran:** tidak ada perilaku yang membedakan Kuadran 1 dari Kuadran 2 setelah alarm dihapus.

## Konsekuensi

- Konsep aplikasi lebih mudah dijelaskan: Rekap adalah dasar, Notifikasi adalah tambahan.
- Mode disimpan sebagai kolom bertipe enum, sehingga tingkat baru bisa ditambahkan nanti tanpa migrasi besar. Kebutuhan yang mungkin memunculkan tingkat baru adalah **jam tenang**: pengingat tertentu mungkin perlu tetap dikirim meski berada di dalam jam tenang.
