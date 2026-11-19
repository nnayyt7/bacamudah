# BacaMudah

**Materi belajar jadi lebih mudah dibaca, didengar, dan dipahami, terutama bagi penyandang disleksia.**

BacaMudah adalah website alat bantu baca berbasis AI. Pengguna menempelkan teks atau mengunggah PDF materi pelajaran. AI lalu mengubahnya menjadi bahasa yang lebih sederhana, poin-poin, analogi, atau daftar istilah. Hasilnya tampil dengan pengaturan baca ramah disleksia, bisa dibacakan sambil kalimatnya disorot, dan bisa dicek pemahamannya lewat kuis singkat.

| | |
|---|---|
| Website | **https://bacamudah.vercel.app** |
| Video demo | [ISI: tautan YouTube] |
| Lomba | IT RUSH 2026 (HIMATIF UNIB), subtema 5: Pendidikan, Tata Kelola, dan Teknologi Inklusif |
| Tim | Sangatta Keren |

> **Cara tercepat mencoba:** buka website, lalu tekan **Coba demo**. Contoh materi langsung disederhanakan tanpa menunggu AI. Setelah itu tekan **Dengarkan hasil** dan **Kerjakan kuis** di panel demo.

---

## Daftar isi

1. [Masalah yang ingin dibantu](#masalah-yang-ingin-dibantu)
2. [Fitur](#fitur)
3. [Aksesibilitas](#aksesibilitas)
4. [Cara kerja](#cara-kerja)
5. [Menjalankan di komputer sendiri](#menjalankan-di-komputer-sendiri)
6. [Deploy ke Vercel](#deploy-ke-vercel)
7. [Penggunaan AI](#penggunaan-ai)
8. [Tim dan peran](#tim-dan-peran)
9. [Library, font, dan aset](#library-font-dan-aset)
10. [Privasi dan batasan](#privasi-dan-batasan)

---

## Masalah yang ingin dibantu

- Materi pelajaran sering ditulis dengan kalimat panjang, istilah teknis, dan paragraf padat.
- Penyandang disleksia sulit mengenali kata dan mudah kehilangan baris saat membaca. Huruf yang rapat dan latar putih terang membuatnya makin berat.
- Alat bantu baca yang ada biasanya berbahasa Inggris, berbayar, atau hanya menyelesaikan satu masalah (misalnya hanya suara).

Akibatnya, siswa yang kesulitan membaca bisa tertinggal **bukan karena kurang mampu, tetapi karena akses ke materinya tidak setara**.

BacaMudah menggabungkan tiga hal dalam satu website gratis berbahasa Indonesia, tanpa akun dan tanpa instalasi:

1. **Menyederhanakan isi** dengan AI (apa yang dibaca).
2. **Menyesuaikan tampilan** agar nyaman bagi disleksia (bagaimana dibaca).
3. **Mendukung pemahaman** lewat suara dan kuis (apakah sudah paham).

**Untuk siapa?** Siswa SMP/SMA dengan disleksia atau kesulitan membaca, mahasiswa yang membaca modul padat, serta guru, guru BK, guru inklusi, dan orang tua yang menyiapkan materi untuk murid atau anaknya.

---

## Fitur

| Fitur | Penjelasan |
|---|---|
| **Input materi** | Tempel teks (maks. 15.000 karakter) atau unggah PDF berisi teks (maks. 4 MB). Teks dari PDF muncul di kolom input agar bisa dicek dulu. |
| **4 mode AI** | **Teks Sederhana** (kalimat pendek, kata sehari-hari), **Poin-Poin** (5-10 poin utama), **Analogi** (perumpamaan sehari-hari), **Istilah Sulit** (daftar istilah dan artinya). Istilah penting ditebalkan. |
| **Tampilan ramah disleksia** | Huruf OpenDyslexic / Lexend / biasa, ukuran 16-28 px, jarak baris 1,5-2,5, jarak antarhuruf, 4 warna latar lembut (krem, kuning pucat, biru muda, gelap lembut), penggaris baca, dan Bionic Reading. Pilihan tersimpan di browser. |
| **Baca nyaring + sorot kalimat** | Hasil dibacakan dengan suara bahasa Indonesia dari browser. Kalimat yang sedang dibaca disorot agar tidak kehilangan baris. Kecepatan 0,75x / 1x / 1,25x. |
| **Kamus kontekstual** | Ketuk istilah bergaris putus-putus, atau blok kata lain di hasil, untuk melihat artinya sesuai kalimatnya, tanpa pindah halaman. |
| **Kuis pemahaman** | AI membuat 3-5 soal pilihan ganda dari materi. Satu soal per layar, langsung ada umpan balik dan penjelasan, lalu skor akhir. |
| **Salin dan unduh** | Salin ke clipboard, unduh .txt, atau unduh **PDF "ramah baca"** yang memakai huruf, ukuran, warna, dan Bionic Reading pilihan pengguna. |
| **Riwayat materi** | Lima materi terakhir beserta hasil dan skor kuisnya tersimpan di browser, bisa dibuka lagi tanpa menunggu AI. |
| **Demo satu klik** | Tombol **Coba demo** menampilkan contoh hasil, perbandingan sebelum dan sesudah, lalu menuntun ke baca nyaring dan kuis. |
| **Halaman Tentang** | Penjelasan masalah, cara pakai, penggunaan AI, privasi, dan batasan. |

---

## Aksesibilitas

Aksesibilitas adalah inti dari BacaMudah, bukan tambahan.

- HTML semantik: judul berurutan, landmark (`header`, `main`, `nav`, `footer`), dan label pada semua input.
- Semua fitur bisa dipakai dengan keyboard. Fokus terlihat jelas.
- `aria-live` mengumumkan proses, hasil, dan pesan error ke pembaca layar.
- Keempat warna latar memenuhi kontras minimal WCAG AA. Latar putih murni dan hitam murni sengaja dihindari.
- Tombol berukuran besar dan berlabel teks, bukan ikon saja.
- Tampilan dibuat mobile-first. Semua fitur bisa dipakai di layar 360 px tanpa geser ke samping.
- Pesan error memakai bahasa yang ramah, dan setiap kegagalan AI punya tombol **Coba lagi**.

---

## Cara kerja

```
Browser (HTML, CSS, JavaScript)
   │  teks materi / file PDF
   ▼
Server Flask (Python)
   ├─ pypdf ............ mengambil teks dari PDF
   ├─ prompts.py ....... menyusun perintah untuk AI
   ├─ ai.py ............ memanggil Gemini API dan memeriksa jawabannya
   └─ rate_limit.py .... membatasi 20 permintaan per menit per perangkat
   │
   ▼
Gemini API (Google)
```

- **API key hanya ada di server**, tidak pernah dikirim ke browser.
- Materi pengguna dibungkus tanda batas khusus, dan AI diminta memperlakukannya sebagai bahan bacaan, bukan perintah (mencegah *prompt injection* sederhana).
- Jawaban kuis dari AI diperiksa server (jumlah soal, 4 pilihan, kunci jawaban). Jika tidak valid, server mencoba sekali lagi.
- Jika Gemini sedang sibuk (error 5xx), server mencoba ulang sekali sebelum menampilkan pesan ramah.
- Baca nyaring memakai Web Speech API bawaan browser, jadi tidak memakai kuota API.
- PDF "ramah baca" dibuat langsung di browser dengan jsPDF.

### Endpoint

| Method | Path | Fungsi |
|---|---|---|
| GET | `/` | Halaman utama |
| GET | `/tentang` | Halaman Tentang |
| GET | `/health` | Cek status server |
| POST | `/api/extract-pdf` | Unggah PDF → `{ "text": "..." }` |
| POST | `/api/process` | `{ "text", "mode" }` → `{ "result": "..." }` |
| POST | `/api/quiz` | `{ "text" }` → `{ "questions": [...] }` |
| POST | `/api/define` | `{ "term", "context" }` → `{ "term", "definition" }` |

Semua error API dikembalikan sebagai JSON `{ "error": "pesan ramah" }` dengan status code yang sesuai.

### Struktur folder

```
bacamudah/
├── app.py              # Aplikasi Flask: halaman, endpoint API, penanganan error
├── ai.py               # Pemanggil Gemini, coba ulang, dan validasi kuis
├── prompts.py          # Template perintah AI untuk tiap mode, kuis, dan kamus
├── pdf_utils.py        # Mengambil teks dari PDF
├── rate_limit.py       # Pembatas jumlah permintaan per perangkat
├── templates/          # Halaman HTML (Jinja2)
├── public/static/
│   ├── css/style.css   # Seluruh tampilan, termasuk 4 tema warna
│   ├── js/             # app, settings, tts, quiz, dictionary, history, pdf-export, demo
│   ├── fonts/          # Lexend dan OpenDyslexic (di-host sendiri) + font untuk PDF
│   └── img/logo.svg
├── sample/             # Contoh materi dan data demo
├── requirements.txt
└── .env.example        # Contoh variabel lingkungan (tanpa key asli)
```

---

## Menjalankan di komputer sendiri

Langkah ini untuk macOS. Butuh **Python 3.11 atau lebih baru** (proyek diuji dengan Python 3.13) dan **API key Gemini** gratis dari [Google AI Studio](https://aistudio.google.com/apikey). Cek versi Python dengan `python3 --version`. Jika belum ada atau versinya lebih lama, pasang dari [python.org](https://www.python.org/downloads/).

1. Unduh kode dan masuk ke foldernya:

   ```bash
   git clone https://github.com/Bimm-Boo/bacamudah.git
   cd bacamudah
   ```

2. Buat lingkungan Python terpisah, lalu pasang library:

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```

3. Salin file contoh pengaturan, lalu buka file `.env` dan isi `GEMINI_API_KEY` dengan API key kamu:

   ```bash
   cp .env.example .env
   ```

4. Jalankan server:

   ```bash
   python app.py
   ```

5. Buka **http://127.0.0.1:5001** di browser. Tekan `Ctrl + C` di Terminal untuk berhenti.

> File `.env` sudah masuk `.gitignore`, jadi API key tidak ikut ter-upload ke GitHub.

### Variabel lingkungan

| Nama | Wajib? | Keterangan |
|---|---|---|
| `GEMINI_API_KEY` | Ya | API key Gemini. Jangan pernah di-commit. |
| `GEMINI_MODEL` | Tidak | Nama model Gemini. Default: `gemini-3.5-flash-lite`. |

---

## Deploy ke Vercel

Website publik berjalan di Vercel (paket Hobby, gratis). Vercel mengenali aplikasi Flask di `app.py` secara otomatis, dan file di folder `public/` disajikan langsung sebagai file statis.

1. Di [vercel.com](https://vercel.com), pilih **Add New → Project**, lalu impor repository ini.
2. Di **Environment Variables**, tambahkan `GEMINI_API_KEY` (dan `GEMINI_MODEL` jika perlu).
3. Tekan **Deploy**. Setiap push ke branch `main` akan ter-deploy otomatis.

Endpoint `/health` bisa dipakai untuk memeriksa apakah server menyala.

---

## Penggunaan AI

Lomba ini mewajibkan transparansi penggunaan AI. Berikut rinciannya.

**1. AI sebagai fitur di dalam website: Gemini API (Google)**

- Dipakai untuk 4 mode penyederhanaan, kamus kontekstual, dan pembuatan kuis, melalui library `google-genai`.
- AI diminta memakai bahasa Indonesia sederhana dan tidak menambah informasi di luar materi asli.
- Hasil dan kuis untuk tombol **Coba demo** dibuat sekali dengan Gemini dari contoh materi, lalu diperiksa dan dirapikan sebelum disimpan di folder `sample/`. Hal ini juga dijelaskan di panel demo dan halaman Tentang.
- AI tetap bisa keliru, sehingga pengguna diingatkan untuk membandingkan hasil dengan materi asli.

**2. AI sebagai asisten pemrograman: Claude Code (Anthropic)**

- Kode website ini ditulis dengan bantuan **Claude Code** sebagai *AI coding assistant*.
- Tim menentukan masalah, fitur, prioritas, dan arah desain. Claude Code menulis dan menguji kode berdasarkan arahan tersebut, lalu tim meninjau hasilnya dan memberi masukan.
- Riwayat commit dibiarkan utuh sesuai aturan lomba. Sebagian commit mencantumkan Claude sebagai *co-author*, sehingga Claude juga tampil di daftar contributor GitHub.

---

## Tim dan peran

**Tim Sangatta Keren**

| Nama | Peran |
|---|---|
| [ISI: nama anggota 1] | [ISI: peran anggota 1] |
| [ISI: nama anggota 2] | [ISI: peran anggota 2] |

Yang dikerjakan tim:

- Menyusun dokumen kebutuhan produk (PRD): masalah, target pengguna, fitur, batasan, dan jadwal.
- Memilih arah desain dari beberapa mockup, lalu meminta penyesuaian tata letak untuk HP dan desktop.
- Mengumpulkan masukan penguji dan menentukan perbaikan serta fitur tambahan.
- Menentukan alur demo lomba, menguji website, dan menyiapkan proposal serta video demo.

---

## Library, font, dan aset

Semua aset di-host sendiri. Website tidak memuat file apa pun dari CDN luar.

| Nama | Dipakai untuk | Lisensi |
|---|---|---|
| [Flask](https://flask.palletsprojects.com/) 3.1 | Server web | BSD-3-Clause |
| [google-genai](https://github.com/googleapis/python-genai) 3.3 | Memanggil Gemini API | Apache-2.0 |
| [pypdf](https://github.com/py-pdf/pypdf) 7.1 (dengan `cryptography`) | Mengambil teks dari PDF | BSD-3-Clause |
| [python-dotenv](https://github.com/theskumar/python-dotenv) 1.2 | Membaca file `.env` | BSD-3-Clause |
| [gunicorn](https://gunicorn.org/) 27.0 | Server produksi jika tidak memakai Vercel | MIT |
| [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 | Membuat PDF "ramah baca" di browser | MIT |
| [Lexend](https://github.com/googlefonts/lexend) | Huruf utama website dan PDF | SIL Open Font License 1.1 |
| [OpenDyslexic](https://opendyslexic.org/) | Huruf ramah disleksia | SIL Open Font License 1.1 |
| Web Speech API | Baca nyaring (bawaan browser) | - |

Catatan font:

- Teks lisensi ada di `public/static/fonts/OFL-Lexend.txt` dan `public/static/fonts/OFL-OpenDyslexic.txt`.
- Untuk ekspor PDF, OpenDyslexic dipangkas ke huruf Latin dan diubah ke format TrueType. Karena "OpenDyslexic" adalah *Reserved Font Name*, versi yang diubah ini diberi nama **BacaMudah Disleksia** sesuai ketentuan OFL. Rinciannya ada di `public/static/fonts/pdf/README.txt`.
- Logo, maskot, dan ikon dibuat khusus untuk proyek ini dalam bentuk SVG.

---

## Privasi dan batasan

**Privasi**

- Tidak ada akun, dan materi **tidak disimpan di server**. Materi hanya diproses, lalu dibuang.
- Agar bisa diolah, materi dikirim ke Gemini API versi gratis. Google dapat memakai data tersebut untuk meningkatkan layanannya. Karena itu, pengguna diingatkan untuk tidak memasukkan data pribadi.
- Pengaturan tampilan dan riwayat materi hanya disimpan di browser pengguna (`localStorage`) dan bisa dihapus kapan saja.

**Batasan**

- Teks maksimal 15.000 karakter sekali proses. PDF maksimal 4 MB dan harus berisi teks (PDF hasil scan belum bisa dibaca).
- Suara bahasa Indonesia paling lengkap ada di Google Chrome (desktop dan Android).
- Satu perangkat bisa mengirim hingga 20 permintaan AI per menit, agar kuota tetap aman untuk semua pengunjung.
- Website memakai server gratis, sehingga akses pertama setelah lama tidak dibuka bisa sedikit lebih lambat.

---

Dibuat oleh tim **Sangatta Keren** untuk IT RUSH 2026.
