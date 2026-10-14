# SECURITY_PRIVACY_ERRORS — Keamanan, privasi, dan penanganan error

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/SECURITY_PRIVACY_ERRORS.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 (Desain Teknis) |
| Dokumen induk | `docs/planning/TECHNICAL_DESIGN.md` |
| Rujukan | K-07, K-09, R-01, R-02, R-03, R-19, FR-05.5, FR-09.6, FR-09.7, FR-12.5, FR-13, NFR-06, NFR-08, NFR-09, AC-11, AC-45, AC-46, AC-47, AC-60 |
| Status | T4-USULAN. Dokumen ini bukan nasihat hukum. Pertanyaan izin RSUD (R-01, R-02, V-03) tetap pada pemilik dan pihak berwenang |

## 1. Ruang lingkup dan model ancaman

Situs statis tanpa akun, formulir, penyimpanan data pengunjung, atau kode server (K-09). Risiko utama **bukan serangan terhadap server**, melainkan:

1. **Kebocoran data** (RSUD, pribadi) lewat konten, aset, metadata, repositori, riwayat commit, atau hasil build.
2. **Klaim atau konten tak berizin** yang terpublikasi.
3. **Rantai pasok dependency** dan pihak ketiga yang tidak disengaja saat runtime.
4. **Kegagalan halaman** yang menyembunyikan konten atau menghasilkan layar kosong.

| Ancaman | Vektor | Kontrol | Bukti |
|---|---|---|---|
| Data sensitif RSUD terbit | Tangkapan layar asli, teks, metadata, nama berkas, `alt`, repositori | Aturan varian (D-09), gambar dummy, tinjauan manual, `CHK-CNT-11`, `CHK-CNT-17`, `CHK-OUT-03`, audit repositori (bagian 3) | AC-11, AC-45, AC-46 |
| Nama institusi bocor lewat repositori publik | Dokumen perencanaan, riwayat commit | **QT4-01.** Bagian 2 | AC-45 |
| Placeholder atau data uji ikut terbit | Data konten | `CHK-CNT-03`, `CHK-OUT-01` | AC-16 |
| Klaim tak terkonfirmasi terbit | Data klaim | Aksesor menyaring, `CHK-OUT-06` | AC-61 |
| Email penulis commit terlihat | Metadata Git | `CHK-REPO-03`, keputusan pemilik (bagian 2.3) | AC-45 |
| Kebocoran jalur lokal atau kode sumber | Source map, string di bundel | Tanpa source map, `CHK-OUT-04` | AC-45 |
| Pihak ketiga saat runtime | Font CDN, skrip, embed, gambar eksternal | `CHK-ARCH-06`, `CHK-OUT-02`, inspeksi jaringan | AC-47 |
| Dependency berbahaya atau berlisensi bermasalah | Paket npm | Versi dipin, `package-lock.json`, `npm ci`, lisensi dicatat, dependency minimal (bagian 5) | AC-51 |
| Injeksi lewat data konten | Tautan `javascript:`, HTML mentah | Skema menolak skema tautan selain `https:` dan `mailto:`, tanpa `dangerouslySetInnerHTML` (`CHK-ARCH-04`), React meloloskan teks sebagai teks | `CHK-CNT-15` |
| Tabnabbing | Tautan keluar | `rel="noopener noreferrer"` pada `ExternalLink` | AC-14 |
| Clickjacking | Pembingkaian situs | `frame-ancestors` lewat header host. **Tidak dapat dijamin** bila host tak mendukung header (bagian 4) | Uji header |
| Permukaan input pengguna | n/a | Tidak ada formulir, akun, atau penyimpanan | Tinjauan |

## 2. Temuan: repositori dan nama institusi (C-01, QT4-01)

### 2.1 Fakta yang diperiksa

| Fakta | Cara diperiksa | Kepastian |
|---|---|---|
| `git clone` tanpa kredensial berhasil atas `lkmandef/creative-developer-portfolio` | Dijalankan dari lingkungan Claude | Terbukti. Mengindikasikan repositori dapat dibaca publik. **Visibilitas pastinya tidak dikonfirmasi** (API GitHub terkena batas laju) |
| `PROJECT_BRIEF.md` memuat nama RSUD sebanyak 4 kali (baris 87, 88, 90, 265). Dokumen lain tidak | `grep` pada berkas yang di-clone | Terbukti |
| Satu commit, dengan nama dan email penulis yang sama dengan pembuat commit. Email berupa alamat Gmail pribadi (tidak dicetak di sini) | `git log` | Terbukti |
| Tidak ada `.gitignore`, `.env`, kode, atau aset | Daftar berkas | Terbukti |

### 2.2 Mengapa ini penting

Brief bagian 12 butir 7, NFR-09.4, R-01, dan NFR-08.5 menetapkan: nama institusi tidak dipublikasikan sebelum izin terkonfirmasi, dan repositori dibuka untuk publik hanya setelah audit. Gerbang itu mengatur **situs** dan **repositori yang dibuka**. Bila repositori sudah publik, dokumen perencanaan di dalamnya sudah "terbuka". Apakah itu masalah bergantung pada izin yang belum diketahui. **Saya tidak menilai sisi hukumnya.**

### 2.3 Opsi (keputusan pemilik)

| Opsi | Tindakan | Kelebihan | Kekurangan |
|---|---|---|---|
| **O-1 Jadikan privat sekarang** | Ubah visibilitas repositori menjadi privat sampai audit (bagian 3) selesai dan keputusan DP-04 dibuat | Menghentikan paparan baru. Selaras NFR-08.5 | Tidak menghapus apa yang mungkin sudah disalin atau di-cache pihak lain. Riwayat tetap memuat nama |
| **O-2 Privat + bersihkan** | O-1, lalu ganti penyebutan nama institusi di brief dengan istilah netral dan, bila perlu, tulis ulang riwayat (tindakan destruktif, perlu persetujuan eksplisit) | Paling bersih | Penulisan ulang riwayat berisiko. Tidak dapat memulihkan paparan lampau |
| **O-3 Terima** | Biarkan publik dengan keputusan sadar | Tanpa usaha | Bertentangan dengan gerbang yang sudah ditulis. Risiko R-01 dan R-02 tetap |
| **O-4 Tetap publik, bersihkan dokumen ke depan** | Ganti nama di brief pada commit baru tanpa menulis ulang riwayat | Mudah | Nama tetap ada di riwayat |

Perilaku bawaan sampai dijawab: tidak ada konten sensitif baru ditambahkan, tidak ada dokumen diubah oleh Claude, dan **Tahap 5 tidak menambah berkas sebelum QT4-01 dijawab**.

Email penulis commit: pemilik dapat mempertimbangkan alamat `noreply` dari penyedia Git untuk commit mendatang. Mengubahnya tidak mengubah commit lama. Ini keputusan pemilik, bukan kebutuhan teknis.

## 3. Pengamanan repositori

### 3.1 Berkas konfigurasi awal (Fase 1)

`.gitignore` wajib memuat sedikitnya: `node_modules/`, `.next/`, `out/`, `reports/`, `public/generated/`, `src/generated/`, `assets-src/private/`, `.env*` (kecuali templat tanpa nilai, bila ada), `*.local`, `*.pem`, `*.key`, `.private/` (daftar istilah lokal, bagian 3.3). Ditegakkan oleh `CHK-REPO-01`.

### 3.2 Daftar tinjauan manual aset (sebelum commit)

Dikerjakan pemilik untuk setiap gambar atau berkas baru di `assets-src/images/` dan `assets-src/cv/`:

| # | Pertanyaan |
|---|---|
| 1 | Apakah gambar memuat nama institusi, logo, atau nama orang? |
| 2 | Apakah ada data pasien, pegawai, nomor identitas, atau nomor telepon? |
| 3 | Apakah ada denah, lokasi ruangan, atau alamat? |
| 4 | Apakah ada URL internal, alamat IP, nama server, token, atau kredensial pada layar atau bilah alamat? |
| 5 | Apakah nama berkas, `alt`, atau keterangan membocorkan hal di atas? |
| 6 | Apakah data pada layar seluruhnya dummy atau dibuat sendiri? |
| 7 | Untuk CV: apakah ada alamat rumah, nomor telepon, atau data pribadi yang tidak dikonfirmasi? |

Jawaban "ya" pada 1 sampai 5 atau "tidak" pada 6 berarti berkas **tidak** masuk repositori.

### 3.3 Daftar istilah sensitif lokal (D-10)

| Aspek | Spesifikasi |
|---|---|
| Lokasi | `.private/forbidden-terms.txt` di mesin pemilik (gitignored). Jalurnya dapat diubah lewat `FORBIDDEN_TERMS_FILE` |
| Isi | Satu istilah per baris: nama institusi, nama unit, nama orang, nama sistem internal, dan variasi ejaan yang dipakai pemilik |
| Dipakai oleh | `CHK-CNT-17`, `CHK-OUT-03`, `CHK-AST-05`, `CHK-REPO-04` |
| Bila berkas tidak ada | Mode dev: peringatan. **Mode rilis: kode keluar 2 (tidak dapat memeriksa), diperlakukan sebagai gagal** |
| Mengapa tidak di repositori | Daftar itu sendiri akan memuat istilah yang ingin dilindungi |

### 3.4 Audit sebelum repositori dibuka (NFR-08.5, AC-45)

Prosedur (tanpa alat khusus yang belum diverifikasi):

| # | Langkah | Hasil yang dicatat |
|---|---|---|
| RA-01 | Jalankan `CHK-REPO-01` sampai `CHK-REPO-05` pada keadaan terbaru | Temuan dan kode keluar |
| RA-02 | Pindai **seluruh riwayat commit**, bukan hanya pohon kerja, untuk pola rahasia dan istilah lokal (`CHK-REPO-02`, `CHK-REPO-04` berjalan pada setiap commit) | Daftar commit dengan temuan |
| RA-03 | Tinjau metadata commit: nama dan email penulis (`CHK-REPO-03`) | Keputusan pemilik soal email |
| RA-04 | Tinjau manual berkas biner (gambar, PDF) | Tanda tangan pemilik pada daftar 3.2 |
| RA-05 | Bila ada temuan di riwayat: minta persetujuan sebelum tindakan destruktif (penulisan ulang riwayat). **Tidak ada tindakan destruktif tanpa persetujuan** | Catatan persetujuan |
| RA-06 | Alat pemindai rahasia pihak ketiga dapat melengkapi, tetapi **belum dievaluasi** dan tidak diwajibkan (V-18) | Bila dipakai: versi, lisensi, hasil |

Hasil audit dicatat di catatan bukti Tahap 6, dengan kategori bukti A (dilakukan nyata oleh pemilik).

## 4. Header keamanan dan CSP (NFR-08.6, R-33)

### 4.1 Kendala

1. `next.config` tidak dapat menyetel `headers` pada static export (EV-03). Header datang dari host atau tidak ada.
2. Apakah HTML hasil export memuat **skrip inline** belum diketahui (V-17). Bila ya, CSP berbasis `script-src 'self'` saja akan memblokirnya.
3. Host belum dipilih (ADR-0009). Beberapa host statis tidak dapat menyetel header kustom.
4. CSP lewat `<meta http-equiv>` dapat membatasi sumber, tetapi diketahui tidak mendukung `frame-ancestors` dan mode report-only. **Dugaan ini dari pengetahuan umum tentang spesifikasi CSP dan perlu dikonfirmasi dari sumber resmi saat dipakai.**

### 4.2 Dua tingkat CSP (diputuskan setelah S-05 dan pemilihan host)

| Tingkat | Isi | Syarat |
|---|---|---|
| **CSP-A (ketat)** | `default-src 'self'`; `script-src 'self'` ditambah hash untuk setiap skrip inline yang ada (dihasilkan skrip pasca-build dari `CHK-OUT-13`); `img-src 'self' data:`; `font-src 'self'`; `connect-src 'none'`; `object-src 'none'`; `base-uri 'self'`; `frame-ancestors 'none'`; `form-action 'none'` | Host dapat menyetel header per respons dan hash dapat dikelola setiap build |
| **CSP-B (pragmatis)** | Sama, tetapi `script-src 'self' 'unsafe-inline'` bila skrip inline tak terhindarkan. Masih memblokir semua sumber pihak ketiga | Host hanya dapat menyetel satu kebijakan statis, atau lewat `<meta>` yang disisipkan skrip pasca-build |

`style-src` ditentukan setelah melihat apakah HTML hasil build memuat gaya inline. Kebijakan yang dipilih diuji dengan membuka seluruh rute dan memeriksa konsol terhadap pelanggaran. **Tidak ada klaim bahwa CSP-A atau CSP-B didukung host mana pun sebelum diuji.**

### 4.3 Header lain

`X-Content-Type-Options: nosniff`, `Referrer-Policy` (nilai ditetapkan saat host dipilih), `Permissions-Policy` yang menonaktifkan kamera, mikrofon, geolokasi, dan fitur lain yang tidak dipakai. Tautan keluar sudah memakai `noreferrer` sehingga tidak mengirim referer.

## 5. Dependency dan rantai pasok (NFR-07.1, NFR-07.2)

| Aturan | Spesifikasi |
|---|---|
| Dependency minimal | Daftar awal di `TECHNICAL_DESIGN.md` bagian 7. Tambahan memerlukan catatan alasan |
| Pinning | Versi eksak di `package.json`. `package-lock.json` di-commit. Instalasi bersih dengan `npm ci` |
| Lisensi | Setiap dependency langsung dicatat: nama, versi, lisensi, alasan, tanggal verifikasi, sumber. Berkas pencatatan ditetapkan di Fase 1. **Lisensi tidak diasumsikan dari ingatan** |
| Skrip siklus hidup | Paket dengan skrip pemasangan (mis. pengunduh biner) dicatat dan diuji di Windows 10 (R-37) |
| Pembaruan | Tidak memperbarui toolchain tanpa uji ulang (R-25, R-31) |
| Audit kerentanan | Dijalankan sebagai langkah tercatat sebelum rilis. Cara dan ambang diputuskan di Tahap 6. Hasilnya dicatat apa adanya. Tidak ada klaim "tanpa kerentanan" |
| Paket baru dalam 14 hari terakhir | Dihindari untuk dependency langsung kecuali ada alasan tercatat (praktik kehati-hatian, bukan kewajiban dokumen sumber) |

## 6. Error handling dan perilaku saat gagal

### 6.1 Prinsip

1. Konten tidak pernah bergantung pada keberhasilan efek (NFR-06.1).
2. Kegagalan satu bagian tidak menjatuhkan halaman (NFR-06.2).
3. Tidak ada layar kosong. Selalu ada jalur navigasi (FR-13.4).
4. Tidak ada galat yang ditelan diam-diam di tempat yang menyembunyikan konten.

### 6.2 Matriks

| Kegagalan | Perilaku pengguna | Mekanisme | Rujukan |
|---|---|---|---|
| Rute atau slug tak dikenal | Halaman 404 dengan header, footer, tautan ke beranda dan Projects | `not-found.tsx`, `dynamicParams = false` | FR-01.4, FR-13.1, AC-15 |
| JS mati atau gagal | Seluruh teks dan tautan terbaca. Menu `<details>` tetap berfungsi. Tidak ada elemen tersembunyi | HTML awal terlihat (`MOTION_SPEC.md` bagian 4) | NFR-06.1, AC-19 |
| Modul efek gagal dimuat atau galat inisialisasi | Konten tampil tanpa animasi | F2 dan F3 | FR-10.8, AC-19 |
| Galat render di salah satu Client Component | Bagian itu diganti fallback sederhana. Halaman lain tetap tampil | `error.tsx` pada segmen, pembatas galat per pulau interaksi | FR-13.4, AC-60 |
| Galat fatal pada root | Halaman fallback minimal berisi nama, tautan ke `/`, `/#projects`, `/#contact` memakai `<a>` biasa (bukan komponen router) | `global-error.tsx` | FR-13.4 |
| Gagal memuat potongan JS saat navigasi klien | Navigasi keras ke URL tujuan sebagai cadangan. Perilaku bawaan Next **belum diverifikasi**. Bila tidak ditangani, dibuat penanganan di pulau interaksi | Diuji dengan memblokir potongan di DevTools | NFR-06, V-24 |
| Gambar gagal dimuat | Kotak tetap pada ukurannya, `alt` terbaca, teks pendamping tetap ada | `aspect-ratio`, `width`/`height`, `alt` | FR-13.2, EC-06, AC-60 |
| Font gagal atau lambat | Font cadangan dengan metrik dekat | `next/font` + `adjustFontFallback` atau manual | EC-07, NFR-01.5 |
| Stylesheet gagal | HTML semantik tetap terbaca (urutan DOM logis) | Struktur HTML | NFR-06.1 |
| `matchMedia` atau `IntersectionObserver` tidak ada | Tingkat `reduced`. Tidak ada elemen dipersenjatai | F4, resolver | EC-03 |
| `ViewTransition` tidak didukung | Navigasi instan | EV-04 | FR-11 |
| Tidak ada aplikasi surel | Alamat terlihat dan dapat disalin | `EmailLink` | EC-17, AC-62 |
| Data opsional kosong (foto, demo, repositori, CV) | Komponen terkait tidak dirender, tanpa tautan mati dan tanpa ruang kosong | Aksesor mengembalikan `null` atau daftar kosong | EC-20, FR-05.7 |
| Data konten tidak valid | **Build gagal**, bukan situs rusak | `CHK-CNT-*` | FR-12.6 |
| Tab tersembunyi atau perangkat hemat daya | Animasi dijeda. Penyederhanaan lewat tingkat `adapted` | F5 | EC-18, EC-26 |

### 6.3 Pencatatan galat

| Aspek | Aturan |
|---|---|
| Tujuan | Hanya konsol browser dengan awalan `[portfolio]`. **Tidak ada pengiriman ke server atau pihak ketiga** (NFR-08.3, NFR-08.4) |
| Isi | Jenis galat dan ID efek. Tidak memuat data pengunjung, tidak memuat konten pribadi |
| Produksi | Pesan singkat. Tanpa jejak panjang |

### 6.4 Penyuntikan galat untuk uji (AC-19, AC-60)

Hook ada di `MOTION_SPEC.md` bagian 12. Aturan keamanan: variabel `NEXT_PUBLIC_FAULT_INJECTION` tidak boleh ada pada build rilis, dan `CHK-OUT-10` gagal bila string pemicunya ditemukan di `out/`.

## 7. Observabilitas lokal

Tidak ada telemetri saat runtime, tidak ada pelacak, tidak ada cookie, tidak ada `localStorage` atau `sessionStorage` (`CHK-ARCH-07`). Observabilitas terdiri dari:

| Sumber | Isi | Lokasi |
|---|---|---|
| Keluaran pemeriksaan build | Temuan `CHK-*` | Terminal dan `reports/*.json` (gitignored) |
| Laporan pengukuran performa dan uji manual | Hasil dengan kondisi uji dan kategori bukti | Lokasi dan format ditetapkan di Tahap 6 (C-07) |
| Konsol browser | Galat dengan awalan `[portfolio]` | Lokal pada perangkat uji |

Tidak ada dokumen observabilitas terpisah karena tidak ada infrastruktur yang perlu dirancang (K-09).

## 8. Privasi pengunjung

| Hal | Keputusan |
|---|---|
| Data yang dikumpulkan | Tidak ada oleh situs. Host mungkin menyimpan log akses standar. **Itu di luar kendali situs dan diperiksa saat host dipilih** |
| Cookie dan penyimpanan | Tidak ada |
| Analytics | Tidak ada di MVP (NFR-08.4). DP-08 masih menunggu konfirmasi (QT4-06). Bila kelak dipakai, ADR baru memuat privasi, dependency, dan persetujuan |
| Pihak ketiga | Tidak ada font, skrip, embed, atau gambar eksternal. Tautan keluar hanya dimuat saat pengunjung mengklik |
| Informasi pribadi pemilik | Hanya yang dikonfirmasi pemilik: nama tampilan, tautan GitHub dan Instagram, email, dan CV bila siap. Tidak ada lokasi, telepon, atau alamat (FR-06.3, FR-09.7) |

## 9. Kriteria verifikasi

| Bukti yang dibutuhkan | Cara | AC |
|---|---|---|
| Varian RSUD sesuai status izin. Varian bernama tidak ada di build tanpa izin | Fixture dan `CHK-CNT-11`, `CHK-CNT-12` | AC-11 |
| Tidak ada kredensial, data pribadi, atau konfigurasi lokal di aset atau repositori yang dibuka, termasuk riwayat | RA-01 sampai RA-05 | AC-45 |
| Metadata gambar dan PDF bersih. Nama berkas dan `alt` tidak membocorkan | `CHK-AST-02`, `CHK-AST-03`, `CHK-AST-05`, `CHK-OUT-03`, tinjauan manual | AC-46 |
| Tidak ada permintaan jaringan pihak ketiga saat runtime | `CHK-OUT-02` dan tab jaringan di Chrome | AC-47 |
| Tautan keluar memakai atribut keamanan | Tinjauan DOM | AC-14 |
| Build gagal untuk placeholder, data tidak valid, dan daftar istilah lokal tidak ada | Fixture dan `RELEASE=1` | AC-16 |
| Fallback saat gambar diblokir dan saat galat disengaja | Uji DevTools dan hook galat | AC-60 |
| Header dan CSP terpasang dan tidak melanggar saat semua rute dibuka | `curl -I` dan konsol | NFR-08.6 |
| Dependency: lisensi, versi, alasan tercatat | Tinjauan berkas pencatatan | AC-51 |

## 10. Belum diputuskan

| ID | Item | Kapan |
|---|---|---|
| QT4-01 | Visibilitas repositori dan nama institusi (O-1 sampai O-4) | **Sebelum Tahap 5 menambah berkas** |
| V-17 | Skrip inline pada HTML hasil export | S-05 |
| V-18 | Alat metadata dan alat pemindai (bila dipakai) | Tahap 5 awal |
| QT4-05 | Tingkat anonimisasi | Penyusunan konten |
| Q-01 sampai Q-03, V-03 | Izin RSUD, hak atas hasil PKL | Pemilik, sebelum rilis |
| Q-19 | Host (menentukan kemampuan header) | Menjelang rilis |
