# ADR-0015: Satu Bubble per Kegiatan dan Rentang `/agenda`

- **Status:** Diterima
- **Tanggal:** 2026-09-28

## Konteks

Pengguna perlu melihat kegiatan dalam rentang waktu tertentu lewat chat. Menampilkan satu bulan penuh berisiko menghasilkan terlalu banyak informasi sekaligus dan membingungkan.

## Keputusan

- Setiap kegiatan ditampilkan dalam **satu bubble chat** berisi judul, tanggal, jam (jika ada), frekuensi, mode beserta H-x, dan ID.
- Frekuensi ditulis dalam **bahasa manusia**, misalnya "Setiap bulan, tanggal 25" atau "Setiap bulan, Jumat pertama", bukan nama kategorinya.
- Untuk pengingat berulang, tanggal yang ditampilkan adalah **kemunculan berikutnya**.
- Rentang `/agenda` di MVP: **hari ini** (`/agenda`) dan **besok** (`/agenda besok`).
- Isi `/agenda` diurutkan menurut jam. Kegiatan tanpa jam ditempatkan di atas.

## Alternatif yang Dipertimbangkan

- **`/agenda bulan`:** ditolak karena berpotensi menghasilkan terlalu banyak bubble dan sulit dibaca.
- **`/agenda minggu`:** ditunda sampai tampilan bubble teruji dalam pemakaian nyata.

## Konsekuensi

- Satu bubble per kegiatan memungkinkan fitur "balas bubble untuk mengubah atau menghapus" di masa depan, karena setiap bubble mewakili tepat satu pengingat.
