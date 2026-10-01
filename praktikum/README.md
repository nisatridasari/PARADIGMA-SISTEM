# Arsitektur Monolith vs Microservices dengan Flask Python

Langkah kerja praktikum membandingkan arsitektur **Monolith** (satu proses, port 5000) dan **Microservices** (Book Service port 5001 dan Order Service port 5002) menggunakan Flask Python.

## Struktur Project

```text
monolith-vs-microservices/
├── venv/
├── monolith/
│   └── monolith_app.py
└── microservices/
    ├── book_service.py
    └── order_service.py
```

## Langkah Kerja

### 1. Membuat Struktur Folder Project

Buka Git Bash, lalu buat folder proyek utama dan dua subfolder.

```bash
mkdir monolith-vs-microservices
cd monolith-vs-microservices
mkdir monolith microservices
```

![Pembuatan Folder Project](img/Pembuatan%20Folder%20Project.jpeg)

### 2. Membuat Virtual Environment

Buat virtual environment pada folder induk proyek.

```bash
python -m venv venv
```

![Pembuatan Virtual Environment](img/Pembuatan%20Virtual%20Environment.jpeg)

### 3. Mengaktifkan Virtual Environment

Aktifkan virtual environment di Git Bash. Jika berhasil, prompt terminal diawali tanda `(venv)`.

```bash
source venv/Scripts/activate
```

![Aktivasi Virtual Environment](img/Aktivasi%20Virtual%20Environment.jpeg)

### 4. Instalasi Flask dan requests

Pasang Flask dan requests di dalam virtual environment.

```bash
pip install Flask requests
```

![Instalasi Flask dan requests](img/Instalasi%20Flask%20dan%20requests.jpeg)
### 5. Membuat dan Menjalankan Aplikasi Monolith

Buat file `monolith_app.py` di dalam folder `monolith` (fitur Buku dan Pesanan dalam satu file, data disimpan in-memory), lalu jalankan dari folder `monolith`. Aplikasi berjalan di `http://127.0.0.1:5000`.

```bash
cd monolith
python monolith_app.py
```

![Menjalankan Aplikasi Monolith](img/Menjalankan%20Aplikasi%20Monolith.jpeg)

### 6. Pengujian Endpoint Aplikasi Monolith

Buka terminal baru, lalu uji endpoint dengan `curl`.

```bash
curl http://localhost:5000/books
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5000/orders
```

![Pengujian GET books pada Monolith](img/Pengujian%20GET%20books%20pada%20Monolith.jpeg)

### 7. Membuat Layanan Microservices

Hentikan aplikasi monolith dengan `Ctrl+C` agar port dibebaskan. Lalu buat dua file di folder `microservices`:

- `book_service.py`: menangani fitur Buku, berjalan di port **5001**.
- `order_service.py`: menangani fitur Pesanan dan memanggil Book Service lewat HTTP request (library `requests`), berjalan di port **5002**.

### 8. Menjalankan Kedua Layanan

Jalankan kedua layanan pada dua jendela Git Bash yang berbeda.

Terminal 1 - Book Service:

```bash
cd microservices
python book_service.py
```

![Menjalankan Book Service](img/Menjalankan%20Book%20Service.jpeg)

Terminal 2 - Order Service (venv diaktifkan ulang):

```bash
source venv/Scripts/activate
cd microservices
python order_service.py
```

![Menjalankan Order Service](img/Menjalankan%20Order%20Service.jpeg)

### 9. Pengujian Komunikasi Antar Layanan

Buka terminal ketiga, lalu uji dengan `curl`.

```bash
curl http://localhost:5001/books/1
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5002/orders
```

![Pengujian GET books1 pada Book Service](img/Pengujian%20GET%20books1%20pada%20Book%20Service.jpeg)

### 10. Eksperimen Fault Isolation

Hentikan Book Service dengan `Ctrl+C`, lalu kirim kembali permintaan pesanan ke Order Service.

```bash
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5002/orders
```

Order Service tetap berjalan dan merespons dengan pesan error `Book Service sedang down!`.

![Hasil Eksperimen Fault Isolation](img/Hasil%20Eksperimen%20Fault%20Isolation.jpeg)
