# CONTENT_MODEL — Model konten, kontrak data, dan validasi

| Item | Nilai |
|---|---|
| Dokumen | `docs/planning/CONTENT_MODEL.md` |
| Versi | 0.1 (draf untuk review) |
| Tanggal | 13 Oktober 2026 |
| Tahap workflow | Tahap 4 (Desain Teknis) |
| Dokumen induk | `docs/planning/TECHNICAL_DESIGN.md` |
| Rujukan | ADR-0002, FR-04.1, FR-05, FR-07, FR-09.6, FR-12, NFR-08.2, NFR-09, AC-07, AC-09, AC-11, AC-13, AC-16, AC-57, AC-61, AC-63 |
| Status keputusan | D-02, D-03, D-05, D-09 adalah **T4-USULAN**. ADR-0002 tetap **Diusulkan** |

Bentuk tipe di bawah adalah **spesifikasi logis**. Implementasi menurunkan tipe dari skema validasi (D-03) sehingga tipe dan aturan tidak terpisah. Pustaka skema masih kandidat (V-19).

## 1. Prinsip

1. Konten hanya hidup di `content/`. Komponen tidak menulis teks produk (FR-04.1, FR-12.1).
2. Komponen menerima *view* berisi string biasa dari `src/lib/content`, bukan peta bahasa.
3. Skema bersifat **ketat**: bidang yang tidak dikenal ditolak. Itu cara struktural melarang bidang seperti `level`, `percent`, `location`, `availability` (FR-07.2, FR-06.3, FR-03.8) tanpa aturan tambahan.
4. Apa yang tidak boleh dirender dicegah di **tiga tempat**: skema/validator, fungsi akses, dan pemindaian `out/` (`CHK-OUT-*`).
5. Teks yang belum dikonfirmasi ditulis `[PLACEHOLDER: ...]`. Pemeriksaan placeholder berlaku pada seluruh nilai string (FR-12.5).

## 2. Pemetaan nama (C-05, D-02)

| ADR-0002 (ilustrasi) | Identifier kode |
|---|---|
| `terkonfirmasi` | `confirmed` |
| tanggal konfirmasi | `confirmedOn` |
| `siap` | `ready` |
| `diperiksaPemilik` | `ownerReviewed` |
| varian `anonim` | `anonymized` |
| varian `bernama` | `named` |
| catatan izin | `permission` |

## 3. Tipe dasar

```ts
type ISODate = string;                         // 'YYYY-MM-DD', divalidasi format dan tanggal nyata
type LocaleCode = string;                      // BCP 47 sederhana: /^[a-z]{2,3}(-[A-Za-z0-9]{2,8})*$/
type LocaleMap = { en: string } & Record<LocaleCode, string>;   // 'en' wajib, kunci lain opsional
const REQUIRED_LOCALES = ['en'] as const;      // SATU tempat. Menambah bahasa wajib = ubah di sini

// Narasi tanpa MDX (ADR-0002). Blok terbatas agar media selalu melewati teks alternatif.
type Block =
  | { type: 'p';      text: LocaleMap }
  | { type: 'list';   items: LocaleMap[] }
  | { type: 'figure'; mediaId: string; caption?: LocaleMap };
type Section = { heading: LocaleMap; blocks: Block[] };

type Evidence = {
  category: 'A' | 'B';                         // 'C' tidak boleh dipakai sebagai bukti klaim (K-05)
  reportRef: string;                           // /^TR-\d{4}-\d{2}-\d{2}-[a-z0-9-]+$/ (C-07)
  conditions: LocaleMap;                       // perangkat, versi Chrome, throttling, jaringan, ulangan (NFR-01.8)
};

type Claim = {
  id: string;                                  // unik global, kebab-case
  kind: 'result' | 'impact' | 'metric' | 'performance' | 'accessibility' | 'usage';
  text: LocaleMap;
  confirmed: boolean;
  confirmedOn?: ISODate;                       // wajib bila confirmed = true
  evidence?: Evidence;                         // wajib bila kind = performance | accessibility
};
```

Aturan `Locale`: `getX(locale)` memilih `map[locale]`, lalu `map.en`. Bahasa tanpa teks jatuh ke Inggris. Menambah bahasa Indonesia berarti menambah kunci `id` dan mengubah `REQUIRED_LOCALES` bila ingin diwajibkan, **bukan** mengubah skema atau komponen (AC-57).

## 4. Entitas

### 4.1 Site (`content/site.ts`)

```ts
type Site = {
  name: LocaleMap;                             // nama tampilan (nilai sementara: Lukmanul Hakim)
  defaultLocale: 'en';
  titleTemplate: LocaleMap;                    // mis. '%s | <name>'
  defaultDescription: LocaleMap;
  nav: { id: 'home'|'projects'|'about'|'expertise'|'contact'; label: LocaleMap }[];
};
```

### 4.2 Profile (`content/profile.ts`)

```ts
type Profile = {
  role: LocaleMap;                             // 'Creative Developer'
  statusStatement: LocaleMap;                  // pernyataan status jujur (mahasiswa S1 Teknik Informatika)
  headline: { text: LocaleMap; confirmed: boolean };   // DP-07. confirmed=false -> dev saja, rilis gagal
  about: Block[];                              // tanpa 'figure' kecuali foto ada
  interests: LocaleMap[];
  approach: LocaleMap[];                       // pendekatan membangun software
  photo?: Media;                               // opsional (DP-05). Kosong = tidak tampil, tanpa ruang kosong
};
```

Tidak ada bidang `location`, `availability`, `yearsOfExperience`. Skema ketat menolaknya.

### 4.3 Teknologi dan grup (`content/tech.ts`, `content/groups.ts`)

```ts
type Tech = { id: string; label: string; group: string };       // label nama diri, tidak diterjemahkan
type TechGroup = { id: string; label: LocaleMap; order: number };
```

Tidak ada bidang tingkat penguasaan.

### 4.4 Project (`content/projects/*.ts`)

```ts
type ProjectBody = {
  title: LocaleMap;
  summary: LocaleMap;                          // satu kalimat (FR-04.2)
  role: LocaleMap;
  status: {
    kind: 'in-development' | 'in-use' | 'completed';
    label: LocaleMap;                          // label jujur yang ditampilkan
  };
  technologies: { techId: string; how: LocaleMap }[];   // 'how' wajib: satu kalimat cara pakai (FR-07.1)
  sections: {                                  // semua wajib (template 10.2, FR-05.1)
    overview: Section;
    problem: Section;
    solution: Section;
    process: Section;
    roleAndLimits: Section;                    // peran pemilik + status dan keterbatasan
  };
  aiUsage: { text: LocaleMap; confirmed: boolean };     // wajib (FR-05.3). confirmed=false -> rilis gagal
  outcome: {
    items: Claim[];                            // hanya yang confirmed dirender
    noMetricsStatement?: LocaleMap;            // wajib bila tidak ada item confirmed (FR-05.2)
  };
  media: Media[];
  links: ProjectLink[];                        // opsional, default []
  seo: { title: LocaleMap; description: LocaleMap };
};

type Project = {
  slug: string;                                // /^[a-z0-9]+(-[a-z0-9]+)*$/, unik
  order: number;                               // bilangan bulat unik
  updatedOn: ISODate;                          // FR-05.6, FR-12.3
  /** Project biasa memakai 'single'. Project RSUD memakai 'variants'. */
  content:
    | { mode: 'single'; body: ProjectBody }
    | {
        mode: 'variants';
        active: 'anonymized' | 'named';
        anonymized: ProjectBody;               // selalu ada
        named?: ProjectBody;                   // TIDAK dibuat di repositori sebelum izin (D-09)
        permission?: Permission;               // wajib bila active = 'named'
      };
};

type ProjectLink = {
  kind: 'demo' | 'repository' | 'article';
  label: LocaleMap;
  href: string;                                // wajib, https saja
  confirmed: boolean;                          // false -> tidak dirender (FR-05.7)
};
```

`Permission` (catatan izin) **tidak menyimpan dokumen izin**. Hanya rujukan:

```ts
type Permission = {
  recordId: string;                            // ID catatan privat milik pemilik, bukan jalur publik
  receivedOn: ISODate;
  scope: ('institution-name' | 'real-screenshots' | 'system-details')[];
  ownerVerified: true;                         // literal: harus true
};
```

### 4.5 Media

```ts
type Media = {
  id: string;                                  // unik per project
  kind: 'screenshot' | 'diagram' | 'illustration';
  origin: 'real' | 'dummy' | 'original-artwork' | 'recreated';
  source: string;                              // relatif ke assets-src/images/, tanpa '..' dan tanpa 'private'
  alt?: LocaleMap;                             // wajib kecuali decorative = true
  decorative?: boolean;                        // saling eksklusif dengan alt (FR-05.4, NFR-02.6)
  // width, height, varian: dari image-manifest.json (diisi build-images), bukan ditulis tangan
};
```

`origin: 'real'` hanya valid bila project aktif pada varian `named` dengan `permission.scope` memuat `real-screenshots`. Pada project lain `real` berarti gambar milik pemilik yang tidak memuat data pihak ketiga, dan wajib melewati tinjauan manual pemilik (`SECURITY_PRIVACY_ERRORS.md` bagian 3).

### 4.6 Kontak (`content/contact.ts`)

```ts
type ContactLink =
  | { kind: 'github';    label: LocaleMap; href: string; confirmed: boolean }
  | { kind: 'instagram'; label: LocaleMap; href: string; confirmed: boolean }
  | { kind: 'email';     label: LocaleMap; address: string; confirmed: boolean };
```

Selama belum dikonfirmasi, nilai ditulis `[PLACEHOLDER: ...]` dan `confirmed: false`. `confirmed: true` mensyaratkan nilai tanpa `[PLACEHOLDER:` dan sesuai pola: GitHub `https://github.com/<nama>`, Instagram `https://www.instagram.com/<nama>/`, email berformat alamat. Tidak ada kind `phone`, `address`, `whatsapp`.

### 4.7 CV (`content/cv.ts`)

```ts
type Cv = {
  ready: boolean;
  ownerReviewed: boolean;
  reviewedOn?: ISODate;                        // wajib bila ownerReviewed = true
  file?: string;                               // relatif ke assets-src/cv/, mis. 'cv-en.pdf'
  label: LocaleMap;                            // mis. 'CV (PDF, English)'
};
```

CV **hanya** disertakan bila `ready && ownerReviewed && reviewedOn && file` ada dan `CHK-AST` lolos. Hasil `getCv()` bernilai `null` selain itu (FR-09.6, AC-63).

### 4.8 How I Built This (`content/build-notes.ts`)

```ts
type BuildNotes = {
  enabled: boolean;                            // false -> bagian tidak dirender (A-09, FR-08.5)
  items: {
    id: string;
    topic: 'animation' | 'performance' | 'accessibility' | 'stack' | 'ai';
    claim: Claim;                              // performance/accessibility wajib punya evidence
  }[];
  limitations: LocaleMap;                      // wajib bila enabled (FR-08.3)
  aiStatement: { text: LocaleMap; confirmed: boolean };   // K-11, ditinjau pemilik
  sourceLink?: ProjectLink;                    // hanya bila repositori dibuka dan lolos audit (FR-08.7)
};
```

### 4.9 Technical Expertise: diturunkan (D-05)

Tidak ada berkas keahlian terpisah. `getExpertiseGroups()` menghitung dari project aktif:

```
untuk setiap project aktif (varian terpilih):
  untuk setiap technologies[{techId, how}]:
     kelompokkan per Tech.group -> { tech.label, usages: [{ projectSlug, projectTitle, how }] }
urutkan grup dengan TechGroup.order, butir dengan label
```

Akibatnya: teknologi yang tidak dipakai project mana pun tidak pernah tampil (FR-07.3). Setiap butir menyebut project dan cara pakai (FR-07.1). Tidak ada tempat untuk persentase (FR-07.2). Data dirender dari data (FR-07.4).

Konsekuensi yang perlu diketahui: tampilan keahlian bergantung pada kelengkapan `technologies` di setiap project dan akan tampak pendek dengan dua project. Itu sesuai prinsip "keahlian berbasis project nyata".

## 5. Contoh (fiktif, bukan konten nyata)

```ts
// content/projects/example-notes-app.ts  (CONTOH, tidak dimasukkan ke repositori)
export const project: Project = {
  slug: 'example-notes-app',
  order: 99,
  updatedOn: '2026-10-13',
  content: {
    mode: 'single',
    body: {
      title:   { en: 'Example Notes App' },
      summary: { en: 'A small offline notes tool used to demonstrate the content model.' },
      role:    { en: 'Sole developer' },
      status:  { kind: 'in-development', label: { en: 'In development' } },
      technologies: [{ techId: 'typescript', how: { en: 'Types the data layer and validation.' } }],
      sections: { /* overview, problem, solution, process, roleAndLimits: Section */ },
      aiUsage: { text: { en: 'Built with AI coding assistance; decisions and review by the owner.' }, confirmed: true },
      outcome: { items: [], noMetricsStatement: { en: 'No measured results are available yet.' } },
      media: [], links: [],
      seo: { title: { en: 'Example Notes App' }, description: { en: 'Case study of an example project.' } },
    },
  },
};
```

## 6. Katalog aturan validasi

Mode: **Dev** = `npm`-skrip pengembangan dan `check-content`. **Rilis** = `RELEASE=1`. "Error" menggagalkan (kode keluar 1). "Warn" dilaporkan saja.

| ID | Aturan | Dev | Rilis | Rujukan |
|---|---|---|---|---|
| CHK-CNT-01 | Seluruh data lolos skema (bidang wajib ada, tipe benar, **bidang tak dikenal ditolak**) | Error | Error | FR-12.6, FR-07.2 |
| CHK-CNT-02 | Setiap `LocaleMap` memuat semua bahasa di `REQUIRED_LOCALES` dan tidak kosong | Error | Error | FR-12.2 |
| CHK-CNT-03 | Tidak ada `[PLACEHOLDER:` di nilai string manapun | Warn | **Error** | FR-12.5, AC-16, EC-23 |
| CHK-CNT-04 | `slug` unik, sesuai pola. `order` unik. `generateStaticParams` memakai daftar yang sama | Error | Error | FR-01.2 |
| CHK-CNT-05 | `techId` dan `projectSlug` yang dirujuk ada | Error | Error | FR-07.4 |
| CHK-CNT-06 | `Claim.confirmed = true` wajib punya `confirmedOn` berformat tanggal nyata dan tidak di masa depan | Error | Error | FR-12.4 |
| CHK-CNT-07 | `Claim.kind` `performance` atau `accessibility` wajib punya `evidence` dengan kategori A atau B dan `reportRef` berformat benar | Error | Error | NFR-09.2, AC-48 |
| CHK-CNT-08 | Bila tidak ada klaim hasil terkonfirmasi, `noMetricsStatement` wajib ada | Error | Error | FR-05.2, AC-09 |
| CHK-CNT-09 | `aiUsage.confirmed` harus `true` | Warn | **Error** | FR-05.3, AC-10 |
| CHK-CNT-10 | `headline.confirmed` harus `true` | Warn | **Error** | DP-07 |
| CHK-CNT-11 | Pada mode `variants` dengan `active: 'named'`: `permission` ada, `ownerVerified` benar, `named` ada, `scope` memuat `institution-name`. Bila `named` memuat media `origin: 'real'`, `scope` memuat `real-screenshots`. Bila memuat rincian sistem, `scope` memuat `system-details` | Error | Error | FR-05.5, AC-11, NFR-09.4 |
| CHK-CNT-12 | Pada mode `variants`, tidak ada berkas bernama `*.named.*` di `content/` bila `permission` tidak ada | Error | Error | D-09 |
| CHK-CNT-13 | Media: `alt` xor `decorative`. `alt` tidak kosong. `source` ada di `assets-src/images/` dan tidak melewati `..` atau `private` | Error | Error | FR-05.4, NFR-02.6 |
| CHK-CNT-14 | Setiap `figure.mediaId` merujuk media yang ada pada project yang sama | Error | Error | FR-05.4 |
| CHK-CNT-15 | `ProjectLink.href` berskema `https:` dan tidak kosong. `ContactLink` mengikuti pola. Skema `javascript:`, `data:`, dan lain-lain ditolak | Error | Error | FR-12.6, FR-05.7 |
| CHK-CNT-16 | Kata terlarang (`senior`, `expert`, `award-winning`, `passionate`), pola tahun pengalaman (`\b\d+\+?\s*years?\b`), dan frasa `WCAG 2.2 AA compliant` / `compliant` tidak muncul pada teks `en` | Warn | **Error** | NFR-09.3, AC-49 |
| CHK-CNT-17 | Daftar istilah sensitif lokal (D-10) tidak ditemukan di nilai string. Berkas daftar wajib ada pada rilis (kode keluar 2 bila tidak ada) | Warn bila berkas tak ada | **Error / 2** | NFR-09.4, EC-24 |
| CHK-CNT-18 | `CV.ready && ownerReviewed` mensyaratkan `reviewedOn` dan `file`. Berkas harus ada | Error | Error | FR-09.6 |
| CHK-CNT-19 | `seo.title` unik antar rute. Deskripsi tidak kosong | Error | Error | FR-14.1 |
| CHK-CNT-20 | `BuildNotes.enabled = true` mensyaratkan `limitations` dan `aiStatement.confirmed` | Error | Error | FR-08.3, FR-08.4 |
| CHK-CNT-21 | Teks tidak memuat pola alamat email, nomor telepon, jalur berkas lokal (`C:\`, `/home/`, `/Users/`), atau kunci/token umum, di luar `ContactLink` yang terkonfirmasi | Error | Error | NFR-08.1 |

Aturan `Warn` pada mode Dev memungkinkan pemilik mengerjakan konten bertahap tanpa build rusak terus-menerus, tetapi **tidak ada yang Warn pada mode Rilis** untuk hal yang melindungi integritas.

## 7. Perilaku fungsi akses (kontrak perilaku)

| Fungsi | Perilaku yang dijamin |
|---|---|
| `getProject(slug)` | Mengembalikan `undefined` untuk slug tak dikenal (halaman memanggil `notFound()`). Memilih varian menurut `active`. Pada mode `named` tanpa izin valid, melempar galat saat build (bukan jatuh diam-diam ke anonim) |
| `getProject` → `outcome` | Hanya klaim `confirmed`. Elemen yang dirender wajib membawa `data-claim-id` (dipakai `CHK-OUT-06`) |
| `listProjects()` | Menyertakan hanya project yang valid. Urut menurut `order`. Menambah entri data memunculkan kartu tanpa mengubah komponen (AC-07) |
| `getContactLinks()` | Mode rilis: hanya `confirmed`. Mode dev: semua, dengan bendera `isPlaceholder` agar UI dapat menandai |
| `getCv()` | `null` kecuali gerbang bagian 4.7 lolos |
| `getBuildNotes()` | `null` bila `enabled` salah. Klaim tak terkonfirmasi disaring |

## 8. Kriteria verifikasi

| Bukti yang dibutuhkan | Cara | AC |
|---|---|---|
| Entri uji ditambah lalu dihapus memunculkan dan menghilangkan kartu tanpa mengubah komponen | Uji menambah fixture (`tests/fixtures`) | AC-07 |
| Teks bahasa kedua dummy pada satu entri tidak mengubah skema atau komponen. Layout rapi dengan teks lebih panjang | Fixture dengan kunci `id` | AC-57 |
| Placeholder uji menggagalkan build rilis | Menyisipkan `[PLACEHOLDER: uji]` pada fixture, build dengan `RELEASE=1`, kode keluar bukan 0 | AC-16 |
| Klaim `confirmed: false` tidak ada di `out/` | Fixture klaim tak terkonfirmasi + `CHK-OUT-06` | AC-61 |
| Varian `named` tanpa `permission` menggagalkan build. Fixture `named` dengan `permission` fiktif lolos validasi tetapi **tidak** dipakai di build nyata | Dua fixture | AC-11 |
| CV `ready` tanpa `ownerReviewed` tidak menghasilkan tautan maupun berkas di `out/` | Dua konfigurasi data | AC-63 |
| Tautan dengan `href` kosong atau `javascript:` ditolak | Fixture | FR-12.6 |

Fixture berisi **data fiktif saja** dan tidak pernah memakai nama institusi atau data nyata.

## 9. Belum diputuskan

| ID | Item | Tahap |
|---|---|---|
| — | Lokasi catatan bukti dan isi `TR-*` | Tahap 6 (C-07) |
| V-19 | Pustaka skema final | Sebelum fase konten |
| QT4-05 | Tingkat anonimisasi teks project RSUD | Penyusunan konten |
| Q-13 | Versi beku Flash Sale Intelligence (`updatedOn` sudah disiapkan) | Penyusunan konten |
| Q-20 | Isi `aiUsage` project RSUD | Penyusunan konten |
