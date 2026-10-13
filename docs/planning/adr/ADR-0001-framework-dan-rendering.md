# ADR-0001: Framework dan strategi rendering

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-09, FR-01, FR-03.4, NFR-06, NFR-07.4 |

## Konteks

Situs tidak boleh memiliki backend, database, CMS, atau auth (K-09). Setiap rute harus dapat dibuka langsung, dimuat ulang, dan dibagikan (FR-01.2). Konten dan navigasi harus tetap ada saat JS bermasalah (NFR-06.1), dan elemen LCP tidak boleh bergantung pada JS (FR-03.4). Pemilik mengajukan Next.js, React, dan TypeScript sebagai kandidat (brief bagian 9).

## Keputusan (usulan)

1. Memakai **Next.js dengan App Router** dan **TypeScript**.
2. Memakai **static export** (`output: 'export'`). Setiap rute menjadi HTML statis dan seluruh situs disajikan oleh host file statis.
3. Konten statis dirender oleh Server Components saat build. Client Components dibatasi pada pulau interaksi (menu, motion, efek).
4. Rute case study dibuat dengan `generateStaticParams()`. Slug tak dikenal menghasilkan 404.
5. Versi Next.js, React, dan TypeScript dipin saat implementasi. Dokumentasi yang dibaca menampilkan Next.js 16.4.0 (EV-01).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| React + Vite sebagai SPA | Tanpa HTML per rute, konten bergantung JS. Bertentangan dengan EC-01 dan FR-03.4 |
| HTML, CSS, dan JS biasa tanpa framework | Kontrol paling besar dan dependency paling sedikit, tetapi perutean, validasi konten, dan templat case study harus dibuat sendiri. Memberatkan kapasitas K-06 |
| Next.js dengan server saat runtime | Tidak dibutuhkan. Menambah biaya dan permukaan serangan |
| Framework statis lain (mis. Astro) | Di luar kandidat pemilik dan **tidak dievaluasi** di sesi ini. Dapat ditinjau bila spike S-01 sampai S-04 gagal |

## Konsekuensi

**Positif**
- Satu HTML per rute dan Server Components dijalankan saat build (EV-03).
- Dapat di-host di host statis mana pun (EV-03). Tersedia template deploy ke GitHub Pages.
- Pilihan sesuai kandidat pemilik dan alat bawaan Next.js (EV-01).

**Negatif atau batasan**
- Tidak didukung pada static export: dynamic routes tanpa `generateStaticParams()`, redirects, rewrites, **headers**, ISR, Server Actions, dan **image optimization dengan loader bawaan** (EV-03). Dampaknya ditangani di ADR-0008 dan ADR-0009.
- App Router memakai rilis canary React bawaan (EV-01). Perilaku dapat berubah antarrilis (R-31).
- Node.js minimum 20.9 (EV-01). Versi Node di laptop belum diketahui (V-02). Dukungan Windows 10 telah berakhir (EV-08, R-25).
- Bundle framework menambah JS awal. Ukurannya diukur di spike S-01 dan masuk anggaran (ARCHITECTURE bagian 16).

## Syarat tinjau ulang

Spike S-01 sampai S-04 menunjukkan JS awal atau waktu build tidak dapat diterima di perangkat uji. Atau host yang dipilih membutuhkan fitur yang tidak didukung static export.

## Rujukan

EV-01, EV-02, EV-03, EV-07, EV-08. Risiko R-25, R-27, R-31.
