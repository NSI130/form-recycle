# ✿ REBLOOM — Share & Sell Your Creation

**Old Things. New Stories.** — *From Waste to Wellness.*

REBLOOM adalah aplikasi web berbasis *serverless* untuk mendokumentasikan dan memasarkan karya daur ulang (*upcycling*). Pengguna dapat membagikan cerita di balik karya mereka ke galeri komunitas atau mendaftarkan produk daur ulang ke marketplace, dengan penyimpanan otomatis ke **Google Sheets** dan **Google Drive**.

---

## ✨ Fitur Utama

### Frontend (`index.html`)
* **Dual-Mode Switcher + URL Sync:** Perpindahan mulus antara mode **Share Creation** (Galeri) dan **Sell Your Creation** (Marketplace) yang tersinkronisasi dengan parameter URL (`?tab=sell`).
* **Kompresi Gambar Otomatis (Client-Side):** Foto hingga 10MB otomatis di-*resize* (maksimal 1280px) dan dikompresi via HTML5 `<canvas>` sebelum dikirim agar proses *upload* cepat dan hemat kuota.
* **Drag & Drop + Instant Preview:** Mendukung seret-lepas file dengan pratinjau instan berbasis `URL.createObjectURL` serta pembersihan memori otomatis.
* **Custom Toast Notification:** Sistem notifikasi interaktif yang menyatu dengan palet warna REBLOOM (mendukung *auto-dismiss*, *pause-on-hover*, dan pembatasan tumpukan).
* **Proteksi Koneksi:** Dilengkapi pengunci *anti double-submit* dan batas waktu koneksi (*timeout* 35 detik via `AbortController`).

### Backend (`Code.gs` — Google Apps Script)
* **Anti-Bentrok (`LockService`):** Menjamin pengiriman data yang masuk di detik yang sama tetap mengantre dengan aman tanpa saling menimpa baris di Google Sheets.
* **Upload Terpisah dari Lock:** Proses penyimpanan gambar ke Google Drive dijalankan di luar blok `LockService` agar tidak menghambat antrean pengguna lain.
* **Auto-Setup & Rollback:** Otomatis membuat folder `REBLOOM_Uploads` di Drive dan menyusun baris *header* di Spreadsheet jika belum tersedia. Jika penulisan ke Sheet gagal, file yang terlanjur terunggah otomatis dihapus (*rollback*).
* **Sanitasi Data:** Mengamankan input nomor kontak (seperti `+62...`) dan karakter khusus agar tidak terbaca sebagai rumus di Google Sheets.

---

## 🗂️ Struktur File & Skema Data

```text
rebloom/
├── index.html   # Antarmuka pengguna, validasi, kompresi gambar & fetch API
├── Code.gs      # Backend Google Apps Script (endpoint doPost & doGet)
└── README.md    # Dokumentasi proyek
