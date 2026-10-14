# IMPLEMENTATION_PLAN — Creative Developer Portfolio Website

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/IMPLEMENTATION_PLAN.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 5 dari 10 (Rencana Implementasi) |
| Status | **Menunggu review dan persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 6)** |
| Dasar | `PROJECT_BRIEF.md` v0.2, `PRD.md` v0.3, `ARCHITECTURE.md` v0.1, ADR-0001 sampai ADR-0009 (semua *Diusulkan*), `TECHNICAL_DESIGN.md` dan lima dokumen pendampingnya (v0.1) |
| Catatan versi repo | Per pemeriksaan 13 Okt 2026, remote hanya memuat commit `0494615`. Enam dokumen Tahap 4 dan dokumen ini belum ada di remote sampai pemilik meng-commit-nya |

## 1. Cara membaca dokumen ini

| Penanda | Arti |
|---|---|
| **P01 sampai P10** | Fase implementasi. Jumlahnya sepuluh agar cocok dengan paket Tahap 8 (`phase-01.md` sampai `phase-10.md`) |
| **G0 sampai G4** | Gerbang pemilik: hal yang hanya dapat diputuskan atau dikerjakan pemilik dan harus selesai sebelum fase tertentu |
| **A-T5-xx** | Asumsi Tahap 5. Bukan fakta. Dikoreksi bila salah |
| **S-xx, V-xx, R-xx, CHK-xxx, D-xx, QT4-xx** | Rujukan ke dokumen sebelumnya |
| **Ukuran** | S, M, L: perbandingan usaha. Rentang jam adalah **asumsi** (A-T5-02) yang dikalibrasi setelah P01 (A-02) |
| **[SEMENTARA]** | Nilai yang sengaja belum dikunci |

Dokumen ini tidak menetapkan strategi uji penuh, perintah uji, atau kriteria lulus per tes. Itu milik Tahap 6. Dokumen ini juga tidak membuat prompt Antigravity (Tahap 8) dan tidak merancang orchestrator (Tahap 9). Perintah yang disebut adalah **nama logis** (`ASSETS_AND_BUILD.md` bagian 7) dan baru diverifikasi saat dibuat di P01.

## 2. Keputusan dan asumsi yang dipakai

Pemilik meminta default Tahap 4 dipakai untuk pertanyaan yang belum dijawab dan tidak ingin ada penantian konfirmasi untuk keputusan kecil. Berikut yang berlaku. **Semuanya dapat diubah pemilik kapan saja.**

| ID | Item | Default yang dipakai | Status |
|---|---|---|---|
| QT4-01 | Repositori dan nama RSUD | **Pendekatan aman, tanpa tindakan otomatis.** Tidak ada perubahan visibilitas, penghapusan repositori, atau penulisan ulang riwayat Git oleh agen mana pun. Tidak ada berkas baru yang menyebut nama institusi. Tindakan pada GitHub dilakukan sendiri oleh pemilik (G0) | Aturan wajib seluruh fase (bagian 3) |
| QT4-02 | Keyframe CSS di atas lipatan | Tidak diizinkan di produksi. Hero produksi memakai HR-3. HR-1 hanya **diukur sebagai prototipe** di S-01. Bila hasilnya baik, pemilik diminta memutuskan | Default |
| QT4-03 | Konsep visual | Pemilik belum memilih (G1). Bila G1 belum terjawab saat P04 dimulai, **konsep A dipakai sebagai satu-satunya prototipe pengukuran** supaya spike tidak tertunda. Ini bukan keputusan visual dan tidak mengunci P05 | A-T5-03 |
| QT4-04 | Headline | Opsi 1 brief Lampiran A sebagai teks uji | Default |
| QT4-05 | Anonimisasi RSUD | "a hospital", tanpa lokasi atau jenis kepemilikan | Default |
| QT4-06 | DP-01, DP-02, DP-04, DP-08 | T0 wajib, T1 Should, T2 Could. Tanpa WebGL. DP-04 mengikuti G0. Tanpa analytics | Default |
| D-01 sampai D-20 | Keputusan Tahap 4 | Berlaku sebagai dasar kerja. ADR tetap *Diusulkan*. Bila Anda menyetujui Tahap 5 tanpa keberatan atas Tahap 4, keduanya dianggap disetujui sebagai dasar kerja, dan dicatat ulang di Tahap 7 | A-T5-04 |

| ID | Asumsi Tahap 5 | Dampak bila salah |
|---|---|---|
| A-T5-01 | Satu fase dikerjakan pada satu waktu. Implementasi oleh agen (Antigravity CLI), pemeriksaan deterministik, audit AI terpisah, lalu persetujuan pemilik (K-06, NFR-11) | Urutan dan waktu berubah |
| A-T5-02 | Rentang jam per ukuran: S ≈ 4–8 jam, M ≈ 8–16, L ≈ 16–30. **Tebakan awal, bukan hasil ukur.** Tidak termasuk waktu pemilik untuk menulis konten, menguji di perangkat, dan meninjau (perkiraan 1–2 jam per fase) | Kalender bergeser. Dikalibrasi setelah P01 |
| A-T5-03 | Konsep A cukup mewakili beban teknis konsep lain (tipografi besar, reveal kata, tanpa gambar berat) untuk keperluan pengukuran | S-01 perlu diulang bila konsep akhir berbeda jauh |
| A-T5-04 | Pemilik menerima D-01 sampai D-20 sebagai dasar kerja | Fase terkait direvisi |
| A-T5-05 | Perangkat uji tersedia kapan pun dibutuhkan: POCO M4 Pro dan laptop (K-05). Versi Chrome dan Node belum diketahui (V-01, V-02) | S-02, S-04 tertunda |
| A-T5-06 | Satu cabang per fase (`phase-NN-slug`) digabung oleh pemilik. Agen tidak menggabung atau mendorong ke `main` | Aturan percabangan disesuaikan di Tahap 8 |
| A-T5-07 | Antigravity CLI 1.3.3 dapat dijalankan fase demi fase. **Belum diverifikasi** (V-04, R-18) | Tahap 8 dan 9 disesuaikan. Tidak menghambat Tahap 5 |

## 3. Aturan lintas fase (mengikat setiap fase)

1. **Satu fase pada satu waktu.** Fase tidak dimulai sebelum fase prasyarat disetujui pemilik (NFR-11.2).
2. **Fase tidak meluluskan dirinya sendiri.** Kelulusan butuh: (a) pemeriksaan deterministik dengan kode keluar, (b) bukti uji manual bila disyaratkan, (c) audit AI terpisah sebagai masukan, (d) persetujuan pemilik. Skrip tidak menulis "PASS" sebagai klaim kualitas (`TECHNICAL_DESIGN.md` bagian 6.3).
3. **Larangan keras untuk agen** (diteruskan ke `AGENTS.md` di Tahap 8):
   - mengubah visibilitas repositori, menghapus repositori, `force push`, menulis ulang riwayat, atau menghapus cabang jarak jauh;
   - menambahkan nama institusi, data rumah sakit, tangkapan layar asli, atau kredensial ke berkas apa pun;
   - mengubah identitas Git global atau konfigurasi pengamanan;
   - memasang dependency yang tidak ada di daftar atau tanpa catatan alasan, versi, lisensi (NFR-07.1, NFR-07.2);
   - memakai `--dangerously-skip-permissions`;
   - mengubah dokumen Tahap 1 sampai 4 tanpa catatan perubahan dan persetujuan.
4. **Tindakan destruktif atau perubahan izin keamanan** selalu menunggu persetujuan eksplisit pemilik.
5. **Setiap fase** menghasilkan laporan: berkas diubah, kriteria dan bukti (A/B/C), temuan yang belum selesai, dan keputusan yang menunggu pemilik. Temuan tidak disembunyikan (NFR-10.4).
6. **Kategori bukti** (K-05): A = perangkat milik pemilik, B = simulasi DevTools, C = belum diverifikasi. Safari dan Firefox tetap C kecuali diuji (K-10).
7. **Pemotongan scope** mengikuti urutan PRD 4.4. Jam tidak ditambah.
8. **Perubahan kontrak** (`TECHNICAL_DESIGN.md` bagian 6) memerlukan catatan perubahan di laporan fase.
9. **Spike** disimpan di `spikes/S-0x/`, tidak masuk build produksi, dan hasilnya dicatat dengan format `TR-<tanggal>-<slug>` (lokasi final ditetapkan Tahap 6, sementara `docs/evidence/`). Kode spike menjadi kode produksi hanya bila disetujui.

## 4. Gerbang pemilik

| ID | Gerbang | Yang dibutuhkan | Dibutuhkan sebelum | Bila terlambat |
|---|---|---|---|---|
| **G0** | Kebersihan repositori | (a) Pemilik memeriksa visibilitas repositori lewat jendela tanpa login dan mencatat hasilnya. (b) Pemilik memutuskan O-1 sampai O-5 (QT4-01) dan menjalankan sendiri tindakan pada GitHub bila ada. (c) Pemilik meng-commit dokumen Tahap 1 sampai 5. (d) Pemilik mengatur email commit (opsional, disarankan `noreply`) | **P01 menambah berkas ke remote** | P01 hanya dikerjakan lokal tanpa push. Tidak ada kode didorong ke remote |
| **G1** | Konsep visual dan referensi (QT4-03, P-05) | Pilih A/B/C (boleh dua), 2–3 referensi tambahan | P05 (dikunci). P04 memakai A sementara (A-T5-03) | P05 tidak dimulai |
| **G2** | Versi Chrome, Android, Node, ukuran viewport (V-01, V-02, V-11) | Jawaban tertulis. Cara mengambilnya diberikan di P01 dan P04 | P01 (V-02), P04 (V-01, V-11) | S-02 dan S-04 tidak dapat dijalankan |
| **G3** | Kesiapan konten | Kontak final (Q-06), teks About, bahan case study Flash Sale (Q-13), bahan case study RSUD versi anonim, jawaban Q-20, keputusan headline (QT4-04) | P07 (kontak, About), P08 (case study) | Bagian terkait memakai `[PLACEHOLDER: ...]`. Build rilis tetap gagal (FR-12.5) |
| **G4** | Gerbang rilis (brief bagian 17) | Izin RSUD atau tetap varian anonim (Q-01), peninjauan penggunaan AI, hosting dan domain (Q-19), keputusan CV (DP-11) | P10 | Situs tidak dirilis (NFR-09.6) |

## 5. Ringkasan fase

| Fase | Nama | Ukuran | Jam (A-T5-02) | Gerbang | Menjawab |
|---|---|---|---|---|---|
| **P01** | Fondasi proyek dan bukti toolchain | M | 8–14 | G0, G2 (V-02) | S-04, S-05, V-17, V-20, V-22, V-23, R-25, R-36, R-37 |
| **P02** | Model konten dan validasi | M | 8–14 | — | V-19, FR-12, AC-07, AC-11, AC-16, AC-57, AC-61 |
| **P03** | Pipeline aset dan pemeriksaan build | M | 8–14 | — | V-18, FR-09.6, AC-45 sampai AC-47, AC-63 |
| **P04** | Spike motion: hero, transisi, biaya scroll | L | 14–24 | G2 (V-01, V-11), G1 (sementara A) | S-01, S-02, S-03, V-06, V-14, V-15, R-04, R-26, R-29, R-34, anggaran |
| **P05** | Sistem desain dan kerangka halaman | L | 14–24 | G1 | S-06, V-21, V-24, NFR-02, NFR-03 |
| **P06** | Sistem motion produksi dan hero | L | 16–26 | — | FR-03, FR-10, NFR-05, NFR-06 |
| **P07** | Beranda berbasis konten | L | 14–24 | G3 (kontak, About) | FR-04, FR-06, FR-07, FR-09 |
| **P08** | Case study dan dua project | L | 16–28 | G3 (konten) | FR-05, AC-09 sampai AC-11 |
| **P09** | Transisi halaman, metadata, How I Built This | M | 8–16 | — | FR-08, FR-11, FR-14 |
| **P10** | Pengerasan dan kesiapan rilis | L | 16–28 | G4 | NFR-01, NFR-02, NFR-08, NFR-09, AC-50 |
| | **Total** | | **122–212** | | |

Dengan 6 sampai 10 jam per minggu (K-06), jumlah di atas berarti sekitar **12 sampai 35 minggu** kalender (122 ÷ 10 sampai 212 ÷ 6). Rentang lebar ini sengaja jujur: ia berasal dari tebakan awal (A-T5-02), bukan pengukuran. Setelah P01 selesai, laju nyata dibandingkan dengan 8–14 jam dan seluruh kolom dikalibrasi ulang (A-02). Waktu peninjauan dan uji perangkat pemilik **tidak** termasuk.

## 6. Graf dependensi

```
G0 ─► P01 ─┬─► P02 ─────────────────────────────┐
           │                                     ├─► P07 ─┐
           ├─► P03 ─────────────────────────────┤        ├─► P09 ─► P10
           │                                     └─► P08 ─┘
           └─► P04 ─► P05 ─► P06 ───────────────────┘ (P06 mendahului P07 dan P08)
                      ▲
                     G1
```

Dependensi keras (A ─► B berarti B tidak dimulai sebelum A disetujui):

| Fase | Prasyarat keras | Prasyarat lunak (dapat berjalan paralel dalam pemikiran, tetapi **tetap berurutan** oleh aturan satu fase pada satu waktu) |
|---|---|---|
| P01 | G0 | G2 (V-02) |
| P02 | P01 | — |
| P03 | P01, P02 (butuh skema untuk `check:content` dan `sync-public`) | — |
| P04 | P01 | P03 (alat ukur anggaran). G2 (V-01, V-11) |
| P05 | P04, G1 | P02 (tipe data untuk komponen) |
| P06 | P05, P04 | — |
| P07 | P02, P03, P05, P06 | G3 |
| P08 | P02, P03, P05, P06, P07 (komponen bersama) | G3 |
| P09 | P07, P08, P04 (hasil S-02) | — |
| P10 | P09, P03 | G4 |

**Urutan kerja yang disarankan:** P01 → P02 → P03 → P04 → P05 → P06 → P07 → P08 → P09 → P10. Jalur kritis: P01 → P04 → P05 → P06 → P07 → P08 → P09 → P10. P02 dan P03 berada di luar jalur kritis tetapi harus selesai sebelum P07. Pemilik dapat menulis konten (G3) selama P04 sampai P06 untuk memperpendek penantian.

Alasan P04 sebelum P05: hero diuji lebih awal sebelum desain dikunci (A-07, DP-13). Hasil P04 menentukan varian hero, nilai token motion, dan anggaran ukuran (C-04).

## 7. Rincian fase

Setiap fase memuat: tujuan, ruang lingkup, berkas dan modul, tugas, deliverable, kriteria penerimaan dengan langkah verifikasi, risiko, garis potong (urutan pemotongan), dan kondisi berhenti. Kriteria penerimaan di sini adalah **syarat lulus fase**. Bukti ukuran dan bukti uji manual dikumpulkan sesuai Tahap 6. Tidak ada kriteria yang sudah dinyatakan lulus.

### P01 — Fondasi proyek dan bukti toolchain (M)

**Tujuan.** Repositori yang dapat diinstal, dibuild, dan dihasilkan sebagai static export di Windows 10, serta bukti bahwa asumsi toolchain benar.

**Ruang lingkup.** Kerangka proyek, pengamanan repositori awal, konfigurasi, dan dua spike teknis (S-04, S-05). **Bukan** halaman atau konten.

**Berkas dan modul.** `package.json`, `package-lock.json`, `next.config.ts`, `tsconfig.json`, `.gitignore`, `.gitattributes`, `.editorconfig`, `README.md`, `app/layout.tsx` (kerangka kosong), `src/styles/` (kosong berkomentar), kerangka `scripts/`, `spikes/S-04/`, `spikes/S-05/`, dokumen catatan dependency (`docs/dependencies.md`, nama usulan), konfigurasi lint dengan aturan `CHK-ARCH-01` sampai `07`.

**Tugas.**
1. Pastikan G0 terpenuhi atau kerjakan lokal tanpa push (bagian 4).
2. Catat versi Node dan npm di laptop (V-02). Pin versi (`engines`). Tidak menebak.
3. Buat kerangka Next.js App Router dengan TypeScript `strict` dan Tailwind v4 memakai panduan resmi versi yang dipin (V-22). Tidak memasang dependency di luar bagian 7 `TECHNICAL_DESIGN.md`.
4. Konfigurasi `output: 'export'`, `trailingSlash: true`, `BASE_PATH` (D-04).
5. Buat `.gitignore` sesuai `SECURITY_PRIVACY_ERRORS.md` bagian 3.1, `.gitattributes` (LF), `.editorconfig`.
6. **S-04:** instal bersih (`npm ci`), jalankan server pengembangan, jalankan build. Catat waktu dan galat di laptop. Pilih penjalan skrip TypeScript atau gunakan `.mjs` (V-20, R-37).
7. **S-05:** build prototipe dengan dua rute dummy untuk membuktikan: `next/font/local` pada static export, `BASE_PATH` untuk aset non-Next, route handler `force-static` untuk `robots.txt` dan `sitemap.xml`, bentuk `out/404.html`, dan inventaris skrip inline (`CHK-OUT-13`, V-17, V-23).
8. Catat hasil spike di `docs/evidence/` dengan kategori bukti. Hasil negatif dicatat apa adanya.
9. Catat dependency: nama, versi, lisensi (dibaca dari sumber resmi), alasan.
10. Tulis `README.md` ringkas: prasyarat, instal, jalankan, build (NFR-07.3, bagian awal).

**Deliverable.** Kerangka proyek, laporan S-04 dan S-05, catatan dependency, README awal.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti |
|---|---|---|
| Instal bersih dan build berhasil di laptop pemilik tanpa langkah di luar README | Pemilik menjalankan dari klon baru. Kode keluar dan waktu dicatat | A |
| Keluaran `out/` berisi `index.html` dan `404.html`, dan struktur rute sesuai `trailingSlash: true` | Daftar isi `out/` | A |
| Hasil S-05 menjawab: font lokal, `BASE_PATH`, route handler statis, skrip inline. Setiap jawaban "berfungsi", "tidak berfungsi", atau "tidak dapat dibuktikan" | Laporan spike | A |
| Aturan `CHK-ARCH-01` sampai `07` aktif dan gagal pada fixture pelanggaran sengaja | Fixture lalu hapus | A |
| `git ls-files` tidak memuat `.env*`, `out/`, `.next/`, atau `assets-src/private/` | Perintah dan tinjauan | A |
| Tidak ada nama institusi di berkas baru | Pencarian teks pada pohon kerja | A |
| Catatan dependency lengkap untuk setiap dependency langsung | Tinjauan | A |

**Risiko.** R-25, R-31, R-36, R-37. Hasil negatif S-05 memicu cadangan: `@font-face` manual, skrip `.mjs`, atau tinjau ADR-0001 (syarat tinjau ulang ADR-0001).

**Garis potong.** Tidak ada. P01 adalah fondasi. Bila S-04 gagal berat (toolchain tidak berjalan), **berhenti** dan laporkan.

**Berhenti bila.** Node atau npm tidak dapat menjalankan Next di laptop. G0 belum terpenuhi dan agen berniat melakukan push. Dependency memerlukan lisensi yang tidak dapat diverifikasi.

### P02 — Model konten dan validasi (M)

**Tujuan.** Satu pintu konten yang bertipe dan divalidasi, termasuk semua gerbang integritas (placeholder, klaim, varian RSUD, CV).

**Ruang lingkup.** `src/schema/`, `src/lib/content/`, `content/` berisi data **kerangka berisi placeholder**, `scripts/check-content`, fixture uji. Tidak ada komponen visual.

**Berkas dan modul.** `src/schema/*`, `src/lib/content/*`, `content/site.ts`, `profile.ts`, `contact.ts`, `cv.ts`, `tech.ts`, `groups.ts`, `build-notes.ts`, `projects/index.ts` dan dua berkas project kerangka (satu `single`, satu `variants` dengan hanya varian `anonymized`), `scripts/check-content.*`, `tests/fixtures/content/*` (fiktif).

**Tugas.**
1. Konfirmasi pustaka skema (V-19: lisensi, versi). Bila tidak layak, pakai validator sendiri (R-37).
2. Implementasikan tipe dan skema sesuai `CONTENT_MODEL.md` (ketat, `REQUIRED_LOCALES`).
3. Implementasikan fungsi akses `TECHNICAL_DESIGN.md` bagian 6.1 dengan janji kontraknya (view berisi string biasa, klaim tersaring, `getCv()` null bila belum siap).
4. Implementasikan `check:content` dengan `CHK-CNT-01` sampai `21`, kode keluar 0/1/2, keluaran JSON.
5. Isi `content/` dengan **kerangka**: struktur lengkap, teks `[PLACEHOLDER: ...]`. Teks anonim RSUD memakai "a hospital" (QT4-05). **Tidak ada nama institusi.**
6. Buat fixture untuk setiap aturan penting: placeholder, klaim tidak terkonfirmasi, varian `named` tanpa dan dengan izin fiktif, CV siap dan tidak siap, bahasa kedua dummy, `href` kosong.

**Deliverable.** Skema, akses, validator, data kerangka, fixture, laporan fase.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Entri project uji ditambahkan lalu dihapus menghasilkan perubahan pada daftar tanpa menyentuh komponen (belum ada komponen: diuji pada keluaran `listProjects()`) | Fixture | A | AC-07 |
| Teks bahasa kedua pada satu entri tidak mengubah skema atau fungsi akses | Fixture | A | AC-57 |
| Placeholder uji menggagalkan `check:content` mode rilis, tidak mode dev | Fixture, kode keluar | A | AC-16 |
| Klaim `confirmed: false` tidak ada pada keluaran akses | Fixture | A | AC-61 |
| Varian `named` tanpa izin gagal. Fixture `named` dengan izin fiktif lolos validasi tetapi tidak dipakai pada data nyata | Dua fixture | A | AC-11 |
| CV `ready` tanpa `ownerReviewed` menghasilkan `getCv() === null` | Dua konfigurasi | A | AC-63 |
| Bidang tak dikenal (`location`, `level`) ditolak | Fixture | A | FR-07.2, FR-06.3 |
| Kode keluar 2 saat daftar istilah lokal tidak ada pada mode rilis | Uji | A | CHK-CNT-17 |
| Tidak ada nilai sensitif dicetak pada keluaran temuan | Tinjauan keluaran | A | NFR-08.1 |

**Risiko.** R-37 (pustaka skema). Skema terlalu kaku untuk narasi (AR-03): ditinjau di P08, bukan sekarang.

**Garis potong.** Kontrak akses dan `CHK-CNT-03`, `-06`, `-11`, `-17`, `-18` **tidak boleh dipotong**. `CHK-CNT-16` dan `-21` dapat disederhanakan.

**Berhenti bila.** Aturan validasi tidak dapat dibuat deterministik. Data fiktif ternyata memuat data nyata.

### P03 — Pipeline aset dan pemeriksaan build (M)

**Tujuan.** Hanya yang diizinkan masuk `out/`, dan pemeriksaan `out/` serta repositori berjalan otomatis.

**Ruang lingkup.** `build-images`, `sync-public`, `check-assets`, `check-repo`, `check-out` (aturan yang dapat diuji tanpa halaman nyata), `budgets.json`. Tidak ada komponen visual selain komponen `Img` minimal.

**Berkas dan modul.** `scripts/build-images.*`, `sync-public.*`, `check-assets.*`, `check-repo.*`, `check-out.*`, `scripts/config/budgets.json`, `src/components/ui/Img.tsx`, `src/lib/site/` (`withBasePath`, `SITE_URL`), `assets-src/images/` (gambar dummy kecil), `tests/fixtures/assets/*`.

**Tugas.**
1. **V-18:** pilih alat pemeriksa metadata gambar dan PDF yang berjalan di Windows 10, baca lisensinya dari sumber resmi, uji pada berkas contoh. Bila tidak ada alat yang layak, catat batasannya dan jadikan pemeriksaan manual wajib (C).
2. Konfirmasi pemroses gambar (V-20). Implementasikan `build-images`: varian lebar, WebP + cadangan, manifest, tanpa metadata.
3. Implementasikan `sync-public` dengan gerbang CV (`ASSETS_AND_BUILD.md` bagian 5).
4. Implementasikan `check-assets` (`CHK-AST-01` sampai `05`), `check-repo` (`CHK-REPO-01` sampai `05`), dan `check-out` (`CHK-OUT-01` sampai `13`). Aturan yang butuh HTML nyata diuji dengan fixture HTML.
5. Buat `budgets.json` dengan nilai `null` dan perilaku mode rilis yang gagal (C-04).
6. Implementasikan `Img` dengan `<picture>`, dimensi, `aspect-ratio`, dan fallback.
7. Jalankan `check:repo` pada repositori saat ini dan catat temuan (laporan saja, tanpa tindakan destruktif).

**Deliverable.** Skrip, konfigurasi, fixture, `Img`, laporan fase.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Fixture berisi placeholder, URL eksternal, `data-claim-id` tak terkonfirmasi, `.pdf` tanpa CV siap, `*.map`, string `FAULT_INJECTION` masing-masing menggagalkan aturan `CHK-OUT` yang bersangkutan | Fixture | A | AC-16, AC-47, AC-61, AC-63 |
| Gambar keluaran tidak memuat metadata yang dilarang, dibuktikan oleh alat yang dipilih pada berkas contoh yang sengaja diberi metadata | Berkas contoh | A | AC-46 |
| CV siap + diperiksa menghasilkan berkas di `public/generated/`. Selainnya menghasilkan direktori kosong | Dua konfigurasi | A | AC-63 |
| Mode rilis gagal selama ada nilai `null` pada `budgets.json` | Uji | A | AC-31 |
| `check:repo` berjalan pada repo ini dan menghasilkan laporan tanpa mencetak nilai sensitif | Laporan | A | AC-45 |
| `Img` menjaga ruang saat gambar diblokir | Uji DevTools | A, B | AC-60 |
| Semua skrip berjalan di Windows 10 dengan Node yang dipin | Instalasi bersih | A | AC-51 |

**Risiko.** R-37, R-27. V-18 tidak terjawab: pemeriksaan metadata menjadi manual.

**Garis potong.** `CHK-OUT-13` (laporan skrip inline) tidak boleh dipotong karena menentukan CSP. `CHK-REPO-05` dapat dipotong.

**Berhenti bila.** Tidak ada cara tepercaya memeriksa metadata gambar atau PDF dan pemilik belum menyetujui alur manual.

### P04 — Spike motion: hero, transisi, biaya scroll (L)

**Tujuan.** Bukti nyata di POCO M4 Pro dan laptop sebelum desain dikunci (A-07): hero, transisi halaman, biaya efek scroll. Menetapkan token motion awal dan anggaran ukuran.

**Ruang lingkup.** Tiga spike berdiri sendiri di `spikes/`. Tidak ada kode produksi baru.

**Berkas dan modul.** `spikes/S-01/` (hero), `spikes/S-02/` (dua rute, transisi), `spikes/S-03/` (efek scroll), skrip pengukuran, laporan di `docs/evidence/`.

**Tugas.**
1. Pastikan G2 (V-01 versi Chrome/Android, V-11 viewport). Berikan pemilik cara mengambilnya bila belum ada.
2. **S-01.** Prototipe hero memakai konsep terpilih (atau A sementara, A-T5-03) pada varian **HR-3** dan **HR-1** (HR-2 bila perlu), dengan font lokal dan font cadangan. Ukur di POCO M4 Pro dan laptop: elemen dan waktu kandidat LCP (`PerformanceObserver`), CLS, perilaku saat font berganti, ukuran JS awal. Uji pemecahan kata: Narrator, TalkBack, cari, salin (V-14, R-29, R-34).
3. **S-02.** Dua rute dummy pada static export: `<ViewTransition>` T1, uji tombol back, navigasi cepat berulang, `pointer-events`, perilaku fokus bawaan App Router (V-06, V-24, R-26). T2 diuji hanya bila T1 stabil.
4. **S-03.** Efek scroll: reveal IntersectionObserver pada daftar, paralaks terkontrol dengan CSS, dan hanya bila CSS tidak memadai, GSAP + ScrollTrigger dibaca dari sumber resmi dulu (V-05, V-15) lalu diukur. Catat biaya dan tersendatnya scroll.
5. Tetapkan kandidat **nilai token motion** (`MOTION_SPEC.md` bagian 3) dan **anggaran ukuran** (`budgets.json`) dari hasil ukur plus margin. Margin diputuskan pemilik (A-05).
6. Rekomendasikan varian hero dan keputusan GSAP (perlu atau tidak) dengan data. Tulis hasil sebagai usulan untuk disetujui pemilik.

**Deliverable.** Tiga laporan spike dengan kondisi uji (perangkat, versi Chrome, throttling, jaringan, cache, jumlah ulangan; NFR-01.8), usulan nilai token dan anggaran, rekomendasi varian hero dan GSAP.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti |
|---|---|---|
| Setiap laporan menyebut perangkat, versi Chrome, throttling, jaringan, cache, jumlah ulangan, dan kategori bukti | Tinjauan laporan | A |
| S-01 menyatakan elemen kandidat LCP untuk setiap varian, nilai LCP dan CLS terukur, dan hasil uji pembaca layar | Laporan | A |
| S-02 menyatakan apakah T1 berjalan pada static export di POCO M4 Pro, termasuk back dan navigasi cepat | Laporan | A |
| S-03 menyatakan biaya dan kelancaran scroll pada kedua perangkat | Laporan | A (B pelengkap) |
| Hasil yang tidak dapat diukur ditulis "belum terverifikasi", bukan ditebak | Tinjauan | — |
| Usulan anggaran terisi dari data | `budgets.json` usulan | A |
| Pemilik menyetujui varian hero dan keputusan GSAP | Persetujuan tercatat | — |

**Risiko.** R-04, R-26, R-29, R-34. Skenario terburuk: HR-1 gagal LCP, T1 gagal. Keputusan: HR-3 dan T0. Fase berikut tidak terhambat.

**Garis potong.** S-03 paralaks dipotong lebih dulu (PRD 4.4 butir 1 dan 3). S-02 T2 dipotong. **S-01 tidak boleh dipotong.**

**Berhenti bila.** Perangkat uji tidak tersedia. Pemilik belum menjawab G2. Pengukuran tidak dapat diulang konsisten (hasil dicatat sebagai tidak meyakinkan, bukan dibulatkan).

### P05 — Sistem desain dan kerangka halaman (L)

**Tujuan.** Token final, tipografi, kerangka layout yang aksesibel, navigasi, dan halaman 404. Fondasi visual yang dipakai semua bagian.

**Ruang lingkup.** Token, font, layout, header, menu, footer, skip link, 404, batas galat. Belum ada konten bagian beranda selain placeholder.

**Berkas dan modul.** `src/styles/tokens.css`, `base.css`, `forced-colors.css`, `src/fonts/` + `LICENSES/`, `app/layout.tsx`, `app/not-found.tsx`, `app/error.tsx`, `app/global-error.tsx`, `src/components/layout/*`, `src/components/ui/*`, `src/islands/MenuEnhancer.tsx`, `spikes/S-06/`.

**Tugas.**
1. **G1 harus terjawab.** Terapkan konsep terpilih: warna, tipografi, jarak ke `tokens.css`. Hitung ulang kontras setiap pasangan yang dipakai dan catat sebagai komentar (`UI_UX_SPEC.md` bagian 3.1 sebagai metode).
2. Pilih font. **Baca berkas lisensi yang menyertai font** (V-21). Subset sesuai alat V-18. Konfigurasikan `next/font/local` dengan cadangan metrik.
3. Pasang token motion dengan nilai hasil P04 (belum ada efek).
4. Implementasikan `SkipLink`, `SiteHeader`, `NavMenu` (`<details>`), `MenuEnhancer`, `SiteFooter`, `not-found`, `error`, `global-error`.
5. **S-06:** uji menu `<details>` dengan keyboard, Narrator, dan TalkBack. Bila gagal, ganti ke tombol `aria-expanded` (D-16).
6. Terapkan `forced-colors`, indikator fokus, `scroll-padding-top` dari `--header-h`.
7. Verifikasi pemetaan token ke tema Tailwind v4 dari dokumentasi resmi (V-22).
8. Uji pada 320, 360, 768, 1024, 1440 piksel CSS dan zoom 200%.

**Deliverable.** Sistem token dan font, kerangka layout, menu, 404, laporan S-06.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Menu dapat dibuka dan ditutup dengan keyboard dan sentuhan, Escape menutup, fokus kembali, tetap berfungsi tanpa JS | Uji keyboard dan POCO M4 Pro, JS dimatikan | A | AC-04 |
| Tab pertama adalah skip link, fokus tidak tertutup header, anchor tidak tertutup | Uji keyboard di tiga kelas lebar | A | AC-03, AC-33 |
| Kontras setiap pasangan teks-latar dan indikator fokus memenuhi 4,5:1 dan 3:1 (NFR-02.4), tercatat per pasangan | Alat dan sampel manual | A, B | AC-34 |
| Tidak ada scroll horizontal pada 320 dan zoom 200% | Emulasi dan perangkat | A, B | AC-37 |
| Target sentuh ≥ 24 piksel, kontrol sering dipakai ≥ 44 | Pengukuran | A, B | AC-36 |
| `forced-colors` menampilkan fokus dan konten | Uji mode kontras tinggi Windows | A | EC-16 |
| 404 dapat dinavigasi, `lang="en"`, satu `h1`, landmark benar | Alat otomatis dan tinjauan | A, B | AC-15, AC-35 |
| Galat yang sengaja dipicu pada komponen menampilkan fallback, bukan layar kosong | Hook galat | A, B | AC-60 |
| Tidak ada durasi atau easing literal | `CHK-ARCH-05` | A | AC-56 |
| Setiap font punya berkas lisensi | `CHK-AST-04` | A | NFR-07.2 |
| Hasil S-06 tercatat | Laporan | A | AC-38 (sebagian) |

**Risiko.** R-35, R-34, R-28. Konsep terpilih menimbulkan masalah kontras: sesuaikan token, bukan turunkan ambang.

**Garis potong.** Mode warna paksa penuh dapat disederhanakan ke fokus dan batas dasar. Navigasi, skip link, menu, kontras, dan fokus **tidak boleh dipotong** (NFR-02).

**Berhenti bila.** G1 belum terjawab. Lisensi font tidak dapat diverifikasi. Menu tidak dapat dibuat aksesibel pada kedua pendekatan.

### P06 — Sistem motion produksi dan hero (L)

**Tujuan.** Tiga tingkat motion, fail-safe, primitif, dan hero produksi sesuai keputusan P04.

**Ruang lingkup.** `src/motion/`, `MotionRoot`, efek E-01, E-02, E-03, E-05, E-07, E-10, dan komponen `Hero`. GSAP **hanya bila** P04 menetapkannya perlu dan lisensinya terkonfirmasi (V-05).

**Berkas dan modul.** `src/motion/*`, `src/islands/MotionRoot.tsx`, `src/components/sections/Hero.tsx`, `src/components/ui/SplitWords.tsx`, `src/styles/motion.css`.

**Tugas.**
1. Resolver tingkat, `onTierChange`, pembaca token CSS.
2. `mountEffect`, registri, pelepasan, jeda saat tab tersembunyi, F1 sampai F6.
3. Efek E-03, E-05, E-07, E-10 dengan CSS dan IntersectionObserver. E-01 dan E-02 sesuai varian hero yang disetujui di P04.
4. Hero dengan pemecahan kata saat build bila varian memakainya.
5. Hook uji (`data-motion="off"`, `NEXT_PUBLIC_FAULT_INJECTION`) dan `CHK-OUT-10`.
6. Bila GSAP disetujui: pasang pinned, impor dinamis hanya di `src/motion/`, verifikasi API dari sumber resmi (V-15). Bila tidak, tidak dipasang.
7. Jalankan ulang pengukuran S-01 pada hero produksi.

**Deliverable.** Sistem motion, hero produksi, laporan.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Elemen kandidat LCP hero muncul dengan JS dan animasi dimatikan | DevTools, JS mati | A, B | AC-06 |
| Nama, peran, status, akses navigasi terlihat pada viewport pertama 360 × 640 | POCO M4 Pro dan emulasi | A, B | AC-05 |
| Tiga tingkat aktif sesuai kondisi. Ubah ukuran, rotasi, dan ubah preferensi saat sesi menyesuaikan tanpa merusak | Uji manual | A, B | AC-24, AC-59 |
| Reduced motion: tanpa gerak spasial besar, konten identik | Pengaturan sistem dan DevTools | A, B | AC-18 |
| Blokir modul efek: seluruh konten tampil | Blokir JS dan modul | A, B | AC-19 |
| Tidak ada penangkapan scroll | Uji keyboard, anchor, sentuh, tombol browser | A | AC-20 |
| Tidak ada kedipan > 3/detik, tidak ada gerak otomatis > 5 detik tanpa kontrol | Tinjauan animasi | A, B | AC-21 |
| Teks hero terbaca utuh oleh Narrator dan TalkBack, dapat dicari dan disalin | Uji pembaca layar | A | AC-22 |
| Properti utama `transform` dan `opacity` | Tinjauan kode dan profil | A, B | AC-23 |
| Efek berat di POCO M4 Pro disederhanakan menurut K-12 dan ditinjau pemilik | Uji perangkat | A | AC-25 |
| Pelepasan saat ganti rute dan tab tersembunyi, navigasi berulang tidak menumpuk | Profil | A, B | AC-55 |
| Navigasi dapat dipakai saat animasi berjalan, CPU diperlambat | POCO M4 Pro, throttling | A, B | AC-58 |
| Tidak ada literal durasi atau easing | `CHK-ARCH-05` | A | AC-56 |
| Galat efek sengaja tidak menjatuhkan halaman | Hook galat | A, B | AC-60 |
| LCP dan CLS prototipe vs produksi dibandingkan dan dicatat | Pengukuran | A | AC-27, AC-28 (awal) |

**Risiko.** R-04, R-29, R-34. Bila hero produksi lebih buruk daripada prototipe: kembali ke varian aman (K-12).

**Garis potong.** Urutan: paralaks → stagger kaya → variasi motion mobile (PRD 4.4). Reduced motion, fail-safe, dan LCP **tidak boleh dipotong**.

**Berhenti bila.** LCP melampaui 2,5 detik pada varian apa pun yang dipilih dan tidak ada alternatif K-12. Perangkat uji tidak tersedia.

### P07 — Beranda berbasis konten (L)

**Tujuan.** Seluruh bagian beranda dirender dari data: Selected Projects, About, Expertise, Contact, CV.

**Ruang lingkup.** Bagian beranda dan komponennya. Isi nyata sebisa mungkin. Konten yang belum siap tetap placeholder.

**Berkas dan modul.** `app/page.tsx`, `src/components/sections/{SelectedProjects,About,Expertise,Contact}.tsx`, `src/components/ui/{ExternalLink,EmailLink,CvLink,Tag}.tsx`, `content/*` (isi).

**Tugas.**
1. Pastikan G3 (kontak, About). Tanpa itu, placeholder dan fixture.
2. `ProjectCard`, `SelectedProjects` dengan tata letak valid untuk 1 sampai N dan padanan hover/fokus/sentuh.
3. `About` tanpa dan dengan foto. Tidak ada lokasi dan ketersediaan.
4. `Expertise` dari `getExpertiseGroups()`.
5. `Contact`, `EmailLink` (entitas HTML, bisa disalin), `ExternalLink` (atribut keamanan), `CvLink` (muncul hanya bila `getCv()` tidak null).
6. Footer memuat kontak dan CV yang sama.
7. Hubungkan efek reveal (E-03, E-05) dan hover (E-04, E-07).

**Deliverable.** Beranda lengkap dengan data nyata atau placeholder.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Menambah entri project uji memunculkan kartu tanpa mengubah komponen | Fixture | A | AC-07 |
| Setiap efek hover memiliki padanan fokus dan sentuhan, tidak ada informasi hanya lewat hover | Uji keyboard dan sentuh | A | AC-08, AC-39 |
| About bekerja dengan dan tanpa foto. Tidak ada lokasi atau ketersediaan | Dua konfigurasi data | A | AC-12 |
| Expertise tanpa persen, bar, skor. Setiap butir terhubung ke project | Tinjauan data dan DOM | A | AC-13 |
| Tautan eksternal menuju tujuan terkonfirmasi, atribut keamanan benar, tidak ada formulir | Klik dan tinjauan DOM | A | AC-14 |
| Email terlihat dan dapat disalin tanpa aplikasi surel, dapat dijangkau keyboard dan pembaca layar | Uji | A | AC-62 |
| Tautan CV hanya bila CV siap dan diperiksa. Tidak ada tautan mati | Dua konfigurasi | A | AC-63 |
| Tiga kelas lebar rapi, tidak ada overflow, dengan teks uji lebih panjang | Emulasi dan perangkat | A, B | AC-42 |
| Anchor dan navigasi menuju bagian yang benar | Uji manual | A | AC-03 |

**Risiko.** R-24 (dua project tanpa foto terasa kosong), R-06. Penanganan lewat komposisi tipografi (UI_UX_SPEC bagian 7).

**Garis potong.** Bagian Expertise dan About diperpendek (PRD 4.4 butir 6). Kontak dan Projects **tidak boleh dipotong**.

**Berhenti bila.** Konten kontak tidak dikonfirmasi dan pemilik meminta rilis. Data konten sensitif muncul.

### P08 — Case study dan dua project (L)

**Tujuan.** Template case study dan dua case study jujur: Flash Sale Intelligence dan project RSUD (varian anonim).

**Ruang lingkup.** Rute `/projects/[slug]/`, komponen case study, isi dua project. **Hanya varian anonim** untuk RSUD sampai izin terkonfirmasi (D-09). Gambar memakai antarmuka dibuat ulang dengan data dummy.

**Berkas dan modul.** `app/projects/[slug]/page.tsx`, `src/components/case-study/*`, `content/projects/*`, `assets-src/images/*`.

**Tugas.**
1. Pastikan G3 (bahan konten, Q-13, Q-20, QT4-05). Konten yang belum siap tetap placeholder.
2. Implementasikan template (UI_UX_SPEC bagian 2.2) dengan `generateStaticParams`, `dynamicParams = false`, `notFound()` untuk slug tak dikenal.
3. Tulis konten bersama pemilik. Pemilik yang menulis dan mengonfirmasi klaim. Agen tidak mengisi metrik.
4. Buat gambar dummy atau antarmuka yang dibuat ulang. Pemilik menjalankan daftar tinjauan manual (`SECURITY_PRIVACY_ERRORS.md` bagian 3.2) sebelum commit.
5. `ProjectPager` dan tautan ke daftar.
6. Keterangan penggunaan AI per project. Isi project RSUD menunggu Q-20 (placeholder sampai dijawab).
7. Tinjau apakah narasi sebagai data cukup nyaman (AR-03). Catat hasilnya. Bila tidak, ajukan peninjauan ADR-0002 (MDX) sebagai revisi, bukan diam-diam.

**Deliverable.** Dua case study, template, laporan, catatan peninjauan AR-03.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Kedua halaman memuat semua bagian template. Bagian hasil hanya klaim terkonfirmasi atau pernyataan jujur tanpa metrik | Tinjauan terhadap checklist dan tabel klaim | A | AC-09 |
| Keterangan penggunaan AI ada di kedua case study sesuai K-11 | Tinjauan pemilik | A | AC-10 |
| Build tidak memuat varian bernama. Varian rilis sesuai status izin | `CHK-CNT-11`, `-12` dan tinjauan build | A | AC-11 |
| Heading berurutan, `alt` pada media bermakna, halaman terbaca tanpa menunggu animasi | Alat dan tinjauan | A, B | AC-35, FR-05.9 |
| URL case study dapat dibuka langsung dan dimuat ulang | Uji lokal dan, nanti, pada host | A | AC-01 (sebagian) |
| Slug salah menghasilkan 404 yang dapat dinavigasi | Uji | A | AC-15 |
| Tidak ada klaim terlarang | Tinjauan pemilik dan `CHK-CNT-16` | A | AC-49 |
| Tidak ada nama institusi atau data sensitif pada teks, `alt`, nama berkas, dan `out/` | `CHK-OUT-03` (dengan daftar lokal), tinjauan manual | A | AC-46, EC-24 |
| Placeholder yang tersisa didaftar dengan jelas | Laporan | A | FR-12.5 |

**Risiko.** R-01, R-03, R-22. Jangan menunda sisa situs karena konten RSUD (varian anonim adalah jalur rilis).

**Garis potong.** Kedalaman bagian proses dapat diperpendek. Dua case study yang jujur **tidak boleh dipotong** (PRD 4.4). Bila konten RSUD belum siap, tetap dua case study dengan varian anonim dan placeholder yang harus diisi sebelum rilis. Mengurangi jumlah case study memerlukan revisi PRD oleh pemilik, bukan keputusan fase ini.

**Berhenti bila.** Data atau gambar yang tampak sensitif muncul di pohon kerja. Pemilik meminta varian bernama tanpa izin terdokumentasi.

### P09 — Transisi halaman, metadata, dan How I Built This (M)

**Tujuan.** Transisi sesuai hasil S-02, metadata dan berbagi, serta bagian teknis ringkas yang hanya memuat hasil nyata.

**Ruang lingkup.** E-08 (T1) dan E-09 (T2, opsional), fokus dan pengumuman rute, FR-14, `robots.txt`, `sitemap.xml`, `BuildNotes`.

**Berkas dan modul.** `src/motion/effects/pageTransition.*`, `src/islands/RouteAnnouncer.tsx` (bila dibutuhkan), `app/robots.txt/route.ts`, `app/sitemap.xml/route.ts`, `public/og/`, `src/components/sections/BuildNotes.tsx`, `content/build-notes.ts`.

**Tugas.**
1. Implementasikan T1 bila S-02 lulus, selain itu T0 dan catat. T2 hanya bila T1 stabil dan lolos performa. Pemilik diberi tahu bila jatuh ke T0.
2. `::view-transition { pointer-events: none }`, reduced motion, pembatalan saat navigasi cepat.
3. Fokus dan pengumuman setelah navigasi: pakai perilaku bawaan bila terbukti cukup (V-24), selain itu `RouteAnnouncer`.
4. Metadata per rute, pratinjau tautan, favicon, `robots`, `sitemap`. `SITE_URL` wajib pada rilis. Domain belum ada (Q-19), sehingga pratinjau tautan baru dapat diuji penuh setelah domain ada.
5. `BuildNotes`: **hanya** butir yang punya bukti (A/B) dengan kondisi uji, atau dipersingkat (FR-08.5). Tanpa klaim "WCAG 2.2 AA compliant". Pernyataan batasan jujur termasuk Safari dan perangkat Apple belum terverifikasi (K-10).
6. Pastikan tidak ada rute terpisah How I Built This (DP-12).

**Deliverable.** Transisi, metadata, bagian teknis ringkas, laporan.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| Transisi tidak menghalangi navigasi, dapat dibatalkan, fokus dikelola | Uji navigasi cepat dan keyboard di POCO M4 Pro | A | AC-26 |
| Tombol back/forward berfungsi, termasuk saat transisi aktif | Uji berurutan | A | AC-02 |
| Pembersihan efek pada perpindahan rute, tidak menumpuk | Profil dan navigasi berulang | A, B | AC-55 |
| Judul, deskripsi, dan pratinjau benar per rute | Tinjauan metadata, uji pratinjau bila domain ada | B | AC-17 |
| Setiap butir performa dan accessibility di situs punya hasil uji tercatat dengan kategori | Tinjauan silang dengan laporan | A | AC-40, AC-48 |
| Himpunan rute di `out/` sesuai, tanpa rute How I Built This | `CHK-OUT-12` | A | AC-64 |
| Tidak ada pernyataan kepatuhan penuh | Tinjauan konten | A | AC-40 |

**Risiko.** R-09, R-26, R-31, A-09 (bukti belum ada). Bila bukti belum tersedia: bagian ditunda atau dipersingkat, tidak diisi sementara.

**Garis potong.** T2, lalu T1 menjadi T0 (PRD 4.4 butir 2 dan 4), lalu `BuildNotes` dipersingkat (butir 5). Metadata dasar dan fokus rute **tidak boleh dipotong**.

**Berhenti bila.** Transisi merusak fokus atau INP dan tidak ada perbaikan sederhana: pakai T0.

### P10 — Pengerasan dan kesiapan rilis (L)

**Tujuan.** Pengukuran dan uji nyata menyeluruh, audit repositori dan keluaran, dokumentasi, pemilihan host, dan gerbang rilis. Menjalankan Tahap 6 pada situs yang sudah ada.

**Ruang lingkup.** Tidak ada fitur baru. Perbaikan temuan. Pengukuran performa, uji aksesibilitas manual, audit keamanan, dry-run deployment.

**Berkas dan modul.** `docs/evidence/*`, `README.md` lengkap, `scripts/config/budgets.json` (terisi), pengaturan host, daftar periksa rilis.

**Tugas.**
1. Isi anggaran final dari hasil P04 dan pengukuran build (C-04).
2. Pengukuran performa terdokumentasi di POCO M4 Pro dan laptop (LCP, CLS, proxy INP, scroll). Laporan per metrik sebagai hasil uji lab.
3. Uji accessibility: keyboard penuh, fokus, reduced motion, zoom 200%, Narrator, TalkBack, kontras. Verifikasi nomor kriteria WCAG terhadap teks resmi W3C (V-10). Temuan yang belum selesai dicatat.
4. Uji tambahan Firefox (bila V-09 memungkinkan) dan Safari (hanya bila ada cara). Yang tidak diuji ditulis "belum terverifikasi".
5. Audit repositori sesuai RA-01 sampai RA-06 (`SECURITY_PRIVACY_ERRORS.md` bagian 3.4), termasuk riwayat commit. **Tidak ada tindakan destruktif tanpa persetujuan.**
6. `release:verify` dengan `RELEASE=1`. Semua `CHK-*` kode keluar 0.
7. Pilih host berdasarkan kriteria ADR-0009 dan harga terkini (V-08). Tentukan CSP-A atau CSP-B (`SECURITY_PRIVACY_ERRORS.md` bagian 4) berdasarkan `CHK-OUT-13`. Dry-run ke host pratinjau bila ada.
8. Dokumentasi: jalankan, uji, tambah project, rilis (NFR-07.3, UF-11). Uji dengan mengikuti dokumentasi dari awal (AC-51).
9. Uji persepsi kecil bila disetujui (DP-10, AC-53).
10. Checklist rilis untuk pemilik (brief bagian 17).

**Deliverable.** Laporan bukti, dokumentasi, keputusan host, daftar temuan terbuka, checklist rilis.

**Kriteria penerimaan dan verifikasi.**

| Kriteria | Verifikasi | Bukti | AC |
|---|---|---|---|
| LCP ≤ 2,5 s dan CLS ≤ 0,1 pada beranda dan case study, di kedua perangkat, kondisi tercatat | Pengukuran lab terdokumentasi | A (B pelengkap) | AC-27, AC-28, AC-32 |
| Responsivitas interaksi dinilai dengan interaksi nyata. Proxy dicatat sebagai proxy | Pengukuran | A, B | AC-29 |
| Scroll lancar di POCO M4 Pro | Uji dan profil | A | AC-30 |
| Anggaran terisi dan tidak terlampaui | `CHK-OUT-11` | A | AC-31 |
| Seluruh fungsi dapat dioperasikan keyboard, fokus terlihat dan tidak tertutup | Uji terdokumentasi pada semua rute | A | AC-33 |
| Alur utama dilalui dengan Narrator dan TalkBack, temuan dicatat | Skrip uji | A | AC-38 |
| Seluruh alur berjalan di Chrome desktop dan Chrome Android | Uji manual | A | AC-41 |
| Laporan membedakan A, B, C | Tinjauan | A | AC-44 |
| Tidak ada kredensial atau data pribadi di repositori dan riwayat yang dibuka | RA-01 sampai RA-05 | A | AC-45 |
| Tidak ada permintaan jaringan pihak ketiga | `CHK-OUT-02` dan tab jaringan | A, B | AC-47 |
| Setiap klaim di situs punya hasil uji tercatat | Tinjauan silang | A | AC-48 |
| Build rilis lulus semua pemeriksaan | `release:verify` | A | AC-16, AC-50 |
| Repositori berjalan di Windows 10 mengikuti dokumentasi | Instalasi bersih | A | AC-51 |
| Setiap fase telah dilaporkan dan ditinjau | Tinjauan laporan fase | A | AC-52 |
| Gerbang rilis brief bagian 17 terpenuhi dan ditandatangani | Checklist | A | AC-50 |

**Risiko.** R-14, R-15, R-19, R-33. Hasil lab berfluktuasi: ulangi dan catat sebaran.

**Garis potong.** Uji persepsi (AC-53) dan uji Safari. **Pengukuran performa, uji aksesibilitas, audit repositori, dan gerbang rilis tidak boleh dipotong.**

**Berhenti bila.** Gerbang rilis tidak terpenuhi (placeholder, izin, data sensitif, kontak belum terkonfirmasi). Situs **tidak dirilis** (NFR-09.6).

## 8. Matriks cakupan Acceptance Criteria

Setiap AC (AC-01 sampai AC-64) memiliki fase implementasi dan fase verifikasi akhir. P10 adalah verifikasi akhir untuk hampir semuanya. Pernyataan "lulus" baru sah bila bukti tercatat.

| AC | Fase implementasi | Verifikasi akhir | | AC | Fase implementasi | Verifikasi akhir |
|---|---|---|---|---|---|---|
| AC-01 | P07, P08 | P10 (host) | | AC-33 | P05 | P10 |
| AC-02 | P09 | P10 | | AC-34 | P05 | P10 |
| AC-03 | P05, P07 | P10 | | AC-35 | P05, P08 | P10 |
| AC-04 | P05 | P10 | | AC-36 | P05 | P10 |
| AC-05 | P06 | P10 | | AC-37 | P05 | P10 |
| AC-06 | P04, P06 | P10 | | AC-38 | P05 (S-06), P10 | P10 |
| AC-07 | P02, P07 | P10 | | AC-39 | P07 | P10 |
| AC-08 | P07 | P10 | | AC-40 | P09 | P10 |
| AC-09 | P08 | P10 | | AC-41 | P06 sampai P09 | P10 |
| AC-10 | P08 | P10 | | AC-42 | P05, P07 | P10 |
| AC-11 | P02, P08 | P10 | | AC-43 | P10 | P10 |
| AC-12 | P07 | P10 | | AC-44 | P10 | P10 |
| AC-13 | P02, P07 | P10 | | AC-45 | P03 | P10 |
| AC-14 | P07 | P10 | | AC-46 | P03, P08 | P10 |
| AC-15 | P05, P08 | P10 | | AC-47 | P03 | P10 |
| AC-16 | P02, P03 | P10 | | AC-48 | P09 | P10 |
| AC-17 | P09 | P10 | | AC-49 | P08, P09 | P10 |
| AC-18 | P06 | P10 | | AC-50 | P10 | P10 |
| AC-19 | P06 | P10 | | AC-51 | P01 | P10 |
| AC-20 | P06 | P10 | | AC-52 | Semua fase | P10 |
| AC-21 | P06 | P10 | | AC-53 | P10 (opsional, DP-10) | P10 |
| AC-22 | P04, P06 | P10 | | AC-54 | Tidak berlaku (tanpa eksperimen) | — |
| AC-23 | P06 | P10 | | AC-55 | P06, P09 | P10 |
| AC-24 | P06 | P10 | | AC-56 | P05, P06 | P10 |
| AC-25 | P06 | P10 | | AC-57 | P02 | P10 |
| AC-26 | P09 | P10 | | AC-58 | P06 | P10 |
| AC-27 | P04, P06 | P10 | | AC-59 | P06 | P10 |
| AC-28 | P04, P06 | P10 | | AC-60 | P03, P05, P06 | P10 |
| AC-29 | P10 | P10 | | AC-61 | P02, P03 | P10 |
| AC-30 | P10 | P10 | | AC-62 | P07 | P10 |
| AC-31 | P03, P04 | P10 | | AC-63 | P02, P03, P07 | P10 |
| AC-32 | P10 | P10 | | AC-64 | P09 | P10 |

AC-54 hanya berlaku bila sebuah eksperimen dibangun (FR-15, Could). Tidak ada fase eksperimen dalam rencana ini (D-14, ADR-0007).

## 9. Pelacakan spike, item verifikasi, dan risiko

| Item | Dijawab di | Catatan |
|---|---|---|
| S-01 | P04 | Hero. Menentukan varian |
| S-02 | P04 | Transisi pada static export |
| S-03 | P04 | Biaya efek scroll. Menentukan perlu tidaknya GSAP |
| S-04 | P01 | Toolchain di laptop |
| S-05 | P01 | Static export, font, `BASE_PATH`, skrip inline |
| S-06 | P05 | Menu `<details>` dengan pembaca layar |
| V-01, V-11 | G2 sebelum P04 | Pemilik |
| V-02 | G2 sebelum P01 | Pemilik |
| V-05, V-15 | P04 (hanya bila GSAP dipertimbangkan) | Lisensi dan API dari sumber resmi |
| V-06, V-24 | P04 (S-02), P05 (S-06), P09 | Fokus dan transisi |
| V-08 | P10 | Harga dan batas host |
| V-09 | P10 | Cara uji Firefox dan Safari |
| V-10 | P10 | Verifikasi nomor kriteria WCAG |
| V-14 | P04 (S-01) | LCP teks beranimasi |
| V-17, V-23 | P01 (S-05) | Skrip inline, `basePath` |
| V-18 | P03 | Alat metadata |
| V-19 | P02 | Pustaka skema |
| V-20 | P01, P03 | Pemroses gambar, penjalan skrip |
| V-21 | P05 | Lisensi font |
| V-22 | P01, P05 | Tailwind v4 |
| V-04 | Sebelum Tahap 8 | Dokumentasi Antigravity CLI |
| V-03, Q-01 sampai Q-03 | G4 | Izin dan hak (pemilik) |

| Risiko | Fase yang menanganinya | Bila terealisasi |
|---|---|---|
| R-04 performa mobile | P04, P06, P10 | Penyederhanaan K-12, tingkat `adapted` |
| R-25, R-37 toolchain Windows | P01 | Cadangan: skrip `.mjs`, validator sendiri |
| R-26 transisi di POCO | P04 | T0 |
| R-29 LCP vs hero | P04 | HR-3 |
| R-32 repositori publik | G0, P03 (laporan), P10 (audit) | Tindakan oleh pemilik |
| R-33 CSP dan skrip inline | P01 (S-05), P10 | CSP-B atau `<meta>` |
| R-34 font vs animasi | P04, P06 | Font cadangan, HR-3 |
| R-35 menu | P05 | Tombol `aria-expanded` |
| R-36 `next/font`, `basePath`, route handler | P01 | `@font-face` manual |
| R-38 anggaran kosong | P04, P10 | Rilis gagal sampai terisi |
| R-20 kapasitas | Semua | Pemotongan sesuai PRD 4.4 |
| R-08 jadwal | Semua | Fase kecil dapat dihentikan |

Risiko baru Tahap 5:

| ID | Risiko | Tingkat | Mitigasi |
|---|---|---|---|
| R-39 | Perkiraan jam keliru (A-T5-02) dan total 122–212 jam melebihi harapan pemilik | Sedang | Kalibrasi setelah P01. Pemotongan scope, bukan tambahan jam (K-06) |
| R-40 | Konten (G3) menjadi penghambat P07 dan P08 | Sedang | Pemilik menulis konten selama P04 sampai P06. Placeholder tetap menggagalkan build rilis, bukan fase |
| R-41 | Hasil S-01 menunjukkan konsep A tidak mewakili konsep terpilih | Rendah sampai sedang | Ulangi S-01 pada konsep final di awal P05 bila perlu (A-T5-03) |
| R-42 | Satu fase terlalu besar untuk diselesaikan agen dalam satu sesi | Sedang | Tahap 8 memecah tugas dalam fase menjadi langkah dengan titik henti. Fase dapat dihentikan dengan aman (NFR-11.1) |
| R-43 | Tahap 4 belum di-commit ke remote, dokumen lokal dan remote berbeda | Rendah | G0 (c) |

## 10. Yang diserahkan ke tahap berikutnya

| Tahap | Menerima |
|---|---|
| **6 Strategi uji** | Matriks AC (bagian 8), kategori bukti, urutan pemeriksaan P0 sampai P6, lokasi dan format catatan bukti (C-07, `TR-*`), metode ukur (V-07), verifikasi WCAG (V-10), kondisi uji performa, ambang audit kerentanan dependency, kriteria berhenti. Menetapkan alat lint |
| **7 Readiness review** | Konsistensi dokumen 1–5. Khusus: A-T5-04 (persetujuan Tahap 4 sebagai dasar kerja), QT4-02 sampai QT4-06 yang memakai default, G0 sampai G4, urutan P04 sebelum P05, C-04 (anggaran), ketergantungan P03 pada P02 |
| **8 Paket Antigravity** | Sepuluh fase di atas menjadi `phase-01.md` sampai `phase-10.md`. Larangan keras (bagian 3) menjadi `AGENTS.md`. Perintah aktual diverifikasi dari P01 dan V-04 |
| **9 Orchestrator** | Aturan satu fase pada satu waktu, kode keluar 0/1/2, pemisahan implementasi, pemeriksaan deterministik, audit AI. Tidak mengubah visibilitas, menghapus repositori, atau menulis ulang riwayat |

## 11. Pekerjaan pemilik yang perlu dijadwalkan

| Kapan | Pekerjaan pemilik |
|---|---|
| Sebelum P01 | G0: periksa visibilitas, putuskan O-1 sampai O-5, commit dokumen, atur email commit. G2: versi Node, Chrome laptop |
| Sebelum P04 | G2: versi Android dan Chrome di POCO M4 Pro, ukuran viewport. Siapkan perangkat untuk uji |
| Sebelum P05 | G1: pilih konsep visual dan referensi. Putuskan font setelah kandidat dan lisensinya ditunjukkan |
| Selama P04 sampai P06 | Mulai menulis: About, kontak final, bahan Flash Sale, bahan RSUD versi anonim, jawaban Q-20, headline |
| Setiap fase | Tinjau laporan dan setujui, 1–2 jam |
| P08 | Daftar tinjauan manual aset (bagian 3.2 dokumen keamanan) |
| P10 | Uji perangkat, uji pembaca layar, keputusan host, tanda tangan checklist rilis |

## 12. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 13 Oktober 2026 | Draf awal Tahap 5 |
