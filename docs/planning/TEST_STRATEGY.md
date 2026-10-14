# TEST_STRATEGY — Strategi uji dan bukti

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/TEST_STRATEGY.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 14 Oktober 2026 |
| Tahap workflow | Tahap 6 dari 10 (Strategi Uji dan Quality Gates) |
| Status | **Menunggu review dan persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 7)** |
| Dokumen pendamping | `QUALITY_GATES.md` (gerbang per fase dan rilis, kontrak laporan, syarat berhenti) |
| Dasar | `PROJECT_BRIEF.md`, `PRD.md` (AC-01 sampai AC-64), `ARCHITECTURE.md` bagian 15 dan 16, `TECHNICAL_DESIGN.md` dan lima dokumen pendampingnya, `IMPLEMENTATION_PLAN.md` |

## 1. Cara membaca dokumen ini

| Penanda | Arti |
|---|---|
| **TP-xx** | Prosedur uji (dijalankan manusia atau semi-otomatis) |
| **TS-xx** | Suite uji otomatis deterministik (fixture dan kode keluar) |
| **TR-AAAA-BB-CC-slug** | Catatan bukti (*test record*). Format rujukan yang dipakai `reportRef` pada klaim |
| **D-T6-xx** | Keputusan Tahap 6 (usulan, menunggu persetujuan) |
| **V-25 dan seterusnya** | Item verifikasi baru. V-24 adalah yang terakhir di Tahap 4 |
| **EV-13, EV-14** | Evidensi baru yang dibaca dari sumber resmi di Tahap 6 (bagian 2) |
| **A / B / C** | Kategori bukti (K-05): A perangkat milik pemilik, B simulasi, C belum terverifikasi |

Dokumen ini menetapkan **cara membuktikan**. Dokumen ini **tidak** menyatakan satu pun kriteria sudah terpenuhi. Tidak ada tes yang dijalankan di Tahap 6 dan tidak ada kode untuk diuji.

## 2. Verifikasi sumber resmi yang dilakukan di Tahap 6

| ID | Fakta yang dibaca | Sumber | Dampak |
|---|---|---|---|
| EV-13 | WCAG 2.2 berstatus W3C Recommendation, versi 12 Desember 2024. Nomor dan nama kriteria yang dirujuk NFR-02 PRD **cocok** dengan teks resmi: 1.1.1, 1.3.1, 1.4.1, 1.4.3, 1.4.4, 1.4.10, 1.4.11, 2.1.1, 2.1.2, 2.3.1, 2.4.2, 2.4.3, 2.4.4, 2.4.6, 2.4.7, 2.4.11, 2.5.8, 4.1.2. Level: 1.1.1, 1.3.1, 1.4.1, 2.1.1, 2.1.2, 2.3.1, 2.4.2, 2.4.3, 2.4.4 adalah A. 1.4.3, 1.4.4, 1.4.10, 1.4.11, 2.4.6, **2.4.7**, 2.4.11, 2.5.8 adalah AA. **2.4.7 Focus Visible berlevel AA di teks final.** Satu sumber sekunder dan satu catatan draf awal menyebut A. Teks resmi yang dipakai | https://www.w3.org/TR/WCAG22/ (dibaca lewat tautan `w3.org/tr/wcag22/`). Level 4.1.2 tidak terbaca pada bagian yang dimuat | **V-10 selesai** untuk daftar tersebut. Perbaikan tercatat di bagian 3 |
| EV-14 | 2.5.8 Target Size (Minimum): target pointer minimal 24 × 24 piksel CSS **kecuali** (a) spasi: target yang lebih kecil tidak berpotongan dengan lingkaran 24 px milik target lain, (b) fungsi yang sama tersedia pada kontrol lain yang memenuhi syarat, (c) target inline dalam kalimat atau dibatasi tinggi baris, (d) ukuran ditentukan user agent, (e) presentasi esensial. 1.4.3: teks normal 4,5:1, teks besar 3:1 (≥ 18 pt atau ≥ 14 pt tebal). 1.4.11: 3:1 untuk komponen UI dan objek grafis. 1.4.10: tanpa gulir dua dimensi pada lebar setara 320 piksel CSS. 2.2.2: gerak otomatis yang berlangsung lebih dari lima detik dan berdampingan dengan konten lain perlu mekanisme jeda, hentikan, atau sembunyikan. 2.3.1: tidak lebih dari tiga kilatan per detik atau di bawah ambang | sama | Memperjelas AC-36, AC-34, AC-37, AC-21 |
| EV-15 | Debug jarak jauh Chrome Android dari Windows: aktifkan opsi pengembang dan USB debugging di ponsel, buka `chrome://inspect#devices` di komputer, centang *Discover USB devices*, setiap tab Chrome di ponsel muncul dengan DevTools sendiri. Halaman itu juga menampilkan **nomor seri perangkat** | https://developer.chrome.com/docs/devtools/remote-debugging | Dasar TP-PERF-01 untuk POCO M4 Pro. Nomor seri tidak boleh masuk catatan bukti publik (D-T6-07) |

**Yang belum diverifikasi** (jangan dianggap fakta): cara mengukur INP atau proxy-nya di lab (V-25), perilaku dan bagian *throttling* yang tepat di DevTools versi yang dipakai (V-25), perintah resmi Narrator dan TalkBack (V-26), penerusan port USB dari ponsel ke server lokal (V-27), apakah penjalan uji bawaan Node tersedia pada versi Node laptop (V-28), kriteria dan level 4.1.3 dan beberapa kriteria tambahan pada bagian 3.2 (dibaca dari teks resmi saat prosedur ditulis di fase terkait).

## 3. Dampak terhadap dokumen sebelumnya (untuk Tahap 7)

Tidak ada dokumen diubah di Tahap 6. Temuan berikut dicatat untuk review konsistensi.

| ID | Dokumen | Temuan | Usulan |
|---|---|---|---|
| I-01 | `PRD.md` NFR-02.2 | Merujuk 2.4.7 tanpa level. Level resmi: AA | Tidak berdampak pada isi kebutuhan. Catat |
| I-02 | `PRD.md` NFR-02.8, AC-36 | 2.5.8 memiliki lima pengecualian (EV-14). PRD menulis minimum 24 × 24 tanpa pengecualian | Prosedur TP-RESP-02 menerapkan pengecualian resmi. Target kunci tetap ≥ 44 px (UI_UX_SPEC 9.2) |
| I-03 | `PRD.md` NFR-02 | Kriteria yang relevan tetapi tidak disebut: 2.4.1 Bypass Blocks (skip link), 2.2.2 Pause, Stop, Hide (gerak otomatis > 5 detik), 3.1.1 Language of Page, 3.2.3 Consistent Navigation, 1.4.12 Text Spacing, 1.4.13 Content on Hover or Focus, 4.1.3 Status Messages | Ditambahkan sebagai **butir uji tambahan** di bagian 3.2. Tidak menambah klaim kepatuhan (K-08) |
| I-04 | `CONTENT_MODEL.md` `CHK-CNT-07` | Memeriksa format `reportRef` saja | Setelah Tahap 6, ditambah pemeriksaan **keberadaan** `docs/evidence/<reportRef>/report.md`. Diterapkan di P02 |
| I-05 | `IMPLEMENTATION_PLAN.md` P03 | Tidak ada server statis untuk menguji `out/` secara lokal dan di perangkat | Tambah `scripts/serve-out.*` kecil tanpa dependency di P03 (D-T6-05) |
| I-06 | `IMPLEMENTATION_PLAN.md` P01/P05 | Pemilihan alat lint dan alat a11y otomatis ditunda ke Tahap 6 | Diputuskan di D-T6-01 dan D-T6-03 |
| I-07 | `ARCHITECTURE.md` bagian 15 | P4 menyebut "kategori bukti A/B/C" tanpa definisi bentuk catatan | Didefinisikan di bagian 5 |
| I-08 | `SECURITY_PRIVACY_ERRORS.md` bagian 5 | Ambang audit kerentanan dependency ditunda ke Tahap 6 | Ditetapkan di D-T6-08 |

### 3.1 Daftar kriteria WCAG yang diuji

Kolom "Dirujuk PRD" berarti disebut di NFR-02. "Tambahan" berarti butir uji baru (I-03). Level hanya ditulis bila terbaca dari teks resmi (EV-13).

| SC | Nama | Level | Dirujuk PRD | Diuji lewat |
|---|---|---|---|---|
| 1.1.1 | Non-text Content | A | Ya | TP-A11Y-03, TP-A11Y-05 |
| 1.3.1 | Info and Relationships | A | Ya | TP-A11Y-01, alat otomatis |
| 1.4.1 | Use of Color | A | Ya | TP-A11Y-03 |
| 1.4.3 | Contrast (Minimum) | AA | Ya | TP-A11Y-03 |
| 1.4.4 | Resize Text | AA | Ya | TP-RESP-01 |
| 1.4.10 | Reflow | AA | Ya | TP-RESP-01 |
| 1.4.11 | Non-text Contrast | AA | Ya | TP-A11Y-03 |
| 1.4.12 | Text Spacing | AA | Tambahan | TP-RESP-01 |
| 1.4.13 | Content on Hover or Focus | AA | Tambahan | TP-A11Y-01 (bila ada konten tambahan saat hover/fokus) |
| 2.1.1 | Keyboard | A | Ya | TP-A11Y-01 |
| 2.1.2 | No Keyboard Trap | A | Ya | TP-A11Y-01 |
| 2.2.2 | Pause, Stop, Hide | A | Tambahan | TP-MOT-05 |
| 2.3.1 | Three Flashes or Below Threshold | A | Ya | TP-MOT-05 |
| 2.4.1 | Bypass Blocks | A | Tambahan | TP-A11Y-01 |
| 2.4.2 | Page Titled | A | Ya | alat otomatis, `CHK-OUT-08` |
| 2.4.3 | Focus Order | A | Ya | TP-A11Y-01 |
| 2.4.4 | Link Purpose (In Context) | A | Ya | TP-A11Y-05, TP-A11Y-06 |
| 2.4.6 | Headings and Labels | AA | Ya | TP-A11Y-05 |
| 2.4.7 | Focus Visible | AA | Ya | TP-A11Y-02 |
| 2.4.11 | Focus Not Obscured (Minimum) | AA | Ya | TP-A11Y-02 |
| 2.5.8 | Target Size (Minimum) | AA | Ya | TP-RESP-02 |
| 3.1.1 | Language of Page | A | Tambahan | `CHK-OUT-08` |
| 3.2.3 | Consistent Navigation | AA | Tambahan | TP-A11Y-01 |
| 4.1.2 | Name, Role, Value | (belum terbaca) | Ya | TP-A11Y-05, TP-A11Y-06 |
| 4.1.3 | Status Messages | (belum terbaca) | Tambahan | TP-NAV-02 (pengumuman rute) |

Hasil uji dilaporkan **per kriteria dan per prosedur**. Tidak ada pernyataan "WCAG 2.2 AA compliant" (K-08, FR-08.6, AC-40).

## 4. Keputusan Tahap 6 (semua usulan, menunggu persetujuan)

| ID | Keputusan | Alasan |
|---|---|---|
| D-T6-01 | **Alat lint: ESLint**, dengan aturan batas impor memakai fitur bawaan ESLint untuk pembatasan impor. Versi dan konfigurasi dipin di P01 dan dicocokkan dengan dokumentasi versi terpasang. `next build` tidak menjalankan lint (EV-01), sehingga lint adalah langkah P0 sendiri | `create-next-app` menyertakan ESLint sebagai bawaan (EV-01). Meminimalkan alat tambahan. Biome tidak dievaluasi |
| D-T6-02 | **Penjalan uji untuk skrip dan validator: penjalan uji bawaan Node** bila tersedia pada versi Node yang dipin (V-28). Cadangan: skrip uji `.mjs` sederhana yang menjalankan fixture dan membandingkan kode keluar. Tanpa framework uji tambahan | Dependency minimal (NFR-07.1) |
| D-T6-03 | **Tanpa kerangka uji browser otomatis (E2E) di MVP.** Uji browser dilakukan lewat prosedur manual berskrip (TP) dan pemeriksaan statis `out/`. Alat a11y otomatis: **panel Lighthouse di Chrome DevTools** untuk pemeriksaan aksesibilitas otomatis sebagai bahan, bukan bukti kepatuhan. Penambahan pemeriksa otomatis lain memerlukan catatan dependency | Kerangka E2E menambah dependency dan beban pemeliharaan, sementara perangkat nyata (POCO, Narrator, TalkBack) tidak dapat diotomasi |
| D-T6-04 | Uji deterministik (TS-xx) memakai **fixture fiktif** di `tests/fixtures/`. Tidak ada data nyata, tidak ada nama institusi | R-01, R-03 |
| D-T6-05 | Server statis lokal `scripts/serve-out.*` tanpa dependency, mengikat `127.0.0.1` secara bawaan. Penggunaan melalui jaringan lokal untuk uji ponsel hanya dengan opsi eksplisit dan peringatan | Menguji artefak `out/`, bukan server pengembangan. Cara ponsel mengakses server lokal (USB atau Wi-Fi) diverifikasi (V-27) |
| D-T6-06 | **Catatan bukti** disimpan di `docs/evidence/TR-AAAA-BB-CC-slug/report.md` dan diindeks di `docs/evidence/INDEX.md`. Hasil memakai kosakata `MET`, `NOT_MET`, `INCONCLUSIVE`, `NOT_TESTED`. Kata "PASS" tidak dipakai sebagai klaim | Bukti terikat pada commit dan kondisi, bukan label |
| D-T6-07 | Catatan bukti **tidak memuat** nomor seri perangkat, email, jalur pribadi, nama institusi, atau tangkapan layar yang belum ditinjau. Berkas besar (jejak performa) disimpan lokal di luar repositori dan hanya hash serta nama berkasnya dicatat | NFR-08.1, EV-15 |
| D-T6-08 | Audit kerentanan dependency: dijalankan sebelum rilis dengan alat bawaan pengelola paket. **Ambang:** tidak ada temuan tingkat *high* atau *critical* pada dependency yang ikut ke `out/` tanpa catatan dan keputusan pemilik. Temuan pada dependency pengembangan dicatat dan dinilai. Hasil dicatat apa adanya, tidak ada klaim "bebas kerentanan" | Kehati-hatian proporsional |
| D-T6-09 | Aturan lulus performa lab: **setiap ulangan** harus ≤ batas (NFR-01.1). Bila median ≤ batas tetapi ada ulangan di atas batas, hasil `INCONCLUSIVE`, ulangi hingga ≥ 10 ulangan dan sebarannya dilaporkan, dan **pemilik memutuskan** | Konservatif. Mengakui fluktuasi lab (R-14). Nilai ini [SEMENTARA] |
| D-T6-10 | Evidensi terikat pada **commit SHA**. Perubahan yang memengaruhi rute terukur membatalkan bukti performa dan aksesibilitas terkait dan memerlukan pengukuran ulang (`QUALITY_GATES.md` bagian 8) | Mencegah bukti usang |

## 5. Model bukti

### 5.1 Kategori

| Kategori | Dipakai untuk | Contoh |
|---|---|---|
| **A** | Dijalankan pada perangkat milik pemilik | Chrome di POCO M4 Pro, Chrome di laptop, Narrator di Windows 10, TalkBack di POCO |
| **B** | Simulasi | Emulasi layar DevTools, *throttling*, emulasi `prefers-reduced-motion` |
| **C** | Tidak diuji | Safari, iOS, Firefox bila tidak diuji, tablet, perangkat Android lain |
| **S** (tambahan, bukan kategori perangkat) | Pemeriksaan statis deterministik pada sumber atau `out/` (`CHK-*`, tes fixture) | Dilaporkan dengan label `S` dan kode keluar. Tidak menggantikan A atau B bila AC mensyaratkan perilaku di browser |

Pernyataan di situs hanya boleh menyebut A dan B (K-05). Hasil `S` tidak dipakai sebagai klaim performa atau aksesibilitas.

### 5.2 Format catatan bukti (`report.md`)

Wajib ada setiap bagian berikut:

| Bagian | Isi |
|---|---|
| ID dan judul | `TR-2026-11-02-hero-lcp-poco`, dengan slug huruf kecil dan tanda hubung |
| Tujuan dan AC | Daftar AC atau SC yang diuji |
| Prosedur | ID TP dan versinya |
| Identitas build | Commit SHA, mode build (dev atau `RELEASE=1`), tanggal build |
| Lingkungan | Perangkat (nama model saja), versi OS, versi Chrome atau pembaca layar, mode (peramban biasa atau tanpa ekstensi), *throttling*, jaringan, cache (dingin atau hangat), jumlah ulangan |
| Hasil mentah | Tabel angka per ulangan atau pengamatan per langkah. Bukan ringkasan semata |
| Hasil | `MET`, `NOT_MET`, `INCONCLUSIVE`, atau `NOT_TESTED`, per AC atau SC |
| Temuan | Daftar temuan dengan tingkat keparahan (`QUALITY_GATES.md` bagian 7). Yang belum selesai ditulis, tidak disembunyikan (NFR-10.4) |
| Keterbatasan | Apa yang tidak diuji dan alasannya |
| Pelaku | "Pemilik" atau "Claude (analisis)". Siapa yang menjalankan di perangkat |
| Kategori bukti | A, B, C, atau S |

`docs/evidence/INDEX.md` memuat satu baris per catatan: ID, tanggal, AC, hasil, commit, kategori.

Hubungan dengan konten: klaim performa atau aksesibilitas di situs memiliki `reportRef` yang menunjuk ke satu `TR-...`. Pemeriksaan konten memastikan berkas itu ada (I-04), bahwa hasilnya `MET`, bahwa kategori A atau B, dan bahwa commit-nya tidak lebih lama daripada perubahan yang membatalkan bukti (bagian 8 `QUALITY_GATES.md`).

## 6. Lapisan uji

| Lapisan | Isi | Alat | Sifat | Menjalankan |
|---|---|---|---|---|
| **L0 Statis** | Tipe `strict`, lint, aturan batas modul (`CHK-ARCH-01` sampai `07`) | `tsc` melalui skrip proyek, ESLint | Deterministik | Agen dan pemilik |
| **L1 Unit skrip** | Validator konten, fungsi akses, skrip pipeline dengan fixture (TS-01 sampai TS-12) | Penjalan uji Node (D-T6-02) | Deterministik | Agen dan pemilik |
| **L2 Build dan `out/`** | `build`, `check:out`, `check:assets`, `check:repo` | Skrip proyek | Deterministik | Agen dan pemilik |
| **L3 Perilaku browser terskrip** | Prosedur manual dengan langkah dan data yang dicatat (TP-xx) | Chrome, DevTools, pembaca layar | Manual, bukti A atau B | **Pemilik** (Claude menyusun skrip dan menganalisis hasil yang diberikan) |
| **L4 Audit AI** | Tinjauan terpisah atas diff, laporan, dan keluaran pemeriksaan | Sesi AI terpisah dari implementasi | Masukan. **Tidak menentukan kelulusan** | Agen audit |
| **L5 Persetujuan pemilik** | Gerbang akhir tiap fase dan rilis | Manusia | Wajib | Pemilik |

Urutan ini selaras dengan P0 sampai P6 di `ARCHITECTURE.md` bagian 15 (pemetaan di `QUALITY_GATES.md` bagian 2).

## 7. Uji otomatis deterministik (L1 dan L2)

Setiap suite memiliki fixture masukan, keluaran yang diharapkan, dan kode keluar yang diharapkan. Fixture adalah data fiktif (D-T6-04). Suite dibuat di fase yang tercantum. Kolom "Harus gagal" menyatakan fixture yang **sengaja salah** dan harus ditolak.

| ID | Suite | Fixture masukan | Harus gagal (kode keluar 1) | Harus lolos (0) | Fase | AC / aturan |
|---|---|---|---|---|---|---|
| TS-01 | Skema konten | Bidang tak dikenal (`location`, `level`), bidang wajib hilang, `href` kosong, tautan `javascript:` | Semua | Data valid minimum | P02 | CHK-CNT-01, 15, FR-12.6 |
| TS-02 | Placeholder | String `[PLACEHOLDER: uji]` | Mode rilis | Mode dev: peringatan | P02 | AC-16, CHK-CNT-03 |
| TS-03 | Klaim | Klaim `confirmed: false`, klaim `confirmed: true` tanpa tanggal, klaim performa tanpa `evidence` | Dua terakhir | Tak terkonfirmasi tidak muncul pada keluaran akses | P02 | AC-61, CHK-CNT-06, 07 |
| TS-04 | Varian RSUD | `named` tanpa `permission`, `named` dengan `permission` fiktif tanpa `scope` | Keduanya | `named` + izin fiktif lengkap (validasi saja) | P02 | AC-11, CHK-CNT-11, 12 |
| TS-05 | CV | CV siap tanpa tinjauan, CV tinjauan tanpa berkas | Keduanya | CV siap + diperiksa + berkas ada | P02, P03 | AC-63, CHK-CNT-18 |
| TS-06 | Bahasa kedua | Entri dengan kunci `id` dummy | — | Lolos tanpa perubahan skema | P02 | AC-57 |
| TS-07 | Daftar istilah lokal | Berkas daftar tidak ada (mode rilis) | Kode keluar **2** | Berkas ada dan tidak ada temuan | P02, P03 | CHK-CNT-17 |
| TS-08 | Pemindaian `out/` | HTML fixture dengan: placeholder, `<script src>` eksternal, `data-claim-id` tak terkonfirmasi, `href=""`, dua `h1`, tanpa `lang`, string `FAULT_INJECTION`, `*.map`, `.pdf` tanpa CV siap, rute tak terduga | Masing-masing | HTML fixture bersih | P03 | CHK-OUT-01 sampai 12 |
| TS-09 | Aset | Gambar contoh dengan metadata yang sengaja ditambahkan, PDF contoh bermetadata, font tanpa lisensi | Masing-masing | Berkas bersih | P03 | CHK-AST-02 sampai 04, AC-46 |
| TS-10 | Repositori | Pohon fixture dengan `.env`, pola rahasia fiktif, jalur lokal | Masing-masing | Pohon bersih | P03 | CHK-REPO-01, 02, AC-45 |
| TS-11 | Anggaran | `budgets.json` dengan nilai `null`, ukuran melampaui nilai | Mode rilis | Nilai terisi dan ukuran di bawahnya | P03 | CHK-OUT-11, AC-31 |
| TS-12 | Aturan batas | Berkas fixture yang mengimpor `content/`, `next/image`, `gsap` dari komponen, memakai `localStorage`, durasi literal | Masing-masing | Berkas sesuai aturan | P01 | CHK-ARCH-01 sampai 07 |

Kriteria suite:
1. Setiap suite punya kasus gagal dan kasus lolos.
2. Hasil tidak bergantung pada jaringan, waktu, atau urutan.
3. Keluaran temuan tidak mencetak nilai sensitif (hanya lokasi dan ID aturan).
4. Suite berjalan di Windows 10 dengan Node yang dipin (AC-51).

## 8. Prosedur uji terskrip (L3)

Setiap prosedur memuat langkah ringkas. Skrip lengkap per langkah ditulis di fase yang menjalankannya dan disimpan bersama catatan bukti. Prosedur belum pernah dijalankan.

### 8.1 Performa

**TP-PERF-01 — Pengukuran lab Core Web Vitals** (AC-27, AC-28, AC-29, AC-32, AC-58)

| Aspek | Ketentuan |
|---|---|
| Rute | `/`, `/projects/<slug-1>/`, `/projects/<slug-2>/` |
| Perangkat | POCO M4 Pro, Chrome Android (A). Laptop, Chrome desktop (A). Emulasi (B) hanya sebagai pelengkap |
| Artefak | `out/` hasil `RELEASE=1` bila tersedia, disajikan oleh `serve-out` (D-T6-05). Versi yang diukur dicatat dengan commit SHA |
| Kondisi | Tanpa ekstensi. Cache dingin sebagai kondisi utama, hangat sebagai tambahan. Jaringan dan *throttling* ditulis persis. Pengaturan pada POCO dikunci (hemat daya mati, kecerahan tetap, aplikasi lain ditutup) |
| Ulangan | Minimal 5 per rute per perangkat per kondisi. Semua ulangan dicantumkan |
| Metrik | LCP (beserta **elemen** kandidatnya), CLS (selama muat, animasi masuk, penggantian font, dan gulir), proxy INP (lihat V-25), ukuran JS dan gambar yang dimuat |
| Pengambilan di POCO | Debug jarak jauh via `chrome://inspect` (EV-15). Penerusan port atau jaringan lokal (V-27) |
| Hasil | Tabel per ulangan, median, rentang. Aturan D-T6-09 |
| Catatan | Hasil disebut **hasil uji lab** (NFR-01.8). INP adalah metrik lapangan. Proxy ditulis sebagai proxy (NFR-01.3). Jika proxy tidak dapat diukur: "belum terverifikasi" |

**TP-PERF-02 — Kelancaran scroll di POCO** (AC-30). Menggulir beranda dan case study dari atas ke bawah pada kecepatan normal dan cepat, dengan rekaman profil performa. Pemilik menilai kelancaran dan mencatat patah-patah yang terlihat dan di bagian mana. Hasil gabungan penilaian subjektif dan data profil.

**TP-PERF-03 — Anggaran ukuran** (AC-31). Keluaran `CHK-OUT-11` terhadap `budgets.json`. Kategori S, dilaporkan bersama TP-PERF-01.

### 8.2 Aksesibilitas

**TP-A11Y-01 — Keyboard penuh** (AC-33, AC-04, AC-39, SC 2.1.1, 2.1.2, 2.4.1, 2.4.3, 3.2.3)
1. Muat rute dengan fokus di awal dokumen.
2. Tab pertama harus "Skip to main content". Aktifkan. Fokus harus berada di konten utama.
3. Tab melewati seluruh elemen interaktif hingga akhir dan kembali (Shift+Tab). Catat urutan dan setiap elemen yang tidak dapat dijangkau atau tidak terlihat fokusnya.
4. Buka dan tutup menu (Enter atau Spasi, Escape). Fokus kembali ke pemicu.
5. Aktifkan tautan project dengan Enter. Pastikan halaman berpindah dan fokus berada di tempat yang logis (TP-NAV-02).
6. Catat adanya jebakan fokus (tidak boleh).
7. Ulangi pada tiga kelas lebar dan pada tiap rute.

**TP-A11Y-02 — Fokus terlihat dan tidak tertutup** (AC-33, SC 2.4.7, 2.4.11). Pada setiap elemen interaktif saat difokuskan: indikator terlihat, dan bukan sepenuhnya tertutup header tetap. Ulangi setelah anchor `#...` dan setelah menggulir.

**TP-A11Y-03 — Kontras dan penyampaian informasi** (AC-34, AC-39, SC 1.4.1, 1.4.3, 1.4.11, 1.1.1). Hitung rasio untuk setiap pasangan warna yang dipakai, termasuk teks di atas gambar dan keadaan hover, fokus, aktif. Alat: kalkulator rasio dan panel Lighthouse (bahan). Periksa bahwa informasi tidak hanya lewat warna atau hover. Catat setiap pasangan dan rasionya.

**TP-A11Y-05 — Narrator, Windows 10** (AC-38, AC-22, SC 1.1.1, 1.3.1, 2.4.4, 2.4.6, 4.1.2). Skrip tugas: (1) buka beranda dan dengarkan judul, landmark, dan heading (2) baca hero utuh dan pastikan teks yang dipecah terbaca sebagai kalimat wajar (3) navigasi ke daftar project dan buka case study (4) dengarkan teks alternatif media (5) buka dan tutup menu (6) dengarkan pengumuman setelah navigasi. Perintah Narrator mengikuti dokumentasi resmi Microsoft (V-26). Catat apa yang didengar, bukan hanya "berhasil".

**TP-A11Y-06 — TalkBack, POCO M4 Pro** (AC-38, AC-22). Tugas yang sama dengan TP-A11Y-05. Perintah mengikuti dokumentasi resmi (V-26).

**TP-A11Y-07 — Reduced motion** (AC-18, AC-59, SC 2.3.1). Aktifkan `prefers-reduced-motion: reduce` lewat pengaturan sistem (A) dan emulasi DevTools (B). Pada hero, daftar project, transisi halaman: tidak ada gerak spasial besar. Semua konten dan fungsi sama. Ubah pengaturan saat sesi berjalan dan catat perilakunya.

**TP-A11Y-08 — Warna paksa** (EC-16). Aktifkan mode kontras tinggi Windows. Fokus dan konten terlihat. Catat bagian yang hilang.

### 8.3 Motion dan ketahanan

**TP-MOT-01 — Tanpa JS / skrip diblokir** (AC-19, AC-06). (1) Matikan JavaScript. Seluruh teks, tautan, anchor, dan menu berfungsi dan tidak ada elemen tersembunyi. (2) Blokir hanya berkas JS motion. Konten tampil dan dapat dibaca. (3) Verifikasi elemen kandidat LCP tetap tampil dengan animasi dimatikan.

**TP-MOT-02 — Injeksi galat** (AC-60, AC-19). Pada build uji dengan `NEXT_PUBLIC_FAULT_INJECTION=1`: picu galat pada satu efek dan pada satu pulau interaksi. Halaman tidak kosong, efek lain berjalan, ada fallback dapat dinavigasi. Blokir gambar: ruang tetap, `alt` terbaca.

**TP-MOT-03 — Tingkat motion** (AC-24, AC-59). Ubah lebar jendela melintasi batas kelas, rotasi layar di POCO, dan preferensi reduced motion saat berjalan. Catat tingkat aktif (`data-motion-tier`) dan bahwa layout tidak rusak.

**TP-MOT-04 — Siklus hidup** (AC-55, AC-58). Navigasi berulang antara beranda dan case study (≥ 20 kali) dan sembunyikan tab. Pantau jumlah pendengar dan node melalui alat DevTools yang tersedia (V-25). Tidak ada pertumbuhan yang terus naik. Animasi dijeda saat tab tersembunyi. Dengan CPU diperlambat, navigasi tetap dapat dipakai saat animasi masuk berjalan.

**TP-MOT-05 — Kedipan dan gerak otomatis** (AC-21, SC 2.3.1, 2.2.2). Tinjau seluruh animasi: tidak ada kedipan > 3 kali per detik. Tidak ada gerak otomatis > 5 detik tanpa kontrol atau alasan tercatat.

**TP-MOT-06 — Scroll native** (AC-20). Uji keyboard (panah, Page Down, Home/End, Spasi), anchor, sentuhan, roda, dan tombol back. Tidak ada penangkapan.

**TP-MOT-07 — Efek berat di POCO** (AC-25). Pemilik memeriksa tiap efek di POCO. Efek yang tersendat dicatat dan diturunkan menurut tangga K-12. Hasil memuat ID efek, tingkat yang dipakai, dan penilaian pemilik bahwa karakter visual inti masih ada.

### 8.4 Navigasi dan fungsi

**TP-NAV-01 — Rute dan navigasi** (AC-01, AC-02, AC-03, AC-15). Buka setiap URL langsung, muat ulang, dan bagikan. Back dan forward setelah beberapa perpindahan dan saat transisi aktif. Anchor di tiga kelas lebar. Rute tidak dikenal menampilkan 404 yang dapat dinavigasi. Dijalankan lokal dan lagi pada host pilihan (P10).

**TP-NAV-02 — Fokus dan pengumuman rute** (AC-26, SC 4.1.3 tambahan). Setelah navigasi klien: letak fokus, judul yang diumumkan, posisi gulir. Navigasi cepat berulang. Hasil dicatat untuk keyboard, Narrator, TalkBack (V-24).

**TP-NAV-03 — Kontak dan CV** (AC-14, AC-62, AC-63). Klik setiap tautan keluar, periksa atribut keamanan di DOM, uji tanpa aplikasi surel (alamat terlihat dan dapat disalin), dua konfigurasi CV.

### 8.5 Responsif

**TP-RESP-01 — Lebar dan zoom** (AC-37, AC-42, AC-05, SC 1.4.4, 1.4.10, 1.4.12). Uji 320, 360, 768, 1024, 1440 piksel CSS (B) dan perangkat nyata (A). Zoom teks 200%. Gulir horizontal tidak muncul. Teks uji lebih panjang daripada asli (AC-42, EC-22). Terapkan ukuran teks 200% dan penggantian spasi teks sesuai SC 1.4.12 dan catat kehilangan konten.

**TP-RESP-02 — Target sentuh** (AC-36, SC 2.5.8). Ukur setiap target pointer. Terapkan pengecualian resmi (EV-14). Bandingkan dengan target desain ≥ 44 px untuk kontrol sering dipakai. Uji sentuhan di POCO dan catat kesulitan.

### 8.6 Keamanan dan integritas

**TP-SEC-01 — Audit repositori** (AC-45). RA-01 sampai RA-06 (`SECURITY_PRIVACY_ERRORS.md` bagian 3.4) termasuk riwayat commit dan metadata penulis. Pemilik menandatangani daftar tinjauan manual aset. **Tidak ada tindakan destruktif tanpa persetujuan eksplisit.**

**TP-SEC-02 — Jaringan pihak ketiga** (AC-47). Buka semua rute dengan tab Network, cache dikosongkan. Tidak ada permintaan ke host selain asal situs. Dikombinasikan dengan `CHK-OUT-02`.

**TP-SEC-03 — Metadata aset** (AC-46). Unduh satu per satu gambar dan CV dari `out/` dan periksa metadata dengan alat terpilih (V-18). Periksa nama berkas dan `alt`.

**TP-SEC-04 — Header di host** (NFR-08.6). Ambil header respons beranda, aset, dan 404 pada host pilihan. Bandingkan dengan daftar tujuan (`ASSETS_AND_BUILD.md` bagian 9.3). Uji CSP dengan membuka semua rute dan memeriksa konsol terhadap pelanggaran. Hasil menyatakan header yang **terpasang**, bukan yang diinginkan.

**TP-SEC-05 — Audit dependency** (D-T6-08). Hasil alat audit paket, catatan lisensi untuk semua dependency langsung, dan tinjauan skrip siklus hidup.

### 8.7 Konten

**TP-CONT-01 — Konten dan klaim** (AC-09, AC-10, AC-12, AC-13, AC-48, AC-49). Pemilik meninjau setiap halaman terhadap tabel klaim: tidak ada metrik tak terkonfirmasi, tidak ada kata terlarang, hasil hanya yang dikonfirmasi, keterangan penggunaan AI sesuai K-11, tidak ada lokasi atau ketersediaan kerja. Setiap klaim performa atau aksesibilitas ditelusuri ke `TR-...`.

**TP-CONT-02 — Varian RSUD** (AC-11, EC-24). Pemilik memastikan varian yang tampil sesuai status izin. Pencarian istilah dari daftar lokal pada seluruh `out/`. Setiap gambar ditinjau terhadap daftar 3.2 dokumen keamanan.

**TP-CONT-03 — Struktur dan metadata** (AC-35, AC-17, AC-64). Alat otomatis (`CHK-OUT-08`, Lighthouse sebagai bahan) ditambah tinjauan manual terhadap heading, landmark, judul, `lang`, nama kontrol, dan `alt`. Himpunan rute sesuai (`CHK-OUT-12`).

### 8.8 Lingkungan dan proses

**TP-ENV-01 — Instalasi bersih di Windows 10** (AC-51). Pemilik mengikuti `README.md` dari klon baru: instal, jalankan, uji, build, tambah project uji (UF-11), hapus. Catat setiap langkah yang tidak dapat diikuti.

**TP-BROW-01 — Firefox dan Safari** (AC-43). Firefox: uji konten, navigasi, layout inti bila dapat dijalankan (V-09). Safari: tidak ada perangkat, ditulis "belum terverifikasi" (C) kecuali ada cara uji yang disetujui. Tidak ada klaim lintas-browser tanpa bukti.

**TP-PERC-01 — Uji persepsi (opsional)** (AC-53, DP-10). Hanya bila pemilik menyetujui. Tanpa itu, T-01 dinilai lewat tinjauan terhadap UF-01 dan UF-02 dan hasilnya ditulis sebagai tinjauan, bukan uji pengguna.

## 9. Pemetaan AC ke uji

Kolom "Eksekusi awal" adalah fase tempat uji **pertama kali dijalankan**. "Eksekusi akhir" adalah pengukuran yang menjadi dasar gerbang rilis, dilakukan pada build rilis di P10. Hasil awal tidak menggantikan hasil akhir.

| AC | Uji | Otomatis | Bukti | Eksekusi awal |
|---|---|---|---|---|
| AC-01 | TP-NAV-01 | — | A, B | P08 (lokal), P10 (host) |
| AC-02 | TP-NAV-01 | — | A | P09 |
| AC-03 | TP-NAV-01, TP-A11Y-01 | — | A, B | P05 |
| AC-04 | TP-A11Y-01 | — | A | P05 |
| AC-05 | TP-RESP-01 | — | A, B | P06 |
| AC-06 | TP-MOT-01, TP-PERF-01 | — | A, B | P04 |
| AC-07 | TS-01 sampai TS-06, TP-CONT-01 | Ya | A | P02, P07 |
| AC-08 | TP-A11Y-01 (hover vs fokus), uji sentuh | — | A | P07 |
| AC-09 | TP-CONT-01 | — | A | P08 |
| AC-10 | TP-CONT-01 | — | A | P08 |
| AC-11 | TS-04, TP-CONT-02 | Ya | A | P02, P08 |
| AC-12 | TP-CONT-01 (dua konfigurasi data) | Sebagian | A | P07 |
| AC-13 | TP-CONT-01, TS-01 | Sebagian | A | P07 |
| AC-14 | TP-NAV-03 | — | A | P07 |
| AC-15 | TP-NAV-01, `CHK-OUT-12` | Sebagian | A, B | P05 |
| AC-16 | TS-02, TS-08 | Ya | A (fixture) | P02 |
| AC-17 | TP-CONT-03 | Sebagian | B | P09 |
| AC-18 | TP-A11Y-07 | — | A, B | P06 |
| AC-19 | TP-MOT-01, TP-MOT-02 | — | A, B | P06 |
| AC-20 | TP-MOT-06 | — | A | P06 |
| AC-21 | TP-MOT-05 | — | A, B | P06 |
| AC-22 | TP-A11Y-05, TP-A11Y-06 | — | A | P04 |
| AC-23 | Tinjauan kode dan profil, TP-PERF-02 | Sebagian | A, B | P06 |
| AC-24 | TP-MOT-03 | — | A, B | P06 |
| AC-25 | TP-MOT-07 | — | A | P06 |
| AC-26 | TP-NAV-02 | — | A | P09 |
| AC-27 | TP-PERF-01 | — | A (B pelengkap) | P04 |
| AC-28 | TP-PERF-01 | — | A, B | P04 |
| AC-29 | TP-PERF-01 (proxy, V-25) | — | A, B | P10 |
| AC-30 | TP-PERF-02 | — | A | P10 |
| AC-31 | TS-11, TP-PERF-03 | Ya | S | P03 |
| AC-32 | Tinjauan laporan TP-PERF-01 | — | A | P10 |
| AC-33 | TP-A11Y-01, TP-A11Y-02 | — | A | P05 |
| AC-34 | TP-A11Y-03 | Sebagian | A, B | P05 |
| AC-35 | TP-CONT-03, Lighthouse (bahan) | Sebagian | A, B | P05 |
| AC-36 | TP-RESP-02 | — | A, B | P05 |
| AC-37 | TP-RESP-01 | — | A, B | P05 |
| AC-38 | TP-A11Y-05, TP-A11Y-06 | — | A | P05 (S-06), P10 |
| AC-39 | TP-A11Y-03, tinjauan komponen | — | A | P07 |
| AC-40 | TP-CONT-01 | — | A | P09 |
| AC-41 | TP-NAV-01 pada kedua perangkat | — | A | P10 |
| AC-42 | TP-RESP-01 | — | A, B | P05 |
| AC-43 | TP-BROW-01 | — | A atau C | P10 |
| AC-44 | Tinjauan seluruh catatan bukti | Sebagian | A | P10 |
| AC-45 | TS-10, TP-SEC-01 | Sebagian | A | P03, P10 |
| AC-46 | TS-09, TP-SEC-03 | Sebagian | A | P03 |
| AC-47 | TP-SEC-02, `CHK-OUT-02` | Sebagian | A, B | P03 |
| AC-48 | TP-CONT-01 (penelusuran ke `TR-`) | Sebagian | A | P09 |
| AC-49 | TP-CONT-01, `CHK-CNT-16` | Sebagian | A | P08 |
| AC-50 | `QUALITY_GATES.md` bagian 5 | Sebagian | A | P10 |
| AC-51 | TP-ENV-01 | — | A | P01 |
| AC-52 | Tinjauan laporan fase | — | A | Semua fase |
| AC-53 | TP-PERC-01 (opsional) | — | A | P10 |
| AC-54 | Tidak berlaku (tanpa eksperimen) | — | — | — |
| AC-55 | TP-MOT-04 | — | A, B | P06 |
| AC-56 | `CHK-ARCH-05`, tinjauan | Ya | S, A | P05 |
| AC-57 | TS-06 | Ya | A | P02 |
| AC-58 | TP-MOT-04, TP-PERF-01 | — | A, B | P06 |
| AC-59 | TP-MOT-03, TP-A11Y-07 | — | A, B | P06 |
| AC-60 | TP-MOT-02 | — | A, B | P05 |
| AC-61 | TS-03, TS-08 | Ya | A (fixture) | P02 |
| AC-62 | TP-NAV-03 | — | A | P07 |
| AC-63 | TS-05, TP-NAV-03, TP-SEC-03 | Sebagian | A | P03 |
| AC-64 | `CHK-OUT-12` | Ya | S | P09 |

## 10. Protokol audit AI (L4)

| Aspek | Ketentuan |
|---|---|
| Independensi | Sesi dan instruksi **terpisah** dari sesi implementasi. Pengaudit tidak melihat penalaran sesi implementasi, hanya artefak |
| Masukan | Diff fase, laporan fase, keluaran pemeriksaan deterministik, daftar AC fase, dokumen sumber yang relevan |
| Tugas | (1) Periksa diff terhadap ruang lingkup dan larangan fase. (2) Periksa setiap klaim di laporan terhadap bukti yang dilampirkan. (3) Cari kontradiksi dengan dokumen sumber. (4) Cari data sensitif, dependency tak tercatat, nilai literal yang dilarang. (5) Daftar AC yang diklaim tetapi tidak berbukti |
| Keluaran | Daftar temuan dengan tingkat keparahan, lokasi, dan bukti. Format di `QUALITY_GATES.md` bagian 6.3 |
| Batas | Pengaudit **tidak boleh** menyatakan fase lulus, tidak boleh memberikan "PASS", dan tidak boleh mengubah kode. Hasilnya masukan bagi pemilik (L5). Audit yang tidak menyertakan satu pun temuan tetap harus mencantumkan apa yang diperiksa dan apa yang tidak dapat diperiksa |
| Pencegahan sikofansi | Audit memuat bagian "klaim yang tidak dapat saya verifikasi". Daftar kosong dicurigai |
| Isi prompt | Disusun di Tahap 8 |

## 11. Penanganan temuan

Tingkat keparahan dan aturan blokir ada di `QUALITY_GATES.md` bagian 7. Temuan yang belum selesai dicatat di `docs/evidence/OPEN_FINDINGS.md` dengan ID, tingkat, AC terkait, status, dan keputusan pemilik. Temuan tidak dihapus. Temuan yang diputuskan diterima ditandai **diterima oleh pemilik**, bertanggal, dan beralasan, dan tidak dihitung sebagai `MET`.

## 12. Privasi dan keamanan bukti

1. Tangkapan layar dan berkas bukti ditinjau dengan daftar 3.2 dokumen keamanan sebelum di-commit.
2. Nomor seri perangkat, email, akun, dan jalur pribadi tidak dicatat (D-T6-07, EV-15).
3. Jejak performa dan rekaman layar disimpan lokal. Hanya nama berkas dan hash yang dicatat.
4. Data uji memakai fixture fiktif. Nama institusi tidak muncul di catatan bukti.

## 13. Item verifikasi baru

| ID | Yang diverifikasi | Cara | Kapan |
|---|---|---|---|
| V-25 | Metode mengukur INP atau proxy lab-nya, nama dan lokasi pengaturan *throttling*, alat memantau pendengar dan node di DevTools versi yang dipakai | Dokumentasi resmi Chrome dan web.dev, lalu dijalankan dan ditulis sebagai langkah aktual di catatan bukti | P04 (TP-PERF-01 pertama) |
| V-26 | Perintah resmi Narrator (Windows 10) dan TalkBack | Dokumentasi resmi Microsoft dan Google | P05 (S-06) |
| V-27 | Cara ponsel mengakses server statis laptop (penerusan port USB atau jaringan lokal) beserta implikasi keamanannya | Dokumentasi resmi Chrome, uji nyata | P04 |
| V-28 | Penjalan uji bawaan Node pada versi Node yang dipin | Dokumentasi resmi Node dan uji nyata | P01 |
| V-29 | Level dan teks 4.1.2, 4.1.3, 2.4.1, 2.2.2, 3.1.1, 3.2.3, 1.4.12, 1.4.13 untuk butir uji tambahan | Teks resmi W3C | Saat prosedur terkait ditulis |

## 14. Yang diserahkan

| Ke | Isi |
|---|---|
| Tahap 7 | Daftar I-01 sampai I-08, keputusan D-T6-01 sampai D-T6-10, pemetaan AC bagian 9, V-25 sampai V-29 |
| Tahap 8 | Daftar TS dan TP per fase sebagai tugas konkret dan perintah (setelah diverifikasi). Format laporan (`QUALITY_GATES.md` bagian 6). Prompt audit |
| Tahap 9 | Kontrak kode keluar 0, 1, 2, pemisahan implementasi, uji deterministik, audit, dan persetujuan manual. Status `UNKNOWN` bila kode keluar tidak dapat dipastikan |

## 15. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 14 Oktober 2026 | Draf awal Tahap 6 |
