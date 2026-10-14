# UI_UX_SPEC — Spesifikasi UI/UX

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/UI_UX_SPEC.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 (Desain Teknis) |
| Dokumen induk | `docs/planning/TECHNICAL_DESIGN.md` |
| Rujukan | K-01, K-03, K-04, K-08, K-12, P-01 sampai P-05, FR-02 sampai FR-09, NFR-02, NFR-03, AC-03 sampai AC-08, AC-33 sampai AC-42 |
| Status | Struktur, kontrak, dan perilaku: **T4-USULAN**. Konsep visual, palet, font, dan headline: **menunggu pilihan pemilik (QT4-03, QT4-04)** |

## 1. Batas dokumen ini

Dokumen ini menetapkan **struktur, perilaku, kontrak komponen, dan aturan token**. Dokumen ini **tidak** mengunci estetika. Selera visual berasal dari penilaian pemilik (P-05). Saya tidak dapat menilai gerak dan tampilan situs referensi, termasuk https://motion.zajno.com/, dari data teks (brief Lampiran B). Karena itu bagian 3 menyajikan tiga arah yang dibedakan menurut **biaya, risiko, dan kecocokan struktural**, bukan menurut kualitas estetika.

## 2. Struktur halaman

### 2.1 Beranda `/`

Urutan bagian (DP-12). Setiap bagian adalah `<section>` dengan `id` dan judul (`h2`), kecuali Hero yang memuat satu-satunya `h1`.

| Urutan | `id` | Isi | Sumber data |
|---|---|---|---|
| 1 | `hero` | Nama, peran, pernyataan status, headline, tindakan ke Projects | `getProfile`, `getSite` |
| 2 | `projects` | Selected Projects: kartu per project | `listProjects` |
| 3 | `about` | Profil singkat, minat, pendekatan. Foto bila ada | `getProfile` |
| 4 | `expertise` | Teknologi per grup, tiap butir menyebut project dan cara pakai | `getExpertiseGroups` |
| 5 | `build-notes` | How I Built This, ringkas. **Tidak dirender** bila `getBuildNotes()` bernilai `null` | `getBuildNotes` |
| 6 | `contact` | GitHub, Instagram, email, tautan CV bila `getCv()` tidak `null` | `getContactLinks`, `getCv` |

Footer (pada semua halaman): kontak dan tautan CV yang sama (FR-09.5), serta tautan "Back to top".

### 2.2 Case study `/projects/[slug]/`

| Urutan | Elemen | Heading |
|---|---|---|
| 1 | Header halaman: judul, ringkasan, peran, label status, tanggal pembaruan, teknologi utama | `h1` |
| 2 | Ikhtisar dan konteks | `h2` |
| 3 | Masalah | `h2` |
| 4 | Solusi | `h2` |
| 5 | Teknologi (daftar `technologies` dengan `how`) | `h2` |
| 6 | Proses pengembangan | `h2` |
| 7 | Peran dan keterbatasan | `h2` |
| 8 | Penggunaan AI (`AiNote`) | `h2` |
| 9 | Hasil (`ResultsBlock`): butir terkonfirmasi, atau pernyataan jujur tanpa metrik | `h2` |
| 10 | Tautan (hanya yang terkonfirmasi) | di dalam bagian 9 atau bagian terpisah `h2`, hanya bila ada |
| 11 | `ProjectPager`: project berikutnya dan kembali ke daftar | `nav` dengan label |

Sub-bagian di dalam `Section` memakai `h3`. Tidak ada loncatan tingkat heading (FR-05.9, NFR-02.5). Urutan 2 sampai 8 mengikuti template 10.2 PRD. Penempatan "Hasil" setelah "Penggunaan AI" adalah usulan agar klaim berada dekat penjelasan peran. Dapat dibalik tanpa mengubah kontrak.

### 2.3 Halaman 404

`h1` "Page not found", satu paragraf, tautan ke beranda dan ke `/#projects`. Memakai header dan footer yang sama (FR-13.1).

### 2.4 Ringkasan landmark dan judul

| Rute | `<title>` | `h1` | Landmark |
|---|---|---|---|
| `/` | dari `site.titleTemplate` + nama | Nama (dalam Hero) | `header` (nav), `main`, `footer` |
| `/projects/[slug]/` | `seo.title` project | Judul project | idem |
| 404 | "Page not found" + nama | "Page not found" | idem |

Tautan "Skip to main content" adalah elemen fokus pertama di semua rute (FR-02.3) dan mengarah ke `main` (`id="main"`, `tabindex="-1"`).

## 3. Tiga arah visual (untuk dipilih pemilik, QT4-03)

Perbandingan struktural. "Kecocokan P-01" menilai apakah karakter yang diminta (ekspresif, berani, sinematik, eksperimental) dapat dicapai dengan teknik baseline, bukan menilai keindahannya.

| | **A. Editorial Gelap** | **B. Kertas Editorial** | **C. Teknis Grid** |
|---|---|---|---|
| Karakter | Latar hampir hitam, tipografi sangat besar, satu aksen terang | Latar krem hangat, hitam pekat, blok warna tegas per project | Latar biru-hitam, grid terlihat, label monospace, aksen biru muda |
| Kecocokan P-01 (sinematik, berani) | Tinggi lewat kontras dan skala tipografi | Sedang. Berani, tetapi kurang "sinematik" | Sedang. Kuat di kesan engineering, kurang dramatis |
| Aset yang dibutuhkan | Nyaris nol. Tipografi memimpin | Rendah. Blok warna menggantikan foto | Rendah. Garis dan angka sebagai ornamen |
| Cocok untuk dua project tanpa foto | Ya | Ya | Ya |
| Risiko performa (R-04) | Rendah. Hindari bayangan besar dan blur | Rendah | Rendah. Garis banyak: hindari ratusan elemen terpisah |
| Risiko aksesibilitas | Teks muted pada latar gelap perlu dijaga | **Aksen merah hanya lolos untuk teks besar dan elemen UI (lihat tabel kontras)** | Garis tipis dapat mengganggu pembacaan di zoom 200% |
| Kecocokan reduced motion | Mudah: perubahan opacity dan warna | Mudah | Mudah |
| Kecocokan tiga tingkat motion | Reveal kata dan stagger bekerja baik | Reveal blok warna bekerja baik | Garis menggambar diri (transform `scaleX`) bekerja baik |
| Risiko "terkesan template" (P-02) | Sedang. Gelap + aksen neon umum | Rendah | Sedang |
| Dampak pada pesan "bukti engineering" | Netral | Netral | Mendukung |

Rekomendasi struktural (bukan estetika): **prototipekan A dan satu dari B atau C di S-01**, karena S-01 membutuhkan perbandingan nyata pada POCO M4 Pro. Pilihan akhir ada pada pemilik.

### 3.1 Kontras palet kandidat (dihitung, bukan perkiraan)

Rumus luminansi relatif dan rasio kontras WCAG dihitung dengan skrip. Nilai ambang yang dipakai berasal dari PRD (NFR-02.4): 4,5:1 teks normal, 3:1 teks besar dan komponen UI. **Verifikasi nomor kriteria WCAG terhadap teks resmi tetap tugas Tahap 6 (V-10).**

| Pasangan | A (`bg #0E0E10`) | B (`bg #F3EEE4`) | C (`bg #101418`) |
|---|---|---|---|
| Teks `text` di atas `bg` | 16,79 (#F2EFE8) | 15,93 (#141414) | 15,57 (#E8ECEF) |
| Teks `text` di atas `surface` | 15,58 (#17171A) | 14,29 (#E9E2D3) | 13,83 (#1A2026) |
| `muted` di atas `bg` | 7,92 (#A8A6A0) | 6,82 (#55514A) | 7,37 (#9AA5AE) |
| `muted` di atas `surface` | 7,35 | 6,12 | 6,54 |
| `accent` di atas `bg` | 16,77 (#D7FF3F) | **4,00** (#D9381E) | 9,83 (#5CC8FF) |
| Teks tombol di atas `accent` | 16,77 | **4,00** | 9,83 |
| Indikator fokus di atas `bg` | 19,28 (#FFFFFF) | 15,93 (#141414) | 18,50 (#FFFFFF) |
| Indikator fokus di atas `surface` | 17,89 | 14,29 | 16,43 |
| `border` dekoratif di atas `bg` | 1,71 (#3A3A40) | 3,21 (#8A8478) | 1,92 (#3B4650) |

Catatan hasil:
1. **B:** aksen `#D9381E` hanya 4,00:1. Tidak memenuhi 4,5:1 untuk teks normal. Boleh untuk teks besar dan komponen UI (≥ 3:1), bukan untuk teks tombol berukuran normal. Perlu aksen yang lebih gelap bila B dipilih.
2. **A dan C:** `border` 1,71 dan 1,92 hanya untuk garis dekoratif. **Batas komponen interaktif** (kolom input, tombol kontur) harus memakai token terpisah `--color-border-strong` yang ≥ 3:1. Token ini perlu dihitung ulang saat nilai final dipilih.
3. Semua nilai di atas adalah **kandidat**. Kontras di atas gambar dan saat animasi tetap harus diuji (AC-34).

### 3.2 Tipografi

| Aspek | Aturan |
|---|---|
| Jumlah | Maksimal 2 keluarga, maksimal 3 berkas (font variabel dianjurkan). Angka final [SEMENTARA], masuk anggaran (ARCHITECTURE bagian 16) |
| Peran | Satu keluarga **display** (Hero, judul bagian), satu keluarga **teks** (isi, UI). Boleh satu keluarga untuk keduanya |
| Lisensi | Hanya font dengan lisensi yang mengizinkan penyematan dan penyebaran di situs. **Lisensi setiap berkas diperiksa dari berkas lisensi yang menyertai font (V-21).** Daftar nama kandidat sengaja tidak diberikan di sini karena lisensinya belum saya baca |
| Subset | Subset Latin. Pemilihan alat subset dan unicode-range di Tahap 5 (V-18, ASSETS_AND_BUILD bagian 4) |
| Cadangan | `next/font/local` dengan `adjustFontFallback` (EV-11) atau `@font-face` cadangan manual dengan `size-adjust` bila S-05 menunjukkan opsi pertama tidak bekerja pada static export |
| Skala | Fluida dengan `clamp()`. Titik acuan: 320 dan 1440 piksel CSS (bagian 4.3) |

## 4. Token desain

### 4.1 Aturan

1. Semua nilai desain didefinisikan **sekali** sebagai CSS custom properties di `src/styles/tokens.css` (ADR-0003 butir 2). Komponen merujuk token melalui utilitas Tailwind yang dipetakan ke token, atau melalui `var(--...)`.
2. Token motion hidup di `tokens.css` bagian motion dan diperinci di `MOTION_SPEC.md` bagian 3.
3. Nilai literal warna, ukuran huruf, jarak, durasi, dan easing di komponen tidak diterima (diperiksa `CHK-ARCH-05` untuk motion, tinjauan kode untuk lainnya).
4. Cara pemetaan token ke tema Tailwind v4 diverifikasi dari dokumentasi resmi saat Fase 1 (V-22). Spesifikasi ini tidak bergantung pada sintaks tertentu.

### 4.2 Daftar token

| Kelompok | Token | Catatan |
|---|---|---|
| Warna | `--color-bg`, `--color-surface`, `--color-text`, `--color-text-muted`, `--color-accent`, `--color-on-accent`, `--color-border`, `--color-border-strong`, `--color-focus` | Nilai dari konsep terpilih. Setiap pasangan teks-latar yang dipakai dicatat kontrasnya di berkas token sebagai komentar |
| Tipografi | `--font-display`, `--font-text`, `--text-xs` … `--text-hero` | Ukuran fluida |
| Jarak | `--space-1` … `--space-10` (skala konsisten) | Satu skala dipakai di seluruh situs |
| Tata letak | `--container-max`, `--gutter`, `--header-h` | `--header-h` dipakai oleh `scroll-padding-top` |
| Bentuk | `--radius-s`, `--radius-m` | |
| Lapisan | `--z-header`, `--z-menu`, `--z-skip` | Skip link di atas semua |
| Fokus | `--focus-ring-width`, `--focus-ring-offset` | Indikator fokus ≥ 3:1 terhadap latar sekitar |
| Motion | `--motion-*` | Lihat `MOTION_SPEC.md` |

### 4.3 Kelas lebar (NFR-03.1, AS-05) [SEMENTARA]

| Kelas | Rentang (piksel CSS) | Acuan desain | Catatan |
|---|---|---|---|
| Kecil | < 768 | 360 (acuan), 320 (ekstrem) | Satu kolom. Menu `<details>` |
| Menengah | 768 sampai 1023 | 768 | Dua kolom di bagian tertentu. Navigasi dapat tampil inline bila muat |
| Besar | ≥ 1024 | 1280 dan 1440 | Komposisi penuh. `--container-max` membatasi pelebaran (EC-08) |

Batas bersifat dugaan. Ukuran viewport nyata POCO M4 Pro dan laptop belum diketahui (V-11). Batas dikalibrasi setelah V-11 dijawab, tanpa mengubah jumlah kelas. Desain wajib bekerja di 320 tanpa scroll horizontal (EC-05, AC-37).

### 4.4 Skala tipografi (aturan, bukan nilai final)

| Peran | Aturan |
|---|---|
| Hero | Ukuran terbesar situs. Fluida. Pada 320 piksel, kata terpanjang headline dan nama harus muat tanpa terpotong atau overflow. `overflow-wrap: break-word` sebagai jaring pengaman (EC-21) |
| Judul bagian (`h2`) | Satu langkah di bawah Hero |
| Isi | Panjang baris 45 sampai 75 karakter. Tinggi baris ≥ 1,5 untuk paragraf (kenyamanan baca, bukan syarat kepatuhan) |
| Label dan metadata | Tidak lebih kecil dari ukuran dasar yang tetap terbaca pada 200% (EC-15) |
| Pemakaian `text-wrap: balance` | Peningkatan opsional. Layout tidak boleh bergantung padanya |

### 4.5 Aturan lintas tema

1. Satu tema, tanpa tombol terang/gelap (D-15). Deklarasikan `color-scheme` sesuai tema.
2. **Mode warna paksa (EC-16):** `@media (forced-colors: active)` memastikan fokus memakai `outline` (bukan hanya `box-shadow`), batas komponen tetap terlihat, dan ornamen dekoratif tidak mengganggu.
3. Indikator fokus: `outline` solid dengan jarak (offset), kontras ≥ 3:1 terhadap kedua sisi, tidak dihilangkan oleh `outline: none` tanpa pengganti.
4. `scroll-padding-top` dan `scroll-margin-top` memakai `--header-h` plus jarak agar fokus keyboard dan anchor tidak tertutup header tetap (FR-02.4, NFR-02.2, EC-10).

## 5. Inventaris komponen dan kontrak

Props dibuat sempit. Komponen menerima *view* yang sudah dipilih bahasanya (`TECHNICAL_DESIGN.md` bagian 6.1). Semua komponen adalah Server Component kecuali ditandai **[klien]**.

### 5.1 Layout

| Komponen | Props | Perilaku dan semantik |
|---|---|---|
| `SkipLink` | `targetId: string` | `<a href="#main">` pertama di DOM. Tersembunyi secara visual sampai difokuskan, lalu terlihat jelas |
| `SiteHeader` | `nav: NavItemView[]`, `current?: string` | `<header>`. Memuat nama situs (tautan ke `/`) dan `NavMenu`. Tidak bersembunyi otomatis saat scroll |
| `NavMenu` | `nav`, `current?` | Kelas kecil: `<details>` (D-16). Kelas menengah ke atas: `<nav>` inline, ringkasan disembunyikan. Satu `<nav aria-label="Primary">`. Item aktif ditandai `aria-current="true"` ditambah penanda non-warna (garis bawah atau bobot) (FR-02.5) |
| `MenuEnhancer` **[klien]** | `—` | Mengatur Escape (tutup dan kembalikan fokus ke `summary`), menutup saat tautan diaktifkan atau rute berubah, menutup saat lebar masuk kelas menengah. Tanpa komponen ini menu tetap dapat dibuka dan ditutup |
| `SiteFooter` | `contact`, `cv` | `<footer>`. Kontak dan CV yang sama dengan bagian Contact |
| `RouteAnnouncer` **[klien]** | — | Wilayah `aria-live="polite"` yang mengumumkan judul halaman baru. Dipertahankan hanya bila perilaku bawaan Next tidak memenuhi AC-26 (V-24) |

### 5.2 Bagian beranda

| Komponen | Props | Catatan |
|---|---|---|
| `Hero` | `name`, `role`, `status`, `headline`, `ctaHref` | Struktur di bagian 6. Elemen kandidat LCP ditandai `data-lcp` |
| `SelectedProjects` | `projects: ProjectCardView[]` | Daftar `<ul>` dengan `<li>` per kartu. Tata letak valid untuk 1 sampai N kartu (FR-04.5) |
| `ProjectCard` | `slug`, `title`, `summary`, `role`, `statusLabel`, `technologies: string[]`, `image?: MediaView` | Satu tautan utama ke `/projects/<slug>/`. Bagian 7 |
| `About` | `blocks`, `interests`, `approach`, `photo?` | Bekerja dengan dan tanpa `photo` (FR-06.2) |
| `Expertise` | `groups: ExpertiseGroupView[]` | `<h3>` per grup, `<ul>` per butir: label teknologi, daftar project, kalimat cara pakai. Tanpa bar, bintang, persen |
| `BuildNotes` | `items`, `limitations`, `aiStatement`, `sourceLink?` | Tidak dirender bila data `null`. Butir performa menyebut kategori bukti (A/B) dan ringkasan kondisi uji |
| `Contact` | `links`, `cv?` | Bagian 8.3 |

### 5.3 Case study

| Komponen | Props | Catatan |
|---|---|---|
| `CaseStudy` | `project: ProjectView` | Menyusun bagian 2.2 |
| `CaseStudySection` | `heading`, `blocks`, `level` | Menentukan `h2`/`h3` dari `level` |
| `AiNote` | `text` | Bagian wajib (FR-05.3) |
| `ResultsBlock` | `items: ClaimView[]`, `noMetricsStatement?` | Setiap butir dirender dengan `data-claim-id`. Bila `items` kosong, hanya pernyataan jujur |
| `StatusBadge` | `kind`, `label` | Menampilkan status **project** ("In development"). **Bukan** badge ketersediaan kerja (FR-03.8). Makna tidak bergantung warna |
| `MediaFigure` | `media: MediaView`, `caption?` | `<figure>` dengan `Img`. `decorative` → `alt=""` |
| `ProjectPager` | `previous?`, `next?` | `<nav aria-label="More projects">`. Sembunyi bila hanya satu project |

### 5.4 UI dasar

| Komponen | Props | Catatan |
|---|---|---|
| `Img` | `media: MediaView`, `sizes: string`, `priority?: boolean` | `<picture>` + `<img>` dengan `width`, `height`, `alt`, `decoding="async"`, `loading="lazy"` kecuali `priority`. Rasio ruang tetap dicadangkan lewat `aspect-ratio`. Bila gambar gagal, kotak tetap, `alt` terbaca |
| `ExternalLink` | `href`, `children`, `newTab?` | `rel="noopener noreferrer"`. Bila `newTab`, tambahkan `target="_blank"` dan teks tersembunyi visual "(opens in a new tab)". Pembaca layar dapat membedakan tautan keluar (FR-09.3) |
| `EmailLink` | `address` | Teks alamat terlihat dan `mailto:` (D-17). Alamat ditulis sebagai entitas HTML saat build. Opsional tombol "Copy" **[klien]** sebagai peningkatan |
| `CvLink` | `href`, `label` | Tautan biasa ke PDF statis. Tidak ada atribut `download` (DP-11) |
| `VisuallyHidden` | `children` | Teks hanya untuk pembaca layar |
| `Tag` | `children` | Label teknologi |

### 5.5 Pagar komponen (aturan)

1. Tidak ada `onClick` pada elemen non-interaktif. Semua yang dapat diklik adalah `<a>` atau `<button>`.
2. Ikon dan ornamen dekoratif: `aria-hidden="true"` dan `focusable="false"` (FR-05.4).
3. Nama aksesibel tautan dan kontrol jelas dari konteksnya. Tidak ada "click here" atau "read more" tanpa konteks (NFR-02.7).
4. Tidak ada elemen yang menyembunyikan teks dengan `display:none` bila teks itu diperlukan pembaca layar.

## 6. Hero

### 6.1 Anatomi

```
[Header: nama situs ........ menu]
  ROLE  Creative Developer                (teks kecil, statis)
  NAME / HEADLINE  (tipografi sangat besar, h1)
  STATUS  Informatics Engineering student (teks kecil)
  [ View selected projects ]  ->  #projects
  (petunjuk gulir: statis, bukan animasi berulang)
```

| Aturan | Sumber |
|---|---|
| `h1` hanya satu. Isi `h1` adalah nama atau headline, ditentukan di keputusan konten (QT4-04). Satu saja yang menjadi `h1`, yang lain `p` | NFR-02.5 |
| Nama, peran, status, dan akses navigasi terlihat tanpa menggulir pada 360 × 640 piksel CSS (sebagai viewport konservatif sampai V-11) | FR-03.5, AC-05 |
| Tidak ada layar loading penuh. Tidak ada badge ketersediaan | FR-03.6, FR-03.8 |
| Teks hero adalah HTML nyata yang dapat dipilih dan dicari | FR-10.9 |
| Tidak ada gambar hero wajib. Kandidat LCP adalah teks. Bila kelak ada gambar, ia **tidak** menjadi kandidat LCP kecuali `priority` dan dimuat tanpa JS | FR-03.4 |
| Reveal dan efek mengikuti `MOTION_SPEC.md` bagian 5 (varian HR-1 sampai HR-3) | C-02, C-03 |

### 6.2 Keadaan

| Kondisi | Tampilan |
|---|---|
| JS mati | Semua teks terlihat dan statis |
| `reduced` | Teks terlihat. Perubahan opacity ringan atau statis (FR-03.7) |
| Font belum dimuat | Font cadangan dengan metrik dekat. Tidak ada pergeseran besar (R-34) |
| Animasi gagal | Semua elemen terlihat (fail-safe `MOTION_SPEC.md` bagian 4) |

## 7. Selected Projects

### 7.1 Kartu

| Aspek | Aturan |
|---|---|
| Struktur | `<li><article>`: judul sebagai `h3` berisi satu `<a>` ke case study. Ringkasan, peran, status, dan teknologi adalah teks biasa yang **selalu terlihat** |
| Tautan | Satu tautan per kartu. Area klik dapat diperluas ke seluruh kartu dengan elemen semu pada tautan judul, sehingga pembaca layar tidak mendengar tautan ganda |
| Gambar | Opsional. Bila tidak ada, kartu memakai blok tipografi atau warna dari konsep terpilih. Kartu tanpa gambar harus tetap tampak disengaja (R-24) |
| Dua project, dan N project | Tata letak berbasis CSS Grid dengan pola yang berulang berdasarkan indeks (mis. offset kolom pada `:nth-child`). Menambah project tidak mengubah komponen (FR-04.5, AC-07) |

### 7.2 Padanan hover, fokus, sentuhan (FR-04.4, AC-08)

| Efek visual | Pointer halus dengan hover | Fokus keyboard (`:focus-visible`) | Sentuhan atau pointer kasar |
|---|---|---|---|
| Image reveal atau perubahan gambar | Saat `:hover` | Efek **sama** saat `:focus-visible` pada tautan kartu | Keadaan akhir efek **ditampilkan permanen** atau animasi masuk sekali saat terlihat. Tidak ada informasi yang hanya muncul lewat efek |
| Perubahan warna judul atau garis | `:hover` | `:focus-visible` | Tidak diperlukan |
| Informasi (judul, ringkasan, peran, status) | Selalu terlihat | Selalu terlihat | Selalu terlihat |
| `reduced` | Perubahan warna atau opacity saja | idem | idem |

Aturan: efek hover dibungkus `@media (hover: hover) and (pointer: fine)` agar tidak tersangkut pada layar sentuh.

## 8. Case study, kontak, dan tautan

### 8.1 Template

Tata letak satu kolom dengan lebar baca terbatas. Media dapat lebih lebar dari teks pada kelas besar. Tabel atau blok kode (bila ada) bergulir di dalam kontainernya (NFR-03.2). Struktur heading mengikuti bagian 2.2.

### 8.2 Gambar dan media

Semua media lewat `MediaFigure` → `Img`. Pada project RSUD, gambar hanya berasal dari antarmuka **dibuat ulang dengan data dummy** (`origin: 'recreated'` atau `'dummy'`) sampai izin terkonfirmasi (FR-05.5, AC-11). Teks alternatif tidak boleh membocorkan data internal (FR-05.4, `CHK-OUT-05`).

### 8.3 Kontak dan tautan keluar

| Hal | Keputusan (T4-USULAN) |
|---|---|
| Email | Teks alamat terlihat + `mailto:`. Tanpa obfuskasi yang menghalangi pembaca layar atau penyalinan. Pengodean entitas HTML saat build sebagai mitigasi spam yang lemah (D-17). Risiko spam diterima dan dicatat |
| GitHub dan Instagram | Dibuka di tab baru dengan `rel="noopener noreferrer"` dan teks tersembunyi visual yang menyebut tab baru. `mailto:` tanpa `target` |
| CV | Tautan biasa ke PDF. Label menyebut format dan bahasa. Tidak ada `download` |
| Perangkat tanpa aplikasi surel | Alamat tetap terlihat dan dapat disalin (EC-17, AC-62). Tombol "Copy" hanya peningkatan |
| Tampil bila belum terkonfirmasi | Pada mode dev, nilai `[PLACEHOLDER: ...]` tampil dengan penanda visual. Pada rilis build gagal |

## 9. Accessibility dan interaksi keyboard

### 9.1 Urutan fokus dan pintasan

| Konteks | Interaksi |
|---|---|
| Awal halaman | Tab pertama: "Skip to main content". Enter memindahkan fokus ke `main` |
| Header | Nama situs, lalu menu. Pada kelas kecil: `summary` menu |
| Menu `<details>` | Enter atau Spasi pada `summary` membuka atau menutup (bawaan browser). Setelah dibuka, Tab berjalan melalui tautan. **Escape** menutup dan mengembalikan fokus ke `summary` (peningkatan JS). Tautan yang diaktifkan menutup menu |
| Beranda | Urutan fokus mengikuti urutan visual dan DOM: hero, daftar project, about, expertise, build notes, contact, footer |
| Kartu project | Satu titik fokus per kartu. Enter membuka case study |
| Setelah navigasi klien | Fokus berpindah ke awal konten halaman baru (`h1` dengan `tabindex="-1"` atau `main`), dan judul halaman diumumkan. Detail dan kondisi pengujian di `MOTION_SPEC.md` bagian 8 |
| Anchor `#projects` dll. | Menggulir ke bagian yang benar. Elemen bagian tidak tertutup header |

### 9.2 Aturan

1. Tidak ada jebakan fokus (WCAG 2.1.2 menurut rujukan PRD). Menu bukan dialog modal, sehingga tidak memerlukan perangkap fokus.
2. `prefers-reduced-motion` tidak menghapus indikator fokus atau perubahan keadaan yang bersifat informasi.
3. Target sentuh: minimum 24 × 24 piksel CSS (NFR-02.8). **Target yang sering dipakai** (tombol menu, tautan navigasi, CTA hero, kartu project) dirancang ≥ 44 piksel CSS pada tinggi atau area sentuh efektif.
4. Zoom 200% dan lebar 320: tidak ada scroll horizontal, tidak ada konten terpotong atau tumpang tindih (NFR-02.9).
5. `lang="en"` pada `<html>`. Bila kelak ada bahasa lain, bagian bahasa lain diberi `lang` sendiri.
6. Teks yang dipecah untuk animasi tetap terbaca utuh (D-13, FR-10.9).

## 10. Keadaan komponen (matriks)

| Komponen | Default | Hover (pointer halus) | Fokus | Aktif | Aktif-halaman | Dinonaktifkan | Gagal/kosong |
|---|---|---|---|---|---|---|---|
| Tautan navigasi | ✓ | ✓ | ✓ jelas | ✓ | `aria-current` + non-warna | n/a | n/a |
| Tombol/CTA | ✓ | ✓ | ✓ jelas | ✓ | n/a | n/a | n/a |
| Kartu project | ✓ | ✓ | ✓ jelas | ✓ | n/a | n/a | Tanpa gambar: blok tipografi |
| `Img` | ✓ | n/a | n/a | n/a | n/a | n/a | Kotak tetap, `alt` terbaca |
| Menu `<details>` | tertutup | ✓ | ✓ | ✓ | n/a | n/a | JS gagal: tetap berfungsi |
| `EmailLink` | ✓ | ✓ | ✓ | ✓ | n/a | n/a | Tanpa aplikasi surel: alamat terlihat |

Tidak ada keadaan *loading* penuh. Tidak ada keadaan dinonaktifkan karena tidak ada formulir.

## 11. Kriteria verifikasi

| Bukti yang dibutuhkan | Cara | AC |
|---|---|---|
| Viewport pertama 360 × 640 menampilkan nama, peran, status, akses navigasi | Pemeriksaan visual di POCO M4 Pro dan emulasi | AC-05 |
| Navigasi dan anchor tidak tertutup header di tiga kelas lebar | Uji manual | AC-03 |
| Menu dapat dibuka, ditutup (termasuk Escape), fokus kembali | Uji keyboard dan sentuhan di Chrome desktop dan POCO M4 Pro | AC-04 |
| Tidak ada informasi hanya lewat hover | Tinjauan per komponen, uji sentuhan | AC-08, AC-39 |
| Struktur heading, landmark, `lang`, nama kontrol | Alat otomatis ditambah tinjauan manual | AC-35 |
| Target sentuh dan zoom 200% | Pengukuran dan uji nyata | AC-36, AC-37 |
| Kontras final memenuhi rasio, termasuk di atas gambar | Alat ditambah sampel manual | AC-34 |
| `forced-colors` memperlihatkan fokus dan konten | Uji manual dengan mode kontras tinggi Windows | EC-16 |
| Narrator dan TalkBack membaca menu, kartu, dan hero secara utuh | Skrip uji Tahap 6 | AC-38, AC-22 |
| Dua dan N project tampak rapi, tanpa gambar | Fixture data | AC-07, R-24 |

## 12. Belum diputuskan

| ID | Item | Siapa | Kapan |
|---|---|---|---|
| QT4-03 | Konsep visual (A, B, C) dan referensi tambahan | Pemilik | Sebelum S-01 |
| QT4-04 | Headline | Pemilik | Sebelum konten hero final |
| V-21 | Font dan lisensinya | Claude dari berkas lisensi, pemilik memilih | Setelah QT4-03 |
| V-11 | Ukuran viewport nyata | Pemilik | Sebelum kelas lebar dikunci |
| S-06 | Perilaku `<details>` dengan Narrator dan TalkBack | Pemilik dan Claude | Tahap 5 |
