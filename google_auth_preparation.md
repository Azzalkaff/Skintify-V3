# 🔐 Persiapan Implementasi Google OAuth (SSO) di Skintify

Dokumen ini adalah panduan konseptual (*blueprint*) mengenai apa saja yang harus dipersiapkan dan dimodifikasi jika tim Skintify ingin mengimplementasikan fitur **"Login with Google"** di masa depan. 

> [!NOTE]
> Sesuai instruksi, dokumen ini hanya bersifat persiapan (Preparation). Tidak ada kode sistem yang benar-benar diubah.

---

## 1. Persiapan Kredensial (Google Cloud Console)
Sebelum menyentuh kode, Anda harus membuat proyek di Google Cloud Platform (GCP).
1. Buat **OAuth 2.0 Client IDs**.
2. Atur *Authorized redirect URIs* (misal: `http://localhost:8080/auth/google/callback`).
3. Dapatkan `CLIENT_ID` dan `CLIENT_SECRET`.

> [!CAUTION]
> Nilai kredensial di atas bersifat sangat rahasia. Kredensial ini HARUS dimasukkan ke dalam file `.env` dan didaftarkan ke `.env.example`.

### Perubahan File: `.env`
```diff
 EMAIL_PENGIRIM=syahid@gmail.com
 PASSWORD_APLIKASI=xxxx
+GOOGLE_CLIENT_ID=isi_dengan_client_id_google
+GOOGLE_CLIENT_SECRET=isi_dengan_client_secret_google
```

---

## 2. Penambahan Dependensi (Library)
Untuk menangani pertukaran token dengan Google, aplikasi memerlukan pustaka tambahan untuk FastAPI.

> [!TIP]
> Kami merekomendasikan menggunakan `Authlib` karena sangat stabil dan terintegrasi baik dengan FastAPI/Starlette.

### Perintah Terminal:
```bash
pip install authlib httpx
```

---

## 3. Modifikasi Model Database
Saat ini, model `User` mewajibkan `password` untuk Autentikasi lokal. Pengguna yang masuk menggunakan Google tidak memiliki *password*.

### [MODIFY] [app/database/models.py](file:///c:/Pemrograman/Kuliah/PPLD/Pra%20ETS/Proyek%20Punya%20Kelompok/main%20program/Skintify-C4/Skintify-C4/app/database/models.py)
Kita perlu mengubah kolom `password` agar boleh kosong (*nullable*), dan menambahkan kolom penanda Google.

```diff
 class User(Base):
     __tablename__ = 'users'
     id = Column(Integer, primary_key=True)
     email = Column(String, unique=True, index=True)
     username = Column(String)
-    password = Column(String) # Wajib isi
+    password = Column(String, nullable=True) # Boleh kosong untuk user Google
+    auth_provider = Column(String, default="local") # "local" atau "google"
+    google_id = Column(String, nullable=True)
```

---

## 4. Modifikasi Layanan Autentikasi
Kita harus menambahkan dua *endpoint* (rute URL) baru yang akan diproses oleh server di belakang layar (*Backend*).

### [MODIFY] [app/auth/auth.py](file:///c:/Pemrograman/Kuliah/PPLD/Pra%20ETS/Proyek%20Punya%20Kelompok/main%20program/Skintify-C4/Skintify-C4/app/auth/auth.py)
Menambahkan logika untuk melempar pengguna ke halaman Google, dan menerima kembalian data (*Callback*) dari Google.

```python
from authlib.integrations.starlette_client import OAuth

# Inisialisasi OAuth
oauth = OAuth()
oauth.register(
    name='google',
    client_id=os.getenv("GOOGLE_CLIENT_ID"),
    client_secret=os.getenv("GOOGLE_CLIENT_SECRET"),
    server_metadata_url='https://accounts.google.com/.well-known/openid-configuration',
    client_kwargs={'scope': 'openid email profile'}
)

class AuthManager:
    # ... kode yang sudah ada ...

    @staticmethod
    async def login_google(request):
        """Melempar pengguna ke halaman login Google."""
        redirect_uri = request.url_for('auth_google_callback')
        return await oauth.google.authorize_redirect(request, redirect_uri)

    @staticmethod
    async def auth_google_callback(request):
        """Menerima token dari Google dan mendaftarkan user ke Database Skintify."""
        token = await oauth.google.authorize_access_token(request)
        user_info = token.get('userinfo')
        # Logika menyimpan user_info['email'] ke tabel User...
```

---

## 5. Modifikasi Antarmuka (UI)
Terakhir, kita harus menambahkan tombol visual di halaman login agar pengguna bisa mengkliknya.

### [MODIFY] [app/ui/pages/login_page.py](file:///c:/Pemrograman/Kuliah/PPLD/Pra%20ETS/Proyek%20Punya%20Kelompok/main%20program/Skintify-C4/Skintify-C4/app/ui/pages/login_page.py)
Tambahkan tombol "Masuk dengan Google" di bawah form login utama.

```diff
                 if state["mode"] == "login":
                     ui.button('Masuk Aplikasi', on_click=proses_login) \
                         .classes('w-full btn-primary text-white rounded-xl py-3 shadow-lg')
                 else:
                     ui.button('Daftar & Kirim OTP', on_click=proses_daftar) \
                         .classes('w-full btn-primary text-white rounded-xl py-3 shadow-lg')

+                # --- GOOGLE OAUTH BUTTON ---
+                ui.separator().classes('my-4')
+                ui.button('Masuk dengan Google', icon='img:/static/google_icon.png', on_click=lambda: ui.navigate.to('/login/google')) \
+                    .classes('w-full bg-white text-gray-700 border border-gray-300 rounded-xl py-3 shadow-sm hover:bg-gray-50')
```

---

## 📋 Kesimpulan Skala Kesulitan
Mengimplementasikan Google Auth di arsitektur Skintify saat ini tergolong **Mudah hingga Menengah**.
Karena arsitektur Anda sudah sangat rapi (*Modular*), penambahan Google Auth tidak akan merusak kode yang ada. Anda hanya perlu "menyelipkan" metode verifikasi baru di dalam `AuthManager` tanpa harus merombak halaman utama.
