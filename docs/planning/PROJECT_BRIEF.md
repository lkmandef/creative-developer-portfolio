# PROJECT BRIEF — Creative Developer Portfolio Website

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/PROJECT_BRIEF.md` |
| Versi | 0.2 (revisi pertama, menunggu persetujuan) |
| Tanggal | 12 Oktober 2026 |
| Tahap workflow | Tahap 1 dari 10 (Discovery dan Product Definition) |
| Status | **Revisi 0.2 menunggu persetujuan pemilik (ketik `LANJUT` untuk ke Tahap 2)** |
| Pemilik proyek | Lukmanul Hakim (nama tampilan sementara) |
| Disusun dengan | Claude (perencanaan). Implementasi nanti memakai Antigravity CLI 1.3.3 di Windows 10 |

## Cara membaca dokumen ini

Setiap pernyataan diberi status agar keputusan, dugaan, dan hal yang belum diketahui tidak tercampur.

| Penanda | Arti |
|---|---|
| **K-xx** | Keputusan yang sudah dikonfirmasi pemilik (dianggap final sampai pemilik mengubahnya) |
| **P-xx** | Preferensi atau arahan yang belum dikunci |
| **A-xx** | Asumsi kerja yang belum terbukti dan harus dikoreksi bila salah. Asumsi bertanda **menunggu persetujuan** adalah rekomendasi perencanaan Claude, bukan keputusan pemilik |
| **R-xx** | Risiko |
| **Q-xx** | Pertanyaan terbuka yang membutuhkan jawaban pemilik atau pihak lain |
| **V-xx** | Item yang perlu diverifikasi (fakta teknis atau administratif yang belum diketahui) |
| `[PLACEHOLDER: ...]` | Konten yang belum dikonfirmasi dan tidak boleh dipublikasikan |

Dokumen ini tidak menetapkan desain visual final, stack teknologi final, jumlah project yang ditampilkan, atau angka target performa. Semuanya diputuskan di tahap berikutnya.

---

## 1. Ringkasan

Proyek ini adalah **website portofolio pribadi** untuk seorang mahasiswa S1 Teknik Informatika yang memposisikan diri sebagai **Creative Developer**. Website harus menjadi bukti langsung kemampuan membangun pengalaman web interaktif: motion design, tipografi ekspresif, transisi, dan interaksi dirancang dengan sengaja, tetapi tetap terstruktur, mudah dinavigasi, accessible, dan lancar di perangkat nyata.

Website itu sendiri adalah bukti utama kemampuan creative development. Kemampuan engineering dibuktikan melalui dua project nyata yang disajikan sebagai case study, ditambah penjelasan teknis yang ringkas dan hanya memuat hasil pengujian yang benar-benar dijalankan.

Referensi kualitas interaksi adalah https://motion.zajno.com/. Referensi itu dipakai sebagai standar kualitas dan pendekatan, **bukan** untuk disalin (identitas, layout, aset, maupun implementasi).

## 2. Tujuan

| ID | Tujuan | Cara dinilai |
|---|---|---|
| T-01 | Pengunjung dari studio kreatif, recruiter, dan klien memahami kemampuan pemilik dalam waktu sekitar dua menit | Review terhadap acceptance criteria di PRD. Lihat Q-16 untuk opsi uji persepsi kecil |
| T-02 | Motion dan interaksi menjadi bagian utama identitas, tanpa mengorbankan akses konten, performa, dan accessibility | Pengujian di perangkat uji (bagian 11) dan checklist di Quality Gates |
| T-03 | Dua project nyata tersaji sebagai case study yang jujur dan dapat diverifikasi | Review konten terhadap aturan klaim (bagian 12) dan gerbang izin publikasi (bagian 17) |
| T-04 | Situs realistis untuk dikembangkan dan dipelihara sendiri oleh pemilik | Batas dependency, struktur konten sederhana, dan dokumentasi pemeliharaan |

Tidak ada target trafik, konversi, atau jumlah tawaran pekerjaan yang ditetapkan. Metrik seperti itu tidak relevan untuk MVP dan tidak boleh dikarang.

## 3. Audiens

Urutan prioritas (K-01):

1. **Studio kreatif dan agensi** yang mencari creative developer (prioritas utama).
2. **Recruiter dan perusahaan teknologi.**
3. **Developer lain dan calon klien.**

Konsekuensi desain: audiens utama menilai kualitas motion dan implementasi secara visual dalam hitungan detik, sedangkan audiens kedua perlu menemukan informasi penting dengan cepat. Keduanya harus terlayani oleh satu pengalaman.

## 4. Positioning

**Kesimpulan yang diinginkan setelah sekitar dua menit (K-01):**

> Developer ini bukan hanya bisa menulis kode, tetapi juga mampu merancang dan membangun pengalaman web interaktif yang menarik, halus, terstruktur, dan tetap memperhatikan usability serta performa.

**Prinsip penyeimbang (K-01):**
- Motion adalah daya tarik utama, tetapi tidak boleh mengalahkan tujuan utama portofolio: memperlihatkan project nyata, kemampuan teknis, cara berpikir, dan kualitas implementasi.
- Informasi penting mudah ditemukan, project mudah dipindai, dan animasi mendukung konten.
- Bukan eksperimen animasi yang sulit dinavigasi, dan bukan portofolio yang bagus secara visual tetapi tidak membuktikan engineering.

**Perlu diketahui:** frasa "terstruktur, usability, dan performa" adalah klaim. Studio kreatif jarang mempercayai klaim, sehingga situs perlu bukti yang dapat diverifikasi (hasil uji, kode yang boleh dipublikasikan, penjelasan keputusan teknis).

## 5. Masalah yang diselesaikan

1. Portofolio developer pada umumnya berupa CV online atau kumpulan kartu project yang tidak membedakan satu developer dari yang lain.
2. Pemilik adalah mahasiswa dengan dua project nyata dan belum memiliki metrik dampak yang dapat diklaim, sehingga kredibilitas harus dibangun dari kualitas eksekusi, kejujuran, dan bukti teknis, bukan dari angka.
3. Kemampuan creative development sulit dibuktikan lewat tulisan. Hasil terbaik adalah pengalaman yang langsung dirasakan pengunjung.

## 6. Inventaris konten (terkonfirmasi apa adanya)

Tidak ada jumlah project yang diasumsikan di luar inventaris ini. Jumlah dan bentuk tampilannya diputuskan di PRD.

| | Hospital and APAR Fire Inspection System | Flash Sale Intelligence |
|---|---|---|
| Jenis | Aplikasi web | Aplikasi desktop Windows lokal |
| Deskripsi | Mendukung kegiatan inspeksi APAR (Alat Pemadam Api Ringan) dan pencatatan terkait di lingkungan rumah sakit | Membantu pengguna menyiapkan dan mengelola flash sale: pencatatan event, target harga, pengingat, checklist, evaluasi hasil |
| Peran pemilik | Pengembang utama. Dikerjakan sendiri dalam konteks PKL di RSUD Ansari Saleh | Dikembangkan sendiri dengan bantuan AI coding tools |
| Status | Digunakan di RSUD Ansari Saleh | Dalam pengembangan bertahap |
| Aset | Source code (repositori ada pada pemilik) dan antarmuka web. Yang aman dipublikasikan belum diinventarisasi | Source code dan dokumentasi pengembangan di repositori lokal. Screenshot antarmuka nantinya relevan |
| Bukti yang boleh diklaim | Digunakan di RSUD Ansari Saleh. **Tidak ada** metrik efisiensi, penghematan waktu, atau jumlah pengguna | Tidak ada metrik penggunaan. Cocok untuk menunjukkan proses engineering, arsitektur, persistence, dan pengembangan aplikasi Windows |
| Motion | Bukan fokus project | Bukan fokus project |
| Peran di portofolio | Case study utama: bukti software di lingkungan kerja nyata | Case study: bukti proses perancangan dan engineering aplikasi desktop |

**Strategi (K-02):** motion eksperimental diterapkan pada website portofolio itu sendiri. Kedua project membuktikan engineering.

**Kelengkapan yang diketahui kurang:** hanya dua project, dan keduanya tidak berfokus pada motion. Lihat R-06.

## 7. Keputusan terkonfirmasi

| ID | Keputusan | Sumber |
|---|---|---|
| **K-01** | Audiens campuran dengan prioritas studio kreatif/agensi, lalu recruiter dan perusahaan teknologi. Kesimpulan dua menit dan prinsip penyeimbang seperti pada bagian 4 | Jawaban 1 |
| **K-02** | Website adalah bukti utama kemampuan creative development (pendekatan A). Bagian teknis tentang cara situs dibuat **dipertimbangkan** (teknik animasi, alasan pendekatan, performa, accessibility) dan hanya boleh memuat hal yang didukung implementasi dan hasil uji nyata. Eksperimen interaktif **0 sampai 3**, opsional, hanya jika menunjukkan kemampuan yang belum terwakili pengalaman utama dan tidak menjadi playground. Prioritas: (1) situs utama dengan identitas visual dan motion kuat, (2) case study project nyata, (3) bukti teknis yang dapat diverifikasi, (4) eksperimen | Jawaban 4 dan 5 |
| **K-03** | Teknik motion: **DOM/CSS/SVG wajib** dengan GSAP dan ScrollTrigger untuk kebutuhan kompleks (animasi sederhana tidak harus memakai GSAP). Utamakan `transform` dan `opacity`. **Suara dihindari** di MVP (lihat K-09). **Video, WebGL/3D/shader, smooth scrolling (mis. Lenis), dan kursor kustom hanya jika terbukti perlu**, dengan syarat: scroll native adalah baseline, kursor sistem tetap tersedia, pointer bukan satu-satunya akses ke informasi, WebGL bukan ketergantungan utama. Tiga tingkat motion: desktop lengkap, mobile disesuaikan, reduced-motion lebih tenang. Konten dan navigasi tidak menunggu animasi. Tanpa scroll hijacking, tanpa animasi berkedip | Jawaban 5 |
| **K-04** | Bahasa situs: **Bahasa Inggris** untuk MVP, dengan struktur konten yang tidak menyulitkan penambahan Bahasa Indonesia di masa depan. Target uji utama: **Chrome desktop dan Chrome Android**. Mobile dirancang sendiri (layout, tipografi, navigasi, animasi), bukan desktop yang diperkecil. Tanpa horizontal overflow yang tidak disengaja, target sentuh memadai, konten dapat diakses walau animasi gagal. Performa dan tampilan mobile masuk ke acceptance criteria | Jawaban 6 |
| **K-05** | Perangkat uji nyata: **POCO M4 Pro (RAM 6 GB)** dan **laptop pengembangan** (Intel Core i3 gen-7, RAM 12 GB, NVIDIA GeForce MX130, Windows 10). Tidak tersedia: iPhone, Mac, tablet. Tiga kategori bukti: **A** teruji nyata, **B** simulasi (DevTools), **C** belum terverifikasi. Tidak boleh mengklaim perangkat yang tidak teruji | Jawaban 7 |
| **K-06** | Tenggat fleksibel, kualitas lebih penting. Kapasitas awal sekitar 6 sampai 10 jam per minggu (bisa berubah karena kuliah, PKL, proyek lain, termasuk Flash Sale Intelligence). Implementasi dalam fase kecil dengan acceptance criteria; pemilik memeriksa laporan dan hasil uji sebelum menyetujui fase berikutnya. Jika scope melebihi kapasitas, **kurangi scope berdasarkan prioritas, bukan menambah jam kerja**. Pengembangan dan pengujian lokal dulu. Hosting dan domain diputuskan mendekati rilis, dengan perbandingan biaya berulang dan keterbatasan | Jawaban 8 |
| **K-07** | Identitas sementara: nama tampilan **Lukmanul Hakim**, peran **Creative Developer** (fokus web interaktif, frontend, motion design, pengalaman digital). Status mahasiswa S1 Teknik Informatika, tanpa kesan senior. Foto diri **tidak wajib** (desain harus kuat tanpa foto). Kontak yang direncanakan: tautan GitHub, Instagram, dan email via `mailto`, setelah URL/alamat dikonfirmasi. Tanpa formulir kontak, backend, atau layanan pihak ketiga. Lokasi tidak ditampilkan. Tanpa badge "Available for work" sampai dikonfirmasi | Jawaban 10 |
| **K-08** | Accessibility: **WCAG 2.2 Level AA sebagai target desain dan pengembangan, bukan klaim kepatuhan**. Pertimbangan sejak awal: keyboard, fokus terlihat, kontras, struktur heading/landmark/label/alt text, `prefers-reduced-motion`, akses saat animasi atau JS bermasalah (sejauh mungkin), tidak ada informasi penting yang hanya lewat hover/warna/animasi, target sentuh dan keterbacaan di smartphone. Efek yang mengganggu akses harus disederhanakan atau diubah. Verifikasi manual dasar bersedia dilakukan pemilik: keyboard, fokus, reduced motion, Narrator (Windows 10), TalkBack (Android). Prosedur bertahap, gabungan otomatis dan manual, temuan yang belum selesai didokumentasikan. Penjelasan accessibility ringkas dalam bagian teknis ("How I Built This"), tanpa halaman accessibility terpisah, dan tidak boleh mengklaim "WCAG 2.2 AA compliant" hanya karena lulus alat otomatis | Jawaban 11 |
| **K-09** | Batasan proses dan lingkup: **tidak ada backend, database, CMS, authentication, formulir kontak, atau multibahasa di MVP** tanpa kebutuhan nyata yang disetujui. **Suara dihindari di MVP.** Ini bukan larangan mutlak untuk masa depan, tetapi suara tidak boleh masuk MVP tanpa kebutuhan nyata dan persetujuan pemilik. Jika kelak dipertimbangkan, suara harus opsional dan tidak diputar otomatis. Tidak ada dependency atau fitur yang ditambahkan hanya agar terlihat canggih. **Tidak ada coding sebelum dokumen discovery dan spesifikasi disepakati.** Satu tahap pada satu waktu dengan persetujuan manual | Pesan awal, jawaban 2, 5, 7, dan revisi 0.2 |
| **K-10** | Dukungan browser selain Chrome: situs harus tetap memiliki **konten, navigasi, dan layout inti yang berfungsi di Safari dan Firefox sejauh didukung oleh implementasi**. Chrome desktop dan Chrome Android tetap target pengujian utama. Pengujian tambahan di Safari dan Firefox dilakukan **jika memungkinkan**. Browser tersebut **tidak boleh dinyatakan teruji sebelum ada bukti** dan tetap berkategori C (K-05) sampai pengujian dicatat. Pemilik tidak memiliki perangkat Apple, sehingga cara menguji Safari belum ada (V-09) | Revisi 0.2 |
| **K-11** | Transparansi penggunaan AI: dijelaskan **jujur dan proporsional**. Claude digunakan untuk membantu perencanaan dan review. Antigravity CLI digunakan untuk implementasi berbantuan AI. Pemilik tetap bertanggung jawab atas keputusan, pemeriksaan, pengujian, dan hasil akhir. Pernyataan ini harus sesuai praktik nyata (pemilik meninjau laporan dan menyetujui tiap fase, K-06). **Tidak ada klaim bahwa seluruh kode ditulis manual, dan tidak ada klaim kemampuan teknis yang belum dapat dibuktikan.** Susunan kalimat dan lokasi penjelasan ditentukan kemudian dan ditinjau pemilik. Peran AI pada project RSUD belum diketahui (Q-20) | Revisi 0.2 |
| **K-12** | Aturan penengah: bila efek motion bertabrakan dengan aksesibilitas atau performa pada perangkat uji, efek tersebut **disederhanakan atau diganti**. Karena identitas visual tetap penting, urutan penanganan: (1) cari alternatif yang lebih efisien dan tetap mempertahankan karakter visual, (2) sederhanakan, (3) hilangkan hanya jika tidak ada alternatif yang memadai. Akses konten dan accessibility tidak dikorbankan demi efek visual | Revisi 0.2 |
| **K-13** | Website sebagai bukti: website ini sendiri adalah bukti utama kemampuan creative development, tetapi **tidak wajib** dibuat sebagai halaman case study terpisah. Bagian teknis singkat dianggap cukup bila menunjukkan keputusan dan hasil yang nyata (tetap bersyarat pada bukti, A-09) | Revisi 0.2 |

## 8. Preferensi visual (belum final)

| ID | Preferensi |
|---|---|
| **P-01** | Arah karakter: ekspresif, berani, premium, eksperimental, sinematik, interaktif. Motion sebagai identitas utama (transisi, scroll animation, typography reveal, image reveal, interaksi yang dirancang sengaja). Tipografi dan komposisi kuat: headline besar, hierarki jelas, layout berani yang tidak selalu simetris. Pengalaman imersif dengan perpindahan antarbagian yang menyatu, tetapi konten dan navigasi mudah dipahami. Detail spacing, timing, easing, hover, dan responsivitas konsisten |
| **P-02** | Dihindari: tampilan portofolio generik atau template SaaS, minimalis polos tanpa karakter, animasi dekoratif tanpa tujuan, efek berat yang membuat mobile tersendat, navigasi membingungkan, scroll hijacking, animasi yang menghalangi akses konten, desain yang kehilangan karakter di mobile |
| **P-03** | Palet warna, font, dan layout belum dikunci. Arah visual tidak boleh bergantung pada logo kompleks (belum ada logo atau monogram) |
| **P-04** | Proses: eksplorasi 2 sampai 3 alternatif konsep visual dengan kelebihan, risiko performa, dan kesesuaian dengan referensi, sebelum disepakati. Saran tambahan Claude (belum disetujui, lihat A-07): prototipe motion kecil yang diukur di POCO M4 Pro sebelum desain dikunci |
| **P-05** | Referensi tambahan selain Zajno belum ada. Kandidat akan dicari dan diajukan di Tahap 4 untuk dipilih langsung oleh pemilik, karena selera visual harus berasal dari penilaian pemilik |

**Aturan penengah (disetujui, K-12):** jika sebuah efek bertabrakan dengan aksesibilitas atau performa di perangkat uji, efek itu disederhanakan atau diganti. Alternatif yang lebih efisien dan tetap menjaga karakter visual dicari lebih dulu sebelum karakter itu dihilangkan.

## 9. Batasan

| Kategori | Batasan |
|---|---|
| Lingkungan | Windows 10 untuk pengembangan. Implementasi memakai Antigravity CLI 1.3.3. Perintah dan fitur CLI **belum diverifikasi** (V-04) |
| Perangkat | Sesuai K-05. Perangkat uji lemah sampai menengah: ini baseline yang konservatif, bukan jaminan untuk perangkat yang lebih lemah |
| Waktu | 6 sampai 10 jam per minggu, tidak pasti (K-06) |
| Teknologi | Kandidat awal pemilik: Next.js, React, TypeScript, Tailwind CSS, GSAP + ScrollTrigger. **Belum final.** Keputusan stack dibuat di Tahap 3 berdasarkan kebutuhan desain dan arsitektur |
| Backend | Tidak ada di MVP. Konten dikelola sebagai data lokal atau konfigurasi terstruktur (K-09) |
| Biaya | Hindari layanan berbayar tanpa kebutuhan nyata. Biaya hosting dan domain dibandingkan menjelang rilis |
| Konten | Hanya klaim yang dapat dikonfirmasi pemilik. Tidak ada metrik yang dikarang |
| Hukum dan etika | Izin publikasi project RSUD belum terkonfirmasi (R-01). Dokumen ini bukan nasihat hukum |

## 10. Cakupan awal (kandidat, belum disepakati)

Daftar ini berasal dari ide awal pemilik. **Ruang lingkup MVP, urutan prioritas, dan pemotongan ditetapkan di PRD (Tahap 2)**, bukan di sini.

**Area yang dibayangkan pemilik:**
- Interactive Hero (tipografi besar, masked text reveal)
- Selected Projects (layout kreatif, hover, image reveal, transisi saat project dibuka)
- Project Case Studies (latar belakang, masalah, solusi, teknologi, proses, hasil)
- About Me
- Technical Expertise berbasis project nyata, **bukan persentase skill**
- Contact (tautan saja, tanpa formulir)
- Sistem motion (scroll-triggered, staggered typography, parallax terkontrol, page transition, shared-element transition "jika memungkinkan")
- Bagian "How I Built This" (dipertimbangkan, K-02)
- Eksperimen interaktif 0 sampai 3 (opsional, K-02)

**Di luar lingkup MVP (sudah diputuskan, K-03/K-09):** backend, database, CMS, authentication, formulir kontak, suara (dihindari di MVP, bukan larangan mutlak untuk masa depan), dukungan dua bahasa, badge ketersediaan kerja, tampilan lokasi.

**Belum diputuskan:** jumlah dan bentuk tampilan project (inventaris saat ini dua), kedalaman bagian teknis singkat, dan eksperimen mana (jika ada). Situs ini sendiri **tidak wajib** tampil sebagai halaman case study terpisah (K-13).

## 11. Persyaratan lintas-sektoral

### 11.1 Motion tiga tingkat (K-03, K-04)

| Tingkat | Perilaku |
|---|---|
| Desktop | Motion lengkap selama performa baik |
| Mobile | Versi disesuaikan. Karakter visual tetap kuat. Efek berat disederhanakan atau diganti |
| `prefers-reduced-motion` | Pengalaman lebih tenang. Gerakan non-esensial dikurangi atau dihilangkan |

Aturan umum: konten dan navigasi tersedia tanpa menunggu animasi. Tidak ada scroll hijacking. Tidak ada animasi berkedip. Saran (A-06): definisikan tiga tingkat ini sebagai satu sistem terpusat di PRD dan arsitektur, bukan keputusan per komponen.

### 11.2 Performa

Ambang resmi "good" Core Web Vitals (diverifikasi dari web.dev dan sumber pendukung pada 12 Oktober 2026; INP menggantikan FID sejak 12 Maret 2024):

| Metrik | Good | Needs improvement | Poor |
|---|---|---|---|
| LCP | ≤ 2,5 detik | ≤ 4,0 detik | > 4,0 detik |
| INP | ≤ 200 ms | ≤ 500 ms | > 500 ms |
| CLS | ≤ 0,1 | ≤ 0,25 | > 0,25 |

Dinilai pada persentil ke-75 pengguna nyata.

**Rekomendasi (belum disetujui, A-05):**
- Ambang di atas dipakai sebagai batas atas.
- **Angka target Lighthouse dan anggaran ukuran (KB JavaScript, bobot gambar, jumlah font) tidak dikunci sebelum prototipe motion diukur**, karena bergantung pada stack final dan efek hero yang belum ada.
- Data lapangan (CrUX) membutuhkan trafik dan kemungkinan tidak tersedia untuk situs ini. Bukti performa kemungkinan berupa **uji lab yang terdokumentasi** (konfigurasi, throttling, perangkat, versi Chrome, jumlah pengulangan), dan harus disebut sebagai hasil uji lab.
- Gunakan batas per metrik dan bukan hanya skor komposit Lighthouse.
- Risiko desain per metrik: LCP tertunda jika teks hero disembunyikan sampai JS/animasi siap (elemen LCP tidak boleh bergantung pada animasi untuk muncul pertama kali); CLS dari pemecahan teks, pemuatan font, dan gambar tanpa dimensi; INP dari handler scroll/hover berat dan pekerjaan JS panjang saat transisi.

### 11.3 Accessibility

Sesuai K-08. Target WCAG 2.2 AA. Bukti dari kombinasi pemeriksaan otomatis dan manual yang terdokumentasi, termasuk temuan yang belum terselesaikan.

### 11.4 Perangkat dan kategori bukti (K-05)

| Kategori | Arti | Contoh |
|---|---|---|
| A. Teruji nyata | Dijalankan pada perangkat fisik milik pemilik | Chrome di POCO M4 Pro; Chrome di laptop pengembangan |
| B. Simulasi | Chrome DevTools, emulasi layar, throttling | Berbagai ukuran layar, pelambatan CPU dan jaringan |
| C. Belum terverifikasi | Tidak diuji | Safari, iOS, Firefox, Mac, tablet, perangkat Android lain |

Klaim hanya boleh menyebut kategori A dan B sebagai hasil uji, dengan menyebut kategorinya. Kategori C ditulis sebagai "belum diverifikasi". Safari dan Firefox ditargetkan berfungsi pada konten, navigasi, dan layout inti (K-10), tetapi tetap kategori C sampai ada hasil uji yang tercatat.

### 11.5 Cakupan pengujian minimum (K-04)

Chrome desktop dan Chrome Android; tiga kelas ukuran layar (kecil, menengah, besar); navigasi, scroll, interaksi, perpindahan halaman; kondisi aset atau animasi belum termuat; Core Web Vitals dan Lighthouse pada kondisi yang terdokumentasi; dukungan `prefers-reduced-motion`; uji keyboard. Tambahan jika memungkinkan (K-10): pemeriksaan konten, navigasi, dan layout inti di Firefox dan Safari, dicatat dengan kategori buktinya.

## 12. Aturan klaim dan konten

1. Tidak ada metrik, angka dampak, jumlah pengguna, atau penghematan waktu yang belum dikonfirmasi.
2. Tidak ada klaim "cepat", "accessible", atau "WCAG compliant" tanpa hasil uji yang tercatat dan menyebut kategori buktinya.
3. Flash Sale Intelligence ditampilkan jujur sebagai "dalam pengembangan".
4. Penggunaan AI dijelaskan jujur dan proporsional (K-11), termasuk untuk Flash Sale Intelligence dan situs ini sendiri. Tidak ada klaim bahwa seluruh kode ditulis manual, dan tidak ada klaim kemampuan teknis yang belum dapat dibuktikan.
5. Hindari kata yang tidak dapat dibuktikan: "senior", "expert", "award-winning", "passionate", dan klaim tahun pengalaman.
6. Tidak ada informasi kontak, tautan profil, foto, atau klaim profesional yang diasumsikan. Gunakan `[PLACEHOLDER: ...]` sampai dikonfirmasi.
7. Project RSUD: tidak ada nama institusi, screenshot asli, atau repositori publik sebelum izin terkonfirmasi. Alternatifnya: case study teranonimkan dengan UI dibuat ulang memakai data dummy.

## 13. Asumsi

| ID | Asumsi | Status | Dampak jika salah |
|---|---|---|---|
| A-01 | Tidak ada tenggat eksternal | Asumsi kerja | Scope harus ditinjau ulang |
| A-02 | Kapasitas 6 sampai 10 jam per minggu hanyalah perkiraan awal dan akan dikalibrasi dari kecepatan fase pertama | Asumsi kerja | Estimasi fase dan urutan prioritas berubah |
| A-03 | Konten situs dikelola oleh pemilik sendiri sebagai data lokal yang terstruktur, tanpa CMS | Asumsi kerja | Jika editor non-teknis diperlukan, K-09 ditinjau ulang |
| A-04 | Praktik implementasi standar (fitur web yang didukung luas, dengan fallback) cukup untuk menjaga baseline konten, navigasi, dan layout inti di Safari dan Firefox sesuai K-10, tanpa klaim uji | Asumsi kerja | Layout atau animasi bisa rusak di browser yang tidak teruji (R-10) |
| A-05 | Angka target performa final ditetapkan setelah prototipe motion diukur. Pemilik terbuka pada pendekatan ini, selama tidak membuat scope melampaui kapasitas waktu (K-06) | **Menunggu persetujuan** (rekomendasi Claude) | Target dini bisa terlalu longgar atau tidak realistis |
| A-06 | Tiga tingkat motion didefinisikan sebagai satu sistem terpusat | **Menunggu persetujuan** (rekomendasi Claude) | Kondisi tersebar di komponen, pengujian dan pemeliharaan lebih mahal |
| A-07 | Prototipe motion kecil (mis. hero) diukur di POCO M4 Pro lebih awal sebelum desain dikunci. Pemilik terbuka pada pendekatan ini, selama tidak membuat scope melampaui kapasitas waktu (K-06) | **Menunggu persetujuan** (rekomendasi Claude) | Risiko menemukan masalah performa terlambat |
| A-08 | Situs ini tidak membutuhkan akun, data pengguna, atau pelacakan. Apakah analytics dipakai belum dibahas | Asumsi kerja | Jika analytics ditambahkan, ada implikasi privasi dan dependency |
| A-09 | Bagian teknis ("How I Built This") hanya masuk bila hasil uji nyata tersedia saat rilis | Asumsi kerja | Jika bukti belum ada, bagian ditunda atau memuat batasan secara jujur |

A-05 sampai A-07 tidak diperlakukan sebagai keputusan di PRD dan arsitektur sampai pemilik menyetujuinya (Q-21).

## 14. Risiko

| ID | Risiko | Tingkat | Mitigasi awal |
|---|---|---|---|
| R-01 | Izin publikasi nama RSUD, screenshot, dan detail sistem belum terkonfirmasi | **Tinggi** | Gerbang rilis (bagian 17). Konfirmasi tertulis dari pihak berwenang. Cadangan: versi teranonimkan dengan data dummy |
| R-02 | Pihak selain RSUD (mis. kampus atau pembimbing PKL) mungkin memiliki hak atas hasil PKL. **Belum diketahui, hanya perlu diperiksa** | Sedang | V-03. Periksa ketentuan PKL |
| R-03 | Data sensitif (pegawai, pasien, denah, kredensial) bocor lewat screenshot atau repositori | **Tinggi** | Inventarisasi data sebelum screenshot. Gunakan data dummy. Audit repositori sebelum dibuka |
| R-04 | Efek motion tersendat di POCO M4 Pro atau laptop i3/MX130 | **Tinggi** | Prototipe terukur lebih awal (A-07, menunggu persetujuan), tiga tingkat motion (A-06, menunggu persetujuan), aturan penengah K-12: cari alternatif yang lebih efisien dulu sebelum karakter visual dihilangkan |
| R-05 | Konflik "imersif" versus "tanpa scroll hijacking dan konten selalu dapat diakses" | Sedang | Kontinuitas lewat transisi dan tipografi, bukan mengambil alih scroll |
| R-06 | Hanya dua project dan keduanya bukan project motion, sehingga galeri bergaya Zajno terasa kosong | Sedang | Tentukan bentuk tampilan di PRD berdasarkan konten nyata. Situs sendiri sebagai bukti utama (K-02, K-13) lewat bagian teknis singkat, bukan halaman case study wajib. Jangan menambah project fiktif |
| R-07 | Klaim engineering dan kualitas tidak terbukti oleh isi situs | Sedang | Bagian teknis hanya dari hasil uji nyata. Source code publik hanya jika aman |
| R-08 | Jadwal tersendat (kuliah, PKL, proyek lain) | Sedang | Fase kecil yang bisa dihentikan dengan aman. Pengurangan scope sebelum penambahan jam |
| R-09 | Page transition dan shared-element transition pada Next.js App Router sulit dan rapuh | Sedang | Verifikasi kelayakan dan dukungan browser di Tahap 3. Siapkan alternatif lebih sederhana |
| R-10 | Safari/macOS (umum di studio kreatif) tidak teruji | Sedang | Baseline fungsional sesuai K-10 (konten, navigasi, layout inti). Gunakan fitur dengan fallback. Uji tambahan Firefox di laptop bila memungkinkan (V-09). Safari tetap kategori C sampai ada bukti uji |
| R-11 | Versi, lisensi, dan ketentuan GSAP serta plugin berubah | Rendah sampai sedang | Verifikasi dari sumber resmi di Tahap 3 (V-05). Pin versi dependency |
| R-12 | Scope creep (eksperimen, WebGL, fitur tambahan) | Sedang | Daftar di luar lingkup, kriteria masuk eksperimen (K-02), pemotongan berdasarkan prioritas |
| R-13 | Flash Sale Intelligence terus berubah sehingga konten case study usang | Rendah | Bekukan konten pada satu versi bertanggal (Q-13) |
| R-14 | Hasil uji lab berfluktuasi di laptop lemah dan tidak mewakili pengguna nyata | Sedang | Prosedur uji berulang dengan kondisi tercatat. Sebut sebagai hasil uji lab |
| R-15 | Pengalaman pemilik dengan screen reader terbatas, sehingga uji manual bisa melewatkan masalah | Rendah sampai sedang | Prosedur bertahap dengan skrip uji. Catat keterbatasan secara jujur |
| R-16 | Persepsi: situs yang dijadikan bukti kemampuan ternyata sebagian besar ditulis oleh agen AI | Sedang | Prinsip transparansi sudah ditetapkan (K-11). Kalimat final ditinjau pemilik sebelum bagian teknis ditulis. Pernyataan tanggung jawab hanya valid bila pemeriksaan dan pengujian benar-benar dilakukan pemilik |
| R-17 | Tautan profil publik (mis. Instagram) tidak sesuai citra profesional | Rendah | Pemilik meninjau isi profil sebelum ditautkan |
| R-18 | Fitur dan perintah Antigravity CLI 1.3.3 belum diverifikasi, sehingga orkestrasi bisa gagal atau tidak aman | Sedang | V-04: sediakan dokumentasi resmi atau output `--help` sebelum Tahap 8 |
| R-19 | Placeholder atau data uji ikut terpublikasi | Sedang | Gerbang rilis dengan pencarian otomatis terhadap penanda `[PLACEHOLDER:` |

## 15. Pertanyaan terbuka

Pertanyaan yang diperlukan **sebelum Tahap 2 selesai** ditandai "PRD". Sisanya boleh menunggu tahap lain.

| ID | Pertanyaan | Dibutuhkan untuk |
|---|---|---|
| Q-01 | Apakah RSUD Ansari Saleh (pihak berwenang) mengizinkan publikasi nama institusi, screenshot, dan detail sistem? Siapa pihak yang berwenang, dan dalam bentuk apa konfirmasinya? | Sebelum rilis. Bentuk case study (PRD) |
| Q-02 | Jenis data apa di aplikasi RSUD yang aman ditampilkan? | Pembuatan aset case study |
| Q-03 | Apakah repositori project RSUD boleh dibuka untuk publik? | Konten bukti teknis |
| Q-04 | Apakah repositori situs ini sendiri akan dibuka untuk publik? | Strategi bukti dan workflow (PRD, Tahap 3) |
| Q-05 | Apakah foto diri ditampilkan? | PRD (About) |
| Q-06 | Kontak final: URL GitHub, URL Instagram, alamat email | Sebelum rilis |
| Q-07 | Apakah lokasi umum ditampilkan, dan jika ya, apa? | Sebelum rilis |
| Q-08 | Pernyataan ketersediaan (magang, kerja penuh waktu, freelance) | Sebelum rilis |
| Q-10 | Seberapa penting page transition dan shared-element transition dibanding risiko teknisnya? | PRD dan Tahap 3 |
| Q-11 | Apakah WebGL menjadi eksperimen opsional? Efek mana yang dibayangkan? | PRD dan Tahap 3 |
| Q-13 | Apakah konten case study Flash Sale Intelligence dibekukan pada satu versi bertanggal? | PRD dan konten |
| Q-16 | Apakah ingin uji persepsi kecil (beberapa orang dari target audiens) untuk menilai T-01? | Quality gates (Tahap 6) |
| Q-17 | Headline mana yang dipilih atau diubah (Lampiran A)? | PRD dan Tahap 4 |
| Q-18 | Apakah analytics dipakai? Jika ya, jenis dan implikasi privasinya | Tahap 3 |
| Q-19 | Hosting dan domain (opsi gratis atau berbayar, domain sendiri) | Menjelang rilis |
| Q-20 | Apakah alat AI juga dipakai pada project Hospital and APAR Fire Inspection System, dan sejauh mana? (agar penjelasan peran AI konsisten di kedua case study dan di bagian teknis, K-11) | PRD (konten) |
| Q-21 | Apakah A-05, A-06, dan A-07 disetujui sebagai keputusan (target performa setelah prototipe diukur, sistem motion terpusat, hero diuji lebih awal), dengan syarat tidak melampaui kapasitas (K-06)? | PRD, Tahap 3, dan Tahap 5 |

### 15.1 Pertanyaan yang terjawab pada revisi 0.2

| ID | Pertanyaan semula | Jawaban dicatat sebagai |
|---|---|---|
| Q-09 | Tingkat dukungan Safari dan Firefox | K-10. Konten, navigasi, dan layout inti berfungsi sejauh didukung implementasi. Uji tambahan jika memungkinkan, tanpa klaim sebelum ada bukti |
| Q-12 | Cara menjelaskan peran alat AI | K-11. Prinsip ditetapkan, kalimat final menyusul dan ditinjau pemilik. Detail peran AI pada project RSUD masuk Q-20 |
| Q-14 | Persetujuan aturan penengah | K-12. Disetujui, dengan urutan: alternatif efisien, penyederhanaan, baru penghilangan |
| Q-15 | Situs sebagai case study ketiga | K-13. Tidak wajib halaman case study terpisah. Bagian teknis singkat cukup bila nyata |

## 16. Item verifikasi

| ID | Yang perlu diverifikasi | Oleh |
|---|---|---|
| V-01 | Versi Android dan Chrome di POCO M4 Pro, serta varian (nomor model) | Pemilik |
| V-02 | Versi Chrome dan Node.js di laptop pengembangan | Pemilik |
| V-03 | Ketentuan PKL terkait hak atas hasil kerja (kampus atau pembimbing) | Pemilik |
| V-04 | Dokumentasi resmi Antigravity CLI 1.3.3 atau output `--help` (diperlukan sebelum Tahap 8) | Pemilik |
| V-05 | Lisensi, versi, dan ketentuan GSAP serta plugin dari sumber resmi (saat Tahap 3) | Claude, dengan konfirmasi pemilik |
| V-06 | Kelayakan page transition di Next.js App Router dan dukungan browser (Tahap 3) | Claude, dengan prototipe oleh pemilik |
| V-07 | Metode pengukuran di Android nyata (mis. remote debugging Chrome via USB) dari dokumentasi resmi (Tahap 6) | Claude |
| V-08 | Harga dan batasan hosting terkini saat mendekati rilis | Claude, dari sumber resmi |
| V-09 | Cara menguji Firefox dan Safari: apakah Firefox terkini masih mendukung Windows 10 di laptop pemilik, dan apakah ada cara menguji Safari tanpa perangkat Apple (mis. meminjam perangkat atau layanan pihak ketiga, beserta biaya dan batasannya). Dievaluasi dari sumber resmi di Tahap 6 | Pemilik dan Claude |

## 17. Gerbang rilis (syarat sebelum publikasi)

Situs **tidak boleh dipublikasikan** selama salah satu kondisi berikut belum terpenuhi:

1. Tidak ada lagi penanda `[PLACEHOLDER:` di konten atau kode.
2. Izin publikasi project RSUD terkonfirmasi (Q-01), atau situs memakai versi teranonimkan dengan data dummy.
3. Tidak ada data sensitif, kredensial, atau konfigurasi lokal pada aset yang dipublikasikan atau pada repositori yang dibuka.
4. Kontak, tautan profil, dan klaim profesional telah dikonfirmasi pemilik.
5. Setiap klaim performa dan accessibility di situs memiliki hasil uji tercatat dengan kategori bukti (A/B/C).
6. Quality gates (Tahap 6) terpenuhi dengan bukti nyata, bukan klaim.
7. Penjelasan penggunaan AI sesuai K-11 sudah ada, ditinjau pemilik, dan tidak memuat klaim yang tidak dapat dibuktikan.

## 18. Hal yang diserahkan ke tahap berikutnya

| Tahap | Keputusan atau pekerjaan yang diturunkan dari brief ini |
|---|---|
| 2: PRD | Ruang lingkup MVP dan di luar lingkup, kebutuhan fungsional dan nonfungsional, user flows, edge cases, jawaban Q yang bertanda PRD, acceptance criteria termasuk performa, accessibility, dan mobile |
| 3: Arsitektur dan ADR | Stack final (kandidat: Next.js, React, TypeScript, Tailwind, GSAP), rendering dan struktur konten, strategi page transition, keputusan WebGL, Lenis, video, dan kursor (hanya bila terbukti perlu), hosting, anggaran performa |
| 4: Desain teknis | Struktur folder, model data konten, spesifikasi UI/UX dan sistem motion tiga tingkat, 2 sampai 3 alternatif konsep visual, referensi tambahan, security dan privacy (termasuk audit data sebelum publikasi), error dan fallback |
| 5: Rencana implementasi | Fase kecil sesuai kapasitas, prototipe motion terukur lebih awal, dependency antar fase |
| 6: Strategi uji dan Quality Gates | Prosedur uji Chrome desktop dan Android, accessibility otomatis dan manual bertahap, kondisi uji performa terdokumentasi, bukti kelulusan |

---

## Lampiran A. Opsi headline (bahasa Inggris, belum dipilih)

Headline adalah janji yang harus dibuktikan oleh situs. Pilihan ini perlu dikonfirmasi (Q-17).

1. "Creative developer building interactive web experiences." Lugas dan mudah dipindai. Kekuatannya bergantung pada bukti dari situs.
2. "Informatics Engineering student crafting interactive interfaces with motion and care." Menyebut status dengan jujur. Lebih panjang dan kesan awalnya lebih kecil.
3. "Frontend craft and motion design, grounded in software that real people use." Menghubungkan dengan aplikasi yang dipakai di dunia nyata. **Hanya dipakai bila izin project RSUD terkonfirmasi.**
4. "Creative developer in the making. Interfaces, motion, and systems that hold up." Terbuka tentang posisi yang sedang berkembang. Bisa terbaca sebagai belum siap.

Subjudul pendek berisi status ("Informatics Engineering student") dapat dipasang di bawah headline mana pun agar tetap jujur.

## Lampiran B. Observasi referensi (https://motion.zajno.com/)

Diperoleh dari teks dan struktur halaman pada saat discovery. **Gerak, ritme, dan tampilan visualnya tidak dapat dinilai dari data yang tersedia.**

- Situs edukasi tentang prinsip motion (easing, delay, fade, morph, masking, dimension, parallax, zoom), bukan portofolio.
- Dibangun di Webflow. Stack proyek ini tidak perlu meniru implementasinya.
- Memuat banyak video demo dan aset suara (musik latar serta efek hover/klik), yang mahal untuk performa dan accessibility.
- Navigasi satu halaman dengan anchor, tanpa halaman detail atau transisi antarhalaman seperti yang dibayangkan untuk case study.

Kesimpulan untuk proyek ini: yang diambil adalah standar kualitas, bukan struktur atau teknik implementasinya.

## Lampiran C. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 12 Oktober 2026 | Draf awal berdasarkan sesi discovery Tahap 1 |
| 0.2 | 12 Oktober 2026 | Revisi atas review pemilik: K-09 diperjelas (suara dihindari di MVP, bukan larangan mutlak); ditambah K-10 (Safari/Firefox), K-11 (transparansi AI), K-12 (aturan penengah disetujui), K-13 (situs sebagai bukti tanpa halaman case study wajib); A-05 sampai A-07 ditandai menunggu persetujuan, A-04 ditulis ulang; R-04, R-06, R-10, R-16 diperbarui; Q-09, Q-12, Q-14, Q-15 dipindah ke bagian terjawab; ditambah Q-20, Q-21, V-09 dan syarat ketujuh pada gerbang rilis |
