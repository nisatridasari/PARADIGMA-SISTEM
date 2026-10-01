LAPORAN PRAKTIKUM PARADIGMA SISTEM
ARSITEKTUR MONOLITH VS MICROSERVICES DENGAN FLASK PYTHON
LANGKAH-LANGKAH KERJA :
1.	Membuat folder proyek utama dan dua subfolder untuk memisahkan implementasi monolith dan microservices, menggunakan Git Bash
	```bash
   mkdir monolith-vs-microservices
   ```
  ```bash
  cd monolith-vs-microservices
  ```
  ```bash
  mkdir monolith microservices
  ```
2. Membuat Virtual Environment
Membuat virtual environment Python pada folder utama proyek.
 ```bash
python -m venv venv
 ```
Kemudian mengaktifkan virtual environment menggunakan Git Bash:
 ```bash
source venv/Scripts/activate
 ```
Jika berhasil, pada terminal akan muncul tanda (venv) yang menunjukkan bahwa virtual environment sudah aktif.

3. Menginstal Dependency
Menginstal Flask dan Requests yang dibutuhkan untuk menjalankan aplikasi:
 ```bash
pip install Flask requests
 ```
4. Implementasi Arsitektur Monolith
Membuat file monolith_app.py di dalam folder monolith. File ini berisi seluruh logika fitur Buku dan Pesanan dalam satu aplikasi.

Menjalankan aplikasi:
 ```bash
cd monolith
 ```
 ```bash
python monolith_app.py
```
Aplikasi berjalan pada:

http://127.0.0.1:5000
Melakukan pengujian endpoint:
 ```bash
curl http://localhost:5000/books
 ```
 ```bash
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5000/orders
 ```
5. Implementasi Arsitektur Microservices
Membuat dua service yang terpisah:

Book Service → berjalan pada port 5001
Order Service → berjalan pada port 5002

Menjalankan Book Service:
 ```bash
cd microservices
 ```
 ```bash
python book_service.py
 ```
Kemudian menjalankan Order Service pada terminal lain:
 ```bash
source venv/Scripts/activate
 ```
 ```bash
cd microservices
 ```
 ```bash
python order_service.py
 ```
Melakukan pengujian komunikasi antar-service:
 ```bash
curl http://localhost:5001/books/1
 ```
 ```bash
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5002/orders
 ```
Order Service akan melakukan HTTP request ke Book Service untuk memeriksa ketersediaan stok sebelum membuat pesanan.

6. Pengujian Fault Isolation
Menghentikan Book Service menggunakan Ctrl+C untuk mensimulasikan kondisi service mengalami gangguan.
Kemudian mengirim kembali request pemesanan:
 ```bash
curl -X POST -H "Content-Type: application/json" -d '{"book_id": 1}' http://localhost:5002/orders
 ```
Order Service tetap berjalan dan memberikan pesan error:
 ```bash
{
  "error": "Book Service sedang down!"
}
 ```
Pengujian ini menunjukkan bahwa kegagalan pada Book Service tidak menyebabkan Order Service ikut berhenti.
