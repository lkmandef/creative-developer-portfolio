# ASSETS_AND_BUILD — Aset, pipeline build, static export, dan deployment

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/ASSETS_AND_BUILD.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 (Desain Teknis) |
| Dokumen induk | `docs/planning/TECHNICAL_DESIGN.md` |
| Rujukan | ADR-0001, ADR-0008, ADR-0009, FR-04.6, FR-09.6, FR-09.7, FR-12.5, FR-14, NFR-01.5, NFR-01.7, NFR-08, AC-15, AC-16, AC-31, AC-46, AC-47, AC-63, AC-64 |
| Status | **T4-USULAN.** Nama skrip dan perintah di dokumen ini adalah **nama logis**. Perintah aktual ditetapkan saat dibuat (Fase 1) dan diverifikasi sebelum Tahap 8 |

## 1. Prinsip

1. Hanya apa yang sengaja diizinkan yang masuk `out/`. `public/` tidak dipakai sebagai tempat menyimpan bahan mentah (D-08).
2. Pemeriksaan berjalan pada **sumber dan hasil build** (ARCHITECTURE bagian 15).
3. Tidak ada alat atau pustaka yang dianggap tersedia atau berlisensi aman sebelum diverifikasi (V-18, V-19, V-20, V-21).
4. Tidak ada klaim hasil di dokumen ini. Semua kriteria adalah **yang harus dibuktikan**.

## 2. Hasil verifikasi sumber resmi (V-12, V-13, `trailingSlash`)

| Pertanyaan | Yang terbukti dibaca | Yang belum terbukti | Konsekuensi desain |
|---|---|---|---|
| V-12 Gambar pada static export | Dokumentasi static export Next.js 16.4.0 hanya menyebut optimasi gambar lewat **loader kustom**, dan menyatakan loader bawaan **tidak didukung** (EV-03, EV-10). Opsi `images.unoptimized` tidak muncul di halaman itu | Perilaku `images.unoptimized` secara resmi. Sumber pihak ketiga menyebutnya (EV-12), tidak dipakai sebagai dasar | D-06: komponen `Img` sendiri, varian dibuat skrip, `next/image` dilarang (`CHK-ARCH-03`) |
| V-13 Font lokal | Dokumentasi `next/font` menyatakan font di-host sendiri, CSS dan berkas font diunduh saat build, `next/font/local` memiliki `adjustFontFallback` (EV-11) | Perilaku pada static export, ukuran berkas keluaran, perilaku `preload` | D-07 dengan S-05 sebagai pembuktian. Cadangan: `@font-face` manual |
| `trailingSlash` | Dokumentasi menyatakan `trailingSlash: true` menghasilkan `/rute/index.html` dan mengubah tautan menjadi `/rute/`. Tanpa itu, rute dinamis menjadi `/rute/slug.html`, dan contoh Nginx memerlukan aturan penulisan ulang (EV-09) | Perilaku host tertentu (belum dipilih) | D-04: `trailingSlash: true`. Dikonfirmasi saat host dipilih (ADR-0009) |
| `basePath` | Tidak dibahas di halaman yang dibaca | Seluruh perilakunya pada static export dan aset non-Next | Variabel `BASE_PATH`, helper `withBasePath()` untuk aset non-Next, diuji di S-05 (V-23) |

## 3. Aset gambar

### 3.1 Alur

```
assets-src/images/*.{png,jpg,...}   <- hanya yang AMAN dipublikasikan
        |  build-images (skrip)
        v
public/generated/img/<id>-<lebar>.<format>      (gitignored)
src/generated/image-manifest.json               (gitignored): { id, width, height, variants[], sha256 }
        |
        v
<Img media=...> -> <picture><source type="image/webp" srcset=...><img src=fallback width height ...></picture>
```

| Aspek | Spesifikasi |
|---|---|
| Sumber | `assets-src/images/`. **Bahan asli sensitif tidak pernah di sini**, melainkan di `assets-src/private/` (gitignored) |
| Variabel keluaran | Kumpulan lebar (mis. 480, 768, 1080, 1600) dan format WebP dengan cadangan format asal. Daftar final [SEMENTARA]. AVIF tidak dipakai sampai dukungannya pada baseline diverifikasi dari sumber resmi |
| Dimensi | `width` dan `height` ditulis dari manifest ke `<img>`. Ruang dicadangkan juga lewat `aspect-ratio` (FR-04.6, NFR-01.5) |
| Pemuatan | `loading="lazy"` dan `decoding="async"` kecuali elemen LCP. Hero tidak memakai gambar (UI_UX_SPEC bagian 6). Bila kelak ada, hanya satu `priority` |
| Latar sementara | Warna solid dari token. Tanpa *blur-up* |
| Gagal dimuat | Kotak tetap, `alt` terbaca, teks pendamping tidak tersembunyi (FR-13.2, EC-06) |
| Metadata | Hasil re-encode **diperiksa**, tidak diasumsikan bersih. Dugaan bahwa pemroses gambar menghapus EXIF **belum terbukti**. `CHK-AST-02` memeriksa keluaran (alat: V-18, V-20) (NFR-08.2, AC-46) |
| Nama berkas | Tidak memuat nama institusi, nama orang, atau istilah dari daftar lokal (`CHK-AST-05`) |
| `basePath` | URL aset dibangun oleh `withBasePath()`, bukan ditulis tangan |

### 3.2 Aturan konten visual (pencegahan, bukan deteksi otomatis)

Pemindaian otomatis **tidak dapat** memastikan sebuah tangkapan layar bebas data sensitif. Karena itu:

1. Gambar project RSUD hanya dari antarmuka **dibuat ulang dengan data dummy** sebelum izin terkonfirmasi (FR-05.5, AC-11).
2. Setiap gambar yang masuk `assets-src/images/` melewati **daftar tinjauan manual pemilik** (`SECURITY_PRIVACY_ERRORS.md` bagian 3.2) sebelum di-commit.
3. `Media.origin` mencatat asal gambar. `origin: 'real'` hanya valid bila izin mencakup `real-screenshots` (`CONTENT_MODEL.md` bagian 4.5).

## 4. Font

| Aspek | Spesifikasi |
|---|---|
| Mekanisme | `next/font/local` dengan berkas di `src/fonts/` (D-07). Tidak memakai `next/font/google` agar build tidak bergantung pada jaringan dan berkas serta lisensinya berada di repositori |
| Batas | Maksimal 2 keluarga, 3 berkas (font variabel dianjurkan) [SEMENTARA]. Dihitung ke anggaran (bagian 7) |
| Lisensi | Setiap berkas font memiliki berkas lisensi di `src/fonts/LICENSES/`. `CHK-AST-04` memeriksa kecocokan satu-satu. Lisensi dibaca dari berkas yang menyertai font (V-21) |
| `font-display` | `swap` dengan font cadangan berukuran disesuaikan (`adjustFontFallback` atau `size-adjust` manual). Tujuan: CLS minimal (NFR-01.5, EC-07) |
| Subset | Latin. Alat subset dipilih di Tahap 5 setelah dicek di Windows 10 (V-18) |
| Dampak pada hero | Pergeseran lebar kata saat font berganti dapat memengaruhi animasi kata dan CLS (R-34). Diukur di S-01 |
| Bukti runtime | `CHK-OUT-02` memastikan tidak ada permintaan ke host font eksternal. Inspeksi tab jaringan (AC-47) |

## 5. CV dan berkas yang diizinkan masuk build (D-08, AC-63)

```
assets-src/cv/cv-en.pdf      (bahan, tidak publik)
        |  sync-public (dijalankan sebelum build)
        |    1. hapus public/generated/ sepenuhnya
        |    2. baca content/cv.ts
        |    3. JIKA ready && ownerReviewed && reviewedOn && file ada && CHK-AST-03 lolos:
        |         salin ke public/generated/cv/<nama>.pdf
        |       SELAIN ITU: tidak menyalin apa pun
        v
getCv() -> null atau { href, label }  (komponen tidak menampilkan tautan bila null)
```

| Aturan | Spesifikasi |
|---|---|
| Gerbang | Kondisi di atas. Dua konfigurasi diuji (CV siap dan belum siap) (AC-63) |
| Metadata PDF | `CHK-AST-03`: bidang pengarang, pembuat, produsen, judul, dan kata kunci tidak memuat data yang tidak dikonfirmasi pemilik. Alat pemeriksa: V-18 |
| Isi PDF | Tinjauan manual pemilik: tanpa alamat rumah dan nomor telepon yang tidak dikonfirmasi (FR-09.7). Pemeriksaan pola teks pada PDF membantu tetapi tidak menggantikan tinjauan |
| `out/` | `CHK-OUT-09`: tidak ada `.pdf` di `out/` bila `getCv()` bernilai `null`, dan tidak ada tautan ke CV di HTML |

## 6. Static export dan konfigurasi

### 6.1 Konfigurasi `next.config.ts` (maksud)

| Opsi | Nilai | Alasan |
|---|---|---|
| `output` | `'export'` | ADR-0001 |
| `trailingSlash` | `true` | D-04, EV-09 |
| `basePath` | dari `BASE_PATH`, kosong bila tidak ada | Host bisa berupa subpath (V-23) |
| `images` | tidak dipakai | `next/image` dilarang (D-06) |
| `reactStrictMode` | `true` | Deteksi efek yang tidak dibersihkan di pengembangan |
| `headers`, `redirects`, `rewrites`, Server Actions, ISR | **tidak dipakai** | Tidak didukung pada static export (EV-03) |
| `productionBrowserSourceMaps` | tidak diaktifkan | Mencegah jalur lokal dan kode sumber bocor. `CHK-OUT-04` memeriksa `*.map` |

Perilaku opsi di atas pada versi yang dipin diverifikasi di S-05 sebelum Fase hero. Sintaks yang tepat dibaca dari dokumentasi versi yang dipin.

### 6.2 Rute

| Rute | Mekanisme | Keluaran |
|---|---|---|
| `/` | Statis | `out/index.html` |
| `/projects/<slug>/` | `generateStaticParams()` dari `listProjectSlugs()`, `dynamicParams = false` | `out/projects/<slug>/index.html` (dengan `trailingSlash: true`) |
| 404 | `app/not-found.tsx` | `out/404.html` (contoh di EV-03). Cara host menyajikannya diuji (bagian 9) |
| `robots.txt`, `sitemap.xml` | Route Handler `GET` dengan `dynamic = 'force-static'` (didukung, EV-03) | Berkas statis. Sitemap membutuhkan `SITE_URL` (wajib pada rilis) |
| Rute tak terduga | Tidak ada. Mis. tidak ada `/how-i-built-this/` (DP-12, AC-64) | `CHK-OUT-12` membandingkan himpunan rute |

### 6.3 Metadata

| Aspek | Keputusan |
|---|---|
| `metadataBase` | Dari `SITE_URL`. Pada rilis variabel wajib ada dan berformat `https://`. Domain belum diketahui (Q-19), sehingga pratinjau tautan (FR-14.2) baru dapat diuji penuh setelah domain ada |
| Judul dan deskripsi | Dari `seo.*` project dan `site.*`. Unik (`CHK-CNT-19`) |
| Gambar pratinjau | Satu gambar statis aman di `public/og/`. Tidak memuat data sensitif. Diperiksa metadata dan isi oleh pemilik |
| `lang` | `en` pada `<html>` (FR-01.5) |

## 7. Pipeline build dan rilis (P0 sampai P3)

Urutan tahapan dan penamaan **logis**. Setiap tahap dapat menggagalkan rilis. Semua deterministik dan tanpa jaringan.

| Tahap | Nama logis | Isi | Aturan | Rilis memperketat? |
|---|---|---|---|---|
| P0 | `check:types`, `check:lint` | Tipe (`strict`) dan lint, termasuk aturan batas | `CHK-ARCH-01` sampai `CHK-ARCH-07` | Tidak |
| P1a | `build:assets` | `build-images`, `sync-public` (bagian 3 dan 5) | `CHK-AST-01` sampai `CHK-AST-05` | Ya |
| P1b | `check:content` | Skema dan aturan konten | `CHK-CNT-01` sampai `CHK-CNT-21` | Ya (lihat `CONTENT_MODEL.md` bagian 6) |
| P1c | `check:repo` | Pemindaian berkas terlacak | `CHK-REPO-01` sampai `CHK-REPO-05` | Ya |
| P2 | `build` | `next build` (static export) | Build gagal bila galat tipe atau render | n/a |
| P3 | `check:out` | Pemindaian `out/` | `CHK-OUT-01` sampai `CHK-OUT-13` | Ya |

Urutan: P0 → P1a → P1b → P1c → P2 → P3. `release:verify` (nama logis) merangkai semuanya dengan `RELEASE=1`. Kontrak kode keluar dan keluaran: `TECHNICAL_DESIGN.md` bagian 6.3. Tidak ada tahap yang menulis "lulus" sebagai klaim kualitas. Bukti kelulusan fase dikumpulkan di Tahap 6.

### 7.1 Aturan pemindaian aset (`CHK-AST`)

| ID | Aturan | Rujukan |
|---|---|---|
| CHK-AST-01 | Setiap `Media.source` ada, berada di `assets-src/images/`, berukuran di bawah batas sumber [SEMENTARA], dan menghasilkan entri manifest | FR-04.6 |
| CHK-AST-02 | Berkas gambar keluaran tidak memuat metadata EXIF, XMP, atau profil yang bukan dibutuhkan (alat V-18) | NFR-08.2, AC-46 |
| CHK-AST-03 | PDF CV tidak memuat metadata yang tidak dikonfirmasi pemilik (alat V-18) | FR-09.7, AC-63 |
| CHK-AST-04 | Setiap berkas font memiliki berkas lisensi yang cocok | NFR-07.2 |
| CHK-AST-05 | Nama berkas aset tidak memuat istilah dari daftar lokal | NFR-08.2 |

### 7.2 Aturan pemindaian repositori (`CHK-REPO`)

| ID | Aturan | Rujukan |
|---|---|---|
| CHK-REPO-01 | Berkas terlacak Git tidak mencakup `assets-src/private/**`, `.env*`, `*.pem`, `*.key`, `out/`, `.next/`, `reports/`, `public/generated/**` | NFR-08.1 |
| CHK-REPO-02 | Pola rahasia umum, alamat email, nomor telepon, jalur lokal (`C:\Users`, `/home/`) tidak ditemukan di berkas terlacak selain yang diizinkan (kontak terkonfirmasi) | NFR-08.1, AC-45 |
| CHK-REPO-03 | Melaporkan nama dan email penulis commit di seluruh riwayat (laporan saja) | C-01, NFR-08.1 |
| CHK-REPO-04 | Istilah dari daftar lokal tidak ada di berkas terlacak **dan** riwayat commit. Berkas daftar wajib ada pada rilis (kode keluar 2 bila tidak) | NFR-09.4, NFR-08.5 |
| CHK-REPO-05 | Melaporkan berkas biner besar atau tipe tak dikenal | NFR-08.1 |

### 7.3 Aturan pemindaian `out/` (`CHK-OUT`)

| ID | Aturan | Rujukan |
|---|---|---|
| CHK-OUT-01 | Tidak ada `[PLACEHOLDER:` di berkas teks apa pun di `out/` (HTML, JS, JSON, TXT, XML, CSS) | FR-12.5, AC-16 |
| CHK-OUT-02 | Tidak ada referensi muat ke host eksternal: `<script src>`, `<link href>`, `<img src>`, `<source>`, `url()`, `@import`, `<iframe>`, `<form action>`. Tautan `<a href>` keluar hanya ke tujuan yang berasal dari `ContactLink` dan `ProjectLink` terkonfirmasi | NFR-08.3, AC-47 |
| CHK-OUT-03 | Istilah dari daftar lokal tidak ditemukan di seluruh teks `out/` (termasuk atribut `alt`) | NFR-09.4, EC-24 |
| CHK-OUT-04 | Tidak ada `*.map`, jalur lokal, pola email atau telepon selain kontak terkonfirmasi, dan tidak ada pola rahasia | NFR-08.1 |
| CHK-OUT-05 | Setiap `<img>` memiliki atribut `alt` (boleh kosong untuk dekoratif), `width`, dan `height` | FR-05.4, FR-04.6 |
| CHK-OUT-06 | Setiap `data-claim-id` di HTML cocok dengan klaim `confirmed` dan bertanggal. Tidak ada klaim tak terkonfirmasi yang dirender | FR-12.4, AC-61 |
| CHK-OUT-07 | Tautan internal merujuk berkas yang ada di `out/` dan anchor yang ada. Tidak ada `href=""` atau `href="#"` kosong. Tidak ada tautan keluar tanpa tujuan | FR-12.6, FR-05.7 |
| CHK-OUT-08 | Setiap HTML: `lang="en"`, tepat satu `h1`, `<title>` unik dan tidak kosong, `meta description`, ada `main` | FR-01.5, NFR-02.5, FR-14.1 |
| CHK-OUT-09 | Gerbang CV: tidak ada `.pdf` dan tidak ada tautan CV bila `getCv()` `null`. Bila ada, berkasnya cocok dengan sumber | AC-63 |
| CHK-OUT-10 | String pemicu galat uji (`FAULT_INJECTION`) tidak ada | `MOTION_SPEC.md` bagian 12 |
| CHK-OUT-11 | Ukuran terhadap anggaran (bagian 7.4) | NFR-01.7, AC-31 |
| CHK-OUT-12 | Himpunan rute di `out/` sama dengan yang diharapkan: beranda, satu per slug, `404.html`, `robots.txt`, `sitemap.xml`. Tidak ada rute lain | AC-64, AC-15 |
| CHK-OUT-13 | Melaporkan semua skrip inline dan hash-nya (laporan saja, bahan V-17 dan CSP) | R-33 |

### 7.4 Anggaran ukuran (C-04)

`scripts/config/budgets.json` menyimpan kategori dari ARCHITECTURE bagian 16:

```json
{
  "initialJsHomeKB": null,
  "extraJsCaseStudyKB": null,
  "motionJsKB": null,
  "aboveFoldImageKB": null,
  "fontFamilies": null,
  "fontFiles": null,
  "fontTotalKB": null
}
```

| Mode | Perilaku bila nilai `null` |
|---|---|
| Dev | `CHK-OUT-11` melaporkan ukuran terukur tanpa batas (INFO) |
| Rilis | **Error.** Rilis tidak dapat dibuat sebelum setiap nilai terisi |

Nilai diisi dari hasil S-01 dan S-03 plus margin yang diputuskan pemilik (A-05, DP-13). Nilai hanya boleh **diperketat**. Cara pengukuran ukuran (terkompresi atau tidak) ditetapkan di Tahap 6 dan dicatat bersama nilai.

## 8. Kompatibilitas

### 8.1 Kebijakan

Mengikuti `TECHNICAL_DESIGN.md` bagian 8. Satu baris register per fitur yang lebih baru dari baseline Chrome 111, Safari 16.4, Firefox 128.

### 8.2 Register fitur (awal)

| Fitur | Dipakai untuk | Versi minimum | Sumber | Perilaku bila tidak ada |
|---|---|---|---|---|
| `<ViewTransition>` / View Transitions | E-08, E-09 | Chromium 125+ untuk fitur transition types dan `view-transition-class`. Safari dan Firefox "terbaru" | EV-04 | Navigasi normal tanpa animasi |
| Tailwind v4 | Styling | Chrome 111, Safari 16.4, Firefox 128 | EV-05 | Gaya dapat hilang di bawah batas (R-28) |
| `<details>` sebagai menu | NavMenu | Belum dicatat | **Belum diverifikasi** | Fungsi tersedia di semua browser modern. Semantik pembaca layar diuji (S-06) |
| `IntersectionObserver` | E-03, E-05 | Belum dicatat | **Belum diverifikasi** | Tidak ada elemen dipersenjatai (F4) |
| `:focus-visible`, `@media (hover: hover)` | Fokus, hover | Belum dicatat | **Belum diverifikasi** | Gaya hover/fokus tidak berlaku. Informasi tetap terlihat |

Baris yang bertanda "belum diverifikasi" diisi dari sumber resmi saat fitur pertama kali dipakai di fase implementasi. Tidak ada angka yang ditulis dari ingatan.

### 8.3 Validasi pada Windows 10

| Hal | Aturan |
|---|---|
| Akhir baris | `.gitattributes` memaksa LF untuk berkas teks sumber |
| Jalur | Skrip memakai `node:path`. Keluaran laporan memakai pemisah `/` |
| Panjang jalur | Struktur folder dangkal. Tidak ada jalur sumber > 200 karakter |
| Instalasi | `npm ci`. Skrip siklus hidup paket (mis. unduhan biner pemroses gambar) diperiksa di S-04 dan S-05 (R-37) |
| Versi Node | Dipin setelah V-02 dan S-04 |

## 9. Deployment

### 9.1 Artefak

Folder `out/` hasil P2, **hanya bila P3 lolos** pada mode rilis. Host belum dipilih (ADR-0009). Tidak ada penerbitan publik sebelum gerbang rilis terpenuhi (NFR-09.6, AC-50).

### 9.2 Kebutuhan terhadap host (menjadi kriteria seleksi)

| Kebutuhan | Alasan | Cara diuji setelah host dipilih |
|---|---|---|
| Menyajikan `/projects/<slug>/` langsung, tanpa melewati beranda | FR-01.2, AC-01 | Buka URL langsung dan muat ulang |
| Menyajikan `404.html` dengan status 404 untuk rute tak dikenal | FR-13.1, AC-15 | Buka rute acak, periksa status dan isi |
| Perilaku tanpa dan dengan garis miring penutup konsisten dengan `trailingSlash: true` | D-04 | Buka `/projects/<slug>` dan `/projects/<slug>/` |
| HTTPS | Dasar keamanan | Periksa sertifikat dan pengalihan |
| Dapat menyetel header respons (atau opsi `<meta>` yang disepakati) | NFR-08.6, `SECURITY_PRIVACY_ERRORS.md` bagian 4 | `curl -I` pada beranda dan aset |
| Cache aset dengan nama berhash dapat panjang, HTML tidak | Performa | Periksa header `Cache-Control` |
| Tidak menyuntik skrip atau pelacak | NFR-08.3 | Inspeksi tab jaringan (AC-47) |
| Dukungan `BASE_PATH` bila host berupa subpath | V-23 | Build dengan `BASE_PATH` |

### 9.3 Header yang diinginkan (daftar tujuan, dikonfirmasi di host)

`Content-Security-Policy` (bagian 4 dokumen keamanan), `X-Content-Type-Options: nosniff`, `Referrer-Policy`, `Permissions-Policy` (menonaktifkan fitur yang tidak dipakai), `Strict-Transport-Security` (dari host), pembatasan pembingkaian (`frame-ancestors` lewat CSP). Tidak satu pun dapat disetel dari `next.config` pada static export (EV-03). Daftar ini adalah tujuan, bukan klaim bahwa host mana pun mendukungnya.

## 10. Kriteria verifikasi

| Bukti yang dibutuhkan | Cara | AC |
|---|---|---|
| Build rilis gagal dengan placeholder atau data tidak valid | Fixture dan `RELEASE=1` | AC-16 |
| `out/` tidak memuat URL pihak ketiga yang dimuat otomatis | `CHK-OUT-02` dan tab jaringan | AC-47 |
| Metadata gambar dan PDF bersih | `CHK-AST-02`, `CHK-AST-03` dan pemeriksaan manual contoh | AC-46, AC-63 |
| Tidak ada `.pdf` di `out/` saat CV belum siap | Dua konfigurasi, `CHK-OUT-09` | AC-63 |
| Himpunan rute tepat, termasuk tanpa rute How I Built This | `CHK-OUT-12` | AC-64 |
| Anggaran terisi dan tidak terlampaui | Pengukuran build | AC-31 |
| Rute dapat dibuka langsung, dimuat ulang, dibagikan di host terpilih | Uji pada host (menjelang rilis) | AC-01 |
| Toolchain berjalan dari awal sesuai dokumentasi di Windows 10 | Instalasi bersih | AC-51 |
| Instalasi pemroses gambar dan skema berjalan di Windows 10 | S-04 dan S-05 | R-37 |

## 11. Belum diputuskan

| ID | Item | Kapan |
|---|---|---|
| S-05 | Pembuktian `next/font/local`, `basePath`, route handler statis, HTML keluaran (skrip inline) | Tahap 5 |
| V-18 | Alat metadata gambar dan PDF dan alat subset font | Tahap 5 awal |
| V-20 | Pemroses gambar dan penjalan skrip | Sebelum dipasang |
| Q-19 | Host dan domain | Menjelang rilis |
| — | Nilai anggaran | Setelah S-01 dan S-03 |
