# 🏗️ Arsitektur Sistem Skintify: Frontend vs Backend

Meskipun Skintify dibangun menggunakan kerangka kerja (framework) **NiceGUI** yang berbasis Python (di mana Frontend dan Backend ditulis dalam satu bahasa), secara konsep *Software Engineering*, struktur folder kita sangat mematuhi prinsip **Separation of Concerns** (Pemisahan Tugas).

Berikut adalah validasi dan penjelasan lengkap mengenai mana yang bertindak sebagai Backend dan mana yang bertindak sebagai Frontend:

---

## 🧠 BACKEND (Logika Sistem, Mesin, & Data)
Backend adalah bagian aplikasi yang bekerja "di balik layar". Ia tidak terlihat oleh pengguna, namun bertugas mengatur data, melakukan kalkulasi berat (AI/Scraping), dan mengamankan aplikasi.

Pemahamanmu sudah **100% Benar**. Berikut adalah folder-folder yang masuk kategori Backend:

*   **`app/database/` (Sistem ORM & Data Manager)**
    Ini adalah jantung penyimpanan data. File seperti `engine.py` mengatur koneksi ke SQLite, `models.py` mengatur tabel database menggunakan ORM (Object-Relational Mapping), dan `database_manager.py` mengatur CRUD (Create, Read, Update, Delete).
*   **`app/scraping/` (Mesin Penambang Data)**
    Berisi skrip (*web scraper*) yang berjalan di *background* untuk menyedot data dari Tokopedia, Shopee, Lazada, dan Sociolla, lalu membersihkannya.
*   **`app/services/` (Layanan Inti / Logika Bisnis)**
    *   `analyzer.py`: Otak AI dan pencocokan komposisi bahan skincare.
    *   `routine_service.py`: Logika untuk mengatur jadwal rutinitas pengguna.
    *   `weather.py`: Menarik data cuaca langsung dari satelit API.
*   **`app/auth/` (Sistem Keamanan)**
    Menangani pengiriman email OTP (`email_service.py`) dan sesi login pengguna.
*   **`scripts/` (Skrip Operasional / DevOps)**
    Alat bantu untuk *Developer*, seperti membuat akun admin (`create_admin.py`), mengurus *migration* database, dan merapikan data (`data_ops`).
*   **`main.py`**
    File ini adalah jembatan yang menyalakan server Backend (FastAPI/Uvicorn) yang mendasari NiceGUI.

---

## 🎨 FRONTEND (Antarmuka Pengguna / UI)
Frontend adalah segala sesuatu yang **dilihat, diklik, dan diinteraksikan** oleh pengguna di layar *browser* atau aplikasi mereka.

Lagi-lagi kamu **100% Benar**. Frontend Skintify tersentralisasi dan diisolasi dengan sangat rapi hanya di dalam satu pintu, yaitu folder **`app/ui/`**.

*   **`app/ui/pages/` (Halaman Web Utama)**
    Ini adalah kumpulan halaman yang dirakit oleh timmu. Contohnya: `login_page.py`, `home_page.py`, `wishlist_page.py`, `compare_page.py`, `onboarding_page.py`, dll. Setiap file ini hanya fokus mengatur tampilan (warna, letak tombol, teks).
*   **`app/ui/components.py` & Modal**
    Berisi "Lego" atau komponen UI yang bisa dipakai berulang-ulang, seperti *Navbar* (menu atas), *Sidebar*, dan *Product Card*. 
*   **`app/ui/style/` (Aset Visual & Klien)**
    Berisi file `style.css` untuk mempercantik warna/tata letak, gambar-gambar logo/ilustrasi, dan sesekali sedikit injeksi JavaScript lokal untuk animasi yang tidak perlu melibatkan server Backend.

---

### 💡 Kesimpulan untuk Dosen Penguji
Jika dosen menanyakan arsitektur kalian besok, kamu bisa menjelaskan dengan bangga:

> *"Kami menggunakan **Monolithic Architecture dengan Logical Separation**, Pak. Walaupun keseluruhan kode ditulis di Python, kami secara disiplin memisahkan folder `app/ui` yang sepenuhnya bersifat Frontend Presentation Layer (Dikerjakan oleh Divisi UI/UX: Falisha, Syaqila, Najla). Sedangkan folder seperti `database`, `services`, `scraping`, dan `scripts` sepenuhnya bertindak sebagai Backend Logic & Data Layer (Diarsiteki oleh Syahid). Pemisahan ini membuat kode kami sangat rapi dan bebas dari Spaghetti Code."*
