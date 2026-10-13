# ADR-0005: Tiga tingkat motion dan keadaan awal yang aman

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-03, K-04, K-12, FR-03.4, FR-10.1, FR-10.7, FR-10.8, NFR-05, NFR-06, AC-19, AC-24, AC-25, AC-59 |

## Konteks

Motion memiliki tiga tingkat: desktop lengkap, mobile disesuaikan, dan reduced motion (K-03). Aturan penengah K-12 menetapkan urutan: alternatif efisien, penyederhanaan, baru penghilangan. Elemen LCP hero tidak boleh bergantung pada JS atau animasi (FR-03.4). Konten harus tetap tersedia bila animasi gagal (FR-10.8).

Pemilik memutuskan (DP-13) bahwa pemusatan sistem motion di satu tempat **bukan kewajiban**, hanya rekomendasi implementasi.

## Keputusan (usulan)

**Yang wajib**
1. Tiga tingkat motion tersedia dan teruji perilakunya (AC-24).
2. Semua tingkat memakai **token yang sama** untuk durasi, easing, dan stagger (FR-10.4, AC-56).
3. Tingkat ditentukan dari **kemampuan yang dapat diperiksa**: preferensi reduced motion, kemampuan pointer dan hover, dan kelas lebar viewport. Tidak memakai deteksi merek atau jenis perangkat.
4. Keadaan awal aman: HTML awal berisi konten terlihat. Elemen yang disembunyikan untuk animasi hanya yang berada di bawah lipatan dan hanya bila tingkat bukan reduced motion. Elemen kandidat LCP hero tidak disembunyikan oleh JS.
5. **Fail-safe**: bila skrip animasi gagal dimuat, gagal diinisialisasi, atau melewati batas waktu, semua elemen yang disembunyikan ditampilkan (FR-10.8).
6. Perubahan preferensi atau ukuran viewport saat sesi memicu penyesuaian tingkat (EC-04, EC-25, AC-59).
7. Efek dipasang dan dilepas lewat primitif yang membersihkan otomatis saat pindah rute, saat komponen dilepas, dan menjeda saat tab tersembunyi (FR-10.7, EC-18).
8. Setiap efek memiliki **tangga degradasi** tertulis di spesifikasi motion (K-12, FR-10.10).

**Rekomendasi (tidak wajib)**
9. Penentuan tingkat dipusatkan dalam satu modul, supaya kondisi tidak tersebar di komponen dan pengujian lebih murah (A-06). Bila praktik di lapangan menyimpang, deviasi dicatat dan AC-24 tetap harus lulus.

**Belum diputuskan (Tahap 4, setelah spike S-01)**
10. Cara mencegah kilatan saat elemen disembunyikan setelah hidrasi. Opsi dan konsekuensinya ada di ARCHITECTURE bagian 10.

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Kondisi tingkat ditulis per komponen | Fleksibel, tetapi rawan tidak konsisten dan mahal diuji. Hanya diterima bila AC-24 dan AC-56 tetap terpenuhi |
| Deteksi perangkat dari user agent atau model | Rapuh dan mendorong klaim yang tidak teruji (K-05). Ditolak |
| Satu tingkat motion untuk semua | Bertentangan dengan K-03 dan K-04 |

## Konsekuensi

**Positif**
- Aturan keadaan awal dan fail-safe melindungi LCP, aksesibilitas, dan ketahanan.
- Tangga degradasi membuat penerapan K-12 dapat diaudit per efek.

**Negatif atau batasan**
- Spike S-01 harus dilakukan lebih awal. Tabrakan antara masked text reveal dan syarat LCP (R-29) bisa memaksa hero didesain ulang.
- Tidak ada deteksi kinerja perangkat otomatis. Efek yang berat di perangkat lemah ditangani lewat desain tingkat mobile, bukan deteksi.

## Syarat tinjau ulang

Spike S-01 atau S-03 menunjukkan tingkat mobile tidak cukup. Atau ada efek yang tidak dapat dipetakan ke tiga tingkat.

## Rujukan

K-03, K-12, DP-13. Risiko R-04, R-29. Spike S-01, S-03.
