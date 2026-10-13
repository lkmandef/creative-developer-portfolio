# ADR-0008: Aset, pihak ketiga, dan analytics

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-09, FR-04.6, FR-09.6, FR-09.7, NFR-01.5, NFR-08, DP-08, AC-46, AC-47, AC-63 |

## Konteks

Tidak boleh ada pemanggilan pihak ketiga saat runtime tanpa keputusan tercatat, termasuk implikasi privasi dari font eksternal (NFR-08.3). Situs tidak membuat akun dan tidak mengumpulkan data pengunjung (NFR-08.4, A-08). Gambar harus punya dimensi eksplisit dan font harus punya cadangan agar layout tidak bergeser (NFR-01.5, FR-04.6). Pada static export, loader gambar bawaan tidak didukung (EV-03). Metadata gambar dan PDF harus dibersihkan (NFR-08.2, FR-09.7).

## Keputusan (usulan)

1. **Font di-host sendiri** dan jumlah keluarga serta bobot dibatasi. Ada font cadangan dengan metrik yang dekat untuk mencegah pergeseran layout. Mekanisme pada static export diverifikasi lebih dulu (V-13).
2. **Gambar**: dimensi eksplisit, varian responsif disiapkan sebelum atau saat build, atau lewat loader kustom (didukung menurut EV-03). Opsi `unoptimized` dan alat konversi belum diverifikasi, sehingga alur gambar diputuskan setelah V-12. Pemuatan malas dipakai kecuali untuk elemen LCP.
3. **Metadata gambar dan PDF** dibersihkan sebelum masuk repositori yang dibuka atau build rilis. Nama berkas dan teks alternatif tidak memuat data internal (NFR-08.2, AC-46).
4. **CV** berupa PDF statis. Tidak ada fitur unduh khusus. Tampil hanya bila CV berbahasa Inggris siap dan sudah diperiksa pemilik (DP-11, FR-09.6, AC-63).
5. **Tidak ada pihak ketiga saat runtime**: tanpa font CDN, embed, skrip eksternal, atau pelacak. Pemeriksaan otomatis memindai `out/` dan inspeksi jaringan (AC-47).
6. **Analytics tidak dipakai di MVP.** Keputusan ini masih menunggu konfirmasi pemilik (DP-08, OQ-02). Bila kelak dipakai, dibuat ADR baru yang membahas privasi, dependency, dan persetujuan.
7. Tautan keluar (GitHub, Instagram, `mailto`) adalah tautan biasa dan bukan integrasi. Atribut keamanannya ditentukan di Tahap 4 (FR-09.3).

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Font dari layanan pihak ketiga | Menambah permintaan eksternal dan implikasi privasi (NFR-08.3) |
| Loader gambar layanan eksternal | Didukung secara teknis (EV-03) tetapi menambah ketergantungan pihak ketiga dan melanggar NFR-08.3 tanpa keputusan tercatat |
| Analytics ringan | Belum diminta pemilik. Menambah implikasi privasi dan dependency (A-08) |

## Konsekuensi

**Positif**
- Tidak ada ketergantungan runtime. Privasi pengunjung terjaga.
- Alur aset dapat diperiksa secara otomatis.

**Negatif atau batasan**
- Alur gambar harus dibuat sendiri sehingga menambah pekerjaan fase awal (R-27).
- Tanpa analytics, tidak ada data trafik. Itu sesuai brief: tidak ada target trafik (brief bagian 2).

## Syarat tinjau ulang

V-12 atau V-13 menunjukkan alur yang diusulkan tidak praktis. Atau pemilik memutuskan memakai analytics (DP-08).

## Rujukan

EV-03. Risiko R-03, R-27. V-12, V-13.
