# ADR-0006: Strategi transisi halaman

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-03, FR-01.3, FR-11, DP-01, R-09, V-06, AC-02, AC-26 |

## Konteks

Pemilik membayangkan page transition dan shared-element transition "jika memungkinkan". PRD menetapkan transisi dasar sebagai **Should** (FR-11.1 sampai FR-11.4) dan shared-element sebagai **Could** (FR-11.5). Risiko R-09 menyebut transisi pada App Router sulit dan rapuh. Seberapa penting transisi bagi pemilik belum dijawab (DP-01).

## Evidensi (EV-04, dokumentasi Next.js 16.4.0)

- Transisi tampilan berjalan di App Router tanpa konfigurasi melalui `<ViewTransition>` React. Navigasi rute adalah transition sehingga animasi aktif otomatis.
- Memakai fitur API yang tersedia di Chromium 125+ dan Safari serta Firefox terbaru. Perilaku bisa berbeda di Safari. Tanpa dukungan, aplikasi berjalan normal tanpa animasi.
- Morph terbentuk hanya bila halaman tujuan dirender pada commit yang sama dengan navigasi (halaman ter-prefetch). Bila tujuan masuk ke fallback lebih dulu, tidak ada pasangan dan konten memakai animasi masuk biasa.
- Overlay `::view-transition` menangkap event pointer, sehingga klik hilang kecuali `pointer-events: none` dipasang. Elemen bernama dilewati hit-testing selama transisi.
- Navigasi back/forward browser tidak membawa transition type.
- Panduan menyertakan contoh CSS untuk reduced motion.

## Keputusan (usulan)

Tiga tingkat, dipilih berdasarkan hasil spike:

| Tingkat | Perilaku |
|---|---|
| **T0** | Navigasi instan tanpa animasi. Selalu tersedia sebagai jalur aman |
| **T1** | Transisi keluar-masuk terkoordinasi dengan `<ViewTransition>` dan CSS |
| **T2** | Morph elemen bersama antara kartu project dan halaman detail |

1. Spike **S-02** dijalankan lebih dulu pada hasil static export di Chrome Android POCO M4 Pro. Bila lulus, T1 diimplementasikan. T2 dicoba hanya bila T1 stabil dan lolos uji performa.
2. Bila S-02 gagal, dipakai **T0**. Pemilik diberi tahu (PRD bagian FR-11).
3. `::view-transition` memakai `pointer-events: none` (FR-11.2, AC-26).
4. Fokus keyboard dan pengumuman perubahan halaman untuk pembaca layar ditangani aplikasi, bukan diharapkan dari transisi visual (FR-11.4).
5. Reduced motion: transisi diganti perubahan tenang tanpa gerak spasial (FR-11.3).
6. Fungsi navigasi tidak boleh bergantung pada transisi (FR-01.3, FR-02.6).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Transisi dikoordinasi GSAP dengan pencegatan perutean | Kontrol lebih besar, tetapi rapuh dan kelayakannya pada App Router belum diverifikasi (V-06, R-09). Hanya dipertimbangkan bila T1 tidak cukup |
| Tanpa transisi sama sekali | Itu T0. Jalur aman, tetapi mengurangi "perpindahan antarbagian yang menyatu" (P-01) |

## Konsekuensi

**Positif**
- Tanpa pustaka tambahan untuk T1 dan T2.
- Degradasi anggun: tanpa dukungan browser, navigasi tetap normal (EV-04).

**Negatif atau batasan**
- Chrome di POCO M4 Pro harus Chromium 125+ agar fitur penuh berjalan. Versinya belum diketahui (V-01, R-26).
- Perilaku bisa berbeda di Safari, yang tetap kategori C (K-05).
- Morph bergantung pada halaman ter-prefetch. Harus diuji pada hasil static export, bukan diasumsikan (S-02).

## Syarat tinjau ulang

S-02 gagal atau transisi menurunkan INP atau kelancaran scroll di perangkat uji. Atau pemilik menetapkan transisi bukan prioritas (DP-01), sehingga T0 cukup.

## Rujukan

EV-04. Risiko R-09, R-26. Spike S-02.
