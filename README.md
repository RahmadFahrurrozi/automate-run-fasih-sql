# FASIH Dashboard SQL Lab Scraper (SE2026)

Automasi scraping data Superset SQL Lab di FASIH BPS (`fasih-dashboard.bps.go.id`) menggunakan Selenium dengan metode **Pure UI-Clicking** via Chrome Remote Debugging.

Script ini meniru interaksi manual pengguna di browser:

1. Menulis ulang query SQL ke dalam Ace Editor Superset.
2. Mengklik tombol **RUN**.
3. Membaca hasil baris yang dikembalikan (`X rows returned`).
4. Mengunduh data ke format Excel (**DOWNLOAD TO EXCEL**), merename file, dan menyimpannya secara terstruktur.
5. Pindah secara otomatis antar kecamatan di wilayah target.
6. Memiliki fitur **Checkpoint & Auto-Resume** sehingga aman jika koneksi terputus di tengah jalan.

---

## Prasyarat (Prerequisites)

Sebelum menjalankan script, pastikan perangkat Anda sudah memenuhi syarat berikut:

- **Python 3.8+** sudah terinstall di sistem.
- **Google Chrome** terinstall.
- **Koneksi VPN BPS** (aktif dan terhubung).
- Akses akun dan izin ke **SQL Lab Superset** di FASIH BPS (`https://fasih-dashboard.bps.go.id/superset/sqllab/`).

---

## Panduan Instalasi & Running (Step-by-Step dari 0)

### 1. Clone / Download Repository

Buka terminal (PowerShell / Command Prompt / Git Bash) lalu jalankan:

```bash
git clone https://github.com/<username_github_lu>/fasih-dashboard-sql.git
cd fasih-dashboard-sql
```

*(Jika mendownload dalam format ZIP, extract folder tersebut lalu masuk ke dalamnya via terminal).*

---

### 2. Buat & Aktifkan Python Virtual Environment (venv)

#### Windows (PowerShell):

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

#### Windows (CMD):

```cmd
python -m venv venv
.\venv\Scripts\activate.bat
```

#### Linux / macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

*(Jika venv berhasil diaktifkan, akan muncul tanda `(venv)` di awal prompt terminal Anda).*

---

### 3. Install Dependencies / Library Python

Jalankan perintah berikut di dalam virtual environment:

```bash
pip install -r requirements.txt
```

Package yang diinstall antara lain:

- `selenium`
- `webdriver-manager`
- `pandas`
- `openpyxl`
- `jupyter`
- `requests`
- `beautifulsoup4`

---

### 4. Jalankan Chrome dengan Remote Debugging Mode

Script ini terhubung ke Chrome yang sudah terbuka agar tidak perlu login ulang dan bisa melewati verifikasi auth/CAPTCHA secara manual.

1. Tutup semua jendela Google Chrome yang sedang terbuka (disarankan).
2. Buka **Command Prompt (CMD)** atau **PowerShell**, lalu jalankan perintah berikut:

```cmd
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir="C:\selenium\chrome_profile"
```

*(Catatan: Sesuaikan path `chrome.exe` jika Google Chrome Anda terpasang di lokasi berbeda).*

---

### 5. Persiapan Browser & Tab SQL Lab

Di jendela Chrome khusus yang terbuka dari Langkah 4:

1. Pastikan **VPN BPS** sudah **ON/Aktif**.
2. Akses URL: `https://fasih-dashboard.bps.go.id/superset/sqllab/`
3. Login menggunakan akun FASIH BPS Anda.
4. Pilih **Database** dan **Schema** yang sesuai (misalnya schema `tgr_fd68e454`).
5. Pastikan tab SQL Lab Editor berada di posisi siap dijalankan.

---

### 6. Jalankan Script Automasi

Buka terminal di folder project (pastikan `venv` aktif), lalu jalankan:

```powershell
python fasih-sql-usaha.py
```

Script akan otomatis:

- Menghubungkan Selenium ke browser Chrome di port `9222`.
- Membaca checkpoint terakhir di `output/checkpoint_ui.json`.
- Memproses setiap kecamatan secara berurutan.
- Mengunduh file Excel dan menyimpannya di folder `output/final/`.

---

## Struktur Folder Output

```text
fasih-dashboard-sql/
├── fasih-sql-usaha.py     # Script utama automasi scraping
├── requirements.txt       # Daftar library python
├── README.md              # Dokumentasi petunjuk penggunaan
├── .gitignore             # File penanda abaikan file venv & temporary
└── output/
    ├── downloads/         # Tempat sementara penampungan download browser
    ├── final/             # Tempat hasil file Excel yang sudah di-rename
    └── checkpoint_ui.json # File simpan status kecamatan & offset
```

---

## Fitur Unggulan

- **Checkpoint & Auto-Resume**: Jika koneksi terputus, VPN mati, atau script dihentikan `Ctrl+C`, cukup jalankan ulang `python fasih-sql-usaha.py`. Script akan otomatis melanjutkan dari kecamatan / offset terakhir.
- **Human-like Delay & Anti-Ban**: Menggunakan jeda acak (*sleep random*) antar query dan istirahat panjang (*long break*) secara berkala untuk menjaga stabilitas traffic server Superset.
- **Chrome Attach Mode**: Menggunakan Chrome Remote Debugging port 9222 sehingga Anda bebas melakukan login manual dan mengatur session tanpa kendala cookie/session expired.

---

## Cara Upload Project ini ke GitHub

Jika Anda ingin mengunggah repository ini ke akun GitHub Anda untuk pertama kali:

1. **Inisialisasi Git & Commit Pertama:**

   ```bash
   git init
   git branch -M main
   git add .
   git commit -m "feat: initial commit fasih dashboard sql automation script"
   ```

2. **Buat Repository Baru di GitHub:**
   - Buka [GitHub New Repository](https://github.com/new).
   - Beri nama repository (misal: `fasih-dashboard-sql`).
   - Pilih Public atau Private.
   - **Jangan** centang "Add a README file" (karena kita sudah buat README di lokal).
   - Klik **Create repository**.

3. **Hubungkan & Push ke GitHub:**

   ```bash
   git remote add origin https://github.com/<username_lu>/fasih-dashboard-sql.git
   git push -u origin main
   ```

---

## Troubleshooting & Catatan

1. **Error `Could not connect to Chrome` / `Debugger address 127.0.0.1:9222`:**
   - Pastikan Chrome dijalankan dengan parameter `--remote-debugging-port=9222`.
   - Cek apakah ada proses Chrome hanging di Task Manager, lalu coba tutup dan jalankan kembali command Chrome di langkah 4.

2. **Tombol Run / Editor Tidak Ditemukan:**
   - Jika UI Superset FASIH mengalami update tampilan, sesuaikan CSS Selector / XPath pada dictionary `SELECTORS` di dalam file `fasih-sql-usaha.py`.

---

## Lisensi & Disclaimer

Project ini dibuat khusus untuk keperluan internal scraping dan pengolahan data kegiatan **BPS / SE2026**. Gunakan script ini dengan bijak dan selalu patuhi ketentuan penggunaan infrastructure BPS.
