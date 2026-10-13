# ADR-0004: Teknologi animasi

| Item | Nilai |
|---|---|
| Status | **Diusulkan** (menunggu persetujuan pemilik) |
| Tanggal | 12 Oktober 2026 |
| Dokumen induk | `docs/planning/ARCHITECTURE.md` |
| Rujukan PRD | K-03, K-09, FR-10, NFR-01, NFR-07.2, AC-22, AC-23, AC-56 |

## Konteks

K-03 menetapkan DOM/CSS/SVG sebagai dasar wajib dengan GSAP dan ScrollTrigger untuk kebutuhan kompleks. Animasi sederhana tidak harus memakai GSAP. `transform` dan `opacity` diutamakan. Tidak ada scroll hijacking dan tidak ada animasi berkedip. Dependency tidak boleh ditambah hanya agar terlihat canggih (K-09).

## Keputusan (usulan)

1. **Tingkat 1, CSS** (transition dan keyframes) untuk hover, fokus, dan reveal sederhana.
2. **Tingkat 2, GSAP + ScrollTrigger** hanya untuk animasi berurutan, stagger terkoordinasi, efek terkait scroll, dan masked text reveal yang tidak praktis dengan CSS.
3. GSAP dan plugin dimuat **secara malas** (dynamic import) dan hanya pada halaman atau bagian yang membutuhkannya, agar tidak masuk JS awal yang tidak perlu.
4. GSAP hanya diimpor dari lapisan motion (L5). Komponen tidak memanggil GSAP langsung.
5. Tidak ada pustaka animasi lain di MVP.
6. Versi GSAP dipin. Teks lengkap lisensi dikonfirmasi sebelum dependency ditambahkan (V-05).
7. **Pemecahan teks** untuk animasi harus menjaga keterbacaan oleh pembaca layar, pencarian, dan salin (FR-10.9, AC-22). Pilihan alat (fitur pemecah teks GSAP atau pemecah sederhana buatan sendiri dengan nama aksesibel pada elemen induk) diputuskan di spike S-01 berdasarkan uji Narrator dan TalkBack.

## Alternatif yang dipertimbangkan

| Alternatif | Penilaian |
|---|---|
| Hanya CSS | Dependency nol, tetapi urutan kompleks dan efek scroll yang terkoordinasi sulit dijaga. Dukungan lintas-browser untuk CSS scroll-driven animation **tidak diverifikasi** di sesi ini, sehingga tidak dijadikan dasar (K-10) |
| Pustaka animasi lain (mis. berbasis React) | Bukan kandidat pemilik. Menambah dependency kedua di samping CSS |
| GSAP untuk semua animasi | Menambah JS dan biaya main thread untuk efek yang cukup dengan CSS |

## Konsekuensi

**Positif**
- Sesuai K-03. Biaya JS hanya dibayar saat dibutuhkan.
- Lisensi: README GSAP menyatakan seluruh toolset gratis termasuk penggunaan komersial, dan lisensi mengizinkan penerapan di situs web (EV-06).

**Negatif atau batasan**
- ScrollTrigger tidak disebut namanya pada halaman lisensi yang dibaca. Dianggap tercakup sebagai bagian toolset tetapi **belum terkonfirmasi tertulis** (R-30, V-05).
- Lisensi melarang penggunaan di alat pembuat animasi visual tanpa kode yang bersaing dengan Webflow (EV-06). Situs ini bukan alat semacam itu.
- API GSAP yang akan dipakai untuk pembersihan siklus hidup, pembuatan kondisi media, dan pemecahan teks belum diverifikasi (V-15).
- Biaya GSAP pada JS awal belum diketahui. Diukur di S-01 dan S-03.

## Syarat tinjau ulang

Lisensi tidak dapat dikonfirmasi, ukuran bundle melampaui anggaran yang ditetapkan, atau spike menunjukkan CSS cukup untuk semua efek.

## Rujukan

EV-06. Risiko R-29, R-30. Spike S-01, S-03.
