# MOTION_SPEC — Spesifikasi sistem motion

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/MOTION_SPEC.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 (Desain Teknis) |
| Dokumen induk | `docs/planning/TECHNICAL_DESIGN.md` |
| Rujukan | K-03, K-04, K-12, DP-01, DP-13, FR-03, FR-10, FR-11, NFR-01, NFR-05, NFR-06, ADR-0004, ADR-0005, ADR-0006, ADR-0007, AC-06, AC-18 sampai AC-26, AC-55, AC-56, AC-58, AC-59 |
| Status | **T4-USULAN**. Semua nilai angka adalah **[SEMENTARA]** dan dikalibrasi lewat spike S-01 sampai S-03. Pemusatan penentuan tingkat tetap **rekomendasi** (DP-13) |

## 1. Prinsip yang mengikat

1. Konten dan navigasi tidak menunggu animasi (K-03, FR-02.6).
2. HTML awal berisi konten terlihat. Efek adalah peningkatan, bukan prasyarat (ADR-0005 butir 4, FR-10.8).
3. Aturan penengah K-12: alternatif efisien dulu, lalu penyederhanaan, baru penghilangan. Setiap efek memiliki **tangga degradasi** tertulis (bagian 6).
4. Hanya `transform` dan `opacity` dianimasikan pada baseline. Pengecualian dicatat dengan alasan (FR-10.3, AC-23).
5. Scroll native adalah baseline. Tidak ada penangkapan scroll, `wheel`, atau `touchmove` untuk memindahkan halaman (FR-10.5).
6. Tidak ada angka performa yang dinyatakan lulus di dokumen ini. Semua dibuktikan lewat pengukuran (NFR-10.2).

## 2. Tingkat motion dan penentuannya (D-11)

### 2.1 Tiga tingkat

| Tingkat | Kapan aktif | Karakter |
|---|---|---|
| `reduced` | `(prefers-reduced-motion: reduce)` cocok. **Selalu menang** atas yang lain | Tanpa gerak spasial. Perubahan opacity dan warna singkat atau tanpa transisi. Tanpa paralaks, mask, stagger (FR-03.7, NFR-05.1) |
| `full` | Bukan `reduced`, **dan** lebar ≥ kelas besar, **dan** `(hover: hover) and (pointer: fine)` | Efek lengkap: reveal kata, stagger, reveal gambar, paralaks terkontrol bila lolos S-03 |
| `adapted` | Bukan `reduced` dan bukan `full` (layar kecil atau menengah, atau pointer kasar) | Karakter inti dipertahankan, efek berat disederhanakan: jarak gerak lebih kecil, stagger lebih singkat, tanpa paralaks (FR-10.10, K-12) |

Penentuan memakai **kemampuan yang dapat diperiksa**, bukan merek, model, atau user agent (ADR-0005 butir 3). Bila `matchMedia` tidak tersedia atau tidak dapat diperiksa, tingkat bawaan adalah **`reduced`** (pilihan paling aman).

### 2.2 Resolver

| Aspek | Spesifikasi |
|---|---|
| Lapisan CSS | Semua nilai token per tingkat didefinisikan dengan media query di `src/styles/tokens.css`. Halaman yang tidak memuat JS apa pun tetap memperoleh perilaku `reduced` yang benar |
| Lapisan JS (rekomendasi) | `src/motion/tier.ts` membuat tiga `matchMedia` dan menghitung `Tier`. Menyetel `document.documentElement.dataset.motionTier`. Memanggil pendengar saat berubah (EC-04, EC-25, AC-59) |
| Kondisi di komponen | Komponen **tidak** memeriksa `matchMedia`. Komponen hanya menandai elemen dengan atribut data. Bila praktik menyimpang, deviasi dicatat dan AC-24 tetap harus lulus (DP-13) |
| Perubahan saat sesi | `onTierChange` memicu: lepas semua efek tingkat lama, pasang efek tingkat baru. Elemen yang sudah terlihat tetap terlihat (tidak disembunyikan ulang) |
| Pengujian | AC-24: perilaku per tingkat. AC-59: perubahan preferensi, rotasi, dan ubah ukuran |

## 3. Token motion (FR-10.4, AC-56)

Satu sumber: CSS custom properties. JS **membaca** nilai dari CSS (`getComputedStyle(document.documentElement)`) melalui `readMotionTokens()`. Tidak ada salinan angka di TypeScript.

| Token | `full` | `adapted` | `reduced` | Catatan |
|---|---|---|---|---|
| `--motion-duration-instant` | 0ms | 0ms | 0ms | |
| `--motion-duration-fast` | 150ms | 120ms | 100ms | hover, fokus |
| `--motion-duration-base` | 400ms | 300ms | 150ms | reveal umum |
| `--motion-duration-slow` | 800ms | 600ms | 150ms | reveal hero |
| `--motion-ease-standard` | `cubic-bezier(0.2, 0, 0, 1)` | sama | `linear` | |
| `--motion-ease-enter` | `cubic-bezier(0.05, 0.7, 0.1, 1)` | sama | `linear` | |
| `--motion-ease-exit` | `cubic-bezier(0.3, 0, 0.8, 0.15)` | sama | `linear` | |
| `--motion-stagger-step` | 60ms | 40ms | 0ms | jeda antarelemen |
| `--motion-stagger-max` | 600ms | 360ms | 0ms | batas total stagger |
| `--motion-distance-reveal` | 24px | 12px | 0px | jarak translasi reveal |
| `--motion-distance-hero` | 100% (tinggi baris) | 60% | 0% | kata terangkat dari mask |
| `--motion-parallax-factor` | 0.12 | 0 | 0 | hanya bila E-06 aktif |
| `--motion-failsafe-delay` | 3000ms | 3000ms | 1000ms | bagian 4 |

Semua nilai di atas **[SEMENTARA]**, adalah titik awal yang masuk akal, dan **bukan hasil ukur**. Dikalibrasi setelah S-01 dan S-03 di POCO M4 Pro dan laptop uji. Nama token dibekukan sejak Fase 1. Nilai boleh berubah.

Pada tingkat `reduced`, durasi tidak nol untuk perubahan opacity agar perubahan keadaan tetap terbaca, tetapi tidak ada translasi, skala, atau mask.

## 4. Keadaan awal aman dan fail-safe (D-12)

### 4.1 Aturan keadaan awal

1. HTML hasil build menampilkan **seluruh konten dalam keadaan terlihat**. Tidak ada kelas "tersembunyi" pada HTML awal.
2. Elemen kandidat LCP hero (`data-lcp`) tidak pernah disembunyikan oleh JS (FR-03.4).
3. Hanya elemen yang **sudah berada di bawah batas viewport saat inisialisasi** yang boleh "dipersenjatai" (disembunyikan untuk reveal). Elemen yang terlihat sebagian atau seluruhnya di viewport **tidak** disembunyikan. Itu menghilangkan kilatan tanpa skrip inline dan tanpa bergantung pada CSP (R-33, V-17).
4. Elemen dipersenjatai hanya bila tingkat bukan `reduced` dan IntersectionObserver tersedia.

### 4.2 Mekanisme

```
HTML awal:      <section data-reveal> ... </section>            (terlihat)
Setelah hidrasi (MotionRoot):
  untuk setiap [data-reveal] dengan rect.top > innerHeight:
      el.classList.add('is-armed')                              (opacity 0, translate)
      IntersectionObserver.observe(el)
  saat masuk viewport:  el.classList.add('is-in')               (transisi ke terlihat)
CSS fail-safe (tanpa JS):
  .is-armed:not(.is-in) { animation: failsafe-reveal 0.01s linear var(--motion-failsafe-delay) forwards; }
  @keyframes failsafe-reveal { to { opacity: 1; transform: none; } }
```

Efeknya: bila JS mati **setelah** elemen dipersenjatai, atau pemuatan efek macet, CSS sendiri menampilkan elemen setelah `--motion-failsafe-delay`. Bila JS mati **sebelum** inisialisasi, tidak ada elemen yang pernah disembunyikan.

### 4.3 Fail-safe berlapis

| Lapisan | Pemicu | Tindakan |
|---|---|---|
| F1 CSS | Waktu berlalu setelah `is-armed` | Elemen dipaksa terlihat oleh animasi CSS (bagian 4.2) |
| F2 Inisialisasi | Galat saat `MotionRoot` atau modul efek dijalankan | `try/catch` di tingkat akar: hapus semua `is-armed`, setel `data-motion="failed"`. Efek lain dan halaman tetap berjalan (FR-13.4, NFR-06.2) |
| F3 Impor dinamis | Gagal memuat modul efek (jaringan, potongan hilang) | Tangkap penolakan promise, perlakukan seperti F2 |
| F4 Fitur hilang | `IntersectionObserver` atau `matchMedia` tidak ada | Jangan mempersenjatai apa pun |
| F5 Tab tersembunyi | `visibilitychange` | Jeda: kelas `html.motion-paused` menyetel `animation-play-state: paused`. Loop rAF memeriksa bendera yang sama (EC-18) |
| F6 Dinonaktifkan paksa | `data-motion="off"` pada `<html>` (hook uji) | Perilaku `reduced` penuh |

### 4.4 Yang tidak dilakukan

- Tidak ada skrip inline untuk menyetel kelas sebelum cat pertama.
- Tidak ada `visibility: hidden` atau `opacity: 0` pada HTML awal.
- Tidak ada layar loading, tirai pembuka, atau penahan konten menunggu animasi (FR-03.6, AC-58).

### 4.5 Dampak yang diketahui

- Pengguna yang menggulir sangat cepat sebelum hidrasi dapat melihat elemen yang kemudian dipersenjatai tepat saat berada di tepi viewport. Penanganan: ambang `rect.top > innerHeight + 0` konservatif. Dicatat sebagai hal yang diuji di S-01 (EC-26).
- Pemakai tanpa JS tidak mendapat animasi, tetapi mendapat seluruh konten (EC-01, AC-19).

## 5. Hero

### 5.1 Kendala

FR-03.3 meminta masked text reveal dan stagger. FR-03.4 melarang elemen kandidat LCP bergantung pada JS atau animasi untuk muncul pertama kali. ADR-0005 butir 4 hanya mengizinkan penyembunyian di bawah lipatan. Tabrakan ini tercatat sebagai C-02, C-03, dan R-29. **Tidak diketahui** apakah Chrome menghitung teks yang beranimasi CSS sebagai kandidat LCP pada awal atau akhir animasi (V-14).

### 5.2 Tiga varian hero

| Varian | Deskripsi | Syarat | Risiko |
|---|---|---|---|
| **HR-3 (aman, bawaan)** | Elemen kandidat LCP (blok nama atau headline terbesar) **statis dan terlihat sejak cat pertama**. Reveal berlaku pada elemen sekunder (peran, status, CTA) dengan transisi opacity dan translasi kecil, atau tanpa reveal | Tidak membutuhkan QT4-02 bila elemen sekunder dianimasikan hanya dengan CSS non-menyembunyikan (mis. translasi kecil tanpa opacity 0). Reveal yang mulai dari opacity 0 pada elemen sekunder atas-lipatan membutuhkan persetujuan QT4-02 | Karakter "masked reveal" berkurang pada elemen utama |
| **HR-1** | Masked text reveal murni CSS: tiap kata dibungkus (pembungkus `overflow: hidden`, isi bergerak dengan `transform: translateY`) dengan `animation` CSS yang mulai pada t=0 tanpa menunggu JS | Hanya bila S-01 membuktikan LCP, CLS, AC-22 (pembaca layar), dan pencarian halaman lulus di POCO M4 Pro. Membutuhkan QT4-02 | Teks mulai terpotong. Perilaku LCP belum diketahui. Font swap dapat menggeser lebar kata (R-34) |
| **HR-2** | Seperti HR-1 tetapi kandidat LCP adalah elemen lain yang statis (mis. blok nama statis, headline dianimasikan) | Hanya bila HR-1 gagal pada LCP tetapi komposisi menerima pemisahan | Hierarki visual lebih terbatas |

### 5.3 Aturan pemilihan

1. S-01 menguji HR-1 dan HR-3 (dan HR-2 bila perlu) di POCO M4 Pro dan laptop uji, dengan kondisi uji tercatat (NFR-01.8). Catat: elemen kandidat LCP dan waktunya (lewat `PerformanceObserver` untuk `largest-contentful-paint`), CLS, perilaku saat font cadangan diganti.
2. Pilih varian **paling ekspresif yang lulus** AC-06, AC-27, AC-28, dan AC-22. Bila tidak ada selain HR-3, HR-3 dipakai (K-12 butir 1 dan 2: alternatif efisien dulu).
3. Hasil dicatat dengan kategori bukti. Tidak ada pernyataan "lulus" tanpa data.

### 5.4 Pemecahan teks (D-13, FR-10.9, EC-14)

| Aspek | Spesifikasi |
|---|---|
| Tingkat | **Kata**, bukan huruf. Pemecahan per baris dihindari karena membutuhkan pengukuran tata letak saat runtime |
| Kapan | **Saat build**, pada Server Component. Komponen `SplitWords` memecah teks sumber menjadi `<span class="w"><span class="wi">kata</span></span>` dipisahkan spasi nyata |
| Aksesibilitas | Teks tetap berupa satu aliran teks dengan spasi. **Tanpa `aria-hidden`, tanpa salinan tersembunyi, tanpa `aria-label` pengganti.** Pembaca layar membaca kata demi kata secara wajar. Cari-di-halaman dan salin tetap bekerja |
| Pemecahan per huruf | Tidak dipakai kecuali S-01 membuktikan aman di Narrator dan TalkBack (AC-22) dan cari/salin |
| GSAP SplitText | Tidak dibutuhkan untuk pendekatan ini (D-14) |
| Pengujian | Narrator di Windows 10, TalkBack di POCO M4 Pro, cari-di-halaman, salin teks (AC-22) |

## 6. Katalog efek dan tangga degradasi (K-12, FR-10.10)

Format tangga: **L1** alternatif efisien yang menjaga karakter → **L2** versi sederhana → **L3** hilangkan (hanya bila tidak ada alternatif). "Teknologi" menyatakan kebutuhan GSAP: pada baseline **tidak ada efek yang mewajibkan GSAP** (D-14). GSAP baru dipertimbangkan bila L1 CSS gagal memenuhi hasil ukur.

| ID | Efek | Pemicu | Properti | Teknologi baseline | full | adapted | reduced | Tangga degradasi |
|---|---|---|---|---|---|---|---|---|
| E-01 | Hero: reveal kata bertopeng + stagger (HR-1) | Muat halaman, CSS `animation` | `transform` | CSS | Penuh | Jarak 60%, stagger 40ms | Tidak ada. Teks statis | L1: HR-2/HR-3 (kandidat LCP statis). L2: fade ringan tanpa mask. L3: statis |
| E-02 | Hero: elemen sekunder masuk (peran, status, CTA) | Muat halaman, CSS | `opacity`, `transform` kecil | CSS | Penuh | Jarak 12px | Statis atau fade 150ms | L1: translasi saja tanpa opacity 0. L2: fade. L3: statis |
| E-03 | Judul bagian dan blok teks masuk | IntersectionObserver + kelas | `opacity`, `transform` | CSS + IO | 24px, 400ms | 12px, 300ms | Fade 150ms atau tanpa | L1: IO + CSS. L2: fade saja. L3: tanpa reveal |
| E-04 | Reveal gambar kartu project (hover/fokus) | `:hover`, `:focus-visible` | `transform` pada isi di dalam pembungkus `overflow: hidden`, `opacity` | CSS | Penuh | Keadaan akhir permanen atau masuk sekali | Perubahan warna atau opacity | L1: `transform`+mask pembungkus. L2: fade. L3: gambar statis |
| E-05 | Stagger daftar (kartu project, butir expertise) | IO | `opacity`, `transform` | CSS dengan `transition-delay` dari indeks (`--i`) | step 60ms, maks 600ms | step 40ms, maks 360ms | Tanpa stagger | L1: indeks CSS. L2: reveal serempak. L3: tanpa |
| E-06 | Paralaks terkontrol (satu elemen) | Gulir | `transform` | **Tidak diimplementasikan di baseline.** Kandidat CSS atau GSAP+ScrollTrigger, diputuskan setelah S-03 | Faktor 0,12 bila S-03 lulus | Tidak ada | Tidak ada | L1: versi CSS bila ada. L2: statis. L3: hapus. Efek ini yang **pertama dipotong** (PRD 4.4) |
| E-07 | Mikro-interaksi tautan dan tombol | Hover/fokus | `color`, `opacity`, `transform` kecil | CSS | Penuh | Fokus dan sentuh saja | Warna/opacity | L1: CSS. L2: warna. L3: tanpa |
| E-08 | Transisi halaman T1 (`<ViewTransition>`) | Navigasi rute | Disediakan browser | Next.js + React | Keluar-masuk terkoordinasi | Lebih singkat | Fade silang ≤ 150ms atau instan | L1: T1 dengan CSS. L2: T0 (instan). T0 selalu tersedia (ADR-0006) |
| E-09 | Morph elemen bersama (T2) | Navigasi dari kartu ke detail | Disediakan browser | Next.js + React | Hanya bila T1 stabil dan lolos performa | Tidak | Tidak | L1: T1. L2: T0. Prioritas Could (FR-11.5) |
| E-10 | Buka/tutup menu mobile | Klik/ketuk | `opacity`, `transform` pada panel | CSS pada `<details>` | n/a | Fade/translasi singkat | Instan atau fade 100ms | L1: transform. L2: opacity. L3: instan |

Aturan tambahan semua efek:
1. Tidak ada animasi berulang tanpa batas. Bila ada petunjuk gulir yang beranimasi, ia berhenti dalam ≤ 5 detik atau statis (FR-10.6, AC-21).
2. Tidak ada kedipan lebih dari 3 kali per detik (NFR-02.11).
3. `will-change` hanya dipasang sementara saat animasi dan dilepas setelahnya.
4. Tidak ada animasi `width`, `height`, `top`, `left`, `margin` pada baseline. Pengecualian dicatat dengan alasan (AC-23).

## 7. Primitif dan siklus hidup (FR-10.7, AC-55)

### 7.1 Tipe

```ts
// src/motion/types.ts
export type Tier = 'full' | 'adapted' | 'reduced';
export type EffectId = 'E-01'|'E-02'|'E-03'|'E-04'|'E-05'|'E-06'|'E-07'|'E-08'|'E-09'|'E-10';

export interface MotionTokens {            // dibaca dari CSS, bukan ditulis ulang
  durationBase: number; durationSlow: number; staggerStep: number; staggerMax: number;
  distanceReveal: number; parallaxFactor: number; failsafeDelay: number;
}
export interface EffectContext {
  tier: Tier;
  tokens: MotionTokens;
  signal: AbortSignal;                      // aborted saat efek dilepas
  isPaused: () => boolean;                  // true saat tab tersembunyi
}
export type EffectSetup = (el: HTMLElement, ctx: EffectContext) => void | (() => void);
export function mountEffect(id: EffectId, el: HTMLElement, setup: EffectSetup, opts?: { tiers?: Tier[] }): () => void;
```

### 7.2 Aturan siklus hidup

| Aturan | Spesifikasi |
|---|---|
| Registri | Semua efek JS dipasang lewat `mountEffect`. `mountEffect` mencatat pelepasnya di registri |
| Pelepasan otomatis | Saat komponen dilepas (`useEffect` cleanup di `MotionRoot`), saat rute berubah, dan saat tingkat berubah: `AbortController.abort()` lalu jalankan semua pelepas |
| Pendengar | Semua `addEventListener`, `IntersectionObserver`, dan `ResizeObserver` memakai `ctx.signal` atau `disconnect` di pelepas. Tidak ada pendengar `scroll`, `wheel`, `touchmove` pada baseline |
| Tab tersembunyi | `visibilitychange` menyetel `html.motion-paused`. Tidak ada pekerjaan baru dimulai saat tab tersembunyi |
| Navigasi berulang | Pelepasan idempoten. Memanggil pelepas dua kali aman (EC-13, AC-55) |
| Kegagalan satu efek | `try/catch` per efek. Satu efek gagal tidak melepas yang lain. Elemen terkait dikembalikan ke keadaan terlihat |
| GSAP (bila kelak dipasang) | Hanya diimpor dari `src/motion/**` (`CHK-ARCH-02`), dimuat lewat `import()` dinamis di dalam efek yang membutuhkannya. Pembersihan memakai mekanisme konteks GSAP **setelah API diverifikasi** (V-15). Tidak ada API GSAP yang diasumsikan di dokumen ini |

## 8. Transisi halaman dan fokus (ADR-0006, FR-11)

### 8.1 Tingkat

T0 (instan) selalu tersedia dan menjadi bawaan. T1 hanya bila S-02 lulus di POCO M4 Pro pada hasil static export. T2 hanya bila T1 stabil dan lolos performa. DP-01 belum dijawab, bawaan tetap: T0 wajib, T1 Should, T2 Could (QT4-06).

### 8.2 Aturan dari evidensi EV-04 yang menjadi syarat desain

| Aturan | Implementasi |
|---|---|
| Overlay `::view-transition` menangkap event pointer | `::view-transition { pointer-events: none }` (FR-11.2, AC-26) |
| Elemen bernama dilewati hit-testing selama transisi | Nama transisi hanya dipakai bila T2 aktif. Tautan tidak boleh tergantung pada elemen bernama untuk dapat diklik |
| Back/forward tidak membawa transition type | Fungsi dan arah tidak bergantung pada tipe. Tombol back tidak berubah perilaku (FR-01.3, AC-02) |
| Morph hanya bila tujuan sudah dirender pada commit yang sama | Diuji pada hasil static export (S-02). Bila tidak terbentuk, jatuh ke T1 |
| Browser tanpa dukungan | Navigasi normal tanpa animasi (EV-04). Bukan galat |
| Reduced motion | Fade silang ≤ `--motion-duration-fast` tingkat `reduced`, atau instan. Tanpa gerak spasial (FR-11.3) |
| Navigasi cepat berulang | Transisi sebelumnya dibatalkan atau diganti tanpa tumpang tindih. Status halaman konsisten (EC-13). **Perilaku ini diverifikasi, bukan diasumsikan** (S-02) |

### 8.3 Fokus dan pengumuman setelah navigasi (FR-11.4, NFR-02.12)

Transisi visual **tidak** memberikan fokus atau pengumuman (ARCHITECTURE bagian 11). Hasil yang dituntut:

1. Setelah navigasi klien, fokus keyboard berada pada awal konten halaman baru (`h1` dengan `tabindex="-1"`, atau `main`), bukan tertinggal pada tautan yang menghilang.
2. Pembaca layar mendengar judul halaman baru.
3. Posisi gulir sesuai: awal halaman untuk rute baru, posisi tersimpan untuk back/forward (perilaku browser).

Apakah App Router sudah memberikan (1) dan (2) **belum diverifikasi** (V-24). Urutan kerja: (a) uji perilaku bawaan di S-02 dan S-06 dengan keyboard, Narrator, dan TalkBack, (b) bila belum memenuhi, tambahkan `RouteAnnouncer` dan penanganan fokus di `src/islands/`, (c) catat hasilnya dengan kategori bukti.

## 9. Interaksi keyboard dan motion

1. Tidak ada efek yang mengubah urutan fokus.
2. Hover memiliki padanan `:focus-visible` (FR-04.4).
3. Animasi tidak menahan fokus atau menutup kontrol yang sedang difokuskan.
4. `prefers-reduced-motion` tidak menghapus indikator fokus atau perubahan keadaan.

## 10. Anggaran dan biaya (placeholder terukur)

| Item | Status | Cara menetapkan |
|---|---|---|
| JS awal beranda | [SEMENTARA] | Diukur di S-01 |
| JS efek (modul motion) | [SEMENTARA] | Diukur di S-01 dan S-03 |
| Biaya GSAP bila dipasang | Tidak ada. Tidak dipasang | S-03 menentukan perlu atau tidak |
| Stagger total | ≤ `--motion-stagger-max` | Token |

Prinsip: tidak ada pustaka yang dipasang sebelum hasil ukur menunjukkan CSS dan IntersectionObserver tidak cukup (ADR-0004, ADR-0007).

## 11. Matriks kegagalan motion

| Kegagalan | Perilaku | Mekanisme | AC |
|---|---|---|---|
| JS mati | HTML statis terbaca, anchor bekerja, menu berfungsi | HTML awal terlihat, menu `<details>` | AC-19 |
| Modul efek gagal dimuat | Konten tampil tanpa animasi | F3 | AC-19, AC-60 |
| Galat inisialisasi atau runtime satu efek | Efek lain tetap berjalan, elemen terlihat | F2 | AC-60 |
| `IntersectionObserver` tidak ada | Tidak ada elemen dipersenjatai | F4 | AC-19 |
| Reduced motion aktif atau berubah | Tingkat `reduced`, tanpa muat ulang bila mungkin | Resolver | AC-18, AC-59 |
| Tab tersembunyi | Animasi dijeda | F5 | AC-55 |
| Browser tanpa `ViewTransition` | Navigasi instan | EV-04 | AC-02 |
| Perangkat lambat | Tingkat `adapted` dan penyederhanaan K-12. **Tidak ada deteksi kinerja otomatis** (ADR-0005) | Desain tingkat | AC-25 |

## 12. Hook uji (bukan fitur produk)

Untuk menguji kegagalan secara sengaja (AC-19, AC-60) tanpa mengubah kode produksi:

| Hook | Cara | Larangan |
|---|---|---|
| Matikan motion | Atribut `data-motion="off"` pada `<html>` lewat DevTools | Tidak ada |
| Blokir modul efek | Blokir permintaan berkas JS motion di DevTools | Tidak ada |
| Picu galat efek | Variabel build `NEXT_PUBLIC_FAULT_INJECTION=1` menambahkan perangkap galat. Dinonaktifkan pada build rilis | `CHK-OUT-10` gagal bila string pemicu ditemukan di `out/` |

## 13. Kriteria verifikasi

| Bukti yang dibutuhkan | Cara | AC |
|---|---|---|
| Elemen kandidat LCP hero muncul tanpa bergantung pada animasi atau JS | DevTools dengan animasi dan JS dimatikan, lalu `PerformanceObserver` | AC-06 |
| Tiga tingkat aktif sesuai kondisi, ubah ukuran memicu penyesuaian | Uji manual per tingkat | AC-24, AC-59 |
| Reduced motion: tanpa gerak spasial besar, konten identik | Pengaturan sistem dan emulasi DevTools | AC-18 |
| Fail-safe: blokir modul efek lalu semua konten tampil | Blokir JS dan modul | AC-19 |
| Tanpa penangkapan scroll | Uji keyboard, anchor, sentuh, tombol browser | AC-20 |
| Tidak ada kedipan >3/detik dan tidak ada gerak otomatis >5 detik tanpa kontrol | Tinjauan animasi | AC-21 |
| Teks hero terbaca utuh oleh pembaca layar, dapat dicari dan disalin | Narrator, TalkBack, cari, salin | AC-22 |
| Properti animasi utama `transform` dan `opacity` | Tinjauan kode dan profil | AC-23 |
| Tidak ada durasi atau easing literal di komponen | `CHK-ARCH-05` ditambah tinjauan | AC-56 |
| Efek berat disederhanakan di POCO M4 Pro menurut K-12 | Uji di perangkat dan tinjauan pemilik | AC-25 |
| Pelepasan efek saat rute berubah dan tab tersembunyi. Navigasi berulang tidak menumpuk kerja | Profil performa dan navigasi berulang | AC-55 |
| Navigasi dapat dipakai saat animasi masuk berjalan, CPU diperlambat | POCO M4 Pro dan throttling | AC-58 |
| Transisi halaman tidak menghalangi navigasi, dapat dibatalkan, fokus dikelola | Navigasi cepat dan keyboard | AC-26 |

## 14. Belum diputuskan

| ID | Item | Kapan |
|---|---|---|
| QT4-02 | Tafsir ADR-0005 butir 4 untuk keyframe CSS di atas lipatan | Sebelum S-01 |
| V-14 | Perilaku LCP teks beranimasi | S-01 |
| V-15 | API GSAP (hanya bila dibutuhkan) | S-03 |
| V-24 | Fokus dan pengumuman bawaan App Router | S-02 dan S-06 |
| DP-01 | Pentingnya T1 dan T2 | Konfirmasi (QT4-06) |
| — | Nilai final token | Setelah S-01 dan S-03 |
