# TECHNICAL_DESIGN — Creative Developer Portfolio Website

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/TECHNICAL_DESIGN.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 dari 10 (Desain Teknis) |
| Status | **Menunggu review dan persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 5)** |
| Dasar | `PROJECT_BRIEF.md` v0.2, `PRD.md` v0.3, `ARCHITECTURE.md` v0.1, `adr/ADR-0001` sampai `ADR-0009` |
| Dokumen pendamping | `CONTENT_MODEL.md`, `UI_UX_SPEC.md`, `MOTION_SPEC.md`, `ASSETS_AND_BUILD.md`, `SECURITY_PRIVACY_ERRORS.md` |

Dokumen ini adalah **pintu masuk** Tahap 4: menetapkan deliverable yang relevan, mencatat temuan pemeriksaan dokumen, daftar keputusan, struktur folder, kontrak antarmodul, dan daftar hal yang belum selesai. Isi rinci ada di lima dokumen pendamping.

## 1. Cara membaca dokumen ini

Setiap pernyataan diberi status agar keputusan, usulan, dan hal yang belum diverifikasi tidak tercampur.

| Penanda | Arti |
|---|---|
| **DISEPAKATI** | Keputusan pemilik yang sudah tercatat di brief atau PRD (K-xx, DP-xx yang berstatus diputuskan). Tidak diubah di tahap ini |
| **ADR-USULAN** | Keputusan ADR-0001 sampai ADR-0009. Semuanya masih berstatus *Diusulkan*. Dipakai sebagai **dasar kerja**, bukan keputusan final (sesuai instruksi Tahap 4) |
| **T4-USULAN** | Keputusan baru di Tahap 4 (ID **D-xx**). Menunggu persetujuan pemilik bersama dokumen ini |
| **VERIFIKASI** | Belum terbukti. Diverifikasi lewat spike atau build di Tahap 5 atau sesudahnya (ID **V-17** dan seterusnya, melanjutkan V-16 di ARCHITECTURE) |
| **KEPUTUSAN-PEMILIK** | Tidak dapat diputuskan Claude (ID **QT4-xx**). Perilaku bawaan sampai dijawab dicantumkan |
| **[SEMENTARA]** | Nilai sengaja belum dikunci (A-05, DP-13) |

Penomoran baru di Tahap 4: **D-xx** (keputusan), **C-xx** (temuan kontradiksi atau celah), **QT4-xx** (pertanyaan pemilik), **R-32 dan seterusnya** (risiko, melanjutkan R-31), **S-05 dan S-06** (spike tambahan, diusulkan), **CHK-xxx** (aturan pemeriksaan otomatis).

## 2. Yang diperiksa dan temuan

### 2.1 Sumber yang dibaca

Repositori `lkmandef/creative-developer-portfolio` di-clone dan dibaca langsung (satu commit: `0494615 docs: add software architecture`). Berkas yang ada: `PROJECT_BRIEF.md`, `PRD.md`, `ARCHITECTURE.md`, dan sembilan ADR. Versi di repositori cocok dengan dokumen yang diberikan (brief 0.2, PRD 0.3, ARCHITECTURE 0.1). Tidak ada berkas yang tidak dapat diakses. Repositori belum berisi kode, konfigurasi, `.gitignore`, maupun lisensi.

### 2.2 Verifikasi sumber resmi yang dilakukan di Tahap 4

Hanya yang dibaca dan dicatat di sini yang boleh dianggap terverifikasi. Detail di `ASSETS_AND_BUILD.md` bagian 2.

| ID | Fakta | Sumber | Dipakai untuk |
|---|---|---|---|
| EV-09 | Halaman static export Next.js (versi 16.4.0, diperbarui 9 Agu 2026) menyatakan opsi `trailingSlash: true` mengubah `/me` menjadi `/me/` dan menghasilkan `/me/index.html`. Dengan `trailingSlash` mati, rute dinamis menghasilkan berkas seperti `/blog/post-1.html` dan contoh Nginx memerlukan aturan *rewrite* tambahan untuk kasus itu. Opsi `basePath` tidak dibahas di halaman ini | https://nextjs.org/docs/app/guides/static-exports | D-04 (`trailingSlash`) |
| EV-10 | Halaman yang sama: optimasi gambar pada static export hanya didokumentasikan lewat **loader kustom**. Penggunaan loader bawaan tercantum sebagai tidak didukung. Opsi `images.unoptimized` **tidak** disebut di halaman ini | sama | D-06 (komponen gambar sendiri) |
| EV-11 | Dokumentasi `next/font` (nextjs.org) menyatakan font di-host sendiri, berkas CSS dan font diunduh saat build dan disajikan bersama aset statis, serta `next/font/local` memiliki opsi `adjustFontFallback` untuk mengurangi CLS. Dokumentasi yang terbaca **tidak membahas static export secara khusus** | https://nextjs.org/docs/app/api-reference/components/font | D-07 (font lokal). Perilaku pada static export tetap VERIFIKASI (V-13, S-05) |
| EV-12 | Sumber **pihak ketiga** (blog, README pustaka komunitas) menyebut `images.unoptimized: true` sebagai cara menghindari galat loader bawaan pada static export. Ini **bukan sumber resmi** dan tidak dijadikan dasar keputusan | beberapa halaman hasil pencarian | Hanya menjelaskan mengapa D-06 menghindari ketergantungan itu |

**Tidak diverifikasi di Tahap 4** (jangan dianggap fakta): sintaks konfigurasi tema Tailwind v4 (`@theme`), perilaku `basePath` pada static export, keberadaan skrip inline pada HTML hasil export, perilaku fokus otomatis App Router saat navigasi, dukungan `<details>` sebagai menu di Narrator dan TalkBack, serta lisensi dan versi paket yang disebut sebagai kandidat.

### 2.3 Temuan kontradiksi dan celah

Tidak ada keputusan lama yang diubah. Setiap temuan menyatakan apa yang dilakukan desain ini.

| ID | Temuan | Dampak | Penanganan di desain ini |
|---|---|---|---|
| **C-01** | **Repositori tampak dapat dibaca publik dan memuat nama institusi.** `git clone` tanpa kredensial berhasil. `PROJECT_BRIEF.md` menyebut nama RSUD sebanyak 4 kali (baris 87, 88, 90, 265). Brief bagian 12 butir 7, NFR-09.4, R-01, dan NFR-08.5 menetapkan nama institusi tidak dipublikasikan sebelum izin terkonfirmasi dan repositori dibuka hanya setelah audit. Visibilitas pastinya tidak dapat saya periksa (API GitHub terkena batas laju). Metadata commit juga memuat alamat email penulis | Kemungkinan pelanggaran gerbang yang dibuat sendiri oleh proyek. Bukan pelanggaran hukum yang dapat saya nilai | **QT4-01**. Tidak ada dokumen yang saya ubah. `SECURITY_PRIVACY_ERRORS.md` bagian 2 memuat opsi dan prosedur |
| **C-02** | FR-03.3 (masked text reveal) dan FR-03.4 (elemen LCP tidak disembunyikan sebelum animasi siap) bertabrakan. Sudah dicatat sebagai R-29 | Desain hero tidak dapat dikunci sebelum S-01 | `MOTION_SPEC.md` bagian 5: tiga varian hero dengan aturan pemilihan berbasis hasil ukur. Varian aman (HR-3) selalu tersedia |
| **C-03** | ADR-0005 butir 4 menyatakan hanya elemen **di bawah lipatan** yang boleh disembunyikan untuk animasi, sedangkan FR-03.3 meminta animasi masuk hero. Animasi masuk hero secara alami dimulai dari keadaan tersembunyi atau bergeser | Ambigu: apakah *keyframe* CSS di atas lipatan termasuk "disembunyikan" | **QT4-02.** Usulan tafsir: keyframe CSS di atas lipatan boleh bila (a) bukan kandidat LCP, (b) murni CSS tanpa menunggu JS, dan (c) bila CSS gagal elemen tetap terlihat. Tanpa persetujuan, HR-3 dipakai |
| **C-04** | NFR-01.7 dan AC-31 menyatakan anggaran ukuran "ditetapkan di Tahap 3", tetapi ARCHITECTURE bagian 16 sengaja membiarkannya [SEMENTARA]. AC-31 tidak dapat diuji sebelum ada angka | Celah pada gerbang | `ASSETS_AND_BUILD.md` bagian 7: berkas anggaran dengan nilai `null`. Build rilis **gagal** bila ada nilai `null`. Pengisian dijadwalkan setelah S-01 dan S-03 (Tahap 5) |
| **C-05** | ADR-0002 memakai nama bidang berbahasa Indonesia (`terkonfirmasi`, `diperiksaPemilik`, `siap`, `anonim`, `bernama`) sebagai ilustrasi dan menyerahkan nama final ke Tahap 4 | Bukan kontradiksi. Perlu keputusan penamaan | D-02: identifier kode berbahasa Inggris, pemetaan di `CONTENT_MODEL.md` bagian 2 |
| **C-06** | ARCHITECTURE bagian 15 menyebut P0 (tipe dan lint) terpisah karena `next build` tidak menjalankan linter (EV-01). Pemilihan ESLint atau Biome ditunda ke Tahap 6, padahal Tahap 4 membutuhkan aturan batas impor | Aturan batas modul harus dapat ditegakkan otomatis | Aturan ditulis **netral terhadap alat** (bagian 5.3). Alat dipilih di Tahap 6 |
| **C-07** | Lokasi bukti uji (referensi `reportRef` pada klaim performa dan accessibility) belum ditetapkan. ARCHITECTURE bagian 17 menyerahkannya ke Tahap 4 dan 6 | Validator klaim perlu format rujukan | Format rujukan `TR-<tanggal>-<slug>` ditetapkan sekarang. Lokasi dan isi catatan bukti ditetapkan di Tahap 6 |
| **C-08** | PRD AS-05 menyebut lebar acuan 360 piksel CSS, tetapi ukuran viewport nyata POCO M4 Pro belum diketahui (V-11) | Batas kelas lebar bersifat dugaan | Batas diberi label [SEMENTARA]. Tata letak dirancang hingga 320 piksel (EC-05) |

## 3. Deliverable Tahap 4 yang relevan

| Kebutuhan dari prompt Tahap 4 | Relevan | Dokumen | Alasan |
|---|---|---|---|
| Struktur folder dan tanggung jawab modul | Ya | Dokumen ini, bagian 5 | NFR-07.1, ARCHITECTURE bagian 7 |
| UI/UX: halaman, komponen, responsif, token, tipografi, warna, state | Ya | `UI_UX_SPEC.md` | ARCHITECTURE bagian 21 butir 3 |
| Kontrak komponen, tipe data, content model, validasi | Ya | `CONTENT_MODEL.md`, `UI_UX_SPEC.md` bagian 8 | ADR-0002, FR-12 |
| Arsitektur motion, enhancement bertahap, reduced motion, fallback | Ya | `MOTION_SPEC.md` | ADR-0004 sampai ADR-0006 |
| Aset, gambar, font, pemuatan | Ya | `ASSETS_AND_BUILD.md` bagian 3 sampai 5 | V-12, V-13, ADR-0008 |
| Accessibility dan interaksi keyboard | Ya | `UI_UX_SPEC.md` bagian 9, `MOTION_SPEC.md` bagian 9 | NFR-02 |
| Error handling dan kegagalan fitur atau resource | Ya | `SECURITY_PRIVACY_ERRORS.md` bagian 6 | FR-13 |
| Security dan privacy, pengamanan repositori | Ya | `SECURITY_PRIVACY_ERRORS.md` bagian 1 sampai 5 | NFR-08, NFR-09 |
| Strategi testing dan bukti | **Sebagian.** Strategi uji penuh milik Tahap 6. Tahap 4 hanya memuat kriteria verifikasi dan bukti per spesifikasi | Tiap dokumen, bagian "Kriteria verifikasi" | Menghindari duplikasi dengan Tahap 6 |
| Integrasi build, static export, deployment, kompatibilitas | Ya | `ASSETS_AND_BUILD.md` bagian 6 sampai 9 | ADR-0001, ADR-0009 |
| Kontrak antarfase | Ya, ringkas | Dokumen ini, bagian 6 | Mencegah konflik antarmodul |
| Database schema, API contract, backend, infrastruktur observability | **Tidak dibuat** | n/a | Tidak ada backend, database, atau telemetri di MVP (K-09, NFR-08.4). Observabilitas lokal diringkas di `SECURITY_PRIVACY_ERRORS.md` bagian 7 |

## 4. Daftar keputusan Tahap 4

Semua berstatus **T4-USULAN** kecuali dinyatakan lain. Menyetujui dokumen ini berarti menyetujui keputusan yang tidak ditandai sebagai pertanyaan pemilik.

| ID | Keputusan | Alasan singkat | Bergantung pada | Dokumen |
|---|---|---|---|---|
| D-01 | Lapisan kode memakai folder `content/`, `src/`, `scripts/`, `assets-src/`, `app/` dengan aturan batas impor di bagian 5 | Menegakkan L1 sampai L6 (ARCHITECTURE bagian 7) | ADR-0001 | Dokumen ini |
| D-02 | Identifier kode (bidang data, tipe, fungsi) berbahasa Inggris. Dokumen dan komentar berbahasa Indonesia | Konsisten dengan ekosistem alat. Nama di ADR-0002 dipetakan | ADR-0002 | `CONTENT_MODEL.md` |
| D-03 | Konten berupa berkas TypeScript bertipe di `content/`, divalidasi dengan skema saat build. Pustaka skema: **kandidat Zod**, dikonfirmasi sebelum dipasang | Tipe diturunkan dari skema, tanpa duplikasi. Lisensi dan versi diverifikasi (V-19, NFR-07.2) | ADR-0002 | `CONTENT_MODEL.md` |
| D-04 | `output: 'export'`, `trailingSlash: true`, `basePath` dari variabel lingkungan | Satu folder per rute (`index.html`) lebih portabel di host statis (EV-09). Dikonfirmasi saat host dipilih | ADR-0001, ADR-0009 | `ASSETS_AND_BUILD.md` |
| D-05 | Technical Expertise **diturunkan** dari data project, bukan berkas terpisah | Memenuhi FR-07.3 dan FR-07.4 secara struktural | ADR-0002 | `CONTENT_MODEL.md` |
| D-06 | Gambar memakai komponen `Img` sendiri berbasis `<picture>` dan varian yang dibuat skrip build. `next/image` dilarang lewat aturan impor | Tidak bergantung pada perilaku yang belum terverifikasi (EV-10, EV-12) | ADR-0008 | `ASSETS_AND_BUILD.md` |
| D-07 | Font memakai `next/font/local` dengan berkas font dan lisensinya disimpan di repositori. Maksimal 2 keluarga | Tanpa permintaan jaringan saat build dan runtime. Lisensi dapat dilacak | ADR-0008, V-13 | `ASSETS_AND_BUILD.md` |
| D-08 | Berkas CV dan data sensitif **tidak** disimpan di `public/`. Berkas masuk ke build lewat skrip sinkronisasi yang menegakkan gerbang | `public/` otomatis tersalin ke `out/`. Gerbang AC-63 harus deterministik | ADR-0008 | `ASSETS_AND_BUILD.md` |
| D-09 | Varian "bernama" project RSUD **tidak dibuat di repositori** sampai izin terkonfirmasi. Skema dan validator tetap mendukungnya, diuji dengan fixture fiktif | Repositori tidak pernah memuat konten bernama tanpa izin | ADR-0002 | `CONTENT_MODEL.md` |
| D-10 | Daftar istilah sensitif disimpan **lokal dan tidak masuk repositori** | Daftar itu sendiri membocorkan istilah yang dicari | NFR-09.4 | `SECURITY_PRIVACY_ERRORS.md` |
| D-11 | Tiga tingkat motion bernama `full`, `adapted`, `reduced`. Penentuan lewat media query, token bersumber dari CSS custom properties dan dibaca JS | Satu sumber nilai (FR-10.4). Pemusatan tetap rekomendasi (DP-13) | ADR-0005 | `MOTION_SPEC.md` |
| D-12 | Mencegah kilatan: elemen hanya "dipersenjatai" untuk reveal bila **sudah berada di bawah viewport** saat hidrasi, dengan fail-safe CSS murni. Tanpa skrip inline | Menghindari ketergantungan CSP pada skrip inline (V-17) | ADR-0005 butir 10 | `MOTION_SPEC.md` |
| D-13 | Pemecahan teks hero dilakukan **saat build** pada tingkat **kata**, tanpa `aria-hidden` dan tanpa pemecahan per huruf | Aksesibel secara bawaan dan tanpa biaya JS. Per huruf hanya bila S-01 membuktikannya aman | ADR-0004 butir 7 | `MOTION_SPEC.md` |
| D-14 | CSS dan IntersectionObserver dipakai untuk semua efek baseline. GSAP **tidak dipasang** di fase awal dan masuk hanya bila S-01 atau S-03 menunjukkan kebutuhan nyata | Selaras ADR-0004 (GSAP untuk kebutuhan kompleks) dan ADR-0007 | ADR-0004 | `MOTION_SPEC.md` |
| D-15 | Satu tema visual tanpa tombol terang/gelap. Mendukung `forced-colors` | Tidak diminta pemilik (PRD 4.3). Mengurangi permukaan uji | PRD 4.3 | `UI_UX_SPEC.md` |
| D-16 | Menu mobile berbasis `<details>` native dengan peningkatan JS (Escape, tutup saat navigasi). Cadangan: tombol dengan `aria-expanded` bila S-06 gagal | Berfungsi tanpa JS dan tanpa pergeseran saat hidrasi | FR-02.2 | `UI_UX_SPEC.md` |
| D-17 | Alamat email dirender sebagai teks yang terlihat dengan entitas HTML. Tanpa obfuskasi yang menghalangi pembaca layar atau penyalinan | Mitigasi spam lemah tetapi tanpa biaya aksesibilitas | FR-09.4 | `UI_UX_SPEC.md` |
| D-18 | Pemeriksaan otomatis memakai kode keluar 0, 1, dan 2, ID aturan `CHK-*`, dan keluaran JSON | Kontrak yang dapat dipakai orchestrator Tahap 9 | NFR-10.3 | Dokumen ini, bagian 6.3 |
| D-19 | Pengelola paket: **npm**, `package-lock.json` di-commit, instalasi dengan `npm ci` | Terpasang bersama Node. Mengurangi alat di Windows 10 | NFR-07.2, V-02 | Dokumen ini, bagian 7 |
| D-20 | Dua spike tambahan diusulkan: **S-05** (static export, font, gambar, `basePath`, HTML hasil build) dan **S-06** (menu `<details>` dan fokus navigasi dengan Narrator dan TalkBack) | Menutup VERIFIKASI yang menopang keputusan di atas | ARCHITECTURE bagian 16 | Bagian 9 |

## 5. Struktur folder dan tanggung jawab modul

Nama folder adalah usulan. Tahap 5 dapat menggeser file ke fase, bukan mengubah lapisan.

```
creative-developer-portfolio/
├─ app/                          # L6 + L3: rute App Router, metadata, 404
│  ├─ layout.tsx                 # html lang="en", font, skip link, header, footer
│  ├─ page.tsx                   # beranda: bagian berurutan
│  ├─ projects/[slug]/page.tsx   # case study (generateStaticParams, dynamicParams=false)
│  ├─ not-found.tsx              # 404 -> out/404.html
│  ├─ error.tsx, global-error.tsx
│  ├─ robots.txt/route.ts        # force-static (FR-14.3, Should)
│  └─ sitemap.xml/route.ts       # force-static, butuh SITE_URL
├─ content/                      # L1: DATA SAJA
│  ├─ site.ts  profile.ts  contact.ts  cv.ts  build-notes.ts  tech.ts  groups.ts
│  └─ projects/
│     ├─ index.ts                # registri slug -> entri
│     ├─ flash-sale-intelligence.ts
│     └─ hospital-apar-inspection.anonymized.ts
├─ src/
│  ├─ schema/                    # L1/L2: skema, tipe turunan, aturan klaim
│  ├─ lib/content/               # L1: fungsi akses bertipe (satu-satunya pintu konten)
│  ├─ lib/site/                  # SITE_URL, basePath, helper URL
│  ├─ components/
│  │  ├─ ui/                     # Img, ExternalLink, EmailLink, Tag, VisuallyHidden, ...
│  │  ├─ layout/                 # SiteHeader, NavMenu, SkipLink, SiteFooter
│  │  ├─ sections/               # Hero, SelectedProjects, About, Expertise, BuildNotes, Contact
│  │  └─ case-study/             # CaseStudy, Section, ResultsBlock, AiNote, MediaFigure, ProjectPager
│  ├─ islands/                   # L4: Client Components (MenuEnhancer, RouteAnnouncer, MotionRoot)
│  ├─ motion/                    # L5: tier, tokens reader, lifecycle, failsafe, primitives, effects
│  ├─ fonts/                     # berkas font + LICENSES/ (D-07)
│  ├─ styles/                    # tokens.css, base.css, motion.css, forced-colors.css
│  └─ generated/                 # keluaran skrip (gitignored): image-manifest.json
├─ scripts/                      # L2: pemeriksaan dan pipeline aset (Node)
│  ├─ check-content.*  check-repo.*  check-assets.*  check-out.*
│  ├─ build-images.*   sync-public.*
│  └─ config/ (budgets.json, release-rules.json)
├─ assets-src/
│  ├─ images/                    # sumber gambar yang AMAN dipublikasikan (dummy atau milik sendiri)
│  ├─ cv/                        # cv-en.pdf (setelah ada). TIDAK otomatis publik
│  └─ private/                   # GITIGNORED. Bahan asli yang sensitif. Tidak pernah di-commit
├─ public/                       # hanya berkas statis kecil yang aman (favicon, og default)
│  └─ generated/                 # GITIGNORED. Diisi sync-public dan build-images
├─ tests/                        # dibentuk di Tahap 6 (fixtures uji ada di tests/fixtures)
├─ docs/planning/ ...            # dokumen tahap 1 sampai 4
├─ .gitignore  .gitattributes  .editorconfig  .nvmrc(opsional)  package.json  package-lock.json
└─ next.config.ts  tsconfig.json  README.md
```

### 5.1 Tanggung jawab per modul

| Modul | Tanggung jawab | Tidak boleh |
|---|---|---|
| `content/` | Menyimpan data apa adanya | Mengimpor dari `src/components`, `src/motion`, atau `app/`. Berisi logika |
| `src/schema/` | Skema, tipe, aturan klaim, aturan varian RSUD, aturan CV | Mengakses sistem berkas atau jaringan |
| `src/lib/content/` | Membaca `content/`, memvalidasi (mode rilis), memilih varian, menyaring klaim, memilih bahasa, mengembalikan *view* berisi string biasa | Mengembalikan peta bahasa mentah ke komponen |
| `src/components/**` | Presentasi, HTML semantik, tanpa logika animasi | Mengimpor `content/` langsung, `gsap`, atau `next/image`. Menulis durasi atau easing literal |
| `src/islands/` | Interaksi sisi klien yang minimal (peningkatan menu, pengumuman rute, akar motion) | Menyembunyikan konten yang tidak punya fail-safe |
| `src/motion/` | Tingkat, token, siklus hidup, fail-safe, primitif, efek | Mengimpor komponen. Dipanggil dari efek yang tidak terdaftar |
| `scripts/` | Pemeriksaan dan pipeline aset. Berjalan di Node saat pengembangan dan build | Mengubah file di `content/` atau `src/` |
| `assets-src/private/` | Bahan asli sensitif milik pemilik | Masuk Git, masuk `public/`, masuk `out/` |

### 5.2 Aliran dependensi yang diizinkan

```
content/  <--  src/lib/content/  <--  app/, src/components/, src/islands/
                     ^
src/schema/  --------+------------------  scripts/ (check-content memakai schema + lib)
src/motion/  <--  src/islands/, app/ (hanya lewat MotionRoot)
src/components/ui/  <--  components/layout, sections, case-study
```

Panah menunjukkan "dipakai oleh". Tidak ada siklus. `src/motion/` tidak mengenal komponen. Komponen hanya menandai elemen dengan atribut data (`data-reveal`, `data-stagger`), bukan memanggil efek.

### 5.3 Aturan batas yang harus dapat ditegakkan otomatis (netral alat, dipasang di P0)

| ID | Aturan | Menegakkan |
|---|---|---|
| CHK-ARCH-01 | Berkas di `src/components/**` dan `app/**` tidak mengimpor `content/**` | D-01, ADR-0002 butir 7 |
| CHK-ARCH-02 | `gsap` dan plugin hanya diimpor dari `src/motion/**` | ADR-0004 butir 4 |
| CHK-ARCH-03 | Tidak ada impor `next/image` | D-06 |
| CHK-ARCH-04 | Tidak ada `dangerouslySetInnerHTML` | `SECURITY_PRIVACY_ERRORS.md` bagian 4 |
| CHK-ARCH-05 | Tidak ada nilai durasi atau easing literal (`ms`, `s`, `cubic-bezier(`, `ease`) di komponen atau CSS selain `src/styles/tokens.css` dan `motion.css`, kecuali ada komentar pengecualian `motion-exempt: <alasan>` | FR-10.4, AC-56 |
| CHK-ARCH-06 | Tidak ada URL `http(s)://` ke host non-lokal pada sumber `src/` dan `app/` kecuali daftar putih (tautan keluar berasal dari `content/`) | NFR-08.3 |
| CHK-ARCH-07 | Tidak ada pemakaian `localStorage`, `sessionStorage`, `document.cookie` | NFR-08.4 |

## 6. Kontrak antarmodul

Kontrak ini mencegah konflik antarfase. Mengubah kontrak setelah fase yang memakainya selesai memerlukan **catatan perubahan kontrak** di laporan fase (apa yang berubah, modul yang terdampak, tes yang diulang).

### 6.1 Akses konten (stabilitas: tinggi, dibekukan sebelum komponen dibuat)

```ts
// src/lib/content/index.ts  (tanda tangan final di Fase 1; isi tipe di CONTENT_MODEL.md)
export type Locale = string;                       // 'en' sekarang
export function getSite(locale?: Locale): SiteView;
export function getProfile(locale?: Locale): ProfileView;
export function listProjects(locale?: Locale): ProjectCardView[];     // terurut `order`
export function listProjectSlugs(): string[];                          // untuk generateStaticParams
export function getProject(slug: string, locale?: Locale): ProjectView | undefined;
export function getExpertiseGroups(locale?: Locale): ExpertiseGroupView[]; // diturunkan (D-05)
export function getContactLinks(locale?: Locale): ContactLinkView[];
export function getCv(locale?: Locale): CvView | null;                // null bila gerbang belum lolos
export function getBuildNotes(locale?: Locale): BuildNotesView | null;
```

Janji kontrak: *view* hanya berisi string dan angka biasa. Klaim yang tidak terkonfirmasi tidak muncul. Varian project sudah dipilih. Tautan tanpa tujuan tidak ada. `getCv()` mengembalikan `null` bila CV belum siap atau belum diperiksa.

### 6.2 Motion (stabilitas: sedang, dibekukan setelah S-01)

```ts
// src/motion/index.ts
export type Tier = 'full' | 'adapted' | 'reduced';
export function getTier(): Tier;                                   // sinkron, aman dipanggil di klien
export function onTierChange(cb: (t: Tier) => void): () => void;  // mengembalikan fungsi pelepas
export type EffectSetup = (el: HTMLElement, ctx: EffectContext) => void | (() => void);
export function mountEffect(id: EffectId, el: HTMLElement, setup: EffectSetup): () => void;
```

Detail tipe dan aturan siklus hidup di `MOTION_SPEC.md` bagian 7. Komponen **tidak** memanggil `mountEffect`. Hanya `src/islands/MotionRoot` dan modul efek yang memanggilnya.

### 6.3 Pemeriksaan otomatis (stabilitas: tinggi, dipakai orchestrator Tahap 9)

| Aspek | Kontrak |
|---|---|
| Kode keluar | `0` lolos. `1` ada temuan yang menggagalkan. `2` pemeriksaan **tidak dapat dijalankan** (konfigurasi hilang, berkas tidak terbaca, daftar istilah lokal tidak ada pada mode rilis). Kode `2` diperlakukan sebagai gagal, bukan lolos |
| Mode | `RELEASE=1` memperketat aturan (bagian 7 `ASSETS_AND_BUILD.md`). Tanpa itu, mode pengembangan |
| Keluaran manusia | Satu baris per temuan: `[ERROR|WARN|INFO] CHK-XXX-NN path:baris pesan` |
| Keluaran mesin | `reports/<nama-pemeriksaan>.json` (gitignored): `{ check, mode, startedAt, exitCode, findings: [{id, severity, file, line, message}] }`. Tidak memuat nilai sensitif yang ditemukan, hanya lokasinya |
| Determinisme | Tidak memakai jaringan. Hasil sama untuk masukan sama |
| Pernyataan lolos | Skrip hanya mengeluarkan kode keluar dan temuan. **Tidak** menulis kata "PASS" sebagai klaim kualitas. Bukti kelulusan fase dikumpulkan di Tahap 6 |

### 6.4 Kepemilikan berkas (pedoman untuk Tahap 5)

| Area | Fase pemilik (perkiraan, Tahap 5 menetapkan) | Fase lain boleh mengubah? |
|---|---|---|
| `src/schema/`, `src/lib/content/`, `content/` kerangka | Fondasi konten | Hanya lewat catatan perubahan kontrak |
| `src/styles/tokens.css`, `src/styles/motion.css` | Fondasi gaya dan motion | Nilai token boleh dikalibrasi setelah spike. Nama token tidak |
| `src/motion/` | Sistem motion | Efek baru ditambahkan di `src/motion/effects/` tanpa mengubah inti |
| `scripts/` | Pipeline dan pemeriksaan | ID aturan `CHK-*` tidak dihapus. Ditambah saja |
| `app/` dan `src/components/` | Fase bagian halaman | Satu bagian per fase. Komponen `ui/` bersama dibekukan lebih awal |

## 7. Konfigurasi dasar (maksud, bukan perintah)

| Item | Keputusan | Status |
|---|---|---|
| TypeScript | Mode `strict` aktif. `noUncheckedIndexedAccess` aktif. Alias `@/` ke `src/` | T4-USULAN |
| Versi Node | Dipin lewat `engines` dan berkas versi setelah V-02 dan S-04. Tidak ditebak sekarang | VERIFIKASI |
| Pengelola paket | npm. `package-lock.json` di-commit. `npm ci` untuk instalasi bersih (D-19) | T4-USULAN |
| Akhir baris | `.gitattributes` memaksa `eol=lf` untuk berkas teks sumber agar skrip dan hash konsisten di Windows | T4-USULAN |
| Variabel lingkungan | `SITE_URL` (wajib pada rilis), `BASE_PATH` (opsional, default kosong), `RELEASE` (0 atau 1), `NEXT_PUBLIC_FAULT_INJECTION` (hanya uji, harus tidak ada di rilis), `FORBIDDEN_TERMS_FILE` (jalur lokal, wajib pada rilis). Tidak ada rahasia di variabel ini. Berkas `.env*` masuk `.gitignore` | T4-USULAN |
| Dependency awal yang diusulkan | `next`, `react`, `react-dom`, `typescript`, `tailwindcss` (+ plugin PostCSS atau Vite sesuai panduan resmi saat pemasangan), pustaka skema (kandidat Zod). Pengembangan: alat lint, pemroses gambar untuk skrip (kandidat `sharp`), penjalan skrip TypeScript (kandidat `tsx`). **Setiap paket dicatat alasan, versi, lisensi sebelum dipasang** (NFR-07.1, NFR-07.2) | T4-USULAN. Lisensi dan versi: VERIFIKASI (V-19, V-20) |
| Tidak dipasang di awal | `gsap`, Lenis, pustaka animasi lain, pustaka UI, analytics (D-14, ADR-0007) | ADR-USULAN |

## 8. Kompatibilitas browser (kebijakan)

Dasar: Next.js mendukung Chrome 111+, Edge 111+, Firefox 111+, Safari 16.4+ (EV-02). Tailwind v4 dirancang untuk Chrome 111, Safari 16.4, Firefox 128 (EV-05). Maka baseline fungsional proyek adalah **Chrome 111+** untuk target uji (K-04), dan Safari 16.4+ serta Firefox 128+ sebagai batas yang *diumumkan*, **tanpa klaim uji** (K-10).

Kebijakan fitur:
1. Fitur CSS atau API yang lebih baru dari baseline hanya dipakai di dalam `@supports` atau deteksi fitur, dengan perilaku cadangan yang tetap terbaca.
2. Setiap fitur seperti itu dicatat di register kompatibilitas (`ASSETS_AND_BUILD.md` bagian 8) dengan versi minimum **dari sumber resmi yang dibaca**.
3. Versi Chrome di POCO M4 Pro dan laptop belum diketahui (V-01, V-02). Keduanya harus ≥ 111 agar baseline berlaku. Bila tidak, ADR-0003 ditinjau.

## 9. Hal yang belum selesai

### 9.1 Pertanyaan untuk pemilik (KEPUTUSAN-PEMILIK)

| ID | Pertanyaan | Perilaku sampai dijawab | Dibutuhkan |
|---|---|---|---|
| **QT4-01** | Apakah repositori ini publik? Bila ya, apakah diubah menjadi privat sekarang, atau dokumen perencanaan dibersihkan dari nama institusi dan riwayat commit ditinjau, atau risikonya diterima? (C-01) | Tidak ada perubahan dokumen. Tidak ada konten sensitif baru ditambahkan ke repositori | **Sebelum Tahap 5 menambah berkas apa pun** |
| **QT4-02** | Setujukah tafsir pada C-03 (keyframe CSS di atas lipatan boleh untuk elemen non-LCP, murni CSS, tetap terlihat bila CSS gagal)? | Hero memakai varian aman HR-3 | Sebelum spike S-01 |
| **QT4-03** | Konsep visual mana (A, B, C di `UI_UX_SPEC.md` bagian 3) yang akan diprototipekan di S-01, dan referensi tambahan apa yang menurut Anda mewakili kualitas yang diinginkan (P-05)? Saya tidak dapat menilai gerak dan tampilan situs referensi dari teks | S-01 memakai konsep yang Anda pilih. Tanpa pilihan, S-01 tidak dapat mulai | Sebelum S-01 |
| **QT4-04** | Headline mana (DP-07, brief Lampiran A)? | Opsi 1 dipakai sebagai teks pengganti uji | Sebelum konten hero final |
| **QT4-05** | Seberapa umum anonimisasi project RSUD? Misalnya apakah boleh menulis "rumah sakit daerah", atau hanya "rumah sakit"? Deskripsi yang terlalu spesifik dapat mengidentifikasi institusi | Teks anonim memakai "a hospital" tanpa lokasi atau jenis kepemilikan | Penyusunan konten |
| **QT4-06** | Konfirmasi keputusan lama yang bergantung pada Tahap 4 (ARCHITECTURE bagian 21 butir 7): DP-01 (transisi halaman), DP-02 (WebGL), DP-04 (repositori publik), DP-08 (analytics). Bawaan di desain ini: T0 wajib dan T1 Should; WebGL mati; repositori mengikuti QT4-01; tanpa analytics | Bawaan berlaku | Sebelum Tahap 5 selesai |

Pertanyaan lama yang tetap terbuka dan tidak menghambat Tahap 4: Q-05 (foto, bawaan tanpa foto), Q-06 sampai Q-08 (kontak, lokasi, ketersediaan), Q-13 (pembekuan versi Flash Sale Intelligence), Q-19 (hosting), Q-20 (peran AI project RSUD).

### 9.2 Item verifikasi baru

| ID | Yang diverifikasi | Cara | Kapan |
|---|---|---|---|
| V-17 | Apakah HTML hasil static export memuat skrip inline, dan bagaimana ini memengaruhi CSP (hash atau `unsafe-inline`) | Periksa `out/*.html` setelah build prototipe. Baca dokumentasi resmi CSP Next.js | S-05 |
| V-18 | Alat untuk memeriksa metadata gambar dan PDF yang berjalan di Windows 10, beserta lisensinya | Dari sumber resmi alat, lalu uji pada berkas contoh | Tahap 5 awal |
| V-19 | Lisensi dan versi pustaka skema (kandidat Zod) | Sumber resmi | Sebelum dipasang |
| V-20 | Lisensi dan versi pemroses gambar (kandidat `sharp`) dan penjalan skrip (kandidat `tsx`), serta kemampuan berjalan di Windows 10 dengan Node yang tersedia | Sumber resmi, lalu S-04 dan S-05 | Sebelum dipasang |
| V-21 | Lisensi setiap berkas font yang dipilih (bukan hanya nama keluarga font) | Baca berkas lisensi yang menyertai font | Saat font dipilih |
| V-22 | Sintaks dan perilaku konfigurasi tema Tailwind v4 untuk memetakan token CSS | Dokumentasi resmi Tailwind | Fase 1 |
| V-23 | Apakah `basePath` bekerja untuk aset non-Next (`<img>`, `<picture>`) pada static export | Build prototipe dengan `BASE_PATH` | S-05 |
| V-24 | Perilaku fokus dan pengumuman otomatis App Router saat navigasi klien, dan perilaku `<ViewTransition>` terhadap fokus | Dokumentasi resmi, lalu uji keyboard dan pembaca layar | S-02 dan S-06 |

### 9.3 Risiko baru untuk Tahap 5

R-01 sampai R-31 tetap berlaku.

| ID | Risiko | Tingkat | Mitigasi |
|---|---|---|---|
| R-32 | Repositori publik memuat nama institusi dan email penulis commit (C-01) | **Tinggi** | QT4-01. Prosedur audit di `SECURITY_PRIVACY_ERRORS.md` bagian 2 |
| R-33 | CSP ketat bertabrakan dengan skrip inline hasil export (V-17), dan host statis tertentu tidak dapat mengatur header | Sedang | Dua tingkat CSP dan opsi `<meta>` (`SECURITY_PRIVACY_ERRORS.md` bagian 4). Diputuskan setelah S-05 dan pemilihan host |
| R-34 | Pemuatan font setelah animasi hero mulai menggeser lebar kata, memicu CLS atau potongan teks | Sedang | Font cadangan dengan metrik dekat, uji di S-01. Varian HR-3 sebagai cadangan |
| R-35 | `<details>` sebagai menu dibacakan kurang jelas oleh Narrator atau TalkBack | Sedang | S-06. Cadangan tombol dengan `aria-expanded` (D-16) |
| R-36 | `next/font/local`, `basePath`, atau route handler statis berperilaku berbeda dari harapan pada static export (EV-11 tidak membahas static export) | Sedang | S-05 sebelum fase hero. Cadangan: `@font-face` manual di CSS |
| R-37 | Pustaka skema, pemroses gambar, atau penjalan skrip tidak berjalan baik di Windows 10 atau Node yang tersedia (R-25) | Sedang | S-04 dan S-05. Cadangan: validator tulisan sendiri dan skrip `.mjs` murni |
| R-38 | Anggaran ukuran tetap kosong sampai akhir, sehingga AC-31 tidak pernah teruji | Sedang | Build rilis gagal bila nilai `null` (C-04). Tugas pengisian dijadwalkan di Tahap 5 setelah S-01 dan S-03 |

## 10. Keterlacakan ke ADR

| ADR | Dipenuhi atau diperinci oleh | Perubahan terhadap ADR |
|---|---|---|
| ADR-0001 | Bagian 5, D-04, `ASSETS_AND_BUILD.md` bagian 6 | Tidak ada. Menambah `trailingSlash: true` dan `basePath` (D-04) |
| ADR-0002 | `CONTENT_MODEL.md`, D-02, D-03, D-05, D-09 | Tidak ada pembatalan. D-05 (expertise diturunkan) adalah perincian dari "expertise terhubung ke data project" |
| ADR-0003 | `UI_UX_SPEC.md` bagian 5, D-15 | Tidak ada |
| ADR-0004 | `MOTION_SPEC.md`, D-13, D-14 | D-14 menandai bahwa GSAP belum dibutuhkan pada baseline. Konsisten dengan butir 2 dan 5 ADR-0004 |
| ADR-0005 | `MOTION_SPEC.md` bagian 2 sampai 4, D-11, D-12 | Butir 10 (belum diputuskan) dijawab sebagai usulan D-12. C-03 menunggu QT4-02 |
| ADR-0006 | `MOTION_SPEC.md` bagian 8 | Tidak ada |
| ADR-0007 | D-14, `MOTION_SPEC.md` bagian 6 | Tidak ada |
| ADR-0008 | `ASSETS_AND_BUILD.md`, `SECURITY_PRIVACY_ERRORS.md`, D-06 sampai D-10 | Tidak ada. V-12 dan V-13 dijawab sebagian (EV-10, EV-11) |
| ADR-0009 | `ASSETS_AND_BUILD.md` bagian 9, D-04 | Tidak ada. Host tetap belum dipilih |

Semua ADR **tetap berstatus Diusulkan** sampai Anda menyetujuinya secara eksplisit.

## 11. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 13 Oktober 2026 | Draf awal Tahap 4 |
