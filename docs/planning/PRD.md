# PRD — Creative Developer Portfolio Website

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/PRD.md` |
| Versi | 0.3 (mencatat keputusan DP-11, DP-12, DP-13) |
| Tanggal | 12 Oktober 2026 |
| Tahap workflow | Tahap 2 dari 10 (Product Requirements) |
| Status | **Keputusan DP-11, DP-12, DP-13 dicatat atas jawaban pemilik (12 Oktober 2026). Teks revisi 0.3 ditinjau pemilik bersama laporan Tahap 3** |
| Dasar | `PROJECT_BRIEF.md` versi 0.2 (keputusan K-01 sampai K-13) |
| Pemilik produk | Lukmanul Hakim (nama tampilan sementara) |

## Cara membaca dokumen ini

| Penanda | Arti |
|---|---|
| **FR-xx / NFR-xx** | Kebutuhan fungsional / nonfungsional. Sub-butir ditulis FR-03.1, FR-03.2, dan seterusnya |
| **M / S / C** | Prioritas: **M**ust (wajib di MVP), **S**hould (diusahakan di MVP, boleh bergeser), **C**ould (opsional, hanya bila ada kapasitas dan alasan nyata) |
| **UF-xx** | User flow |
| **EC-xx** | Edge case |
| **AC-xx** | Acceptance criteria (dapat diuji) |
| **DP-xx** | Titik keputusan tertunda: pemilik belum memutuskan, PRD tidak mengasumsikan jawabannya |
| **AS-xx** | Asumsi PRD yang ditandai. Harus dikoreksi bila salah |
| **K-xx, A-xx, R-xx, Q-xx, V-xx** | Rujukan ke `PROJECT_BRIEF.md` |
| **[SEMENTARA]** | Angka atau rincian yang sengaja belum dikunci dan ditetapkan di tahap berikutnya |
| **A / B / C** | Kategori bukti uji (K-05): **A** teruji nyata di perangkat milik pemilik, **B** simulasi (DevTools), **C** belum terverifikasi |

PRD ini **tidak** menetapkan stack final, arsitektur, desain visual final, palet, font, anggaran ukuran (KB), atau angka target Lighthouse final. Semua itu milik Tahap 3 dan 4. PRD menetapkan **apa** yang harus benar dan **bagaimana membuktikannya**, bukan cara membangunnya.

---

## 1. Tujuan dokumen

1. Menerjemahkan keputusan di brief menjadi kebutuhan yang dapat diuji.
2. Menetapkan ruang lingkup MVP, urutan pemotongan bila kapasitas kurang (K-06), dan hal yang di luar lingkup.
3. Menyediakan dasar untuk arsitektur (Tahap 3), desain teknis (Tahap 4), rencana fase (Tahap 5), dan strategi uji (Tahap 6).
4. Menjaga agar setiap klaim di situs memiliki bukti (K-02, K-11, NFR-09).

## 2. Ringkasan produk

Website portofolio pribadi berbahasa Inggris (K-04) yang membuktikan kemampuan creative development lewat pengalamannya sendiri (K-02, K-13). Pengunjung menjelajahi hero yang berkesan, project dalam bentuk case study yang jujur, ringkasan kemampuan teknis berbasis project nyata, dan cara menghubungi pemilik. Motion adalah daya tarik utama tetapi tunduk pada akses konten, accessibility, dan performa (K-03, K-08, K-12).

### 2.1 Persona kerja (hipotesis, bukan riset pengguna)

Persona ini disusun dari audiens di brief (K-01). Tidak ada data demografis atau perilaku yang dikarang.

| ID | Persona | Tujuan di situs | Yang dinilai | Kebiasaan yang diduga |
|---|---|---|---|---|
| P1 | Pimpinan kreatif / developer di studio atau agensi (utama) | Menilai apakah pemilik layak dipertimbangkan sebagai creative developer | Kualitas motion, detail, tipografi, kematangan implementasi | Membuka di desktop dan mungkin di ponsel. Mungkin memakai Safari atau Chrome (AS-04) |
| P2 | Recruiter / engineer di perusahaan teknologi | Memahami kemampuan engineering, status, dan cara menghubungi | Kejelasan, bukti teknis, kejujuran klaim | Memindai cepat. Sering membuka dari tautan di CV atau profil |
| P3 | Developer lain atau calon klien | Melihat contoh kerja dan cara berpikir | Studi kasus, struktur proses | Membaca lebih dalam, mungkin meninjau kode bila tersedia |

## 3. Prinsip produk

Prinsip ini menjadi pedoman keputusan bila ada konflik antar kebutuhan.

1. **Konten dulu, motion mendukung.** Konten dan navigasi tidak menunggu animasi (K-03).
2. **Akses di atas efek.** Efek yang mengganggu akses atau performa diganti dengan alternatif yang lebih efisien, lalu disederhanakan, dan baru dihilangkan bila tidak ada pilihan (K-12).
3. **Bukti sebelum klaim.** Tidak ada klaim performa, accessibility, dampak, atau kemampuan tanpa bukti yang tercatat dan kategorinya disebut (K-05, K-08, K-11).
4. **Mobile dirancang, bukan dikecilkan.** Karakter visual kuat di layar kecil (K-04).
5. **Sederhana dan terpelihara.** Tidak ada dependency atau fitur yang hanya demi terlihat canggih (K-09, T-04).
6. **Motion yang konsisten.** Tiga tingkat motion memakai token yang sama (FR-10.4). Pemusatan penentuan tingkat di satu tempat adalah rekomendasi implementasi (A-06), bukan kewajiban (DP-13).

---

## 4. Ruang lingkup

### 4.1 Dalam lingkup MVP

| Area | Prioritas | Ukuran relatif* | Rujukan |
|---|---|---|---|
| Struktur halaman dan navigasi (FR-01, FR-02) | M | M | K-04 |
| Hero interaktif (FR-03) | M | M | K-01, K-03 |
| Selected Projects (FR-04) | M | L | K-01, K-02 |
| Dua case study (FR-05) | M | L | K-02 |
| About (FR-06) | M | S | K-07 |
| Technical Expertise berbasis project (FR-07) | M | S | K-02 |
| Contact dan tautan (FR-09) | M | S | K-07 |
| Tautan CV (FR-09.6, FR-09.7) | S | S | DP-11 |
| Sistem motion tiga tingkat (FR-10) | M | L | K-03, A-06 |
| Konten terstruktur dan gerbang placeholder (FR-12) | M | M | K-07, K-09 |
| 404 dan fallback (FR-13) | M | S | K-04 |
| How I Built This, versi singkat (FR-08) | S (bersyarat bukti, A-09) | M | K-02, K-13 |
| Transisi halaman dasar (FR-11) | S | L | K-03, DP-01 |
| Metadata dan berbagi (FR-14) | S | S | AS-06 |

\* Ukuran relatif adalah **perkiraan perbandingan usaha**, bukan jam kerja (AS-07). Estimasi fase dibuat di Tahap 5 dan dikalibrasi dari fase pertama (A-02).

### 4.2 Opsional (hanya bila kapasitas ada dan alasan teknis nyata terbukti)

| Item | Syarat masuk | Rujukan |
|---|---|---|
| Shared-element transition | Kelayakan terbukti di Tahap 3 dan lolos uji performa | K-03, DP-01, V-06 |
| Eksperimen interaktif (0 sampai 3) | Menunjukkan kemampuan yang belum terwakili pengalaman utama, tidak menjadi playground (FR-15) | K-02, DP-02 |
| Kursor kustom | Hanya perangkat dengan pointer halus, kursor sistem tetap tersedia | K-03 |
| Video preview project | Memberi informasi yang tidak dapat diberikan gambar. Tidak autoplay berat | K-03 |
| Smooth scrolling (mis. Lenis) | Terbukti memperbaiki efek tertentu dan lolos uji keyboard/anchor | K-03 |
| WebGL/3D/shader | Satu efek yang terbukti meningkatkan identitas, ada fallback, lolos uji POCO M4 Pro dan laptop uji | K-03, DP-02 |

### 4.3 Di luar lingkup MVP

Backend, database, CMS, authentication, formulir kontak, suara (dihindari, bukan larangan mutlak, K-09), dukungan dua bahasa (struktur konten disiapkan agar tidak menyulitkan penambahan, K-04), badge "Available for work" sebelum dikonfirmasi, tampilan lokasi sebelum dikonfirmasi, halaman accessibility terpisah (K-08), halaman case study terpisah untuk situs ini sendiri (K-13).

Belum diputuskan sehingga **tidak** dimasukkan: analytics (DP-08), blog, mode terang/gelap, pencarian, komentar, dan halaman admin. Mode terang/gelap tidak diminta pemilik dan diputuskan, bila relevan, di Tahap 4.

### 4.4 Urutan pemotongan bila kapasitas kurang (usulan, AS-08)

Sesuai K-06, pengurangan scope dilakukan menurut prioritas, bukan penambahan jam. Dipotong dari atas ke bawah:

1. Eksperimen interaktif, WebGL, video, Lenis, kursor kustom.
2. Shared-element transition.
3. Variasi motion di tingkat mobile (disederhanakan, karakter inti dipertahankan).
4. Transisi halaman menjadi bentuk paling sederhana.
5. How I Built This dipersingkat menjadi hanya butir yang sudah ada buktinya.
6. Kedalaman Technical Expertise dan About.

**Tidak boleh dipotong:** baseline accessibility (NFR-02), dukungan reduced motion (NFR-05), gerbang integritas konten dan izin (NFR-09, FR-12), pengukuran performa terdokumentasi (NFR-01), dan dua case study yang jujur.

---

## 5. Struktur informasi (disetujui, DP-12)

Struktur ini disetujui pemilik pada 12 Oktober 2026 (DP-12).

| Rute | Isi | Catatan |
|---|---|---|
| `/` | Satu halaman dengan bagian berurutan: Hero, Selected Projects, About, Technical Expertise, How I Built This (bila masuk), Contact | Navigasi antarbagian memakai anchor yang berfungsi tanpa JS |
| `/projects/[slug]` | Case study per project | Dua rute pada MVP. Dapat dibuka langsung lewat tautan (UF-08) |
| Halaman 404 | Penanganan rute tidak dikenal | FR-13 |

Keputusan DP-12: About dan How I Built This adalah bagian di halaman utama, dan How I Built This berbentuk ringkas. Halaman terpisah tidak dibuat kecuali ada cukup materi untuk menjelaskan proses teknis secara mendalam. Bila itu terjadi, perubahan diajukan sebagai revisi PRD.

---

## 6. Kebutuhan fungsional

### FR-01 Struktur halaman dan perutean

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-01.1 | M | Situs menyediakan halaman utama dan satu halaman detail per project (case study) | K-02 |
| FR-01.2 | M | Setiap rute dapat dibuka langsung lewat URL, dimuat ulang, dan dibagikan, serta menampilkan konten yang benar tanpa harus melewati halaman utama | UF-08 |
| FR-01.3 | M | Tombol back/forward browser berfungsi wajar, termasuk saat transisi halaman aktif | EC-12 |
| FR-01.4 | M | Rute yang tidak dikenal menampilkan halaman 404 yang dapat dinavigasi | FR-13 |
| FR-01.5 | M | Atribut bahasa dokumen adalah Bahasa Inggris | K-04 |

### FR-02 Navigasi global

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-02.1 | M | Navigasi menyediakan akses ke Projects, About, Expertise, Contact, dan Beranda dari setiap halaman | K-01 |
| FR-02.2 | M | Pada layar kecil, navigasi berbentuk menu sederhana yang nyaman disentuh. Dapat dibuka dan ditutup dengan keyboard, mengelola fokus dengan benar, dan dapat ditutup dengan tombol Escape | K-04, K-08 |
| FR-02.3 | M | Tersedia tautan "skip to main content" yang terlihat saat difokuskan | K-08 |
| FR-02.4 | M | Anchor ke bagian di halaman utama menggulir ke bagian yang benar dan tidak tertutup header tetap | K-03, EC-10 |
| FR-02.5 | M | Penanda bagian aktif (bila ada) tidak bergantung pada warna saja | K-08 |
| FR-02.6 | M | Navigasi tidak menunggu animasi selesai untuk dapat digunakan | K-03 |
| FR-02.7 | S | Dari halaman case study tersedia jalur kembali ke daftar project dan ke project berikutnya | UF-04 |

### FR-03 Hero interaktif

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-03.1 | M | Hero menampilkan nama tampilan, peran (Creative Developer), pernyataan status yang jujur (mahasiswa S1 Teknik Informatika), dan headline. Teks headline dipilih pemilik (DP-07) | K-07, K-11 |
| FR-03.2 | M | Hero menyediakan tindakan utama menuju Selected Projects | K-01 |
| FR-03.3 | M | Hero menggunakan tipografi besar dan animasi masuk terarah (masked text reveal, stagger) sebagai pembuka pengalaman | K-03, P-01 |
| FR-03.4 | M | Elemen terbesar hero (kandidat LCP) **tidak boleh bergantung pada JavaScript atau animasi untuk muncul pertama kali** dan tidak boleh disembunyikan sebelum animasi siap | NFR-01 |
| FR-03.5 | M | Pada viewport pertama di layar kecil acuan (AS-05), nama, peran, status, dan akses ke navigasi terlihat tanpa menggulir | K-01 |
| FR-03.6 | M | Tidak ada layar loading penuh yang memblokir konten atau navigasi. Durasi animasi masuk ditetapkan [SEMENTARA] di desain motion (Tahap 4) | K-03 |
| FR-03.7 | M | Pada tingkat reduced motion, hero tampil statis atau dengan perubahan opacity yang tenang, dan pesan yang sama tetap tersampaikan | K-03, NFR-05 |
| FR-03.8 | M | Tidak ada badge ketersediaan kerja kecuali dikonfirmasi pemilik (Q-08) | K-07 |

### FR-04 Selected Projects

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-04.1 | M | Daftar project dirender dari data konten, bukan ditulis tetap di komponen. Menambah project berarti menambah data dan aset, bukan mengubah komponen | T-04, FR-12 |
| FR-04.2 | M | Setiap entri menampilkan judul, satu kalimat deskripsi, peran pemilik, label status jujur (mis. "dalam pengembangan"), dan teknologi utama | K-02, NFR-09 |
| FR-04.3 | M | Setiap entri dapat dibuka ke case study lewat tautan yang dapat difokuskan keyboard | K-08 |
| FR-04.4 | M | Interaksi hover (mis. image reveal) memiliki padanan untuk fokus keyboard dan sentuhan. Tidak ada informasi yang hanya muncul lewat hover | K-03, K-08 |
| FR-04.5 | M | Tampilan terlihat disengaja untuk dua project dan tetap valid bila project bertambah, tanpa mengubah kode komponen | R-06 |
| FR-04.6 | M | Gambar memiliki dimensi eksplisit untuk mencegah pergeseran layout, dan pemuatan malas dipakai kecuali untuk elemen LCP | NFR-01 |
| FR-04.7 | M | Dengan reduced motion, interaksi diganti perubahan non-gerak (mis. perubahan warna atau opacity) | NFR-05 |

### FR-05 Case study project

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-05.1 | M | Halaman case study memuat bagian: ikhtisar dan konteks, masalah, solusi, teknologi, proses pengembangan, peran pemilik, hasil, dan status serta keterbatasan | K-02 |
| FR-05.2 | M | Bagian **hasil** hanya memuat butir yang dikonfirmasi pemilik. Bila belum ada metrik terukur, bagian itu menyatakannya secara jujur dan tidak diisi angka | NFR-09 |
| FR-05.3 | M | Setiap case study memuat keterangan penggunaan AI dalam project tersebut. Isi untuk Flash Sale Intelligence: dikembangkan sendiri dengan bantuan AI coding tools. Isi untuk project RSUD: **menunggu Q-20** | K-11, DP-09 |
| FR-05.4 | M | Setiap media bermakna memiliki teks alternatif yang tidak membocorkan data internal. Media dekoratif ditandai agar diabaikan pembaca layar | K-08, NFR-08 |
| FR-05.5 | M | Case study Hospital and APAR Fire Inspection System mendukung dua varian konten: **(A)** dengan nama institusi dan visual asli, **(B)** teranonimkan dengan UI dibuat ulang memakai data dummy. Varian ditentukan oleh status izin (DP-03). Varian A hanya dapat dirilis bila izin terkonfirmasi | K-07, R-01, R-03 |
| FR-05.6 | M | Case study Flash Sale Intelligence menyatakan status "dalam pengembangan" dan tanggal pembaruan konten. Pembekuan versi menunggu DP-06 | K-02, R-13 |
| FR-05.7 | M | Tautan ke demo, repositori, atau sumber lain hanya ditampilkan bila diizinkan dan dikonfirmasi. Bila tidak ada, komponen tidak tampil (tanpa tautan mati) | DP-04 |
| FR-05.8 | S | Di akhir halaman tersedia navigasi ke project berikutnya dan kembali ke daftar | FR-02.7 |
| FR-05.9 | M | Halaman dapat dibaca penuh tanpa menunggu animasi dan memakai struktur heading yang berurutan | K-03, K-08 |

### FR-06 About

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-06.1 | M | Memuat profil singkat, minat, pendekatan membangun software, dan status mahasiswa S1 Teknik Informatika tanpa kesan senior | K-07 |
| FR-06.2 | M | Bagian bekerja baik **dengan dan tanpa foto**. Foto tidak wajib (DP-05) | K-07 |
| FR-06.3 | M | Tidak menampilkan lokasi atau alamat. Tidak menampilkan pernyataan ketersediaan kerja sebelum dikonfirmasi | K-07 |
| FR-06.4 | M | Tidak memuat klaim tahun pengalaman atau kata yang tidak dapat dibuktikan | NFR-09 |

### FR-07 Technical Expertise

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-07.1 | M | Menampilkan teknologi dan keahlian **berdasarkan project nyata**. Setiap butir menyebut project tempat digunakan dan satu kalimat tentang cara penggunaannya | K-02 |
| FR-07.2 | M | **Tidak ada** persentase, bar level, bintang, atau skor kemampuan | Brief, bagian 10 |
| FR-07.3 | M | Butir yang belum ditunjukkan project mana pun tidak ditampilkan sebagai keahlian | NFR-09 |
| FR-07.4 | M | Daftar dirender dari data konten yang terhubung ke data project | FR-12 |

### FR-08 How I Built This (versi singkat, bersyarat bukti)

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-08.1 | S | Bagian ringkas yang menjelaskan keputusan teknis situs: pendekatan animasi dan alasannya, optimasi performa, pendekatan accessibility, serta alasan keputusan stack (tanpa dependency berlebihan) | K-02, K-13 |
| FR-08.2 | S | Setiap butir performa atau accessibility **hanya** memuat hasil uji yang benar-benar dijalankan, dengan kondisi uji, perangkat, dan kategori bukti (A/B/C) | K-05, K-08, A-09 |
| FR-08.3 | S | Menyatakan batasan pengujian secara jujur, termasuk bahwa Safari dan perangkat Apple belum terverifikasi bila memang belum | K-10 |
| FR-08.4 | S | Memuat penjelasan penggunaan AI sesuai K-11, tanpa klaim bahwa seluruh kode ditulis manual | K-11 |
| FR-08.5 | S | Bila hasil uji belum tersedia saat rilis, bagian ini ditunda atau dipersingkat hingga hanya butir yang berbukti. Tidak diisi klaim sementara | A-09 |
| FR-08.6 | S | Tidak menyatakan situs "WCAG 2.2 AA compliant" hanya karena lulus alat otomatis | K-08 |
| FR-08.7 | S | Tautan ke kode sumber situs hanya bila repositori dibuka dan lolos pemeriksaan (DP-04) | Q-04 |
| FR-08.8 | S | Bagian ini berbentuk ringkas di halaman utama. Tidak ada rute terpisah kecuali ada materi yang cukup untuk menjelaskan proses teknis secara mendalam, dan perubahan itu diajukan sebagai revisi PRD | DP-12, K-13 |

### FR-09 Contact dan tautan eksternal

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-09.1 | M | Menyediakan tautan GitHub, Instagram, dan email melalui `mailto`. Nilai kontak adalah `[PLACEHOLDER: ...]` sampai dikonfirmasi pemilik (Q-06) | K-07 |
| FR-09.2 | M | **Tidak ada formulir kontak**, backend, atau layanan pihak ketiga | K-07, K-09 |
| FR-09.3 | M | Tautan eksternal dapat dibedakan dari tautan internal bagi pembaca layar, dan dibuka dengan atribut keamanan yang sesuai | NFR-08 |
| FR-09.4 | M | Alamat email dapat dibaca dan dijangkau pengguna keyboard dan pembaca layar. Pendekatan mengurangi spam ditentukan di Tahap 4 | EC-17 |
| FR-09.5 | M | Kontak tersedia dari bagian Contact dan footer pada setiap halaman | K-01 |
| FR-09.6 | S | Tersedia tautan ke CV berbahasa Inggris berupa berkas PDF statis, dengan tautan biasa tanpa fitur unduh khusus. Tautan **hanya ditampilkan** setelah CV siap dan sudah diperiksa pemilik. Bila belum siap, tautan dan berkas CV tidak ada di build rilis (tanpa tautan mati) | DP-11, EC-20 |
| FR-09.7 | S | Berkas CV yang dirilis tidak memuat data sensitif atau data pribadi yang tidak dikonfirmasi pemilik (mis. alamat rumah, nomor telepon), dan metadata PDF dibersihkan | DP-11, NFR-08.2 |

### FR-10 Sistem motion

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-10.1 | M | Motion terdiri dari tiga tingkat: desktop lengkap, mobile disesuaikan, dan reduced motion. Tiga tingkat ini adalah keputusan (K-03). **Pemusatan penentuan tingkat di satu tempat adalah rekomendasi implementasi (A-06), bukan kewajiban** (keputusan pemilik, DP-13). AC-24 menguji perilaku tiga tingkat, bukan letak kodenya | K-03, A-06 |
| FR-10.2 | M | Pola motion yang didukung: masked text reveal, staggered typography, scroll-triggered animation, image reveal, parallax terkontrol, interaksi hover/fokus | K-03 |
| FR-10.3 | M | Properti animasi mengutamakan `transform` dan `opacity`. Animasi properti yang memicu layout dihindari, kecuali ada alasan tercatat | K-03, NFR-01 |
| FR-10.4 | M | Durasi, easing, dan stagger bersumber dari satu kumpulan token motion yang ditetapkan di Tahap 4. Tidak ada nilai ajaib yang tersebar | T-04 |
| FR-10.5 | M | Tidak ada scroll hijacking. Scroll native browser adalah baseline. Keyboard, anchor, sentuhan, dan tombol browser berfungsi normal | K-03 |
| FR-10.6 | M | Tidak ada animasi berkedip. Tidak ada konten bergerak otomatis lebih dari lima detik tanpa kontrol berhenti/jeda atau tanpa alasan tercatat | K-08 |
| FR-10.7 | M | Animasi dan pendengar dibersihkan saat berpindah rute, saat tab tidak terlihat, dan saat komponen dilepas. Tidak ada kebocoran yang menumpuk | NFR-06 |
| FR-10.8 | M | Bila animasi gagal dimulai atau JS bermasalah, konten tetap tampil dan dapat dibaca (peningkatan bertahap, bukan prasyarat) | K-04, NFR-06 |
| FR-10.9 | M | Teks yang dipecah untuk animasi tetap terbaca benar oleh pembaca layar dan dapat dicari serta disalin | EC-14 |
| FR-10.10 | M | Setiap efek di tingkat mobile yang terlalu berat disederhanakan atau diganti, dengan karakter visual inti dipertahankan (K-04, K-12) | K-12 |

### FR-11 Transisi halaman

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-11.1 | S | Perpindahan antara halaman utama dan case study memakai transisi terkoordinasi (keluar dan masuk) yang tidak menghalangi navigasi | K-03, DP-01 |
| FR-11.2 | S | Transisi dapat dibatalkan atau diganti tanpa merusak status halaman bila pengguna menavigasi cepat berulang kali | EC-13 |
| FR-11.3 | S | Pada reduced motion, transisi diganti perpindahan tenang tanpa gerak spasial | NFR-05 |
| FR-11.4 | S | Fokus keyboard dan pengumuman perubahan halaman untuk pembaca layar ditangani benar setelah transisi | K-08 |
| FR-11.5 | C | Shared-element transition antara kartu project dan halaman detail, hanya bila kelayakan terbukti dan lolos uji performa | K-03, V-06 |

Kelayakan teknis FR-11 diverifikasi di Tahap 3 (V-06). Bila tidak layak, dipilih bentuk yang lebih sederhana dan pemilik diberi tahu.

### FR-12 Konten terstruktur dan gerbang placeholder

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-12.1 | M | Semua konten (project, expertise, profil, kontak) disimpan sebagai data terstruktur lokal, terpisah dari komponen tampilan | K-04, A-03 |
| FR-12.2 | M | Struktur data tidak mengunci bahasa tunggal sehingga penambahan Bahasa Indonesia kelak tidak memerlukan perancangan ulang model | K-04 |
| FR-12.3 | M | Data project memiliki, sekurangnya: slug, judul, ringkasan, peran, status, teknologi, bagian narasi, media beserta teks alternatif, tautan opsional, varian visibilitas (A/B untuk project RSUD), catatan penggunaan AI, dan tanggal pembaruan | K-11 |
| FR-12.4 | M | Setiap klaim pada hasil atau dampak memiliki penanda verifikasi (terkonfirmasi atau tidak). Klaim tidak terkonfirmasi tidak dapat dirender | NFR-09 |
| FR-12.5 | M | Konten yang belum dikonfirmasi ditulis `[PLACEHOLDER: ...]`. **Pemeriksaan otomatis menggagalkan build rilis** bila penanda tersebut masih ada | R-19, brief bagian 17 |
| FR-12.6 | M | Pemeriksaan data (skema) menggagalkan build bila bidang wajib hilang atau bila ada tautan tanpa tujuan | T-04 |
| FR-12.7 | M | Pemilik dapat menambah atau mengubah project tanpa mengubah kode komponen (UF-11) | T-04 |

### FR-13 Penanganan error dan fallback

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-13.1 | M | Halaman 404 memuat navigasi dan tautan kembali ke beranda | FR-01.4 |
| FR-13.2 | M | Kegagalan memuat gambar atau media tidak merusak layout dan tidak menyembunyikan teks pendamping | EC-06 |
| FR-13.3 | M | Kegagalan skrip animasi tidak menyembunyikan konten (FR-10.8) | NFR-06 |
| FR-13.4 | M | Error runtime tidak menampilkan layar kosong. Tersedia fallback yang dapat dinavigasi | NFR-06 |

### FR-14 Metadata dan berbagi

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-14.1 | S | Setiap halaman memiliki judul dan deskripsi yang akurat dan unik | AS-06 |
| FR-14.2 | S | Tautan yang dibagikan menampilkan pratinjau yang benar (judul, deskripsi, gambar). Gambar pratinjau tidak memuat data sensitif | AS-06, NFR-08 |
| FR-14.3 | S | Tersedia favicon dan berkas pendukung mesin pencari dasar | AS-06 |

### FR-15 Eksperimen interaktif (opsional)

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| FR-15.1 | C | Maksimal tiga eksperimen. Tiap eksperimen harus menunjukkan kemampuan yang **tidak** terwakili pengalaman utama, dan alasannya dicatat tertulis sebelum dibangun | K-02, DP-02 |
| FR-15.2 | C | Eksperimen mematuhi seluruh NFR (performa, accessibility, reduced motion) dan tidak membebani pemuatan halaman utama | NFR-01, NFR-02 |
| FR-15.3 | C | Eksperimen dapat dilepas tanpa memengaruhi bagian lain | R-12 |

---

## 7. Kebutuhan nonfungsional

Angka bertanda [SEMENTARA] adalah batas atas yang tidak boleh dilanggar. Target final bisa lebih ketat dan ditetapkan setelah prototipe diukur (A-05 dan A-07, disetujui pemilik pada DP-13).

### NFR-01 Performa

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-01.1 | M | Batas atas sementara (ambang "good" Core Web Vitals, diverifikasi di brief bagian 11.2): LCP ≤ 2,5 detik, CLS ≤ 0,1, dan responsivitas interaksi (INP) ≤ 200 ms. Diukur pada perangkat uji dan kondisi yang didokumentasikan | K-04, A-05 |
| NFR-01.2 | M | Metrik dilaporkan **per metrik**, bukan hanya skor komposit Lighthouse | Brief 11.2 |
| NFR-01.3 | M | INP adalah metrik lapangan. Bila tidak dapat diukur langsung, proxy lab dicatat **sebagai proxy**, bukan sebagai INP | V-07 |
| NFR-01.4 | M | Elemen LCP tidak bergantung pada animasi atau JS untuk tampil (FR-03.4) | FR-03.4 |
| NFR-01.5 | M | Gambar, font, dan media memiliki strategi pemuatan yang mencegah pergeseran layout. Font memiliki fallback yang jelas | FR-04.6 |
| NFR-01.6 | M | Pekerjaan JavaScript panjang dan handler scroll/hover yang berat dihindari. Animasi memakai properti yang murah | FR-10.3 |
| NFR-01.7 | M | Anggaran ukuran (KB JS, bobot gambar, jumlah font) **ditetapkan [SEMENTARA] di Tahap 3** setelah stack final dan prototipe terukur | A-05 |
| NFR-01.8 | M | Hasil performa ditulis dengan kondisi uji (perangkat, versi Chrome, throttling, jaringan, cache, jumlah ulangan) dan disebut sebagai hasil uji lab | R-14 |

### NFR-02 Accessibility (target WCAG 2.2 AA, bukan klaim kepatuhan)

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-02.1 | M | Semua fungsi dapat dioperasikan dengan keyboard saja, tanpa jebakan fokus (WCAG 2.1.1, 2.1.2) | K-08 |
| NFR-02.2 | M | Indikator fokus selalu terlihat jelas dan tidak tertutup elemen lain, termasuk header tetap (WCAG 2.4.7, 2.4.11) | K-08 |
| NFR-02.3 | M | Urutan fokus sesuai urutan visual dan logis (WCAG 2.4.3) | K-08 |
| NFR-02.4 | M | Kontras teks minimal 4,5:1 (teks besar 3:1) dan elemen antarmuka serta indikator fokus minimal 3:1 (WCAG 1.4.3, 1.4.11), termasuk di atas gambar dan saat animasi | K-08 |
| NFR-02.5 | M | Struktur dokumen benar: satu H1 per halaman, urutan heading logis, landmark (header, nav, main, footer), judul halaman bermakna (WCAG 1.3.1, 2.4.2, 2.4.6) | K-08 |
| NFR-02.6 | M | Semua gambar bermakna memiliki teks alternatif, gambar dekoratif dikecualikan dari pohon accessibility (WCAG 1.1.1) | FR-05.4 |
| NFR-02.7 | M | Tautan dan kontrol memiliki nama yang jelas dari konteksnya (WCAG 2.4.4, 4.1.2) | K-08 |
| NFR-02.8 | M | Target sentuh minimal 24 × 24 piksel CSS (WCAG 2.5.8, minimum). Pada smartphone, target yang sering dipakai dirancang lebih besar daripada minimum | K-04, K-08 |
| NFR-02.9 | M | Konten dapat dibaca tanpa scroll horizontal pada lebar 320 piksel CSS dan saat teks diperbesar 200% (WCAG 1.4.10, 1.4.4) | K-04 |
| NFR-02.10 | M | Tidak ada informasi penting yang hanya tersedia lewat hover, warna, atau animasi (WCAG 1.4.1) | K-08 |
| NFR-02.11 | M | Tidak ada konten yang berkedip lebih dari tiga kali per detik (WCAG 2.3.1) | FR-10.6 |
| NFR-02.12 | M | Perubahan konten dinamis dan perpindahan halaman diumumkan atau ditangani fokusnya dengan benar | FR-11.4 |
| NFR-02.13 | M | Rujukan nomor kriteria WCAG di atas **diverifikasi ulang terhadap teks resmi W3C di Tahap 6** | V-10 |

### NFR-03 Responsif dan perangkat

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-03.1 | M | Layout dirancang untuk tiga kelas lebar: kecil, menengah, besar. Batas pastinya [SEMENTARA] di Tahap 4 (AS-05) | K-04 |
| NFR-03.2 | M | Tidak ada horizontal overflow yang tidak disengaja pada kelas lebar mana pun. Konten lebar (tabel, kode) bergulir di dalam kontainernya sendiri | K-04 |
| NFR-03.3 | M | Tipografi, spacing, ukuran gambar, navigasi, dan animasi dirancang khusus untuk layar kecil, bukan sekadar mengecilkan versi desktop | K-04 |
| NFR-03.4 | M | Efek khusus pointer (hover, kursor kustom) dinonaktifkan pada perangkat sentuh bila tidak relevan, dan tidak menjadi satu-satunya akses | K-03 |
| NFR-03.5 | M | Pengalaman diuji pada POCO M4 Pro (Chrome Android) dan laptop pengembangan (Chrome desktop) sebagai perangkat uji nyata | K-05 |
| NFR-03.6 | M | Orientasi dan perubahan ukuran viewport tidak merusak layout atau animasi (EC-09) | EC-09 |

### NFR-04 Browser

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-04.1 | M | Chrome desktop dan Chrome Android adalah target pengujian utama | K-04 |
| NFR-04.2 | M | Konten, navigasi, dan layout inti harus berfungsi di Safari dan Firefox sejauh didukung implementasi. Fitur modern dipakai dengan fallback | K-10, A-04 |
| NFR-04.3 | S | Pengujian tambahan di Firefox dan Safari dilakukan bila memungkinkan, dicatat dengan kategori bukti. Tanpa bukti, ditulis "belum terverifikasi" | K-10, V-09 |

### NFR-05 Kenyamanan motion

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-05.1 | M | Preferensi `prefers-reduced-motion` dihormati: gerakan non-esensial dikurangi atau dihilangkan, tanpa kehilangan konten atau fungsi | K-03, K-08 |
| NFR-05.2 | M | Perubahan preferensi saat sesi berjalan ditangani wajar | EC-04 |
| NFR-05.3 | M | Tidak ada paksaan animasi pada pengguna yang memilih mengurangi gerak | K-04 |
| NFR-05.4 | M | Tidak ada efek yang menimbulkan ketidaknyamanan nyata (berkedip, gerak berlebihan, zoom cepat tanpa kendali) | K-03 |

### NFR-06 Ketahanan

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-06.1 | M | Konten dan navigasi inti tersedia saat JS lambat, gagal, atau animasi tidak berjalan, sejauh mungkin secara teknis | K-04, K-08 |
| NFR-06.2 | M | Kegagalan satu efek tidak menjatuhkan halaman | FR-13 |
| NFR-06.3 | M | Pemuatan aset yang lambat tidak menghasilkan area kosong yang membingungkan atau konten yang melompat | EC-06 |

### NFR-07 Keterpeliharaan

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-07.1 | M | Struktur kode jelas dan dapat dipelihara sendiri oleh pemilik. Jumlah dependency dibatasi dan setiap dependency memiliki alasan tercatat | T-04, K-09 |
| NFR-07.2 | M | Versi dependency dipin dan diperiksa lisensinya sebelum dipakai (V-05) | R-11 |
| NFR-07.3 | M | Tersedia dokumentasi singkat: cara menjalankan, menguji, menambah project, dan merilis | UF-11 |
| NFR-07.4 | M | Pengembangan dan pengujian dapat dilakukan penuh secara lokal di Windows 10 | K-06 |

### NFR-08 Keamanan dan privasi

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-08.1 | M | Tidak ada kredensial, kunci, konfigurasi lokal, atau data pribadi dalam kode, aset, riwayat repositori, atau berkas yang dipublikasikan | R-03 |
| NFR-08.2 | M | Metadata gambar (mis. EXIF) dibersihkan dan nama berkas serta teks alternatif tidak memuat data internal | R-03 |
| NFR-08.3 | M | Tidak ada pemanggilan pihak ketiga saat runtime (skrip, font, embed, pelacak) tanpa keputusan tercatat. Termasuk implikasi privasi dari font eksternal, dievaluasi di Tahap 3 | K-09, DP-08 |
| NFR-08.4 | M | Situs tidak membuat akun, tidak mengumpulkan data pengunjung, dan tidak menyetel cookie pelacakan di MVP (kecuali DP-08 diputuskan lain) | A-08 |
| NFR-08.5 | M | Repositori dibuka untuk publik **hanya** setelah audit data sensitif (DP-04) | K-07 |
| NFR-08.6 | S | Header keamanan dan kebijakan konten disesuaikan dengan hosting yang dipilih (Tahap 3 dan menjelang rilis) | Q-19 |

### NFR-09 Integritas konten dan klaim

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-09.1 | M | Tidak ada metrik, angka dampak, jumlah pengguna, atau penghematan waktu yang belum dikonfirmasi pemilik | Brief bagian 12 |
| NFR-09.2 | M | Tidak ada klaim "cepat", "accessible", atau "compliant" tanpa hasil uji tercatat yang menyebut kategori bukti | K-05, K-08 |
| NFR-09.3 | M | Kata yang tidak dapat dibuktikan dihindari: "senior", "expert", "award-winning", "passionate", klaim tahun pengalaman | Brief bagian 12 |
| NFR-09.4 | M | Nama RSUD, screenshot asli, dan detail sistem tidak dipublikasikan sebelum izin terkonfirmasi (Q-01). Alternatifnya adalah varian B (FR-05.5) | R-01 |
| NFR-09.5 | M | Penggunaan AI dijelaskan jujur dan proporsional (K-11) | K-11 |
| NFR-09.6 | M | Situs tidak dirilis selama gerbang rilis di brief bagian 17 belum terpenuhi | Brief bagian 17 |

### NFR-10 Evidensi dan testabilitas

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-10.1 | M | Setiap acceptance criteria dapat diuji dengan prosedur yang dapat diulang oleh pemilik | AC |
| NFR-10.2 | M | Hasil uji dicatat dengan kategori bukti A, B, atau C. Tidak ada pernyataan "lulus" tanpa bukti | K-05 |
| NFR-10.3 | M | Pemeriksaan otomatis (lint, tipe, build, accessibility otomatis, skema konten) dipisahkan dari pengujian manual dan dari audit AI | Tahap 6 dan 9 |
| NFR-10.4 | M | Temuan yang belum selesai didokumentasikan, tidak disembunyikan | K-08 |

### NFR-11 Kapasitas dan proses

| ID | P | Kebutuhan | Rujukan |
|---|---|---|---|
| NFR-11.1 | M | Pekerjaan dipecah menjadi fase kecil yang dapat dihentikan dengan aman dan berdiri sendiri | K-06 |
| NFR-11.2 | M | Setiap fase memiliki acceptance criteria dan laporan yang ditinjau pemilik sebelum fase berikutnya | K-06 |
| NFR-11.3 | M | Bila estimasi melebihi kapasitas, scope dipotong menurut bagian 4.4, bukan jam kerja ditambah | K-06 |

---

## 8. Alur pengguna

### UF-01 Kunjungan pertama oleh studio kreatif (P1, desktop)
1. Membuka beranda. Hero tampil dengan nama, peran, status, dan headline. Animasi masuk berjalan tanpa menahan konten.
2. Menggulir. Selected Projects muncul dengan interaksi hover dan image reveal.
3. Membuka salah satu project. Transisi halaman terjadi (bila FR-11 masuk).
4. Membaca case study sampai bagian hasil dan status.
5. Menuju bagian How I Built This (bila ada) atau kembali ke daftar project.
6. Menuju Contact dan menghubungi pemilik lewat email atau profil.

**Keberhasilan:** pengunjung dapat menjawab tiga hal tanpa bantuan: apa peran pemilik, project apa yang nyata, dan bagaimana menghubunginya.

### UF-02 Pemindaian cepat oleh recruiter (P2)
1. Membuka beranda dari tautan eksternal.
2. Dalam viewport pertama menemukan nama, peran, status, dan akses ke Projects dan Contact (FR-03.5).
3. Membuka satu case study langsung dari daftar, memindai bagian ikhtisar, teknologi, peran, dan status.
4. Membuka GitHub atau email.

### UF-03 Kunjungan di Chrome Android (POCO M4 Pro)
1. Membuka beranda. Layout mobile dengan tipografi dan komposisi khusus layar kecil.
2. Memakai menu mobile untuk menuju project. Target sentuh nyaman.
3. Menggulir tanpa tersendat. Efek berat telah disederhanakan di tingkat mobile (FR-10.10).
4. Membuka case study dan kembali memakai tombol back.

### UF-04 Membaca case study lalu berpindah
1. Dari daftar project membuka case study.
2. Menggulir bagian demi bagian, termasuk media dengan teks alternatif.
3. Di akhir memilih project berikutnya atau kembali ke daftar.

### UF-05 Pengguna keyboard saja
1. Tab pertama memfokuskan "skip to main content".
2. Tab melewati navigasi, hero, daftar project, dan seterusnya dengan fokus selalu terlihat.
3. Enter membuka project. Setelah transisi, fokus berada di tempat yang logis.
4. Menu mobile dapat dibuka dan ditutup dengan keyboard (Escape menutup).

### UF-06 Pengguna reduced motion
1. Sistem operasi atau browser diatur mengurangi gerak.
2. Hero, daftar project, dan transisi tampil dengan perubahan tenang tanpa gerak spasial besar.
3. Semua konten dan fungsi sama dengan tingkat lain.

### UF-07 Pengguna pembaca layar (Narrator di Windows 10, TalkBack di Android)
1. Mendengar judul halaman, landmark, dan struktur heading yang logis.
2. Teks hero yang dipecah untuk animasi tetap terbaca utuh (FR-10.9).
3. Menavigasi daftar project dan membuka case study. Mendengar teks alternatif media.
4. Perubahan halaman diumumkan atau fokus ditangani dengan benar.

### UF-08 Tautan langsung ke case study
1. Pengunjung membuka URL case study dari pesan atau profil.
2. Halaman dimuat penuh, dapat dibaca, dan memiliki navigasi ke beranda tanpa harus melewati hero.
3. Pratinjau tautan menampilkan judul dan deskripsi yang benar (FR-14).

### UF-09 Jaringan lambat atau animasi tidak berjalan
1. Teks dan navigasi tampil lebih dulu. Gambar mengikuti tanpa menggeser layout.
2. Bila skrip animasi gagal, konten tetap terlihat dan dapat dibaca.

### UF-10 Menghubungi pemilik
1. Menuju Contact atau footer.
2. Memilih email (membuka aplikasi surel melalui `mailto`), GitHub, atau Instagram.
3. Bila tidak ada aplikasi surel, alamat tetap dapat dibaca dan disalin (EC-17).

### UF-11 Pemilik memperbarui konten
1. Menambah atau mengubah data project dan menaruh aset gambar.
2. Menjalankan pemeriksaan data: skema, placeholder, tautan.
3. Menjalankan build dan uji. Build gagal bila ada `[PLACEHOLDER:` atau data tidak valid.
4. Merilis ketika gerbang rilis terpenuhi.

---

## 9. Edge case

| ID | Situasi | Perilaku yang diharapkan | Rujukan |
|---|---|---|---|
| EC-01 | JavaScript dimatikan atau gagal | Konten teks dan navigasi anchor tetap tersedia. Halaman tidak kosong | NFR-06.1 |
| EC-02 | Pustaka animasi gagal dimuat | Konten tampil tanpa animasi | FR-10.8 |
| EC-03 | Pengguna memakai reduced motion | Tingkat reduced motion aktif sepenuhnya | NFR-05.1 |
| EC-04 | Preferensi reduced motion diubah saat sesi berjalan | Perilaku menyesuaikan tanpa memuat ulang bila memungkinkan, atau berlaku pada navigasi berikutnya | NFR-05.2 |
| EC-05 | Layar sangat kecil (320 piksel CSS) | Tidak ada overflow horizontal. Semua kontrol tetap terjangkau | NFR-02.9 |
| EC-06 | Gambar gagal dimuat atau lambat | Tidak ada pergeseran layout. Teks dan alternatif tetap tampil | FR-13.2 |
| EC-07 | Font gagal dimuat | Font fallback dipakai. Layout tidak rusak | NFR-01.5 |
| EC-08 | Layar sangat lebar | Konten tidak meregang tak terkendali. Komposisi tetap terarah | NFR-03.1 |
| EC-09 | Rotasi layar atau perubahan ukuran | Layout dan animasi menyesuaikan tanpa rusak | NFR-03.6 |
| EC-10 | Anchor atau fokus keyboard menuju elemen di bawah header tetap | Elemen tidak tertutup header | FR-02.4, NFR-02.2 |
| EC-11 | Perangkat sentuh tanpa hover | Informasi dan interaksi tersedia lewat sentuhan atau tampil permanen | FR-04.4 |
| EC-12 | Tombol back/forward saat transisi sedang berjalan | Status halaman konsisten dan tidak macet | FR-01.3 |
| EC-13 | Navigasi cepat berulang (klik beruntun) | Transisi sebelumnya dibatalkan atau digantikan tanpa tumpang tindih | FR-11.2 |
| EC-14 | Teks hero dipecah per huruf atau kata | Pembaca layar membaca utuh. Cari-di-halaman dan salin tetap berfungsi | FR-10.9 |
| EC-15 | Pengguna memperbesar teks atau zoom 200% | Tidak ada konten terpotong atau tumpang tindih | NFR-02.9 |
| EC-16 | Mode kontras tinggi atau warna dipaksa | Konten tetap terbaca dan fokus terlihat | NFR-02.4 |
| EC-17 | Perangkat tanpa aplikasi surel | Alamat email tetap terlihat dan dapat disalin | FR-09.4 |
| EC-18 | Tab tidak aktif atau halaman di latar | Animasi berat dijeda dan tidak menumpuk pekerjaan | FR-10.7 |
| EC-19 | Rute tidak dikenal atau slug project salah | Halaman 404 yang dapat dinavigasi | FR-13.1 |
| EC-20 | Data opsional kosong (tanpa foto, tanpa demo, tanpa repo) | Komponen terkait tidak tampil, tanpa tautan mati atau ruang kosong aneh | FR-05.7, FR-06.2 |
| EC-21 | Judul atau teks sangat panjang | Teks membungkus tanpa merusak komposisi | NFR-03.2 |
| EC-22 | Teks dalam bahasa lain di masa depan lebih panjang | Layout tahan terhadap panjang teks yang berbeda | FR-12.2 |
| EC-23 | `[PLACEHOLDER: ...]` tersisa di konten | Build rilis gagal | FR-12.5 |
| EC-24 | Gambar RSUD memuat data sensitif di metadata, teks, atau visual | Tidak dipublikasikan. Dibersihkan atau diganti data dummy | NFR-08.2, R-03 |
| EC-25 | Pengguna berpindah antar tingkat motion (desktop ke mobile) lewat ubah ukuran | Tingkat motion menyesuaikan tanpa merusak halaman | FR-10.1 |
| EC-26 | Mode hemat daya atau perangkat lambat | Efek berat tidak membuat halaman tersendat. Penyederhanaan diterapkan sesuai K-12 | FR-10.10 |

---

## 10. Kebutuhan konten

### 10.1 Inventaris dan status kesiapan

| Konten | Sumber | Status | Penghalang |
|---|---|---|---|
| Case study Hospital and APAR Fire Inspection System | Pemilik | **Belum siap dirilis** | Izin publikasi (Q-01 sampai Q-03), inventaris data aman, varian A atau B (DP-03), peran AI (Q-20) |
| Case study Flash Sale Intelligence | Pemilik | Bahan ada, belum disusun | Keputusan pembekuan versi (Q-13), aset screenshot |
| Profil, About, minat | Pemilik | Belum ditulis | Q-05 (foto) |
| Headline | Pemilik | Opsi di brief Lampiran A | DP-07 |
| Kontak (GitHub, Instagram, email) | Pemilik | `[PLACEHOLDER: ...]` | Q-06 |
| CV berbahasa Inggris | Pemilik | Belum siap | Penulisan dan pemeriksaan oleh pemilik (DP-11) |
| Lokasi dan ketersediaan | Pemilik | Tidak ditampilkan | Q-07, Q-08 |
| Bagian How I Built This | Hasil implementasi dan uji | Belum ada | Bergantung hasil uji (A-09) |

### 10.2 Template case study (bagian wajib)

Ikhtisar dan konteks, masalah, solusi, teknologi, proses, peran dan penggunaan AI, hasil (hanya terkonfirmasi), status dan keterbatasan, serta media dengan teks alternatif.

### 10.3 Aturan klaim yang mengikat PRD

Mengikuti brief bagian 12. Ringkasnya: tanpa metrik tak terkonfirmasi, tanpa klaim performa atau accessibility tanpa bukti, tanpa kata yang tidak dapat dibuktikan, tanpa klaim bahwa seluruh kode ditulis manual, dan status project ditampilkan jujur.

---

## 11. Acceptance criteria

Setiap kriteria harus **dibuktikan dengan hasil uji yang tercatat**. PRD tidak menyatakan satu pun kriteria sudah lulus. Cara verifikasi dirinci di Tahap 6.

### 11.1 Fungsional

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-01 | Beranda dan setiap rute case study dapat dibuka langsung lewat URL, dimuat ulang, dan menampilkan konten yang benar | Uji manual dengan URL langsung dan muat ulang | A, B | FR-01, UF-08 |
| AC-02 | Tombol back/forward berfungsi wajar, termasuk setelah berpindah antar rute dan saat transisi aktif | Uji manual berurutan | A | FR-01, FR-11, EC-12 |
| AC-03 | Navigasi mengarah ke semua bagian dan halaman yang dituju. Anchor tidak tertutup header tetap | Uji manual di tiga kelas lebar | A, B | FR-02, EC-10 |
| AC-04 | Menu mobile dapat dibuka, ditutup (termasuk Escape), dan mengelola fokus dengan benar | Uji keyboard dan sentuhan | A | FR-02, UF-05 |
| AC-05 | Pada viewport pertama di lebar kecil acuan, nama, peran, status, dan akses ke navigasi terlihat tanpa menggulir | Pemeriksaan visual di POCO M4 Pro dan emulasi lebar acuan | A, B | FR-03, AS-05 |
| AC-06 | Elemen kandidat LCP hero muncul tanpa bergantung pada animasi atau JS (diuji dengan animasi dinonaktifkan) | Pemeriksaan DevTools dan uji dengan JS dimatikan | A, B | FR-03, NFR-01 |
| AC-07 | Daftar project dirender dari data. Menambah entri uji pada data memunculkan entri tanpa mengubah komponen | Uji menambah data dummy lalu menghapusnya | A | FR-04, FR-12 |
| AC-08 | Setiap interaksi hover memiliki padanan fokus keyboard dan sentuhan. Tidak ada informasi yang hanya muncul lewat hover | Uji keyboard dan perangkat sentuh | A | FR-04, NFR-02, EC-11 |
| AC-09 | Setiap case study memuat semua bagian wajib template (10.2). Bagian hasil hanya memuat butir terkonfirmasi atau pernyataan jujur bahwa belum ada metrik | Tinjauan konten terhadap checklist dan tabel klaim | A | FR-05, NFR-09 |
| AC-10 | Keterangan penggunaan AI ada di setiap case study dan sesuai K-11 | Tinjauan konten oleh pemilik | A | FR-05, FR-08, NFR-09 |
| AC-11 | Case study RSUD yang dirilis berada pada varian yang sesuai status izin. Varian A tidak ada di build rilis tanpa izin terkonfirmasi | Pemeriksaan dokumen izin dan tinjauan build | A | FR-05.5, NFR-09.4 |
| AC-12 | Bagian About bekerja dengan dan tanpa foto. Tidak ada lokasi atau ketersediaan kerja yang tampil tanpa konfirmasi | Uji dua konfigurasi data dan tinjauan konten | A | FR-06, FR-03.8, EC-20 |
| AC-13 | Technical Expertise tidak memuat persentase, bar, atau skor, dan setiap butir terhubung ke project nyata | Tinjauan konten dan data | A | FR-07 |
| AC-14 | Tautan eksternal (GitHub, Instagram, `mailto`) menuju tujuan yang dikonfirmasi, dibuka dengan atribut keamanan yang sesuai, dan tidak ada formulir kontak | Uji klik dan tinjauan | A | FR-09, UF-10, FR-09.3 |
| AC-15 | Halaman 404 tampil untuk rute tidak dikenal dan dapat dinavigasi | Uji manual | A, B | FR-13, FR-01, EC-19 |
| AC-16 | Build rilis gagal bila ada `[PLACEHOLDER:` atau data tidak valid | Uji disengaja dengan menyisipkan placeholder uji | A | FR-12, EC-23 |
| AC-17 | Judul, deskripsi, dan pratinjau tautan benar untuk setiap rute (jika FR-14 masuk) | Tinjauan metadata dan uji pratinjau | B | FR-14 |

### 11.2 Motion dan kenyamanan

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-18 | Dengan reduced motion aktif, tidak ada gerak spasial besar. Konten dan fungsi identik | Uji manual dengan pengaturan sistem dan emulasi DevTools | A, B | NFR-05, FR-10, FR-03.7, UF-06, EC-03 |
| AC-19 | Konten tetap terbaca dan dapat dinavigasi bila animasi dinonaktifkan atau pustaka animasi diblokir | Uji dengan pemblokiran skrip dan JS dimatikan | A, B | FR-10, NFR-06, EC-01, EC-02, UF-09 |
| AC-20 | Tidak ada scroll hijacking: scroll native, keyboard, anchor, dan sentuhan berfungsi normal di semua bagian | Uji manual per input | A | FR-10.5 |
| AC-21 | Tidak ada animasi berkedip lebih dari tiga kali per detik dan tidak ada konten bergerak otomatis lebih dari lima detik tanpa kontrol (atau alasan tercatat) | Tinjauan animasi dan uji manual | A, B | FR-10.6, NFR-02.11 |
| AC-22 | Teks yang dipecah untuk animasi terbaca utuh oleh pembaca layar, dapat dicari, dan dapat disalin | Uji Narrator, TalkBack, cari-di-halaman, salin | A | FR-10.9, EC-14 |
| AC-23 | Animasi utama memakai `transform` dan `opacity`. Pengecualian dicatat dengan alasan | Tinjauan kode dan profil performa | A, B | FR-10.3 |
| AC-24 | Tiga tingkat motion aktif sesuai kondisi (desktop, mobile, reduced motion) dan perubahan ukuran memicu penyesuaian wajar | Uji manual per tingkat | A, B | FR-10.1, EC-25 |
| AC-25 | Efek yang terlalu berat di POCO M4 Pro telah disederhanakan atau diganti menurut urutan K-12, dengan karakter visual inti tetap ada | Uji di POCO M4 Pro dan tinjauan visual pemilik | A | FR-10.10, K-12, EC-26 |
| AC-26 | Transisi halaman tidak menghalangi navigasi, dapat dibatalkan saat navigasi cepat, dan fokus dikelola setelahnya (jika FR-11 masuk) | Uji manual navigasi cepat dan keyboard | A | FR-11, EC-13 |

### 11.3 Performa

Angka di bawah adalah **batas atas sementara** (NFR-01.1). Target final ditetapkan setelah prototipe diukur (A-05, disetujui pemilik pada DP-13) dan tidak boleh lebih longgar daripada batas ini.

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-27 | LCP ≤ 2,5 detik pada beranda dan case study, di POCO M4 Pro dan laptop uji, dengan kondisi uji tercatat (perangkat, versi Chrome, throttling, jaringan, cache, jumlah ulangan) | Pengukuran lab terdokumentasi | A (B sebagai pelengkap) | NFR-01 |
| AC-28 | CLS ≤ 0,1 pada beranda dan case study, termasuk saat animasi masuk dan pemuatan gambar dan font | Pengukuran lab terdokumentasi | A, B | NFR-01, EC-06, EC-07 |
| AC-29 | Responsivitas interaksi (target INP ≤ 200 ms) dinilai dengan interaksi nyata (menu, hover, navigasi) di POCO M4 Pro. Jika memakai proxy lab, dicatat sebagai proxy | Pengukuran terdokumentasi dan metode dari V-07 | A, B | NFR-01.3 |
| AC-30 | Scroll di POCO M4 Pro terasa lancar pada rute utama, dinilai pemilik dan didukung profil performa | Uji manual dan rekaman profil | A | NFR-01, NFR-03, UF-03 |
| AC-31 | Anggaran ukuran yang ditetapkan di Tahap 3 tidak terlampaui pada build rilis | Pengukuran build | A, B | NFR-01.7 |
| AC-32 | Laporan performa memuat hasil per metrik dan menyebutnya sebagai hasil uji lab | Tinjauan laporan | A | NFR-01.2, NFR-01.8 |

### 11.4 Accessibility

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-33 | Seluruh fungsi dapat dioperasikan hanya dengan keyboard (termasuk tautan skip to main content), fokus selalu terlihat dan tidak tertutup, urutan fokus logis | Uji keyboard terdokumentasi pada semua rute | A | NFR-02.1, NFR-02.2, NFR-02.3, UF-05, FR-02.3 |
| AC-34 | Kontras teks, elemen antarmuka, dan indikator fokus memenuhi rasio yang ditetapkan, termasuk di atas gambar | Pemeriksaan alat dan sampel manual | A, B | NFR-02.4, EC-16 |
| AC-35 | Struktur heading, landmark, judul halaman, bahasa dokumen, nama kontrol, dan teks alternatif benar | Alat otomatis ditambah tinjauan manual | A, B | NFR-02.5, NFR-02.6, NFR-02.7, FR-01.5, FR-05.9 |
| AC-36 | Target sentuh memenuhi minimum 24 × 24 piksel CSS dan nyaman di POCO M4 Pro | Pengukuran dan uji sentuh | A, B | NFR-02.8 |
| AC-37 | Tanpa scroll horizontal pada lebar 320 piksel CSS dan dengan teks diperbesar 200% | Uji emulasi dan uji nyata | A, B | NFR-02.9, EC-05, EC-15 |
| AC-38 | Alur utama dapat dilalui dengan Narrator (Windows 10) dan TalkBack (Android), temuan dicatat termasuk yang belum selesai | Skrip uji manual bertahap | A | UF-07, NFR-02, NFR-10.4 |
| AC-39 | Tidak ada informasi penting yang hanya lewat hover, warna, atau animasi | Tinjauan manual per komponen | A | NFR-02.10 |
| AC-40 | Pernyataan accessibility di situs (jika ada) hanya memuat hasil uji nyata dan batasannya, tanpa klaim kepatuhan penuh | Tinjauan konten terhadap catatan uji | A | FR-08.6, NFR-09.2 |

### 11.5 Perangkat, browser, dan responsif

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-41 | Seluruh alur utama berjalan di Chrome desktop (laptop uji) dan Chrome Android (POCO M4 Pro) | Uji manual terdokumentasi | A | NFR-03.5, NFR-04.1 |
| AC-42 | Tiga kelas lebar (kecil, menengah, besar) tidak memiliki overflow horizontal yang tidak disengaja dan layout tetap rapi, termasuk dengan teks uji yang lebih panjang dari aslinya | Uji emulasi dan perangkat nyata | A, B | NFR-03, EC-08, EC-21, EC-22 |
| AC-43 | Konten, navigasi, dan layout inti diperiksa di Firefox dan Safari bila memungkinkan, dengan kategori bukti dicatat. Yang tidak diuji ditulis "belum terverifikasi" | Uji tambahan sesuai V-09 dan laporan | A atau C | NFR-04.2, NFR-04.3 |
| AC-44 | Laporan uji membedakan jelas hasil nyata (A), simulasi (B), dan belum terverifikasi (C) | Tinjauan laporan | A | NFR-10.2 |

### 11.6 Integritas, keamanan, dan rilis

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-45 | Pencarian pada kode dan konten tidak menemukan kredensial, kunci, data pribadi, atau konfigurasi lokal pada aset publik atau repositori yang dibuka | Pemindaian terdokumentasi dan tinjauan manual | A | NFR-08.1, NFR-08.5 |
| AC-46 | Metadata gambar dibersihkan dan nama berkas serta teks alternatif tidak memuat data internal | Pemeriksaan berkas aset | A | NFR-08.2, EC-24 |
| AC-47 | Tidak ada permintaan jaringan pihak ketiga saat runtime selain yang diputuskan tercatat | Inspeksi tab jaringan | A, B | NFR-08.3, NFR-08.4 |
| AC-48 | Semua klaim performa dan accessibility di situs memiliki hasil uji tercatat dengan kategori bukti | Tinjauan silang konten dengan laporan uji | A | NFR-09.2 |
| AC-49 | Tidak ada klaim terlarang (metrik tak terkonfirmasi, "senior", "expert", klaim kode manual penuh) | Tinjauan konten oleh pemilik | A | NFR-09 |
| AC-50 | Gerbang rilis di brief bagian 17 terpenuhi seluruhnya sebelum publikasi | Checklist rilis yang ditandatangani pemilik | A | NFR-09.6 |
| AC-51 | Repositori dapat dijalankan, diuji, dan dibuild secara lokal di Windows 10 mengikuti dokumentasi, dependency terpin dengan lisensi tercatat, dan menambah project sesuai UF-11 | Uji mengikuti dokumentasi dari awal | A | NFR-07, NFR-11, UF-11, NFR-07.2 |
| AC-52 | Setiap fase implementasi dilaporkan dengan acceptance criteria dan bukti, dan ditinjau pemilik sebelum fase berikutnya | Tinjauan laporan fase | A | NFR-11 |
| AC-53 | Bila uji persepsi disetujui (DP-10), hasilnya dicatat dan dibandingkan dengan kesimpulan di brief bagian 4. Bila tidak, T-01 dinilai lewat tinjauan terhadap UF-01 dan UF-02 | Sesuai keputusan Q-16 | A | T-01, UF-01, UF-02 |
| AC-54 | Setiap eksperimen yang dibangun memiliki alasan tertulis sebelum dibangun, lolos kriteria performa dan accessibility yang relevan, tidak menambah beban pemuatan beranda, dan dapat dilepas tanpa memengaruhi bagian lain (jika FR-15 masuk) | Tinjauan catatan alasan, pengukuran dengan dan tanpa eksperimen, uji pelepasan | A, B | FR-15, NFR-01, NFR-02, R-12 |
| AC-55 | Animasi dan pendengar dibersihkan saat berpindah rute dan dijeda saat tab tidak aktif. Navigasi berulang tidak menumpuk pekerjaan atau menurunkan kelancaran | Profil performa dan uji navigasi berulang | A, B | FR-10.7, EC-18, EC-13, NFR-06 |

### 11.7 Tambahan revisi 0.2 (celah cakupan hasil audit)

Kriteria berikut menutup kebutuhan Must dan edge case yang sebelumnya tidak memiliki acceptance criteria sendiri.

| ID | Kriteria | Verifikasi | Bukti | Menguji |
|---|---|---|---|---|
| AC-56 | Durasi, easing, dan stagger animasi bersumber dari satu kumpulan token. Tinjauan kode tidak menemukan nilai durasi atau easing literal di komponen, kecuali pengecualian yang dicatat dengan alasan | Tinjauan kode dan pencarian terhadap nilai literal | A | FR-10.4 |
| AC-57 | Model data konten tidak mengunci satu bahasa: satu entri uji dapat diberi teks bahasa kedua (dummy) tanpa mengubah skema atau komponen, dan layout tetap rapi dengan teks yang lebih panjang | Uji menambah data dummy lalu menghapusnya | A, B | FR-12.2, EC-22 |
| AC-58 | Navigasi dapat digunakan saat animasi masuk masih berjalan, dan tidak ada layar loading penuh yang memblokir konten atau navigasi, termasuk dengan CPU diperlambat | Uji manual di POCO M4 Pro dan emulasi throttling DevTools | A, B | FR-02.6, FR-03.6 |
| AC-59 | Perubahan preferensi reduced motion saat sesi berjalan, rotasi layar, dan perubahan ukuran viewport tidak merusak layout atau animasi. Perilaku sesuai EC-04 | Uji manual dengan mengubah pengaturan, memutar layar, dan mengubah ukuran jendela | A, B | NFR-05.2, NFR-03.6, EC-04, EC-09 |
| AC-60 | Gambar yang diblokir tidak menggeser layout dan tidak menyembunyikan teks pendamping. Error runtime yang sengaja dipicu pada uji menampilkan fallback yang dapat dinavigasi, bukan layar kosong | Uji dengan memblokir gambar dan memicu error uji | A, B | FR-13.2, FR-13.3, FR-13.4, NFR-06.2 |
| AC-61 | Data klaim hasil atau dampak yang belum terkonfirmasi tidak dirender oleh situs, dan data dengan penanda tidak terkonfirmasi diuji secara sengaja | Uji dengan entri uji bertanda tidak terkonfirmasi | A | FR-12.4, FR-05.2, NFR-09.1 |
| AC-62 | Alamat email terlihat dan dapat disalin pada perangkat tanpa aplikasi surel, dan dapat dijangkau keyboard serta pembaca layar | Uji dengan tautan `mailto` tanpa aplikasi surel terpasang atau diblokir, dan uji keyboard | A | FR-09.4, EC-17 |
| AC-63 | Tautan CV tampil hanya bila berkas CV berbahasa Inggris ada dan ditandai sudah diperiksa pemilik. Bila tidak, tidak ada tautan mati maupun berkas CV di build. CV yang dirilis bebas metadata dan data sensitif | Uji dua konfigurasi data (CV siap dan belum siap) dan pemeriksaan berkas PDF | A | FR-09.6, FR-09.7, EC-20, NFR-08.2 |
| AC-64 | Bagian How I Built This (bila masuk) berada di halaman utama sebagai bagian ringkas, dan tidak ada rute terpisah untuknya pada build | Tinjauan struktur rute hasil build | A | FR-08.8, DP-12 |

---

## 12. Keterlacakan keputusan brief

| Keputusan | Diwujudkan oleh |
|---|---|
| K-01 Audiens dan kesimpulan dua menit | FR-03, FR-04, FR-05, UF-01, UF-02, AC-05, AC-53 |
| K-02 Website sebagai bukti, bagian teknis, eksperimen | FR-05, FR-07, FR-08, FR-15, NFR-09 |
| K-03 Batas teknologi motion | FR-03, FR-10, FR-11, bagian 4.2, NFR-05 |
| K-04 Bahasa, browser, mobile, pengujian | FR-01.5, FR-12.2, NFR-03, NFR-04, AC-41, AC-42 |
| K-05 Perangkat dan kategori bukti | NFR-03.5, NFR-10.2, AC-44 |
| K-06 Waktu, kapasitas, hosting | Bagian 4.4, NFR-11, NFR-07.4, AC-52 |
| K-07 Identitas dan informasi pribadi | FR-03, FR-06, FR-09, FR-12.5 |
| K-08 Accessibility WCAG 2.2 AA (target) | NFR-02, FR-08.6, AC-33 sampai AC-40 |
| K-09 Batasan lingkup | Bagian 4.3, FR-09.2, NFR-08.3, NFR-08.4 |
| K-10 Safari dan Firefox | NFR-04, AC-43 |
| K-11 Transparansi AI | FR-05.3, FR-08.4, NFR-09.5, AC-10 |
| K-12 Aturan penengah | Bagian 3, FR-10.10, AC-25 |
| K-13 Situs sebagai bukti tanpa halaman case study | Bagian 4.3, FR-08 |

---

## 13. Titik keputusan tertunda

PRD **tidak** mengasumsikan jawabannya. Kolom "Perilaku PRD" menjelaskan bagaimana kebutuhan tetap valid apa pun jawabannya.

| ID | Pertanyaan | Perilaku PRD sampai diputuskan | Dibutuhkan paling lambat |
|---|---|---|---|
| DP-01 (Q-10) | Seberapa penting page transition dan shared-element transition? | Transisi dasar = Should (FR-11), shared-element = Could. Kelayakan diperiksa di Tahap 3 (V-06) | Tahap 3 |
| DP-02 (Q-11) | Apakah WebGL menjadi eksperimen opsional dan efek apa? | Tidak masuk MVP baseline. Eksperimen = Could dengan syarat FR-15 | Tahap 3 |
| DP-03 (Q-01 sampai Q-03) | Izin publikasi RSUD, data aman, repositori | Konten mendukung varian A (asli) dan B (teranonimkan dengan data dummy). Tidak ada varian A tanpa izin (AC-11) | Sebelum rilis. Bentuk konten di Tahap 4 |
| DP-04 (Q-04, Q-03) | Repositori situs dan project RSUD dibuka? | Tautan sumber bersifat opsional dan tidak tampil bila tidak ada (FR-05.7, FR-08.7) | Tahap 3 |
| DP-05 (Q-05) | Foto diri? | About bekerja tanpa foto (FR-06.2) | Tahap 4 |
| DP-06 (Q-13) | Pembekuan versi Flash Sale Intelligence | Konten memuat tanggal pembaruan dan status "dalam pengembangan" (FR-05.6) | Penyusunan konten |
| DP-07 (Q-17) | Pilihan headline | Teks headline adalah data konten. Opsi di brief Lampiran A | Tahap 4 |
| DP-08 (Q-18) | Analytics? | Tidak ada pelacakan di MVP (NFR-08.4) | Tahap 3 |
| DP-09 (Q-20) | Peran AI pada project RSUD | Kolom keterangan AI wajib ada. Isi untuk project RSUD menunggu jawaban (FR-05.3) | Penyusunan konten |
| DP-10 (Q-16) | Uji persepsi kecil? | T-01 dinilai lewat tinjauan UF-01 dan UF-02, ditambah uji persepsi bila disetujui (AC-53) | Tahap 6 |
| DP-11 (Q-22) | Apakah tautan CV/resume (PDF) disediakan? | **Diputuskan 12 Oktober 2026**: tautan CV disediakan, tetapi hanya ditampilkan setelah CV berbahasa Inggris siap dan sudah diperiksa pemilik. Tanpa fitur unduh yang rumit (tautan biasa ke PDF statis). Lihat FR-09.6, FR-09.7, AC-63 | Selesai |
| DP-12 (Q-23) | Apakah struktur di bagian 5 disetujui, dan apakah How I Built This berdiri di halaman utama atau halaman sendiri? | **Diputuskan 12 Oktober 2026**: struktur bagian 5 disetujui. How I Built This berupa bagian ringkas di halaman utama. Halaman terpisah tidak dibuat kecuali ada cukup materi untuk menjelaskan proses teknis secara mendalam. Lihat FR-08.8, AC-64 | Selesai |
| DP-13 (Q-21) | Apakah A-05, A-06, A-07 disetujui? | **Diputuskan 12 Oktober 2026**: A-05 dan A-07 disetujui (target performa final ditetapkan setelah prototipe diukur, hero diuji lebih awal di POCO M4 Pro), selama tidak melampaui kapasitas (K-06). A-06 **tidak** diwajibkan: pemusatan sistem motion tetap rekomendasi implementasi. Batas atas sementara NFR-01.1 tetap berlaku sampai target final ditetapkan | Selesai |

Pertanyaan brief lain yang bukan penentu bentuk PRD tetap berstatus terbuka dan dibutuhkan sebelum rilis: Q-06 (kontak), Q-07 (lokasi), Q-08 (ketersediaan), dan Q-19 (hosting dan domain).

---

## 14. Asumsi PRD

| ID | Asumsi | Dampak bila salah |
|---|---|---|
| AS-01 | Struktur informasi berupa satu halaman beranda dengan bagian berurutan ditambah satu rute per case study (bagian 5). **Disetujui pemilik (DP-12)** | Rute dan pengelompokan berubah. Kebutuhan tetap berlaku (DP-12) |
| AS-02 | Dua project yang ada cukup menjadi dasar dua case study di MVP, tanpa menambah project baru | Bila pemilik menambah project, FR-04.5 dan FR-12.7 sudah mengakomodasi |
| AS-03 | Varian B (teranonimkan, data dummy) adalah jalur aman bila izin RSUD belum terkonfirmasi | Bila varian B tidak cukup kuat sebagai bukti, bobot case study ini berkurang (R-01, R-06) |
| AS-04 | Sebagian pengunjung P1 memakai Safari atau macOS, sehingga baseline di luar Chrome penting walau tidak diuji penuh | Bila salah, prioritas NFR-04.2 dapat diturunkan |
| AS-05 | Lebar kecil acuan untuk viewport pertama adalah sekitar 360 piksel CSS, dan lebar uji ekstrem 320 piksel CSS. Batas tiga kelas lebar ditetapkan di Tahap 4 | Angka berubah. Verifikasi di POCO M4 Pro menentukan nilai acuan sebenarnya |
| AS-06 | Metadata dan berbagi (FR-14) berguna karena recruiter dan studio membuka situs dari tautan di profil dan CV. Ini tidak diminta eksplisit oleh pemilik | Bila tidak diinginkan, FR-14 dihapus tanpa dampak lain |
| AS-07 | "Ukuran relatif" S/M/L adalah perbandingan usaha berdasarkan jenis pekerjaan, bukan jam kerja | Estimasi Tahap 5 dikalibrasi ulang dari fase pertama (A-02) |
| AS-08 | Urutan pemotongan di bagian 4.4 mencerminkan prioritas pemilik | Pemilik dapat mengubah urutan sebelum persetujuan |
| AS-09 | Rujukan nomor kriteria WCAG 2.2 pada NFR-02 benar. Belum diverifikasi ulang terhadap teks resmi W3C | Diverifikasi di Tahap 6 (V-10). Bila ada yang keliru, NFR-02 diperbaiki |

## 15. Risiko tambahan dari PRD

| ID | Risiko | Mitigasi |
|---|---|---|
| R-20 | Jumlah kebutuhan Must terlalu besar untuk 6 sampai 10 jam per minggu | Urutan pemotongan (4.4), fase kecil, kalibrasi dari fase pertama |
| R-21 | Pemecahan teks untuk animasi merusak pembaca layar dan pencarian | FR-10.9 dan AC-22 diuji sejak prototipe hero |
| R-22 | Konten case study RSUD menjadi penghalang rilis (izin atau data) | Varian B sebagai jalur siap rilis (FR-05.5, AS-03) dan jangan menunda sisa situs |
| R-23 | Pemeriksaan kriteria accessibility oleh pemilik yang belum berpengalaman melewatkan masalah | Skrip uji bertahap (Tahap 6) dan temuan yang belum selesai dicatat jujur (NFR-10.4) |
| R-24 | Variasi tampilan dua project tidak cukup untuk galeri bergaya referensi | FR-04.5. Komposisi diputuskan berdasarkan konten nyata di Tahap 4 |

Risiko R-01 sampai R-19 tetap berlaku sesuai brief.

## 16. Item verifikasi tambahan

| ID | Yang perlu diverifikasi | Oleh |
|---|---|---|
| V-10 | Nomor dan isi kriteria WCAG 2.2 yang dirujuk di NFR-02 terhadap teks resmi W3C (Tahap 6) | Claude, dari sumber resmi |
| V-11 | Resolusi layar dan ukuran viewport CSS aktual laptop dan POCO M4 Pro untuk menetapkan acuan lebar | Pemilik |

---

## 17. Hal yang diserahkan ke Tahap 3

1. Stack final dan alasannya (kandidat di brief bagian 9), dengan verifikasi lisensi dan versi dependency (V-05).
2. Strategi rendering, perutean, dan konten terstruktur yang mendukung FR-01, FR-12, dan i18n masa depan.
3. Kelayakan transisi halaman dan shared-element (V-06, DP-01).
4. Keputusan tentang Lenis, video, WebGL, dan kursor: masuk hanya bila terbukti perlu (bagian 4.2).
5. Anggaran performa [SEMENTARA] dan metode ukur (NFR-01.7, V-07).
6. Strategi tiga tingkat motion (pemusatan sebagai rekomendasi implementasi, A-06) dan pembersihan animasi (FR-10.7).
7. Pendekatan font, gambar, dan pemuatan aset yang menjaga LCP dan CLS (NFR-01.5, NFR-08.3).
8. Opsi hosting dan batasannya (Q-19, V-08), serta header keamanan (NFR-08.6).
9. Keputusan terbuka yang dibutuhkan Tahap 3: DP-01, DP-02, DP-04, dan DP-08. DP-11, DP-12, dan DP-13 sudah diputuskan (revisi 0.3).

## 18. Riwayat perubahan

| Versi | Tanggal | Perubahan |
|---|---|---|
| 0.1 | 12 Oktober 2026 | Draf awal Tahap 2 berdasarkan PROJECT_BRIEF.md v0.2 |
| 0.2 | 12 Oktober 2026 | Audit kelengkapan: FR-10.1 dipisahkan antara keputusan K-03 (tiga tingkat) dan usulan A-06 (pemusatan); ditambah AC-56 sampai AC-62 (bagian 11.7) untuk FR-02.6, FR-03.6, FR-09.4, FR-10.4, FR-12.2, FR-12.4, FR-13.2 sampai FR-13.4, NFR-05.2, NFR-03.6, EC-04, EC-09, dan EC-17 |
| 0.3 | 12 Oktober 2026 | Mencatat jawaban pemilik: DP-13 (A-05 dan A-07 disetujui, A-06 hanya rekomendasi implementasi, FR-10.1 dan prinsip 6 disesuaikan), DP-11 (tautan CV bersyarat, FR-09.6, FR-09.7, AC-63), DP-12 (struktur disetujui, How I Built This ringkas di halaman utama, FR-08.8, AC-64) |
