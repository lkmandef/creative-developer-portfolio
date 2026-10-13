# ADR-0009: Hosting dan deployment

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (keputusan pemilihan host **ditunda** menjelang rilis) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-06, NFR-08.6, Q-19, V-08, AC-50 |

## Konteks

Hosting dan domain diputuskan mendekati rilis dengan perbandingan biaya berulang dan keterbatasan (K-06). Pengembangan dan pengujian dilakukan lokal dulu. Static export tidak mendukung `headers`, `redirects`, `rewrites`, dan Server Actions dari konfigurasi Next.js (EV-03), sehingga kebutuhan tersebut harus dipenuhi oleh host atau tidak dipakai.

## Keputusan (usulan)

1. Artefak rilis adalah folder `out/` hasil static export. Siap disajikan oleh host file statis mana pun (EV-03).
2. **Host belum dipilih.** Pemilihan dilakukan menjelang rilis berdasarkan kriteria di bawah dan informasi harga serta batas terkini dari sumber resmi (V-08). Tidak ada klaim harga atau batas di dokumen ini.
3. Kriteria seleksi host:

| Kriteria | Alasan |
|---|---|
| Dapat menyajikan HTML statis per rute dengan benar, termasuk `404.html` untuk rute tak dikenal | EV-03, FR-13.1, AC-15 |
| Dapat mengatur header keamanan dan, bila diperlukan, kebijakan konten | Tidak bisa dari `next.config` pada static export (EV-03), NFR-08.6 |
| HTTPS | Dasar keamanan |
| Biaya berulang yang dapat diterima pemilik, termasuk domain | K-06, Q-19 |
| Perilaku `trailingSlash` selaras dengan konfigurasi build | Keputusan Tahap 4 |
| Tidak mewajibkan pelacakan atau skrip pihak ketiga pada situs | NFR-08.3 |
| Pratinjau sebelum rilis (opsional) | Memudahkan uji di perangkat nyata |
| Pemilik dapat mengelola sendiri tanpa keahlian khusus | T-04 |

4. Dokumentasi Next.js menyediakan template deploy ke GitHub Pages (EV-03). Itu satu kandidat. Kemampuan dan batasnya terhadap kriteria di atas belum dievaluasi di sesi ini.
5. Hingga rilis, pengembangan dan pengujian berjalan lokal. Tidak ada penerbitan publik sebelum gerbang rilis terpenuhi (NFR-09.6, AC-50).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Memilih host sekarang | Harga dan batas bisa berubah sebelum rilis. Tidak perlu untuk Tahap 3 |
| Hosting dengan server Node | Tidak dibutuhkan situs statis. Menambah biaya dan permukaan serangan |

## Konsekuensi

**Positif**
- Keputusan biaya ditunda sampai informasi paling baru tersedia.
- Arsitektur tidak terikat satu vendor.

**Negatif atau batasan**
- Header keamanan dan CSP baru dapat dipastikan setelah host dipilih (R-27). Opsi skrip inline pada ARCHITECTURE bagian 10 dipengaruhi hal ini.
- Perilaku `404.html` dan `trailingSlash` di host harus diuji, bukan diasumsikan.

## Syarat tinjau ulang

Tidak ada host yang memenuhi kriteria dengan biaya yang dapat diterima, atau kebutuhan fitur server muncul dan membatalkan ADR-0001.

## Rujukan

EV-03. Risiko R-27. OQ-05, V-08.
