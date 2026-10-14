# QUALITY_GATES — Gerbang kualitas, bukti kelulusan, dan syarat berhenti

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/QUALITY_GATES.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 14 Oktober 2026 |
| Tahap workflow | Tahap 6 dari 10 (Strategi Uji dan Quality Gates) |
| Status | **Menunggu review dan persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 7)** |
| Dokumen pendamping | `TEST_STRATEGY.md` (prosedur TP, suite TS, format bukti TR, pemetaan AC) |
| Dasar | Brief bagian 17 (gerbang rilis), PRD (NFR-09.6, NFR-10, NFR-11), `ARCHITECTURE.md` bagian 15, `IMPLEMENTATION_PLAN.md` bagian 3 dan 7 |

Dokumen ini menetapkan **syarat sebuah fase atau rilis boleh dinyatakan selesai**, apa buktinya, siapa yang menyatakan, dan kapan pekerjaan harus berhenti. Tidak ada gerbang yang sudah dilewati. Belum ada kode.

## 1. Prinsip

1. **Tidak ada klaim kelulusan tanpa bukti.** Status hanya boleh `MET` bila catatan bukti `TR-...` ada, terikat pada commit, dan hasil mentahnya tercantum (`TEST_STRATEGY.md` bagian 5).
2. **Fase tidak meluluskan dirinya sendiri.** Penyusun kode, pemeriksaan deterministik, audit AI, dan pemilik adalah empat peran yang berbeda. Hanya pemilik yang menyetujui.
3. **Tidak dapat dipastikan sama dengan gagal.** Kode keluar 2, status `UNKNOWN`, batas waktu, atau proses terputus diperlakukan sebagai gagal sampai ditelusuri.
4. **Berhenti lebih baik daripada menebak.** Syarat berhenti (bagian 9) menghentikan pekerjaan dan melapor, bukan melanjutkan dengan asumsi.
5. **Pemotongan scope, bukan jam** (K-06, NFR-11.3). Urutan pemotongan mengikuti PRD 4.4.
6. **Hasil spike adalah laporan, bukan gerbang angka.** Fase spike (P04) lulus bila laporannya lengkap dan jujur, meskipun hasilnya negatif.

## 2. Model gerbang dan pemetaan ke P0–P6

Setiap fase melewati langkah berikut **berurutan**. Kegagalan pada langkah mana pun menghentikan langkah berikutnya.

| Langkah | Nama | Isi | Pelaku | Sifat | Pemetaan ARCHITECTURE bagian 15 |
|---|---|---|---|---|---|
| **G1** | Implementasi | Mengerjakan tugas fase dalam ruang lingkup | Agen (Antigravity) | — | — |
| **G2** | Pemeriksaan statis | Tipe, lint, aturan batas modul | Skrip | Deterministik | P0 |
| **G3** | Pemeriksaan data dan aset | `check:content`, `build:assets`, `check:repo` (sesuai fase) | Skrip | Deterministik | P1 |
| **G4** | Build | Static export | Skrip | Deterministik | P2 |
| **G5** | Pemindaian `out/` | `check:out` | Skrip | Deterministik | P3 |
| **G6** | Bukti uji | Suite TS dan prosedur TP yang disyaratkan fase | Skrip dan pemilik | Deterministik dan manual | P4 |
| **G7** | Audit AI | Tinjauan terpisah | Agen audit | Masukan saja | P5 |
| **G8** | Persetujuan pemilik | Meninjau laporan, bukti, dan audit, lalu menandatangani | Pemilik | Wajib | P6 |

Aturan:
1. Fase hanya **berstatus siap ditinjau** setelah G2 sampai G7 menghasilkan keluaran. Tidak ada G7 tanpa G6 yang lengkap.
2. Fase hanya **disetujui** oleh G8. Persetujuan dicatat (bagian 6.4). Agen tidak membuat atau mengubah catatan persetujuan.
3. Langkah yang tidak relevan pada sebuah fase dicatat "tidak berlaku" beserta alasannya, bukan dilewati diam-diam.
4. Batas percobaan perbaikan: bila pemeriksaan deterministik yang sama gagal pada **tiga percobaan implementasi berturut-turut** [SEMENTARA], pekerjaan **berhenti** dan melapor (bagian 9).

## 3. Gerbang umum (berlaku untuk semua fase)

| ID | Syarat | Cara diperiksa | Sifat |
|---|---|---|---|
| GC-01 | Perubahan berkas hanya di dalam daftar ruang lingkup fase | Membandingkan `git diff --name-only` terhadap daftar izin pada prompt fase | Deterministik |
| GC-02 | Tidak ada larangan keras dilanggar (`IMPLEMENTATION_PLAN.md` bagian 3 butir 3) | Tinjauan audit, pemindaian `CHK-REPO-*`, riwayat perintah | Deterministik dan audit |
| GC-03 | Dependency baru hanya yang tercatat (nama, versi, lisensi dari sumber resmi, alasan) | Perbandingan perubahan `package.json` dan `package-lock.json` terhadap catatan dependency | Deterministik |
| GC-04 | Tidak ada nama institusi, data sensitif, kredensial, atau `[PLACEHOLDER:` pada berkas yang dihasilkan fase di luar yang memang diharapkan | `CHK-REPO-01` sampai `04` bila tersedia, pencarian teks, audit | Deterministik dan audit |
| GC-05 | Setiap AC yang diklaim di laporan fase memiliki catatan bukti `TR-...` | Laporan memuat tabel AC dengan rujukan. `INDEX.md` memuat TR tersebut | Tinjauan |
| GC-06 | Kategori bukti disebut untuk setiap hasil | Tinjauan laporan | Tinjauan |
| GC-07 | Temuan yang belum selesai terdaftar di `OPEN_FINDINGS.md` | Tinjauan | Tinjauan |
| GC-08 | Perintah yang dipakai di laporan sudah diverifikasi (dijalankan) dan ditulis persis dengan kode keluarnya | Laporan memuat tabel perintah, kode keluar, dan waktu | Tinjauan |
| GC-09 | Tidak ada tindakan destruktif atau perubahan izin tanpa persetujuan tercatat | Riwayat perintah, audit | Audit |
| GC-10 | Audit AI tidak memuat temuan S1 yang terbuka (bagian 7) | Laporan audit | Audit dan pemilik |
| GC-11 | Pemilik menyetujui | Catatan persetujuan | Manusia |

## 4. Gerbang per fase

Untuk setiap fase: **pemeriksaan deterministik yang harus berkode keluar 0**, **bukti yang harus ada**, dan **syarat berhenti khusus**. Semua di samping gerbang umum bagian 3. Kriteria penerimaan lengkap ada di `IMPLEMENTATION_PLAN.md` bagian 7 dan tidak diulang di sini.

| Fase | Pemeriksaan deterministik (kode keluar 0) | Bukti yang harus ada (`TR-`) | Syarat berhenti khusus |
|---|---|---|---|
| **P01** | `npm ci` pada klon bersih. P0 (tipe, lint). `build`. TS-12 (aturan batas) | S-04 dan S-05 (hasil dan kondisi, termasuk hasil negatif). TP-ENV-01 (A). Daftar `git ls-files` bersih | G0 belum terpenuhi dan agen akan melakukan push. Node atau npm tidak dapat menjalankan Next di laptop. Lisensi dependency tidak dapat diverifikasi |
| **P02** | P0. `check:content` (TS-01 sampai TS-07). `build` | Hasil TS-01 sampai TS-07 (S) untuk AC-07, 11, 16, 57, 61, 63 (bagian fixture) | Aturan validasi tidak dapat dibuat deterministik. Fixture memuat data nyata |
| **P03** | P0. TS-08 sampai TS-11. `check:assets`, `check:repo`, `check:out` pada fixture. `build` | Hasil TS (S). Keputusan alat metadata (V-18). `check:repo` terhadap repositori ini (laporan) | Tidak ada cara tepercaya memeriksa metadata dan pemilik belum menyetujui alur manual. `check:repo` menemukan data sensitif baru (BERHENTI dan laporkan, bukan menghapus riwayat) |
| **P04** | P0. `build` prototipe di `spikes/` terisolasi dan tidak masuk build produksi | TR untuk S-01, S-02, S-03 dengan kondisi lengkap (TP-PERF-01 pada POCO dan laptop, TP-A11Y-05/06 untuk pemecahan teks). Usulan token dan anggaran. Keputusan pemilik atas varian hero dan GSAP | Perangkat uji tidak tersedia. G2 belum dijawab. Pengukuran tidak konsisten (dicatat `INCONCLUSIVE`, bukan dibulatkan). **Hasil negatif bukan alasan berhenti**, melainkan masukan keputusan |
| **P05** | P0. `check:content`. `build`. `check:out` (aturan struktur dasar) | TP-A11Y-01, 02, 03, TP-RESP-01, 02, TP-MOT-02 (batas galat), S-06 (TP-A11Y-05/06 untuk menu). Lisensi font tercatat | G1 belum terjawab. Lisensi font tidak dapat diverifikasi. Menu tidak dapat dibuat aksesibel pada kedua pendekatan |
| **P06** | P0. `build`. `check:out` (termasuk `CHK-OUT-10`). `CHK-ARCH-05` | TP-MOT-01 sampai 07, TP-A11Y-07, TP-PERF-01 awal (A) | LCP melampaui 2,5 s pada varian apa pun yang dipilih tanpa alternatif K-12. Perangkat uji tidak tersedia |
| **P07** | P0. `check:content`. `build`. `check:out` | TP-A11Y-01 pada beranda, TP-NAV-03, TP-RESP-01, TP-CONT-01 (bagian beranda), TS dengan entri project uji | Konten kontak tidak dikonfirmasi dan pemilik meminta rilis. Data sensitif muncul |
| **P08** | P0. `check:content` (mode rilis boleh menunjukkan placeholder yang didaftar). `build`. `check:out`. `CHK-CNT-11, 12, 16, 17` | TP-CONT-01, TP-CONT-02, TP-NAV-01 (rute case study, lokal), TP-A11Y-01 pada halaman case study. Daftar placeholder tersisa | Gambar atau data yang tampak sensitif di pohon kerja. Permintaan varian bernama tanpa izin terdokumentasi |
| **P09** | P0. `build`. `check:out` (termasuk `CHK-OUT-12`) | TP-NAV-01, TP-NAV-02 (AC-02, AC-26), TP-MOT-04, TP-CONT-03, TP-CONT-01 (bagian BuildNotes dan klaim) | Transisi merusak fokus atau responsivitas dan tidak ada perbaikan sederhana (pakai T0, bukan berhenti). Klaim performa atau aksesibilitas tanpa TR (hapus klaim) |
| **P10** | Seluruh rantai dengan `RELEASE=1`. Bagian 5 | Seluruh bukti bagian 5 | Gerbang rilis tidak terpenuhi: situs **tidak dirilis** (NFR-09.6) |

Catatan: fase hanya menjalankan pemeriksaan yang **sudah dibuat** di fase sebelumnya. Pemeriksaan yang baru dibuat di sebuah fase hanya wajib untuk fase itu dan sesudahnya.

## 5. Gerbang rilis (P10, brief bagian 17)

Situs **tidak boleh dipublikasikan** selama satu pun baris di bawah belum terpenuhi.

| # | Syarat brief bagian 17 | Pemeriksaan deterministik | Bukti manual | Penandatangan |
|---|---|---|---|---|
| 1 | Tidak ada `[PLACEHOLDER:` di konten atau kode | `CHK-CNT-03` (rilis), `CHK-OUT-01`, `CHK-REPO-02` | TP-CONT-01 | Pemilik |
| 2 | Izin RSUD terkonfirmasi, atau varian anonim dengan data dummy | `CHK-CNT-11`, `CHK-CNT-12`, `CHK-CNT-17`, `CHK-OUT-03`, `CHK-REPO-04` | TP-CONT-02. Bila memakai varian bernama: dokumen izin ada di arsip pribadi pemilik, **catatan rujukan** di data. Dokumen itu **tidak** di repositori | Pemilik |
| 3 | Tidak ada data sensitif, kredensial, atau konfigurasi lokal pada aset yang dipublikasikan atau repositori yang dibuka | `CHK-REPO-01` sampai `05`, `CHK-OUT-04`, `CHK-AST-*` | TP-SEC-01 (termasuk riwayat), TP-SEC-03 | Pemilik |
| 4 | Kontak, tautan profil, klaim profesional dikonfirmasi pemilik | `CHK-CNT-15`, `CHK-OUT-07` | TP-NAV-03, TP-CONT-01 | Pemilik |
| 5 | Setiap klaim performa dan accessibility memiliki hasil uji dengan kategori | `CHK-CNT-07` (termasuk keberadaan TR, I-04), `CHK-OUT-06` | TP-CONT-01 menelusuri tiap klaim ke `TR-` | Pemilik |
| 6 | Quality gates terpenuhi dengan bukti nyata | Rantai `release:verify` seluruhnya kode keluar 0 | Syarat tambahan di bawah | Pemilik |
| 7 | Penjelasan penggunaan AI sesuai K-11, ditinjau pemilik | `CHK-CNT-09`, `CHK-CNT-20` | TP-CONT-01 | Pemilik |

Syarat tambahan untuk baris 6 (semuanya harus ada):

| ID | Syarat | Bukti |
|---|---|---|
| RG-01 | Semua fase P01 sampai P09 berstatus disetujui, masing-masing dengan catatan persetujuan | `docs/evidence/approvals/` |
| RG-02 | Bukti performa pada **build rilis**: LCP ≤ 2,5 s, CLS ≤ 0,1, proxy INP dicatat sebagai proxy (≤ 200 ms), pada beranda dan kedua case study, di POCO dan laptop. Aturan D-T6-09 | TR TP-PERF-01 (A) |
| RG-03 | Scroll POCO dinilai pemilik dan didukung profil | TR TP-PERF-02 |
| RG-04 | Anggaran ukuran terisi dan tidak terlampaui | `CHK-OUT-11`, TR TP-PERF-03 |
| RG-05 | Aksesibilitas: keyboard penuh, fokus, kontras, reflow, target, reduced motion, Narrator, TalkBack, temuan dicatat | TR TP-A11Y-01 sampai 08, TP-RESP-01, 02 |
| RG-06 | Tidak ada temuan S1. Setiap temuan S2 diselesaikan atau diterima pemilik secara tertulis (bagian 7). Temuan yang belum selesai ditampilkan jujur | `OPEN_FINDINGS.md` |
| RG-07 | Header keamanan dan CSP terpasang di host dan diuji | TR TP-SEC-04 |
| RG-08 | Audit dependency dan lisensi tercatat | TR TP-SEC-05 |
| RG-09 | Instalasi bersih mengikuti dokumentasi berhasil | TR TP-ENV-01 |
| RG-10 | Bukti diambil dari commit rilis atau commit tanpa perubahan pada jalur yang membatalkan bukti (bagian 8) | `INDEX.md`, `git diff --name-only` |
| RG-11 | Lingkungan: `SITE_URL` final, daftar istilah lokal ada, `NEXT_PUBLIC_FAULT_INJECTION` tidak ada | Output `release:verify` |
| RG-12 | Cakupan browser: Chrome desktop dan Android diuji (A). Safari dan Firefox ditulis sesuai bukti (C bila tidak diuji) | TR TP-BROW-01, TP-NAV-01 |
| RG-13 | Laporan membedakan A, B, C dan tidak memuat "PASS" atau klaim kepatuhan penuh | Tinjauan |
| RG-14 | Pemilik menandatangani checklist rilis (bagian 10) | Catatan persetujuan rilis |

Status rilis hanya boleh `RELEASE_READY` bila semua baris 1–7 dan RG-01 sampai RG-14 `MET`. Sebelum itu, status adalah `NOT_READY` dengan daftar yang kurang.

## 6. Kontrak laporan dan catatan

### 6.1 Laporan fase (`docs/reports/phase-NN.md`, dihasilkan agen implementasi)

Wajib memuat bagian berikut, dalam urutan ini. Bagian yang kosong ditulis "tidak ada", bukan dihapus.

| # | Bagian | Isi |
|---|---|---|
| 1 | Identitas | Fase, cabang, commit dasar dan commit akhir, tanggal, pelaku, versi alat |
| 2 | Kepatuhan ruang lingkup | Daftar berkas diubah. Pernyataan apakah ada berkas di luar daftar izin (GC-01) |
| 3 | Tugas | Setiap tugas fase: selesai, sebagian, tidak dikerjakan, beserta alasan |
| 4 | Perintah dan pemeriksaan | Tabel: perintah (nama logis dan perintah aktual), kode keluar, durasi, berkas laporan. Hanya yang **benar-benar dijalankan** |
| 5 | Bukti | Daftar `TR-...` dengan AC dan kategori. Bukti yang membutuhkan pemilik ditandai "menunggu pemilik" |
| 6 | Status AC | Tabel AC fase: `MET`, `NOT_MET`, `INCONCLUSIVE`, atau `NOT_TESTED`, dengan rujukan. **Tidak ada "PASS"** |
| 7 | Deviasi | Apa yang berbeda dari rencana atau desain, mengapa, dan dokumen yang terdampak |
| 8 | Dependency | Yang ditambah, versi, lisensi, alasan, sumber verifikasi |
| 9 | Temuan | Baru, terbuka, ditutup, dengan tingkat keparahan |
| 10 | Risiko | Perubahan pada risiko yang dikenal dan risiko baru |
| 11 | Keputusan menunggu pemilik | Daftar pertanyaan |
| 12 | Tidak dijalankan | Hal yang belum dicoba dan alasannya |
| 13 | Pernyataan | Kalimat tetap: "Laporan ini tidak menyatakan fase lulus. Kelulusan ditentukan oleh pemilik." |

### 6.2 Berkas status mesin (`reports/phase-NN.status.json`, gitignored)

```json
{
  "phase": "P03",
  "state": "READY_FOR_REVIEW",
  "baseCommit": "<sha>",
  "headCommit": "<sha>",
  "checks": [ { "id": "check:content", "exitCode": 0 }, { "id": "check:out", "exitCode": 1 } ],
  "unknown": [],
  "approved": false
}
```

Aturan: `state` salah satu dari `DRAFT`, `RUNNING`, `READY_FOR_REVIEW`, `BLOCKED`, `STOPPED`, `UNKNOWN`. Nilai `approved` selalu `false` dari agen. Persetujuan hanya dari catatan di 6.4. Status `UNKNOWN` bila kode keluar tidak dapat dipastikan, proses terputus, atau batas waktu terlampaui. Status `UNKNOWN` ditangani sebagai gagal (Tahap 9 merinci penanganannya).

### 6.3 Laporan audit AI (`docs/reports/audit-phase-NN.md`)

| # | Bagian | Isi |
|---|---|---|
| 1 | Identitas | Fase, commit yang diaudit, tanggal, sesi audit terpisah dari sesi implementasi |
| 2 | Yang diperiksa | Berkas, laporan, keluaran pemeriksaan, dokumen sumber |
| 3 | Yang tidak dapat diperiksa | Hal yang butuh perangkat nyata atau pemilik |
| 4 | Temuan | Tabel: ID, tingkat keparahan (S1–S4), lokasi, deskripsi, bukti, AC atau aturan yang terkait |
| 5 | Klaim di laporan fase tanpa bukti | Daftar. Kosong harus dijelaskan |
| 6 | Kepatuhan larangan | Pernyataan per larangan keras GC-02 beserta dasar pemeriksaan |
| 7 | Rekomendasi (tidak mengikat) | `RECOMMEND_FIX`, `RECOMMEND_REVIEW`, atau `CANNOT_ASSESS`. **Tidak ada nilai yang berarti lulus** |

### 6.4 Catatan persetujuan pemilik (`docs/evidence/approvals/APPROVAL-phase-NN.md`)

Dibuat dan di-commit **oleh pemilik**, bukan agen. Isi: fase, commit yang disetujui (SHA), tanggal, daftar temuan terbuka yang diterima dengan alasan, pernyataan persetujuan, dan tanda tangan berupa nama pemilik. Persetujuan hanya berlaku untuk commit yang tercantum. Perubahan sesudahnya pada jalur yang membatalkan bukti memerlukan persetujuan ulang (bagian 8).

## 7. Tingkat keparahan, aturan blokir, dan pengecualian

| Tingkat | Definisi | Contoh |
|---|---|---|
| **S1 Blokir** | Merusak gerbang integritas, keselamatan data, atau akses dasar | Data sensitif atau nama institusi di aset atau `out/`. Placeholder atau klaim tak terkonfirmasi tampil pada rilis. Varian bernama tanpa izin. Konten tak terbaca tanpa JS. Jebakan fokus. LCP atau CLS di atas batas pada build rilis. Bukti dipalsukan atau tidak ada untuk klaim lulus. Kode keluar 2 pada mode rilis |
| **S2 Mayor** | Kriteria tidak terpenuhi tetapi ada jalan keluar, atau hasil tidak meyakinkan | Masalah aksesibilitas yang menghambat tugas tertentu. Hasil performa `INCONCLUSIVE`. Efek di POCO tersendat dan belum diturunkan menurut K-12 |
| **S3 Minor** | Cacat tampilan atau perilaku kecil | Jarak tidak konsisten. Label kurang jelas |
| **S4 Catatan** | Saran atau pengamatan | Perbaikan kode yang tidak mengubah perilaku |

Aturan blokir:
1. Fase **tidak boleh disetujui** selama ada S1 terbuka.
2. S2 terbuka hanya boleh disetujui dengan **penerimaan tertulis pemilik** (alasan, tanggal, rencana) dan hanya bila bukan kategori yang tidak dapat dikecualikan.
3. S3 dan S4 dapat tetap terbuka dan dicatat.

**Tidak dapat dikecualikan** (tidak ada penerimaan pemilik yang menjadikannya `MET`; pemilik dapat memutuskan mengubah ruang lingkup melalui revisi PRD, yang merupakan keputusan lain): baseline aksesibilitas (NFR-02: keyboard, fokus, kontras teks, reflow, struktur), dukungan reduced motion (NFR-05), gerbang integritas konten dan izin (NFR-09, FR-12), batas performa NFR-01.1, keamanan data (NFR-08.1, NFR-08.2).

**Dapat diterima dengan catatan** (S2 atau lebih rendah): Firefox atau Safari tidak teruji (ditulis "belum terverifikasi"), T1 atau T2 tidak aktif (T0), paralaks tidak ada, uji persepsi tidak dilakukan, kedalaman About atau Expertise.

## 8. Invalidasi bukti dan pengujian ulang

| Aturan | Isi |
|---|---|
| Keterikatan | Setiap `TR` mencatat commit SHA. Bukti berlaku untuk commit itu |
| Jalur yang membatalkan bukti performa dan aksesibilitas | `app/`, `src/components/`, `src/styles/`, `src/motion/`, `src/islands/`, `src/fonts/`, `content/` (untuk rute yang terukur), `assets-src/images/`, `next.config.*`, `package-lock.json`, `scripts/config/budgets.json` |
| Jalur yang tidak membatalkan | `docs/**`, `tests/**`, komentar atau berkas tanpa efek pada `out/` (dibuktikan dengan `out/` identik, yaitu hash isi sama, bila ragu) |
| Pemeriksaan | Skrip membandingkan `git diff --name-only <SHA>..HEAD` dengan daftar jalur. Bila ada irisan, bukti ditandai **usang** di `INDEX.md` dan tidak dihitung |
| Bukti untuk rilis | Harus berasal dari commit rilis atau dari commit tanpa irisan terhadap jalur di atas (RG-10) |
| Pengujian ulang | Hanya prosedur yang terdampak. Perubahan token atau font memicu TP-A11Y-03 dan TP-PERF-01. Perubahan motion memicu TP-MOT dan TP-PERF-01. Perubahan konten memicu TP-CONT |
| Hasil lab berfluktuasi | Aturan D-T6-09. Tidak boleh memilih ulangan terbaik |

## 9. Syarat berhenti

### 9.1 Berhenti global (semua fase)

Pekerjaan **berhenti**, status `STOPPED`, dan agen melaporkan bila:

| ID | Kondisi |
|---|---|
| ST-01 | Data sensitif, nama institusi, kredensial, atau data pribadi yang tidak dikonfirmasi ditemukan pada berkas baru atau terlacak |
| ST-02 | Ada instruksi atau kebutuhan melakukan tindakan terlarang: ubah visibilitas, hapus repositori, `force push`, tulis ulang riwayat, ubah izin keamanan, atau pakai `--dangerously-skip-permissions` |
| ST-03 | Pemeriksaan mode rilis berkode keluar 2 (tidak dapat memeriksa), atau status `UNKNOWN` |
| ST-04 | Pemeriksaan deterministik yang sama gagal pada tiga percobaan berturut-turut [SEMENTARA] |
| ST-05 | Ruang lingkup fase terlampaui, atau pekerjaan memerlukan perubahan kontrak tanpa catatan |
| ST-06 | Perintah, alat, atau fitur yang belum diverifikasi akan dipakai (mis. perintah Antigravity yang tidak terdokumentasi) |
| ST-07 | Dependency yang belum tercatat atau berlisensi tidak jelas akan dipasang |
| ST-08 | Bukti A dibutuhkan tetapi perangkat tidak tersedia. Status fase menjadi `BLOCKED` dan **tidak** diganti dengan bukti B tanpa persetujuan pemilik |
| ST-09 | Keluaran pemeriksaan bertentangan dengan laporan (mis. laporan menyatakan `MET` sementara kode keluar bukan 0) |
| ST-10 | Estimasi sisa pekerjaan melampaui kapasitas. Tindakan: usulkan pemotongan (PRD 4.4), bukan tambahan jam |
| ST-11 | Kondisi tak terduga yang membuat agen harus menebak keputusan penting |

### 9.2 Berhenti khusus fase

Tercantum di tabel bagian 4. Syarat berhenti P10 adalah gerbang rilis tidak terpenuhi.

### 9.3 Yang dilakukan saat berhenti

1. Hentikan perubahan. Jangan memperbaiki dengan cara yang melanggar larangan.
2. Simpan keadaan: commit sementara di cabang fase hanya bila aman dan tidak memuat data sensitif. Bila ada data sensitif, **jangan commit**, laporkan lokasinya.
3. Tulis laporan dengan bagian "Keputusan menunggu pemilik" berisi kondisi berhenti, bukti, dan opsi.
4. Tidak melanjutkan ke fase berikutnya.

## 10. Checklist rilis pemilik (tanda tangan)

Ditandatangani pemilik pada catatan `APPROVAL-release.md`. Setiap butir menyebut `TR` atau `CHK` yang menjadi dasar.

| # | Pernyataan pemilik | Dasar | Tanda |
|---|---|---|---|
| 1 | Saya meninjau bahwa tidak ada placeholder, nama institusi yang tidak diizinkan, atau data pribadi di situs | CHK-OUT-01, 03, 04, TP-CONT-02 | ☐ |
| 2 | Varian project RSUD sesuai status izin yang saya pegang | CHK-CNT-11, TP-CONT-02 | ☐ |
| 3 | Kontak, tautan, dan CV yang tampil sudah saya konfirmasi dan periksa | TP-NAV-03 | ☐ |
| 4 | Pernyataan penggunaan AI sesuai praktik sebenarnya dan saya bertanggung jawab atas keputusan, pemeriksaan, dan hasil akhir | TP-CONT-01 | ☐ |
| 5 | Setiap klaim performa atau aksesibilitas punya catatan bukti dan kategorinya benar | CHK-CNT-07, TP-CONT-01 | ☐ |
| 6 | Saya melihat hasil performa dan aksesibilitas apa adanya, termasuk temuan yang belum selesai | RG-02 sampai RG-06 | ☐ |
| 7 | Browser dan perangkat yang tidak diuji ditulis "belum terverifikasi" | RG-12 | ☐ |
| 8 | Header keamanan di host sesuai yang tercatat | RG-07 | ☐ |
| 9 | Repositori hanya dibuka setelah audit selesai, atau tetap privat | TP-SEC-01 | ☐ |
| 10 | Saya menerima daftar temuan terbuka yang terlampir | `OPEN_FINDINGS.md` | ☐ |

## 11. Hal yang belum diputuskan

| ID | Item | Kapan |
|---|---|---|
| D-T6-09 | Aturan lulus performa (semua ulangan ≤ batas) dan jumlah ulangan minimum 5 adalah usulan [SEMENTARA]. Pemilik memutuskan | Tahap 7 |
| ST-04 | Batas tiga percobaan adalah usulan [SEMENTARA] | Tahap 7 |
| V-25 sampai V-29 | Verifikasi metode dan perintah | Fase yang menjalankannya (`TEST_STRATEGY.md` bagian 13) |
| — | Implementasi penegakan GC-01 (daftar izin berkas) dan penanganan `UNKNOWN` | Tahap 8 dan 9 |
| — | Lokasi akhir laporan fase (`docs/reports/`) dan penulisan aksi pemilik | Tahap 8 |
| — | Format tabel persetujuan yang dapat dibaca orchestrator | Tahap 9 |

## 12. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 14 Oktober 2026 | Draf awal Tahap 6 |
