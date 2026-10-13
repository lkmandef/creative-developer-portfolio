# ADR-0003: Styling dan token desain

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-04, K-10, T-04, FR-10.3, FR-10.4, NFR-03, NFR-04.2, AC-56 |

## Konteks

Pemilik mengajukan Tailwind CSS sebagai kandidat (brief bagian 9). Tailwind juga termasuk pengaturan bawaan `create-next-app` (EV-01). Situs harus tetap berfungsi di Safari dan Firefox sejauh didukung implementasi (K-10). Durasi, easing, dan nilai desain harus berasal dari satu sumber (FR-10.4).

## Keputusan (usulan)

1. Memakai **Tailwind CSS v4** untuk utilitas dan tata letak responsif.
2. Nilai desain (warna, skala tipografi, spasi, durasi, easing, stagger) didefinisikan sekali sebagai **CSS custom properties** dan menjadi sumber token. Komponen merujuk token, bukan nilai literal (FR-10.4, AC-56). Nilai token ditetapkan di Tahap 4.
3. Varian reduced motion memakai media query `prefers-reduced-motion`.
4. Utilitas dengan dukungan browser terbatas tidak dipakai tanpa fallback. Tailwind menyatakan utilitas seperti itu bersifat opsional (EV-05).
5. Properti yang memicu layout tidak dianimasikan tanpa alasan tercatat (FR-10.3).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| CSS biasa atau CSS Modules dengan custom properties | Dependency paling sedikit dan tidak ada batas versi browser dari Tailwind. Lebih banyak kode gaya yang ditulis sendiri |
| Tailwind versi sebelumnya | Tidak dievaluasi di sesi ini. Tidak ada alasan bukti untuk memilihnya |

## Konsekuensi

**Positif**
- Sesuai kandidat pemilik dan alat bawaan.
- Token pusat mempermudah tiga tingkat motion dan pengujian.

**Negatif atau batasan**
- Tailwind v4 dirancang dan diuji untuk Chrome 111, Safari 16.4, dan Firefox 128 (EV-05). Batas Firefox lebih tinggi daripada batas Next.js, yaitu Firefox 111 (EV-02). Safari di bawah 16.4 dan Firefox di bawah 128 bisa kehilangan gaya (R-28). Keterbatasan ini dicatat dan tidak disembunyikan.
- Chrome di POCO M4 Pro dan di laptop belum diketahui versinya (V-01, V-02). Keduanya harus memenuhi batas Chrome 111.

## Syarat tinjau ulang

Uji tambahan K-10 di Safari atau Firefox menunjukkan celah tampilan yang material. Atau pemilik memilih meminimalkan dependency (T-04).

## Rujukan

EV-01, EV-02, EV-05. Risiko R-28.
