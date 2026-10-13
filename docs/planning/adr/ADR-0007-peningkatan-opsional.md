# ADR-0007: Peningkatan opsional yang mati secara bawaan

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-02, K-03, K-09, PRD bagian 4.2, FR-15, NFR-01, R-12, AC-54 |

## Konteks

K-03 menyebut video, WebGL/3D/shader, smooth scrolling (mis. Lenis), dan kursor kustom hanya jika terbukti perlu. Suara dihindari di MVP (K-09). Eksperimen interaktif dibatasi 0 sampai 3 dan harus menunjukkan kemampuan yang belum terwakili pengalaman utama (K-02). Risiko scope creep (R-12) dan kapasitas 6 sampai 10 jam per minggu (K-06) berlaku.

## Keputusan (usulan)

1. Lenis, video preview, WebGL/3D/shader, kursor kustom, suara, dan eksperimen interaktif **tidak dipasang dan tidak dimasukkan sebagai dependency** di baseline.
2. Sebuah item hanya dapat masuk bila memenuhi **syarat masuk di PRD bagian 4.2** dan:
   - ada alasan tertulis tentang kemampuan yang ditunjukkan atau masalah yang diselesaikan (FR-15.1);
   - ada pengukuran dengan dan tanpa item di POCO M4 Pro dan laptop uji (AC-54);
   - ada fallback dan kursor sistem serta scroll native tetap berfungsi (K-03);
   - tercatat dalam ADR atau pembaruan ADR ini.
3. Item masuk sebagai modul **terpisah, dimuat malas, dan dapat dilepas** tanpa memengaruhi bagian lain (FR-15.3).
4. Item tidak boleh menambah beban pemuatan beranda (FR-15.2).
5. Suara hanya dipertimbangkan kembali bila ada kebutuhan nyata dan persetujuan pemilik, harus opsional, dan tidak diputar otomatis (K-09).
6. WebGL tidak menjadi ketergantungan utama pengalaman (K-03). Bila dibangun, harus punya fallback non-WebGL.

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Memasang beberapa pustaka sejak awal "untuk jaga-jaga" | Menambah dependency dan beban tanpa bukti kebutuhan. Bertentangan dengan K-09 |
| Melarang selamanya | Terlalu kaku. Kebutuhan nyata bisa muncul setelah spike |

## Konsekuensi

**Positif**
- Baseline ringan, jelas apa yang diukur, dan scope terkendali.
- Pemotongan scope sesuai PRD 4.4 menjadi mudah karena item sudah terisolasi.

**Negatif atau batasan**
- Karakter "sinematik" (P-01) harus dicapai dari tipografi, komposisi, dan transisi dengan teknik dasar. Bila tidak cukup, keputusan dibuka lagi lewat bukti spike, bukan selera.

## Syarat tinjau ulang

Ada bukti bahwa sebuah efek meningkatkan identitas dan lolos kriteria performa dan accessibility, atau pemilik menjawab DP-02 dengan efek tertentu.

## Rujukan

PRD 4.2 dan 4.4, FR-15, AC-54. Risiko R-12.
