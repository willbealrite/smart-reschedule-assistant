
# Smart Reschedule Assistant

**Intelligent Schedule Rescheduling System for Gymnasium**

Smart Reschedule Assistant merupakan aplikasi berbasis web yang dikembangkan untuk membantu pelanggan Gymnasium mengajukan perubahan jadwal latihan dengan lebih mudah dan membantu Admin mengelola permintaan reschedule secara terstruktur.

Aplikasi ini mengintegrasikan teknologi **Full-Stack Web Development** dan **Generative AI** untuk menganalisis pesan pelanggan, mengenali maksud permintaan, mengekstrak preferensi jadwal, serta menghasilkan draft balasan yang dapat ditinjau dan disunting oleh Admin sebelum dikirimkan.

> Proyek Capstone Dicoding — Full-Stack Web Developer
> Kelompok: DB15-G001

---

## 1. Latar Belakang

Perubahan jadwal latihan dapat menjadi proses yang kurang efisien apabila pelanggan harus menyampaikan permintaan secara manual dan Admin perlu memeriksa setiap pesan serta ketersediaan jadwal satu per satu.

Smart Reschedule Assistant dikembangkan sebagai solusi untuk membantu proses tersebut melalui aplikasi web yang mengintegrasikan manajemen jadwal, autentikasi pengguna, pengelolaan permintaan reschedule, dan dukungan analisis berbasis AI.

AI berperan sebagai asisten untuk membantu Admin memahami permintaan pelanggan dan menyiapkan draft komunikasi. Keputusan akhir tetap berada di tangan Admin.

## 2. Tujuan Proyek

- Mempermudah pelanggan mengajukan perubahan jadwal latihan.
- Membantu Admin mengelola dan meninjau permintaan reschedule.
- Menampilkan ketersediaan slot jadwal alternatif.
- Menggunakan AI untuk menganalisis maksud dan preferensi pelanggan.
- Membantu Admin menyusun draft balasan berdasarkan hasil analisis dan keputusan reschedule.

## 3. Fitur Utama

### Customer

- Registrasi dan login akun.
- Melihat jadwal latihan.
- Melihat slot jadwal yang tersedia.
- Mengajukan permintaan perubahan jadwal.
- Menyampaikan alasan perubahan jadwal.
- Memantau status permintaan reschedule.

### Admin

- Login menggunakan akun Admin.
- Melihat dan mengelola permintaan reschedule.
- Meninjau informasi jadwal pelanggan.
- Menyetujui atau menolak permintaan perubahan jadwal.
- Melihat ketersediaan slot alternatif.
- Menggunakan fitur analisis dan pembuatan draft balasan berbasis AI.

### Generative AI

- Mengklasifikasikan intent pesan pelanggan.
- Mengekstrak preferensi tanggal dan waktu.
- Mengidentifikasi permintaan yang membutuhkan peninjauan Admin.
- Membantu menyaring slot berdasarkan preferensi pelanggan.
- Menghasilkan draft balasan berdasarkan konteks permintaan.
- Menggunakan template fallback apabila layanan LLM tidak tersedia atau gagal memenuhi guardrail.

**Catatan:** AI tidak mengambil keputusan persetujuan reschedule secara otomatis. Keputusan akhir tetap dilakukan oleh Admin melalui Backend.

## 4. Arsitektur Sistem

Aplikasi terdiri dari tiga komponen utama:

1. **Frontend:** antarmuka web untuk Customer dan Admin.
2. **Backend:** REST API untuk autentikasi, pengelolaan jadwal, permintaan reschedule, dan integrasi AI.
3. **AI Service:** layanan analisis pesan dan pembuatan draft balasan.

Alur integrasi:

```text
Customer / Admin
       |
       v
React.js Frontend
       |
       v
Express.js Backend
       |
       +------ PostgreSQL Database
       |
       +------ AI Service Client
                    |
                    v
              FastAPI AI Service
                    |
                    +---- Intent Classification
                    |
                    +---- Preference Extraction
                    |
                    +---- Gemini / Template Fallback
```

Backend bertindak sebagai penghubung antara Frontend, Database, dan AI Service. Proses perubahan jadwal tetap dikendalikan oleh Backend melalui mekanisme autentikasi, otorisasi, validasi, dan transaksi database.

## 5. Teknologi yang Digunakan

| Komponen | Teknologi |
|---|---|
| Frontend | React.js |
| Build Tool | Vite |
| Backend | Node.js dan Express.js |
| Database | PostgreSQL |
| Autentikasi | JWT |
| AI Service | Python dan FastAPI |
| Machine Learning | TF-IDF dan Logistic Regression |
| Generative AI | Google Gemini |
| API Integration | REST API |
| Version Control | Git dan GitHub |

## 6. Struktur Direktori

```text
smart-reschedule-assistant/
│
├── src/                    # Frontend React
│
├── backend/                # Backend Express.js
│   ├── src/
│   ├── sql/
│   └── README.md
│
├── ai-service/             # AI Service FastAPI
│
├── integration/
│   └── backend/            # Client integrasi AI Service
│
├── docs/                   # Dokumentasi dan kontrak API
│
├── package.json
└── README.md
```

## 7. Persyaratan Sistem

Sebelum menjalankan aplikasi, pastikan lingkungan pengembangan memiliki:

- Node.js 22.12+ atau 24.
- npm.
- Python 3.12.
- PostgreSQL.
- Git.

Gunakan database lokal khusus untuk proyek ini dan hindari menggunakan database produksi atau data pelanggan sebenarnya.

## 8. Instalasi dan Konfigurasi

Seluruh perintah dijalankan dari root repository, kecuali jika disebutkan lain.

### A. Persiapan Database

Buat database PostgreSQL lokal untuk Smart Reschedule Assistant.

Atur `DATABASE_URL` untuk CLI PostgreSQL dan `DB_*` pada `backend/.env` agar menunjuk ke database yang sama.

Jalankan file SQL secara berurutan:

```bash
for file in backend/sql/00*.sql; do
  psql "$DATABASE_URL" -v ON_ERROR_STOP=1 -f "$file" || break
done
```

### B. Menjalankan Backend

```bash
cd backend

npm ci

cp .env.example .env
```

Isi konfigurasi database dan `JWT_SECRET` di file `.env`.

Jalankan proses seed akun demo:

```bash
node src/scripts/seed-auth-demo.js
```

Kemudian jalankan Backend:

```bash
npm start
```

Backend berjalan pada port `3000`.

### C. Menjalankan AI Service

Buka terminal kedua dari root repository:

```bash
cd ai-service

python -m venv .venv

source .venv/bin/activate

pip install -r requirements-dev.txt

python scripts/download_lexicon.py

cp .env.example .env
```

Isi `GEMINI_API_KEY` pada `ai-service/.env` untuk mengaktifkan integrasi Gemini.

Jika API Key tidak tersedia atau LLM tidak dapat digunakan, layanan dapat menggunakan template fallback.

Jalankan AI Service:

```bash
uvicorn app.main:app --port 8000 --env-file .env
```

Dokumentasi interaktif AI Service tersedia di:

http://localhost:8000/docs

### D. Menjalankan Frontend

Buka terminal ketiga dari root repository:

```bash
npm ci

npm run dev
```

Frontend berjalan menggunakan Vite. Proxy Vite mengarahkan request `/api` ke Backend pada port `3000`.

## 9. Skenario Pengujian Demo

Untuk mencoba alur utama aplikasi, gunakan akun demo yang telah disiapkan melalui proses seeding.

### Customer

1. Login menggunakan akun `budi.santoso.demo@example.test`.
2. Pilih kelas Pilates Demo.
3. Pilih slot alternatif yang tersedia.
4. Ajukan permintaan perubahan jadwal dengan menyertakan alasan.

### Admin

1. Login menggunakan akun `admin.gymnasium.demo@example.test`.
2. Buka permintaan reschedule yang telah dibuat Customer.
3. Klik **Analisis & buat draft**.
4. Tinjau hasil klasifikasi intent, preferensi jadwal, dan rekomendasi slot.
5. Setujui atau tolak permintaan.
6. Jika disetujui, tinjau kembali draft konfirmasi berdasarkan jadwal akhir.
7. Admin dapat mengedit dan menyalin draft balasan.

Untuk demo slot alternatif, setelah proses seeding tambahkan sesi Customer dan slot yang masih akan datang menggunakan SQL yang tersedia pada dokumentasi atau panduan demo proyek.

## 10. Dokumentasi API

Dokumentasi teknis dan kontrak integrasi tersedia pada direktori berikut:

- [AI Service README](ai-service/README.md)
- [AI API Contract](docs/AI_API_CONTRACT.md)
- [Backend Integration](integration/backend/README.md)
- [AI Verification](docs/AI_VERIFICATION.md)

AI Service menyediakan dokumentasi Swagger UI pada `/docs` dan spesifikasi OpenAPI pada `/openapi.json`.

## 11. Tim Pengembang

**Capstone Group: DB15-G001**

| Nama | Peran |
|---|---|
| Yeremia Alfa Giovanni | Full-Stack Web Developer |
| Ringga Restu Vertika | Full-Stack Web Developer |
| Anisa Aulia Alhaqi | Generative AI |
| Nadine Arindy Octavia | Generative AI |

Proyek dikembangkan secara kolaboratif dengan pembagian tanggung jawab antara pengembangan aplikasi Full-Stack dan pengembangan layanan Generative AI.

## 12. Informasi Proyek

**Nama Proyek:** Smart Reschedule Assistant
**Studi Kasus:** Gymnasium
**Program:** Dicoding Full-Stack Web Developer Capstone
**Kelompok:** DB15-G001

Proyek ini menggunakan data sintetis untuk kebutuhan pengembangan dan pengujian. Tidak menggunakan data pelanggan nyata.

---

*Smart Reschedule Assistant — Mengintegrasikan Full-Stack Development dan Generative AI untuk membantu proses pengelolaan perubahan jadwal latihan.*
