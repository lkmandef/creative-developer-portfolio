# ARCHITECTURE — Creative Developer Portfolio Website

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/ARCHITECTURE.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 12 Oktober 2026 |
| Tahap workflow | Tahap 3 dari 10 (Arsitektur dan ADR) |
| Status | **Menunggu review dan persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 4)** |
| Dasar | `PROJECT_BRIEF.md` v0.2, `PRD.md` v0.3 |
| ADR terkait | `docs/planning/adr/ADR-0001` sampai `ADR-0009` (semua berstatus **Diusulkan**) |

## Cara membaca dokumen ini

| Penanda | Arti |
|---|---|
| **EV-xx** | Evidensi yang dibaca dari sumber resmi pada 12 Oktober 2026 (bagian 3). Hanya fakta di tabel itu yang boleh dianggap terverifikasi |
| **ADR-xxxx** | Keputusan arsitektur. Semua berstatus **Diusulkan** sampai pemilik menyetujui |
| **OQ-xx** | Pertanyaan terbuka milik Tahap 3 |
| **V-xx** | Item verifikasi. V-01 sampai V-11 berasal dari brief dan PRD. V-12 sampai V-16 baru di dokumen ini |
| **R-xx** | Risiko. R-25 sampai R-31 baru di dokumen ini |
| **S-xx** | Spike (prototipe kecil untuk menjawab satu pertanyaan teknis), bagian 16 |
| **[SEMENTARA]** | Angka yang sengaja belum dikunci (A-05, disetujui pemilik pada DP-13) |
| K-xx, A-xx, FR-xx, NFR-xx, AC-xx, DP-xx | Rujukan ke brief dan PRD |

Pemilik telah memutuskan: A-05 dan A-07 disetujui (target performa final setelah prototipe diukur, hero diuji lebih awal). A-06 (pemusatan sistem motion) **bukan kewajiban**, hanya rekomendasi implementasi (DP-13).

## 1. Tujuan dan batas dokumen

Dokumen ini menetapkan **bagaimana** sistem disusun agar PRD dapat dipenuhi dan dibuktikan: stack usulan, komponen, aliran data, strategi rendering, arsitektur motion, aset, keamanan, build dan rilis, serta deployment.

Dokumen ini **tidak** menetapkan: struktur folder terperinci, model data final, spesifikasi UI/UX, palet dan font, token motion bernilai, angka anggaran performa final, versi dependency yang dikunci, perintah CLI Antigravity, atau perintah uji. Itu milik Tahap 4 sampai 6 dan 8.

## 2. Pendorong arsitektur

| Pendorong | Sumber | Implikasi arsitektur |
|---|---|---|
| Tanpa backend, database, CMS, auth, formulir | K-09, FR-09.2 | Situs statis. Konten lokal. Tidak ada kode server saat runtime |
| Elemen LCP tidak bergantung JS atau animasi | FR-03.4, NFR-01.4 | HTML statis berisi konten terlihat. Efek menjadi peningkatan, bukan prasyarat |
| Konten dan navigasi tersedia saat JS gagal | NFR-06.1, EC-01, EC-02 | Render per rute ke HTML. Tautan bekerja sebagai tautan biasa |
| DOM/CSS/SVG wajib, GSAP untuk kebutuhan kompleks | K-03 | CSS dulu, GSAP + ScrollTrigger hanya bila perlu (ADR-0004) |
| Scroll native adalah baseline | K-03, FR-10.5 | Tidak ada pustaka scroll hijacking secara bawaan (ADR-0007) |
| Tiga tingkat motion | K-03, FR-10.1 | Resolver tingkat berbasis media query dan token bersama (ADR-0005) |
| Aturan penengah efek | K-12, FR-10.10 | Setiap efek memiliki tangga degradasi (bagian 10) |
| Safari dan Firefox berfungsi sejauh didukung | K-10, NFR-04.2 | Baseline browser ditetapkan dari evidensi (EV-02, EV-05) |
| Tanpa pihak ketiga saat runtime | NFR-08.3, NFR-08.4 | Font dan aset di-host sendiri. Tanpa pelacak (ADR-0008) |
| Dipelihara sendiri, dependency terbatas | T-04, NFR-07.1 | Dependency minimal dengan alasan tercatat |
| Kapasitas 6 sampai 10 jam per minggu | K-06 | Modul kecil, spike kecil di awal, pemotongan scope sesuai PRD 4.4 |
| Gerbang placeholder dan izin | FR-12.5, NFR-09.4, brief 17 | Pemeriksaan otomatis pada sumber **dan hasil build** (bagian 15) |
| Pengembangan penuh di Windows 10 | NFR-07.4 | Toolchain harus berjalan di Windows 10 (R-25, V-02) |

## 3. Evidensi terverifikasi

Semua dibaca dari halaman resmi pada **12 Oktober 2026**. Sebagian dibaca lewat alat fetch yang meringkas halaman, sehingga teks lengkap tetap perlu dikonfirmasi bila disebut di kolom "Catatan".

| ID | Fakta | Sumber | Catatan |
|---|---|---|---|
| EV-01 | Dokumentasi Next.js yang dibaca menampilkan versi **16.4.0**. Node.js minimum **20.9**. Sistem operasi didukung: macOS, Windows (termasuk WSL), Linux. TypeScript minimum 5.1.0. Pengaturan bawaan `create-next-app`: TypeScript, Tailwind CSS, ESLint, App Router, Turbopack. Sejak Next.js 16, `next build` tidak menjalankan linter otomatis. App Router memakai rilis canary React bawaan | https://nextjs.org/docs/app/getting-started/installation | Halaman diperbarui 2 Okt 2026. Versi yang dipin ditetapkan saat implementasi |
| EV-02 | Browser yang didukung Next.js: Chrome 111+, Edge 111+, Firefox 111+, Safari 16.4+ | https://nextjs.org/docs/architecture/supported-browsers | |
| EV-03 | Static export lewat `output: 'export'` menghasilkan satu HTML per rute di folder `out`. Didukung: Server Components (dijalankan saat build), Client Components, Route Handler `GET` dengan `force-static`. **Tidak didukung**: dynamic routes tanpa `generateStaticParams()` atau dengan `dynamicParams: true`, cookies, rewrites, redirects, **headers**, proxy, ISR, **image optimization dengan loader bawaan**, Draft Mode, Server Actions, intercepting routes. Image optimization dapat dipakai dengan loader kustom. Tersedia template deploy ke GitHub Pages | https://nextjs.org/docs/app/guides/static-exports | Halaman versi 16.4.0, diperbarui 9 Agu 2026 |
| EV-04 | Transisi tampilan di App Router berjalan tanpa konfigurasi melalui komponen `<ViewTransition>` React (disebut tersedia di React 19.3). Memakai fitur View Transitions API yang lebih baru (transition types, `view-transition-class`) yang tersedia di **Chromium 125+** dan Safari serta Firefox terbaru, dan perilakunya bisa berbeda di Safari. Tanpa dukungan browser, aplikasi tetap berjalan normal tanpa animasi. Navigasi rute adalah transition sehingga animasi aktif otomatis. Transisi morph hanya terbentuk bila halaman tujuan dirender pada commit yang sama dengan navigasi (halaman yang sudah di-prefetch). Overlay `::view-transition` menangkap event pointer sehingga klik hilang kecuali `pointer-events: none` dipasang. Elemen bernama dilewati hit-testing selama transisi. Navigasi back/forward browser tidak membawa transition type. Panduan menyertakan contoh CSS reduced motion | https://nextjs.org/docs/app/guides/view-transitions | Halaman diperbarui 10 Sep 2026 |
| EV-05 | Tailwind CSS v4 dirancang dan diuji untuk Chrome 111, Safari 16.4, dan Firefox 128 sebagai versi minimum. Utilitas dengan dukungan browser terbatas bersifat opsional | https://tailwindcss.com/docs/compatibility | Batas Firefox 128 lebih tinggi daripada batas Next.js (EV-02) |
| EV-06 | GSAP memakai lisensi "no charge" GreenSock. README menyatakan seluruh toolset GSAP gratis, termasuk penggunaan komersial dan plugin yang dulu khusus anggota (mis. SplitText). Versi pada tautan CDN README: 3.15. Lisensi melarang penggunaan GSAP di alat pembuat animasi visual tanpa kode yang bersaing dengan Webflow tanpa izin tertulis. Menerapkan GSAP di situs web diizinkan | https://cdn.jsdelivr.net/npm/gsap@3.15.0/README.md dan https://gsap.com/standard-license | ScrollTrigger tidak disebut namanya di halaman lisensi yang dibaca. Dianggap tercakup sebagai bagian toolset, **belum terkonfirmasi tertulis** (V-05, R-30) |
| EV-07 | Dokumen `BUILDING.md` cabang `main` Node.js mencantumkan Windows >= 10 / Server 2016 sebagai Tier 1 (x64). Dokumen itu juga menyatakan Node.js tidak mendukung versi platform yang dukungan vendornya sudah berakhir | https://github.com/nodejs/node/blob/main/BUILDING.md | Berlaku untuk cabang `main`, bukan jaminan untuk versi LTS tertentu |
| EV-08 | Judul halaman dukungan Microsoft menyatakan dukungan Windows 10 berakhir pada 14 Oktober 2025 | https://support.microsoft.com/kb/5056949 | Hanya dari judul hasil pencarian. Halaman penuh belum dibuka |

**Yang belum diverifikasi di sesi ini** (jangan dianggap fakta): perilaku `images.unoptimized` pada static export, mekanisme hosting font lokal pada static export, dukungan lintas-browser untuk CSS scroll-driven animation, API GSAP spesifik (`useGSAP`, `matchMedia`, SplitText dan aksesibilitasnya), perilaku LCP untuk teks yang beranimasi CSS, serta harga dan batas hosting (V-08).

## 4. Ringkasan keputusan (semua Diusulkan)

| ADR | Keputusan | Rujukan PRD | Hal yang membatalkan |
|---|---|---|---|
| ADR-0001 | Next.js App Router, TypeScript, **static export** | K-09, FR-01, NFR-06 | Spike S-01 sampai S-04 gagal, atau host membutuhkan fitur server |
| ADR-0002 | Konten sebagai data lokal bertipe, divalidasi saat build, siap multibahasa | FR-12, NFR-09 | Pemilik membutuhkan editor non-teknis (A-03) |
| ADR-0003 | Tailwind CSS v4 dengan token CSS custom properties | T-04, FR-10.4 | Uji Safari/Firefox menunjukkan celah (K-10) |
| ADR-0004 | CSS dulu, GSAP + ScrollTrigger untuk kebutuhan kompleks, dimuat malas | K-03, FR-10 | Lisensi atau ukuran bundle tidak dapat diterima (V-05) |
| ADR-0005 | Tiga tingkat motion lewat media query dan token bersama. Pemusatan hanya rekomendasi | FR-10.1, DP-13 | Efek tidak dapat dipetakan ke tiga tingkat |
| ADR-0006 | Transisi halaman bertingkat T0/T1/T2 dengan `ViewTransition`, diputuskan lewat spike S-02 | FR-11, DP-01 | Spike S-02 gagal: T0 |
| ADR-0007 | Lenis, video, WebGL, kursor kustom, eksperimen: **mati secara bawaan**, masuk hanya dengan kriteria | PRD 4.2, FR-15 | Bukti kebutuhan nyata |
| ADR-0008 | Aset di-host sendiri, tanpa pihak ketiga saat runtime, tanpa analytics | NFR-08, FR-09.6 | Pemilik memutuskan memakai analytics (DP-08) |
| ADR-0009 | Hosting statis. Pemilihan host ditunda ke menjelang rilis dengan kriteria | Q-19, NFR-08.6 | Host tidak memenuhi kriteria |

## 5. Konteks sistem

```
 Pemilik --mengedit--> content/ (data) + public/ (aset, CV)
                              |
                              v
        [Pemeriksaan statis] -> [Validasi konten] -> [next build, output: export]
         (tipe, lint)           (skema, klaim,          |
                                 placeholder, tautan)   v
                                                   out/ (HTML, CSS, JS, aset)
                                                        |
                              [Pemeriksaan pasca-build: placeholder di out/,
                               URL pihak ketiga, metadata aset, anggaran]
                                                        |
                                              [Host statis (ADR-0009)]
                                                        |
                              Pengunjung: Chrome desktop/Android (target uji),
                                          Firefox, Safari (baseline, belum teruji)
```

Tidak ada server aplikasi, database, atau layanan pihak ketiga saat runtime.

## 6. Stack usulan

Versi dikunci (dipin) saat implementasi dari sumber resmi. Angka versi di bawah hanyalah acuan dari dokumentasi yang dibaca.

| Lapisan | Pilihan | Alasan singkat | Status | ADR |
|---|---|---|---|---|
| Bahasa | TypeScript (>= 5.1.0 per EV-01) | Pilihan pemilik. Menjaga model data dan komponen tetap konsisten | Usulan | 0001 |
| Runtime pengembangan | Node.js >= 20.9 (EV-01), versi LTS yang berjalan di laptop pemilik | Syarat Next.js. Versi laptop belum diketahui (V-02) | Usulan, bersyarat V-02 | 0001 |
| Framework | Next.js App Router, `output: 'export'` (acuan dokumentasi 16.4.0) | Pilihan pemilik. Satu HTML per rute, tanpa server | Usulan | 0001 |
| UI | React (dibawa App Router) | Syarat framework | Usulan | 0001 |
| Styling | Tailwind CSS v4 + CSS custom properties sebagai token | Pilihan pemilik dan bawaan `create-next-app` (EV-01). Batas browser pada EV-05 | Usulan | 0003 |
| Animasi sederhana | CSS transition dan keyframes | Murah, tanpa dependency | Usulan | 0004 |
| Animasi kompleks | GSAP + ScrollTrigger, dimuat malas | Sesuai K-03. Lisensi EV-06 | Usulan, bersyarat V-05 | 0004 |
| Transisi halaman | React `<ViewTransition>` (T1), fallback tanpa animasi (T0) | Bawaan App Router tanpa pustaka tambahan (EV-04) | Usulan, bersyarat S-02 | 0006 |
| Konten | Data lokal bertipe + validasi build | K-09, FR-12 | Usulan | 0002 |
| Lint dan tipe | ESLint atau Biome, pemeriksaan tipe TypeScript (EV-01 mencantumkan keduanya) | Dipilih di Tahap 6 | Ditunda | |
| Pengujian | Ditetapkan di Tahap 6 | | Ditunda | |
| Hosting | Host statis, dipilih menjelang rilis | Q-19, V-08 | Ditunda | 0009 |

**Tidak dipakai di MVP:** backend, database, CMS, auth, formulir kontak, analytics, font atau embed pihak ketiga, suara, Lenis, video, WebGL/3D, kursor kustom (ADR-0007, ADR-0008).

## 7. Komponen dan batas modul

Pembagian logis (nama folder dan berkas ditetapkan di Tahap 4).

| Lapisan | Tanggung jawab | Berjalan di | Aturan batas |
|---|---|---|---|
| L1 Konten | Data project, profil, expertise, kontak, CV, aset gambar | Saat build | Satu-satunya sumber isi. Tidak ada teks produk tertulis tetap di komponen (FR-04.1, FR-12.1) |
| L2 Pemeriksaan | Validasi skema, klaim, placeholder, tautan, metadata aset, anggaran | Pengembangan dan build | Dapat menggagalkan build rilis (FR-12.5, AC-16) |
| L3 Presentasi | Layout, bagian halaman, template case study, metadata | Server Components, dirender ke HTML saat build | Tidak memuat logika animasi |
| L4 Pulau interaksi | Menu mobile, pembungkus motion, reveal hero, efek scroll, interaksi project | Client Components | Dibatasi pada interaksi. Konten tetap ada di HTML L3 |
| L5 Lapisan motion | Token, resolver tingkat, primitif (reveal, stagger, parallax), siklus hidup, gaya transisi | Klien | GSAP hanya diimpor di sini. Semua efek lewat primitif agar pembersihan otomatis (FR-10.7) |
| L6 Konfigurasi | Konfigurasi Next.js, metadata, font, halaman 404, peta situs | Build | Sesuai keterbatasan static export (EV-03) |

Aturan lintas-lapisan:
1. Komponen tidak menulis durasi atau easing literal, hanya token (FR-10.4, AC-56).
2. Akses konten lewat fungsi akses bertipe, bukan impor berkas data langsung di komponen.
3. Satu efek tidak boleh memuat dependency baru tanpa ADR atau catatan alasan (NFR-07.1).
4. Eksperimen opsional hidup di modul terpisah yang dapat dilepas (FR-15.3, ADR-0007).

## 8. Aliran data

**Saat build**
1. Data konten dibaca, divalidasi terhadap skema, dan klaim diperiksa penanda verifikasinya (FR-12.4, FR-12.6).
2. Pemeriksaan placeholder dan tautan mati dijalankan (FR-12.5).
3. `next build` merender setiap rute ke HTML statis. Rute case study dibuat dari daftar slug (EV-03: butuh `generateStaticParams()`).
4. Pemeriksaan pasca-build memindai `out/` (bagian 15).

**Saat runtime di browser**
1. Browser menerima HTML yang sudah berisi konten lengkap dan dapat dibaca.
2. Pulau interaksi dihidrasi. Resolver tingkat motion membaca media query dan memilih tingkat.
3. Efek dipasang lewat primitif. Bila inisialisasi gagal atau melewati batas waktu, elemen yang disembunyikan untuk animasi ditampilkan (bagian 10).
4. Navigasi antarrute berjalan di sisi klien. Transisi mengikuti ADR-0006. Efek halaman lama dibersihkan.
5. Perubahan preferensi reduced motion atau ukuran viewport memicu penyesuaian tingkat (EC-04, EC-25).

Tidak ada permintaan data ke server saat runtime.

## 9. Rendering dan perutean

| Rute | Pembuatan | Catatan |
|---|---|---|
| `/` | Statis | Bagian berurutan: Hero, Projects, About, Expertise, How I Built This (ringkas, DP-12), Contact. Anchor bekerja tanpa JS (FR-02.4) |
| `/projects/[slug]` | Statis lewat `generateStaticParams()` | `dynamicParams` tidak boleh `true` pada static export (EV-03). Slug tak dikenal menghasilkan 404 |
| 404 | Statis | Static export menghasilkan `404.html` (contoh di EV-03). Cara host menyajikannya dicek di ADR-0009 |
| `robots`, peta situs | Route Handler `GET` dengan `force-static` | Didukung (EV-03). Bagian FR-14.3 (Should) |

- Pengaturan `trailingSlash` diselaraskan dengan host (ADR-0009) dan diputuskan di Tahap 4.
- Metadata (judul, deskripsi, pratinjau tautan) dibuat saat build per rute (FR-14).
- Tanpa redirects, rewrites, atau headers dari `next.config` karena tidak didukung pada static export (EV-03). Kebutuhan itu dipenuhi oleh host (ADR-0009).

## 10. Arsitektur motion

Pilihan rinci ada di ADR-0004 sampai ADR-0006. Ringkasan struktur:

**Tingkat dan resolver (FR-10.1).** Tiga tingkat: desktop lengkap, mobile disesuaikan, reduced motion. Penentuan tingkat memakai kemampuan yang dapat diperiksa (preferensi reduced motion, kemampuan pointer dan hover, kelas lebar viewport), **bukan** deteksi merek atau jenis perangkat. Pemusatan penentuan tingkat dalam satu modul adalah **rekomendasi implementasi**, bukan kewajiban (DP-13). Yang wajib: token bersama (FR-10.4) dan perilaku tiga tingkat teruji (AC-24).

**Aturan keadaan awal (melindungi LCP dan akses).**
1. HTML awal berisi seluruh konten dalam keadaan terlihat.
2. Elemen kandidat LCP hero tidak boleh bergantung pada JS untuk muncul (FR-03.4). Elemen di bawah lipatan boleh disembunyikan untuk animasi hanya setelah resolver menentukan tingkat bukan reduced motion.
3. **Fail-safe wajib:** bila skrip animasi gagal dimuat, gagal diinisialisasi, atau melewati batas waktu, semua elemen yang disembunyikan ditampilkan (EC-01, EC-02, FR-10.8).
4. Cara mencegah kilatan saat elemen disembunyikan setelah hidrasi belum diputuskan. Opsi (mis. kelas awal lewat skrip inline, atau animasi CSS murni untuk hero) memiliki konsekuensi pada kebijakan keamanan konten dan LCP. Diputuskan di Tahap 4 setelah spike S-01.

**Siklus hidup (FR-10.7).** Setiap efek dibuat dan dilepas lewat primitif. Dibersihkan saat berpindah rute dan saat komponen dilepas. Dijeda saat tab tidak terlihat (EC-18). ScrollTrigger dihitung ulang saat rute atau layout berubah. Detail API GSAP untuk ini diverifikasi di spike (V-15).

**Teks yang dipecah (FR-10.9, EC-14).** Nama aksesibel tetap utuh di elemen induk dan potongan visual disembunyikan dari pembaca layar, atau memakai fitur pemecah teks yang terbukti benar di Narrator dan TalkBack (AC-22). Pilihan alat diputuskan di spike S-01.

**Tangga degradasi per efek (K-12).** Setiap efek di spesifikasi motion (Tahap 4) mencatat tiga langkah: (1) alternatif yang lebih efisien dan tetap menjaga karakter, (2) versi sederhana, (3) hilangkan hanya bila tidak ada alternatif.

**Matriks kegagalan**

| Kegagalan | Perilaku yang diwajibkan | Rujukan |
|---|---|---|
| JavaScript mati | HTML statis terbaca, anchor dan tautan bekerja | EC-01, AC-19 |
| Pustaka animasi gagal dimuat | Konten tampil tanpa animasi | EC-02 |
| Browser tidak mendukung transisi tampilan | Navigasi instan tanpa animasi (EV-04) | FR-11 |
| Reduced motion aktif | Tingkat reduced motion penuh, konten identik | NFR-05.1 |
| Gambar atau font gagal | Dimensi dan font cadangan menjaga layout | EC-06, EC-07 |
| Error runtime pada satu efek | Efek lain dan halaman tetap berjalan, fallback dapat dinavigasi | FR-13.4 |

## 11. Strategi transisi halaman

Tingkat (ADR-0006):

| Tingkat | Perilaku | Status PRD |
|---|---|---|
| T0 | Navigasi instan tanpa animasi | Jalur aman, selalu tersedia |
| T1 | Transisi keluar-masuk terkoordinasi memakai `<ViewTransition>` | Should (FR-11.1 sampai FR-11.4) |
| T2 | Morph elemen bersama antara kartu project dan halaman detail | Could (FR-11.5) |

Aturan dari evidensi (EV-04) yang menjadi syarat desain:
- `::view-transition` harus `pointer-events: none` agar klik tidak hilang saat transisi (FR-11.2, AC-26).
- Morph hanya terbentuk bila halaman tujuan tersedia pada commit yang sama (halaman ter-prefetch). Perilaku ini diuji pada hasil static export, bukan diasumsikan (S-02).
- Back/forward browser tidak membawa transition type. Arah transisi tidak boleh bergantung pada tipe itu untuk kebutuhan fungsional (FR-01.3).
- Fokus keyboard dan pengumuman untuk pembaca layar **tidak** diberikan otomatis oleh transisi visual. Ditangani aplikasi (FR-11.4, Tahap 4).
- Reduced motion: transisi diganti perubahan tenang tanpa gerak spasial (FR-11.3).
- Dukungan Chromium 125+ perlu dicocokkan dengan versi Chrome di POCO M4 Pro (V-01, R-26).

Seberapa penting T1 dan T2 bagi pemilik (DP-01) masih terbuka. Arsitektur tidak bergantung pada jawabannya: T0 selalu tersedia.

## 12. Aset

| Aset | Pendekatan | Catatan |
|---|---|---|
| Font | Di-host sendiri, jumlah keluarga dan bobot dibatasi, ada font cadangan dengan metrik dekat untuk mencegah CLS | Mekanisme pada static export belum diverifikasi (V-13). Tidak ada font dari CDN pihak ketiga (NFR-08.3) |
| Gambar | Dimensi eksplisit. Varian responsif disiapkan **sebelum atau saat build**, bukan oleh loader bawaan Next.js (EV-03). Pemuatan malas kecuali elemen LCP (FR-04.6) | Opsi: loader kustom (didukung, EV-03) atau berkas yang dioptimalkan sebelumnya. Opsi `unoptimized` dan alat konversi belum diverifikasi (V-12) |
| Metadata gambar | EXIF dan metadata dibersihkan sebelum masuk repositori publik (NFR-08.2, AC-46) | Bagian pemeriksaan aset |
| SVG dan ikon | Inline atau berkas statis | Dekoratif ditandai agar diabaikan pembaca layar (FR-05.4) |
| CV | Berkas PDF statis di folder publik. Dimasukkan ke build **hanya** bila data menandai CV siap dan sudah diperiksa pemilik (FR-09.6, AC-63) | Metadata PDF dibersihkan (FR-09.7) |
| Video dan media berat | Tidak ada di baseline (ADR-0007) | |

## 13. Integrasi dan pihak ketiga

- **Saat runtime:** tidak ada. Tautan GitHub, Instagram, dan `mailto` adalah tautan keluar biasa, bukan integrasi (FR-09). Atribut keamanan tautan keluar ditentukan di Tahap 4 (FR-09.3).
- **Analytics:** tidak ada di MVP (NFR-08.4). Keputusan DP-08 masih menunggu konfirmasi pemilik (OQ-02).
- **Saat build dan pengembangan:** hanya paket npm yang dipin, masing-masing dengan alasan dan lisensi tercatat (NFR-07.1, NFR-07.2).

## 14. Keamanan dan privasi pada tingkat arsitektur

Detail dan prosedur di Tahap 4 dan 6.

| Ancaman atau risiko | Kontrol arsitektur | Rujukan |
|---|---|---|
| Kebocoran data sensitif RSUD lewat screenshot, teks, metadata, atau repositori | Gerbang rilis, varian konten A/B dengan izin tercatat, pembersihan metadata, audit sebelum repositori dibuka | R-03, NFR-08, AC-45, AC-46 |
| Konten tak berizin terpublikasi | Varian "bernama" hanya dapat dibuat bila data memuat catatan izin. Build gagal bila tidak | NFR-09.4, AC-11 |
| Placeholder atau klaim tak terkonfirmasi ikut terbit | Pemindaian sumber **dan** `out/`. Klaim tak terkonfirmasi tidak dirender | FR-12.4, FR-12.5, AC-16, AC-61 |
| Rantai pasok dependency | Versi dipin dengan berkas kunci, lisensi dicek, dependency minimal | NFR-07.2 |
| Pihak ketiga tak terduga | Pemeriksaan URL eksternal pada `out/` dan inspeksi jaringan | AC-47 |
| Header keamanan dan CSP | Tidak dapat diatur lewat `next.config` pada static export (EV-03). Dipasang di host (ADR-0009). CSP juga memengaruhi opsi skrip inline (bagian 10) | NFR-08.6 |
| Permukaan serangan input pengguna | Tidak ada formulir, akun, atau penyimpanan data pengguna | K-09 |

## 15. Pipeline build dan rilis (tingkat desain)

Nama skrip dan perintah aktual **belum ditetapkan** di sini. Perintah ditentukan di Tahap 4 sampai 6 dan diverifikasi sebelum dipakai di Tahap 8. Tahapan dan pemisahannya:

| Tahap | Isi | Jenis | Bisa menggagalkan rilis |
|---|---|---|---|
| P0 | Pemeriksaan tipe dan lint | Otomatis, deterministik | Ya |
| P1 | Validasi konten: skema, penanda klaim, tautan, status CV, varian RSUD, placeholder pada sumber | Otomatis, deterministik | Ya |
| P2 | Build statis (`output: 'export'`) | Otomatis, deterministik | Ya |
| P3 | Pemindaian `out/`: placeholder, URL pihak ketiga, metadata aset, anggaran ukuran (setelah ditetapkan) | Otomatis, deterministik | Ya |
| P4 | Pengukuran performa lab, uji keyboard dan perangkat nyata, uji Narrator dan TalkBack, uji reduced motion | Manual atau semi-otomatis, dengan kategori bukti A/B/C | Ya, lewat Quality Gates |
| P5 | Audit AI (tinjauan terpisah dari implementasi) | AI, hanya sebagai masukan | Tidak berdiri sendiri. Tidak boleh menyatakan lulus tanpa bukti P0 sampai P4 |
| P6 | Checklist rilis oleh pemilik (gerbang brief bagian 17) | Manual | Ya |

Pemisahan ini memenuhi NFR-10.3. Rincian bukti kelulusan menjadi bahan Tahap 6.

## 16. Anggaran performa [SEMENTARA] dan spike

Pemilik menyetujui bahwa target final ditetapkan **setelah prototipe diukur** (A-05) dan hero diuji lebih awal (A-07), selama tidak melampaui kapasitas (K-06).

**Batas atas yang sudah pasti** (NFR-01.1, tidak boleh dilonggarkan): LCP ≤ 2,5 detik, CLS ≤ 0,1, INP ≤ 200 ms. INP adalah metrik lapangan. Bila diukur di lab, dicatat sebagai proxy (NFR-01.3).

**Anggaran ukuran** (NFR-01.7). Kolom nilai sengaja kosong dan diisi dari hasil spike, lalu hanya boleh diperketat.

| Kategori anggaran | Nilai | Cara menetapkan |
|---|---|---|
| JavaScript awal beranda | [SEMENTARA] | Diukur dari prototipe hero (S-01), ditambah margin yang diputuskan pemilik |
| JavaScript tambahan halaman case study | [SEMENTARA] | Diukur setelah template case study ada |
| Porsi GSAP dan plugin dalam JS awal | [SEMENTARA] | Diukur dari S-01 dan S-03 |
| Bobot gambar di atas lipatan | [SEMENTARA] | Ditetapkan dengan keputusan visual (Tahap 4) |
| Jumlah keluarga dan bobot font | [SEMENTARA] | Ditetapkan dengan keputusan tipografi (Tahap 4) |

**Spike.** Kecil, berdiri sendiri, dan hasilnya dicatat dengan kategori bukti. Jadwal dan urutannya di Tahap 5. Spike tidak menjadi kode produksi kecuali disetujui.

| ID | Pertanyaan | Bukti minimum | Menjawab |
|---|---|---|---|
| S-01 | Apakah hero dengan masked text reveal dapat menjaga LCP, CLS, dan teks aksesibel? Apakah teks yang beranimasi CSS dihitung sebagai kandidat LCP pada saat awal atau akhir animasi? Pendekatan pemecahan teks mana yang benar di Narrator dan TalkBack? | A di POCO M4 Pro dan laptop uji, B sebagai pelengkap | R-29, V-14, ADR-0004, ADR-0005 |
| S-02 | Apakah transisi halaman dengan `<ViewTransition>` berjalan pada hasil static export di Chrome Android POCO M4 Pro, termasuk navigasi cepat dan tombol back? | A | R-26, V-06, ADR-0006 |
| S-03 | Berapa biaya efek scroll (ScrollTrigger, parallax terkontrol) pada perangkat uji? | A | R-04, ADR-0004 |
| S-04 | Apakah toolchain (instal, dev, build) berjalan wajar di laptop Windows 10 dengan Node versi yang tersedia? | A | R-25, V-02 |

## 17. Observabilitas

Tidak ada telemetri saat runtime (NFR-08.4). Observabilitas bersifat lokal dan terdokumentasi: keluaran pemeriksaan build (P0 sampai P3), laporan pengukuran performa dan hasil uji manual dengan kondisi tercatat (NFR-01.8, NFR-10.2). Lokasi dan format catatan bukti ditetapkan di Tahap 4 dan 6.

## 18. Pemetaan kebutuhan PRD ke arsitektur

| Kebutuhan | Dipenuhi oleh |
|---|---|
| FR-01 Rute dan navigasi browser | Bagian 9, ADR-0001 |
| FR-02 Navigasi global | Lapisan L3 dan L4 (menu mobile sebagai pulau interaksi), HTML anchor |
| FR-03 Hero | L3 (konten statis) + L5 (reveal), bagian 10, spike S-01 |
| FR-04, FR-05 Projects dan case study | L1, L3, ADR-0002 |
| FR-06, FR-07 About dan Expertise | L1, L3, ADR-0002 (expertise terhubung ke data project) |
| FR-08 How I Built This | L1 dan L3. Bersyarat bukti (A-09), ringkas di halaman utama (DP-12) |
| FR-09 Kontak dan CV | L1, L3, ADR-0008 |
| FR-10 Sistem motion | L5, ADR-0004, ADR-0005 |
| FR-11 Transisi halaman | Bagian 11, ADR-0006 |
| FR-12 Konten dan gerbang placeholder | L1, L2, ADR-0002, bagian 15 |
| FR-13 Error dan fallback | Halaman 404 (L6), matriks kegagalan (bagian 10), batas error pada pulau interaksi |
| FR-14 Metadata | L6, bagian 9 |
| FR-15 Eksperimen | Modul terpisah dan dapat dilepas, ADR-0007 |
| NFR-01 Performa | Bagian 10, 12, 16, ADR-0003 sampai 0005, ADR-0008 |
| NFR-02 Accessibility | HTML semantik di L3, pulau interaksi minimal, teks pecah (bagian 10), fokus saat transisi (bagian 11) |
| NFR-03, NFR-04 Responsif dan browser | ADR-0003, EV-02, EV-05, ADR-0005 |
| NFR-05, NFR-06 Motion nyaman dan ketahanan | ADR-0005, matriks kegagalan |
| NFR-07 Keterpeliharaan | Aturan batas modul (bagian 7), dependency minimal |
| NFR-08, NFR-09 Keamanan dan integritas | Bagian 14, 15, ADR-0002, ADR-0008 |
| NFR-10, NFR-11 Evidensi dan proses | Bagian 15 dan 16, diteruskan ke Tahap 5 dan 6 |

## 19. Risiko arsitektur baru

R-01 sampai R-24 tetap berlaku.

| ID | Risiko | Tingkat | Mitigasi |
|---|---|---|---|
| R-25 | Dukungan Windows 10 telah berakhir (EV-08), dan Node.js menyatakan tidak mendukung platform yang dukungan vendornya berakhir (EV-07). Versi Node atau tool mendatang bisa berhenti berjalan di laptop pemilik | Sedang | Pin versi Node yang terbukti berjalan (V-02, S-04), pakai berkas kunci, jangan memperbarui toolchain tanpa uji ulang |
| R-26 | `<ViewTransition>` memerlukan fitur Chromium 125+ (EV-04). Versi Chrome di POCO M4 Pro belum diketahui | Rendah sampai sedang | V-01, spike S-02, fallback T0 |
| R-27 | Static export tidak mendukung loader gambar bawaan dan header dari `next.config` (EV-03) | Sedang | ADR-0008 dan ADR-0009. Tentukan alur gambar di awal (V-12). Pasang header di host |
| R-28 | Batas minimum Tailwind v4 (Safari 16.4, Firefox 128, EV-05) lebih sempit daripada batas Next.js (Firefox 111, EV-02). Safari lebih lama dari 16.4 dan Firefox lebih lama dari 128 bisa kehilangan gaya | Rendah sampai sedang | Catat batas ini sebagai keterbatasan yang diumumkan. Tinjau ulang ADR-0003 bila uji K-10 menunjukkan masalah |
| R-29 | Masked text reveal pada hero dapat bertabrakan dengan syarat LCP tidak bergantung animasi (FR-03.4) | **Tinggi** | Spike S-01 sebelum desain hero dikunci (A-07). Bila bertabrakan, K-12: alternatif efisien lebih dulu |
| R-30 | Lisensi GSAP: ScrollTrigger tidak disebut eksplisit pada halaman lisensi yang dibaca (EV-06) | Rendah | V-05: konfirmasi dari teks lengkap lisensi sebelum dipakai. Catat versi dan lisensi |
| R-31 | Next.js 16 dan React 19.3 relatif baru. Perilaku fitur, termasuk yang memakai React canary bawaan (EV-01), dapat berubah antarrilis minor | Sedang | Pin versi, baca catatan rilis sebelum memperbarui, uji ulang spike bila ada pembaruan |

## 20. Asumsi, pertanyaan terbuka, dan verifikasi

### 20.1 Asumsi arsitektur

| ID | Asumsi | Dampak bila salah |
|---|---|---|
| AR-01 | Host statis dengan kemampuan yang dibutuhkan (404, header kustom, HTTPS) tersedia dengan biaya yang dapat diterima | ADR-0009 ditinjau. Mungkin perlu host yang berbeda |
| AR-02 | Chrome di POCO M4 Pro berada pada versi yang mendukung fitur transisi (>= 125) | T1 dan T2 tidak berjalan di perangkat uji utama. Pakai T0 |
| AR-03 | Volume konten kecil sehingga data bertipe (tanpa MDX) cukup untuk narasi case study | Bila narasi panjang menyulitkan, tinjau ADR-0002 |
| AR-04 | Build dan dev server berjalan dengan waktu wajar di i3 gen-7, RAM 12 GB, Windows 10 | Tinjau toolchain (S-04) |

### 20.2 Pertanyaan terbuka

| ID | Pertanyaan | Perilaku arsitektur sampai dijawab | Dibutuhkan |
|---|---|---|---|
| OQ-01 | Apakah ADR-0001 sampai ADR-0009 disetujui? Terutama Tailwind (ADR-0003) dan GSAP (ADR-0004) | Semua berstatus Diusulkan | Sebelum Tahap 4 |
| OQ-02 | DP-08: analytics? | Tidak ada analytics (NFR-08.4) | Tahap 4 |
| OQ-03 | DP-04: repositori situs dan project RSUD dibuka untuk publik? | Tautan sumber opsional dan tidak tampil bila tidak ada (FR-05.7, FR-08.7). Audit sebelum membuka (NFR-08.5) | Tahap 4 sampai rilis |
| OQ-04 | DP-01 dan DP-02: seberapa penting transisi halaman dan shared-element, dan apakah WebGL menjadi eksperimen? | T0 selalu ada, T1 Should, T2 Could. WebGL mati (ADR-0007) | Tahap 4 sampai 5 |
| OQ-05 | Q-19: hosting dan domain | Ditunda ke menjelang rilis (ADR-0009) | Menjelang rilis |

### 20.3 Item verifikasi

| ID | Yang perlu diverifikasi | Oleh | Kapan |
|---|---|---|---|
| V-01 | Versi Android dan Chrome di POCO M4 Pro | Pemilik | Sebelum S-02 |
| V-02 | Versi Chrome dan Node.js di laptop, kemampuan menjalankan toolchain | Pemilik | Sebelum S-04 |
| V-05 | Teks lengkap lisensi GSAP, termasuk cakupan ScrollTrigger dan versi yang dipin | Claude, dengan konfirmasi pemilik | Sebelum dependency ditambahkan |
| V-06 | Kelayakan transisi halaman pada hasil static export | Claude dan pemilik | Spike S-02 |
| V-12 | Alur gambar pada static export: opsi `unoptimized`, loader kustom, atau konversi sebelum build, beserta alat dan lisensinya | Claude dari sumber resmi | Tahap 4 |
| V-13 | Mekanisme font lokal pada static export dan cara mencegah pergeseran layout | Claude dari sumber resmi | Tahap 4 |
| V-14 | Perilaku LCP untuk teks hero yang beranimasi | Pemilik, lewat pengukuran | Spike S-01 |
| V-15 | API GSAP yang akan dipakai (pembersihan siklus hidup, `matchMedia`, pemecahan teks dan aksesibilitasnya) | Claude dari sumber resmi, lalu spike | Tahap 4 dan S-01 |
| V-16 | Halaman dukungan Microsoft dibuka penuh, dan kebijakan dukungan Node.js untuk versi LTS di Windows 10 | Claude dari sumber resmi | Sebelum memilih versi Node |

V-03, V-04, V-07 sampai V-11 tetap sesuai brief dan PRD.

## 21. Hal yang diserahkan ke Tahap 4

1. Struktur folder dan modul sesuai bagian 7.
2. Model data konten dan skema validasi (ADR-0002), termasuk varian RSUD, penanda klaim, status CV, dan bentuk teks multibahasa.
3. Spesifikasi UI/UX: batas tiga kelas lebar (AS-05, V-11), 2 sampai 3 alternatif konsep visual, referensi tambahan (P-05), palet, font, headline (DP-07).
4. Spesifikasi motion: token bernilai, tangga degradasi per efek, aturan keadaan awal dan fail-safe, pendekatan pemecahan teks, pengelolaan fokus setelah transisi.
5. Keputusan font dan gambar (V-12, V-13), `trailingSlash`, pendekatan header keamanan sesuai host.
6. Keamanan dan privasi terperinci, termasuk audit data sebelum publikasi.
7. Keputusan DP-01, DP-02, DP-04, DP-08 (OQ-02 sampai OQ-04).

Tahap 5 menerima: jadwal spike S-01 sampai S-04 di awal dan ketergantungan antarfase. Tahap 6 menerima: metode ukur (V-07), pemisahan P0 sampai P6, dan verifikasi nomor kriteria WCAG (V-10).

## 22. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 12 Oktober 2026 | Draf awal Tahap 3 berdasarkan PROJECT_BRIEF v0.2 dan PRD v0.3 |
