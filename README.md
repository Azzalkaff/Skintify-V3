# Skintify 🌟
*Smart Skincare Recommendation & Analysis System*

Skintify adalah aplikasi cerdas berbasis web (menggunakan arsitektur Python *Asynchronous*) yang dirancang untuk membantu pengguna menemukan produk perawatan kulit terbaik. Aplikasi ini mampu mencocokkan bahan yang aman (*Ingredient Match*), memberikan rekomendasi personal, dan menganalisis tren produk terkini.

## 🚀 Fitur Utama
1. **Scraping Terintegrasi**: Mengumpulkan data produk secara otomatis dan real-time dari marketplace terkemuka.
2. **Smart Conflict Engine**: Menganalisis ketidakcocokan antar bahan kimia produk untuk mencegah iritasi kulit.
3. **Personalized Routine**: Fitur *onboarding* yang menyesuaikan rutinitas perawatan dengan profil wajah pengguna secara dinamis.
4. **AI Chat Assistant**: Asisten pintar yang dilatih khusus untuk menjadi dermatolog pribadi Anda.
5. **Executable Ready**: Dilengkapi dengan skrip otomatisator untuk mengemas aplikasi web menjadi file Desktop Windows (.exe).

## 🛠️ Panduan Instalasi (Cara Menjalankan)

### Prasyarat
Pastikan komputer Anda sudah terinstal **Python 3.9** atau versi yang lebih baru.

### Langkah-langkah
1. **Unduh (Clone) Repositori**
   ```bash
   git clone https://github.com/Azzalkaff/Skintify-V3.git
   cd Skintify-V3
   ```

2. **Buat Virtual Environment (Sangat Disarankan)**
   Buat lingkungan virtual agar library tidak bentrok dengan Python di komputermu:
   ```bash
   python -m venv venv
   ```
   Lalu aktifkan (Windows):
   ```bash
   venv\Scripts\activate
   ```
   Atau aktifkan (Mac/Linux):
   ```bash
   source venv/bin/activate
   ```

3. **Install Library yang Dibutuhkan**
   Jalankan perintah ini di Terminal untuk mengunduh semua perlengkapan:
   ```bash
   pip install -r requirements.txt
   ```

4. **Nyalakan Mesin Aplikasi**
   Gunakan perintah ini untuk menghidupkan server lokal:
   ```bash
   python main.py
   ```
   *Aplikasi akan terbuka secara otomatis di browser Anda pada alamat `http://localhost:8081`.*

## 📂 Struktur Utama Repositori
*   `app/` : Jantung logika aplikasi (Berisi komponen UI, Mesin Scraper, Pengatur Database, dan Layanan AI).
*   `data/` : Ruang penyimpanan database lokal dan *cache* referensi produk.
*   `main.py` : Titik masuk (*Entry Point*) untuk menyalakan sistem.
*   `cli.py` : Versi antarmuka Command-Line (Terminal) dari aplikasi.
*   `build_exe.py` : Modul CI/CD sederhana untuk membangun file `.exe`.

---
*Dibuat dan dirancang dengan penuh dedikasi oleh Tim Skintify (Syaqila, Najla, Falisha, Syahid).*
