# Pelan Pembangunan: Dashboard Pertandingan 2i2c KK Bentong

## Gambaran Keseluruhan

Membina sebuah dashboard web penuh untuk pertandingan **2i2c (Inovasi, Idea, Cipta, Ciptaan)** yang dianjurkan oleh **Kolej Komuniti Bentong**. Dashboard ini merangkumi modul pendaftaran peserta, jadual acara, papan markah masa nyata (leaderboard), pengumuman, dan panel juri/admin.

**Stack Teknologi:**
- **Frontend**: HTML5 + CSS3 + JavaScript (Vanilla JS, tanpa framework)
- **Styling**: CSS tulen dengan CSS Variables untuk tema warna
- **Backend / Database**: Firebase (Firestore + Authentication + Storage) — disambung terus melalui Firebase JavaScript SDK (CDN)
- **Hosting**: Firebase Hosting (percuma)

**Skop:**
- 6 kategori pertandingan (Kategori 1–6, nama boleh dikemaskini kemudian)
- Kriteria pemarkahan boleh dikemaskini kemudian (placeholder disediakan)
- Berbilang juri — setiap juri log masuk berasingan, sistem purata markah secara automatik
- Seorang atau lebih admin untuk urus keseluruhan sistem

---

## Sub-Tugas

---

### Sub-Tugas 1: Scaffolding & Persediaan Projek

**Status:** [x] done

**Intent:**
Menyediakan struktur folder projek yang kemas dengan semua fail HTML asas, fail CSS global, fail JavaScript utiliti, dan konfigurasi Firebase melalui CDN — tanpa memerlukan Node.js atau build tools.

**Expected Outcomes:**
- Struktur folder projek lengkap dicipta
- Fail `css/style.css` dengan CSS Variables untuk tema warna biru dan emas
- Fail `js/firebase-config.js` yang muatkan Firebase SDK melalui CDN dan eksport konfigurasi
- Fail `js/auth.js` dengan fungsi autentikasi asas
- Fail `js/db.js` dengan fungsi CRUD Firestore asas
- Fail `firebase.json` untuk konfigurasi Firebase Hosting
- Fail `.env` (atau `config.js`) dengan placeholder untuk Firebase keys

**Todo List:**
1. Cipta struktur folder projek (lihat bahagian Struktur Folder di bawah)
2. Cipta `css/style.css` dengan CSS Variables tema (`--warna-utama`, `--warna-emas`, dll.), reset CSS, dan gaya asas
3. Cipta `css/komponen.css` untuk gaya komponen berulang (kad, butang, borang, navbar, jadual)
4. Cipta `js/firebase-config.js` yang muatkan Firebase melalui CDN dan inisialisasi app
5. Cipta `js/auth.js` dengan fungsi `loginPengguna`, `logoutPengguna`, `semakSesi`
6. Cipta `js/db.js` dengan fungsi CRUD Firestore: `tambah`, `kemaskini`, `padam`, `dapatkan`, `senarai`
7. Cipta `js/utiliti.js` dengan fungsi pembantu: `formatTarikh`, `tunjukMesej`, `sahkanBorang`
8. Cipta `firebase.json` untuk konfigurasi Firebase Hosting

**Relevant Context:**
- `js/firebase-config.js` — titik masuk semua operasi Firebase
- `css/style.css` — tema warna dan gaya global

---

### Sub-Tugas 2: Sistem Autentikasi (Admin & Juri)

**Status:** [x] done

**Intent:**
Membina halaman log masuk dan sistem pengurusan sesi pengguna menggunakan Firebase Authentication (Email/Password), dengan kawalan akses berdasarkan peranan yang disimpan dalam Firestore.

**Expected Outcomes:**
- Halaman `login.html` dengan borang e-mel dan kata laluan berfungsi
- Admin yang log masuk diarahkan ke `admin/index.html`
- Juri yang log masuk diarahkan ke `juri/markah.html`
- Halaman admin dan juri semak sesi secara automatik — pengguna tanpa log masuk diarahkan balik ke `login.html`
- Maklumat peranan (admin/juri) disimpan dalam Firestore koleksi `pengguna`

**Todo List:**
1. Aktifkan Firebase Authentication (Email/Password) dalam konsol Firebase
2. Cipta Firestore koleksi `pengguna` dengan medan: `email`, `peranan` (admin/juri), `nama`
3. Cipta `login.html` dengan borang log masuk dan skrip `js/auth.js`
4. Kemaskini `js/auth.js` — tambah fungsi `semakPeranan` yang baca Firestore untuk tentukan peranan pengguna
5. Tambah fungsi `lindungiHalaman(peranandiperlukan)` dalam `js/auth.js` yang redirect jika sesi tidak sah
6. Panggil `lindungiHalaman('admin')` di atas setiap halaman dalam folder `admin/`
7. Panggil `lindungiHalaman('juri')` di atas setiap halaman dalam folder `juri/`
8. Tambah butang log keluar pada navbar admin dan juri

**Relevant Context:**
- `js/auth.js` — semua fungsi autentikasi dan semakan peranan
- Firestore koleksi: `pengguna`

---

### Sub-Tugas 3: Modul Pendaftaran Peserta (Awam)

**Status:** [x] done

**Intent:**
Membina halaman pendaftaran awam yang boleh diisi oleh peserta tanpa log masuk. Data disimpan ke Firestore. Admin boleh luluskan atau tolak penyertaan melalui panel admin.

**Expected Outcomes:**
- Halaman `daftar.html` boleh diakses awam tanpa log masuk
- Borang pendaftaran berfungsi dengan pengesahan JavaScript: nama, institusi, nama projek, kategori (dropdown 6 kategori), e-mel, muat naik abstrak PDF
- Data peserta disimpan dalam Firestore koleksi `peserta` dengan status awal `menunggu`
- Halaman `semak-status.html` untuk peserta semak status pendaftaran menggunakan e-mel
- Panel admin `admin/peserta.html` untuk lihat, lulus, atau tolak pendaftaran

**Todo List:**
1. Cipta Firestore koleksi `peserta` dengan skema: `nama`, `institusi`, `namaProjek`, `kategori`, `email`, `abstrakURL`, `status`, `tarikhDaftar`
2. Cipta `daftar.html` dengan borang HTML lengkap dan pengesahan JavaScript dalam `js/daftar.js`
3. Tambah fungsi upload fail PDF ke Firebase Storage dalam `js/db.js`
4. Cipta `js/peserta.js` dengan fungsi: `daftarPeserta`, `dapatkanPesertaByEmail`, `kemaskiniStatus`
5. Cipta `semak-status.html` dengan borang carian e-mel dan paparan status
6. Cipta `admin/peserta.html` dengan jadual senarai peserta, butang Lulus/Tolak, dan penapis mengikut status dan kategori

**Relevant Context:**
- `js/peserta.js` — CRUD untuk data peserta
- `js/db.js` — fungsi upload Firebase Storage
- Firestore koleksi: `peserta`

---

### Sub-Tugas 4: Modul Jadual Acara

**Status:** [x] done

**Intent:**
Membina halaman jadual acara awam dalam format timeline dan panel admin untuk urus entri jadual.

**Expected Outcomes:**
- Halaman `jadual.html` memaparkan timeline acara pertandingan mengikut masa
- Setiap entri jadual menunjukkan: masa, tajuk acara, lokasi, penerangan ringkas
- Panel admin `admin/jadual.html` boleh tambah, edit, dan padam entri jadual
- Jadual dimuatkan dari Firestore secara masa nyata

**Todo List:**
1. Cipta Firestore koleksi `jadual` dengan skema: `masa`, `tajuk`, `lokasi`, `penerangan`, `urutan`
2. Cipta `js/jadual.js` dengan fungsi: `muatJadual`, `tambahAcara`, `kemaskiniAcara`, `padamAcara`
3. Cipta `jadual.html` dengan paparan timeline CSS menegak yang muatkan data dari Firestore
4. Cipta `admin/jadual.html` dengan borang tambah/edit acara dan jadual senarai semua entri
5. Tambah fungsi sort automatik mengikut nilai `urutan` apabila data dipaparkan

**Relevant Context:**
- `js/jadual.js` — CRUD untuk data jadual
- Firestore koleksi: `jadual`

---

### Sub-Tugas 5: Modul Pemarkahan Juri

**Status:** [x] done

**Intent:**
Membina panel juri untuk memasukkan markah peserta mengikut kriteria ditetapkan. Sistem mengira purata markah dari semua juri secara automatik.

**Expected Outcomes:**
- Halaman `juri/markah.html` memaparkan senarai peserta yang perlu dinilai
- Juri boleh klik peserta dan isi borang markah mengikut setiap kriteria (placeholder: Kriteria 1–5)
- Markah disimpan dalam Firestore koleksi `markah` dikaitkan dengan `pesertaId` dan `juriId`
- Sistem mengira purata markah semua juri untuk setiap peserta dan simpan dalam dokumen peserta
- Juri hanya boleh edit markah mereka sendiri
- Petunjuk visual (ikon centang/silang) tunjukkan peserta yang sudah/belum dinilai oleh juri tersebut

**Todo List:**
1. Cipta Firestore koleksi `markah` dengan skema: `pesertaId`, `juriId`, `kriteria` (objek dengan Kriteria1–5), `jumlah`, `tarikhDikemaskini`
2. Cipta `js/markah.js` dengan fungsi: `simpanMarkah`, `dapatkanMarkahJuri`, `kiraPurata`
3. Fungsi `kiraPurata` dalam `js/markah.js` — ambil semua dokumen markah untuk satu peserta, kira purata, dan simpan balik ke dokumen `peserta`
4. Cipta `juri/markah.html` dengan senarai peserta (tapis mengikut kategori) dan modal borang pemarkahan
5. Tambah petunjuk visual: ikon hijau (sudah dinilai) dan merah (belum dinilai) pada senarai peserta
6. Konfigurasikan Firestore Security Rules untuk hadkan juri hanya boleh baca/tulis markah mereka sendiri

**Relevant Context:**
- `js/markah.js` — logik pemarkahan dan pengiraan purata
- Firestore koleksi: `markah`, `peserta`
- Firestore Security Rules perlu dikemaskini

---

### Sub-Tugas 6: Modul Papan Markah (Leaderboard)

**Status:** [x] done

**Intent:**
Membina halaman leaderboard awam yang memaparkan ranking peserta mengikut purata markah secara masa nyata, boleh ditapis mengikut 6 kategori.

**Expected Outcomes:**
- Halaman `leaderboard.html` boleh diakses awam
- Peserta dipaparkan mengikut ranking (purata markah tertinggi dahulu)
- Butang penapis mengikut 6 kategori berfungsi
- Markah dikemas kini secara masa nyata menggunakan Firestore `onSnapshot`
- 3 tempat teratas diberi lencana visual (Emas, Perak, Gangsa)
- Admin boleh tunjuk/sembunyi leaderboard melalui tetapan dalam Firestore

**Todo List:**
1. Cipta halaman `leaderboard.html` dengan jadual ranking dan butang penapis kategori
2. Cipta `js/leaderboard.js` yang pasang `onSnapshot` pada koleksi `peserta` dan papar data secara masa nyata
3. Tambah fungsi `renderLeaderboard(data, kategori)` yang sort peserta mengikut `puratMarkah` dan tapis mengikut kategori
4. Tambah lencana HTML/CSS untuk 3 tempat teratas
5. Cipta Firestore dokumen `tetapan/umum` dengan medan `leaderboardVisible` (boolean)
6. Tambah semakan dalam `js/leaderboard.js` — jika `leaderboardVisible` adalah `false`, paparkan mesej "Penilaian sedang berlangsung"
7. Tambah togol `leaderboardVisible` dalam `admin/tetapan.html`

**Relevant Context:**
- `js/leaderboard.js` — logik paparan masa nyata
- Firestore koleksi: `peserta`, `tetapan`

---

### Sub-Tugas 7: Modul Pengumuman

**Status:** [x] done

**Intent:**
Membina halaman pengumuman awam dan panel admin untuk urus siaran pengumuman terkini pertandingan.

**Expected Outcomes:**
- Halaman `pengumuman.html` memaparkan senarai pengumuman terkini (terbaru dahulu)
- Admin boleh tambah, edit, dan padam pengumuman dalam `admin/pengumuman.html`
- Pengumuman penting bertanda `utama: true` dipaparkan pada laman utama
- Hanya pengumuman bertanda `diterbit: true` dipaparkan kepada awam

**Todo List:**
1. Cipta Firestore koleksi `pengumuman` dengan skema: `tajuk`, `kandungan`, `tarikhHantar`, `utama` (boolean), `diterbit` (boolean)
2. Cipta `js/pengumuman.js` dengan fungsi: `muatPengumuman`, `tambahPengumuman`, `kemaskiniPengumuman`, `padamPengumuman`
3. Cipta `pengumuman.html` dengan senarai kad pengumuman yang dimuatkan dari Firestore
4. Cipta `admin/pengumuman.html` dengan borang tambah/edit, senarai semua pengumuman, dan butang Terbit/Padam
5. Tambah seksyen "Pengumuman Terkini" pada `index.html` yang tarik 3 pengumuman terbaru bertanda `utama: true`

**Relevant Context:**
- `js/pengumuman.js` — CRUD pengumuman
- Firestore koleksi: `pengumuman`

---

### Sub-Tugas 8: Laman Utama & Navigasi Global

**Status:** [x] done

**Intent:**
Membina laman utama sebagai pintu masuk dashboard dengan semua maklumat ringkas pertandingan, countdown timer, dan navigasi global yang konsisten merentas semua halaman.

**Expected Outcomes:**
- Laman utama `index.html` memaparkan: hero section, countdown timer, statistik peserta, pengumuman utama, dan pautan ke semua modul
- Navbar konsisten merentas semua halaman awam dengan pautan: Laman Utama, Jadual, Leaderboard, Pengumuman, Daftar
- Reka bentuk responsif (mobile-friendly) menggunakan CSS Flexbox/Grid
- Warna tema biru dan emas menggunakan CSS Variables
- Footer dengan maklumat KK Bentong

**Todo List:**
1. Kemaskini `css/style.css` dengan CSS Variables tema: `--biru: #003087`, `--emas: #FFD700`, `--biru-muda: #0057B8`
2. Cipta komponen navbar HTML dalam fail `komponen/navbar.html` dan muatkan ke semua halaman menggunakan `fetch` JavaScript
3. Cipta komponen footer HTML dalam fail `komponen/footer.html` dan muatkan ke semua halaman
4. Cipta `js/countdown.js` dengan fungsi countdown timer yang kira undur ke tarikh pertandingan
5. Cipta `index.html` dengan: hero section, countdown timer, seksyen statistik, seksyen pengumuman utama, dan grid kad pautan modul
6. Pastikan semua halaman adalah responsif (mobile-friendly) menggunakan CSS media queries
7. Tambah animasi mudah (CSS transitions) untuk hover, loading, dan kemas kini data

**Relevant Context:**
- `css/style.css` — CSS Variables dan tema warna global
- `js/countdown.js` — logik countdown timer
- `komponen/navbar.html`, `komponen/footer.html` — komponen dikongsi

---

## Struktur Folder Projek

```
2i2c-dashboard/
├── index.html                    # Laman Utama
├── login.html                    # Log Masuk
├── daftar.html                   # Pendaftaran Peserta
├── semak-status.html             # Semak Status Pendaftaran
├── jadual.html                   # Jadual Acara
├── leaderboard.html              # Papan Markah
├── pengumuman.html               # Pengumuman
├── firebase.json                 # Konfigurasi Firebase Hosting
│
├── admin/
│   ├── index.html                # Dashboard Admin
│   ├── peserta.html              # Urus Peserta
│   ├── jadual.html               # Urus Jadual
│   ├── pengumuman.html           # Urus Pengumuman
│   └── tetapan.html              # Tetapan Sistem
│
├── juri/
│   └── markah.html               # Panel Juri
│
├── css/
│   ├── style.css                 # Gaya Global + CSS Variables
│   └── komponen.css              # Gaya Komponen (kad, borang, navbar)
│
├── js/
│   ├── firebase-config.js        # Konfigurasi Firebase (CDN)
│   ├── auth.js                   # Autentikasi & Semakan Sesi
│   ├── db.js                     # Fungsi CRUD Firestore & Storage
│   ├── utiliti.js                # Fungsi Pembantu Umum
│   ├── countdown.js              # Countdown Timer
│   ├── peserta.js                # Logik Modul Peserta
│   ├── markah.js                 # Logik Modul Pemarkahan
│   ├── jadual.js                 # Logik Modul Jadual
│   ├── leaderboard.js            # Logik Leaderboard Masa Nyata
│   └── pengumuman.js             # Logik Modul Pengumuman
│
├── komponen/
│   ├── navbar.html               # Navbar dikongsi
│   └── footer.html               # Footer dikongsi
│
└── imej/
    └── logo-kkbentong.png        # Logo KK Bentong
```

---

## Nota Penting

- Semua fail Firebase SDK dimuatkan melalui **CDN** — tiada Node.js atau `npm install` diperlukan
- Nama 6 kategori pertandingan perlu dikemaskini sebelum Sub-Tugas 3 dilaksanakan
- Kriteria pemarkahan perlu dikemaskini sebelum Sub-Tugas 5 dilaksanakan
- Pembangun perlu mencipta projek Firebase sendiri dan isi nilai dalam `js/firebase-config.js`
- Firestore Security Rules perlu dikonfigurasikan dengan teliti terutama untuk modul pemarkahan
- Firebase Hosting boleh digunakan secara percuma untuk host semua fail statik ini
