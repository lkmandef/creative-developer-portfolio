# ADR-0002: Model konten dan validasi

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-04, K-09, FR-04.1, FR-05.5, FR-09.6, FR-12, NFR-09, AC-07, AC-16, AC-57, AC-61 |

## Konteks

Seluruh konten (project, expertise, profil, kontak, CV) harus berupa data terstruktur lokal yang terpisah dari komponen (FR-12.1). Pemilik harus dapat menambah project tanpa mengubah komponen (FR-12.7). Struktur tidak boleh mengunci satu bahasa (FR-12.2). Klaim yang tidak terkonfirmasi tidak boleh dirender (FR-12.4). Build rilis harus gagal bila ada `[PLACEHOLDER:` atau data tidak valid (FR-12.5, FR-12.6). Case study RSUD memiliki dua varian sesuai status izin (FR-05.5). Tautan CV hanya tampil bila CV siap dan sudah diperiksa (FR-09.6).

## Keputusan (usulan)

1. Konten disimpan sebagai **berkas data lokal bertipe** (TypeScript atau JSON dengan tipe). Tidak ada CMS dan tidak ada MDX.
2. Setiap teks yang dapat diterjemahkan disimpan sebagai **peta bahasa** dengan `en` sebagai satu-satunya bahasa wajib sekarang. Menambah bahasa kemudian berarti menambah kunci, bukan mengubah skema (FR-12.2, AC-57).
3. Data **divalidasi saat build** terhadap skema. Bidang wajib hilang atau tautan tanpa tujuan menggagalkan build (FR-12.6).
4. Setiap klaim hasil atau dampak memiliki penanda `terkonfirmasi` beserta tanggal konfirmasi. Renderer menolak klaim tak terkonfirmasi (FR-12.4, AC-61).
5. Project RSUD memiliki **varian visibilitas** `anonim` atau `bernama`. Varian `bernama` hanya valid bila data memuat rujukan catatan izin. Tanpa itu build gagal (FR-05.5, AC-11).
6. Data CV memiliki penanda `siap` dan `diperiksaPemilik`. Tautan dan berkas CV disertakan di build hanya bila keduanya benar (FR-09.6, AC-63).
7. Komponen mengakses konten **hanya lewat fungsi akses bertipe**.
8. Bentuk skema rinci, nama bidang, dan pustaka validasi (atau skrip validasi sendiri) diputuskan di Tahap 4, dengan alasan dependency tercatat (NFR-07.1).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| MDX untuk narasi case study | Nyaman untuk teks panjang, tetapi menambah dependency dan tooling. Bisa ditinjau bila narasi terlalu sulit diurus sebagai data (AR-03) |
| Headless CMS | Dilarang di MVP (K-09) |
| Teks tertulis langsung di komponen | Melanggar FR-04.1 dan FR-12.1, menyulitkan penambahan bahasa dan pemeriksaan placeholder |

## Konsekuensi

**Positif**
- Pemeriksaan placeholder, klaim, izin, dan CV dapat otomatis dan deterministik.
- Menambah project atau bahasa tidak menyentuh komponen.

**Negatif atau batasan**
- Narasi panjang sebagai data kurang nyaman daripada MDX.
- Skema harus dirawat bersama konten.

## Syarat tinjau ulang

Pemilik membutuhkan editor non-teknis (A-03), atau narasi case study terbukti terlalu sulit dikelola sebagai data.

## Rujukan

PRD bagian 6 (FR-12, FR-05.5, FR-09.6) dan bagian 11 (AC-07, AC-11, AC-16, AC-57, AC-61, AC-63). ARCHITECTURE bagian 7 sampai 9 dan 15.
