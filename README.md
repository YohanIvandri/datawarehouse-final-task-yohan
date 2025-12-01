# 📦 Data Warehouse Final Task – Yohan

Proyek ini merupakan final task untuk membangun end-to-end Data Warehouse yang meliputi:
- Pengolahan data sumber (CSV/XLSX)
- Proses ETL dengan Python
- Pembuatan staging, warehouse, dan data mart dengan SQL
- Workflow automation menggunakan Apache Airflow
- Containerization menggunakan Docker agar deployment lebih konsisten dan portable

Struktur repository dibuat modular agar mudah dipahami dan direplikasi.

## 📁 Project Folder Overview

- **AIRFLOW/**  
  Berisi DAG dan script pendukung yang digunakan untuk mengatur workflow ETL di Apache Airflow.

- **DATA SOURCE/**  
  Menyimpan file sumber mentah yang digunakan sebagai input ETL.

- **PYTHON SCRIPT/**  
  Berisi script Python untuk proses Extract, Transform, dan Load menuju Data Warehouse.

- **SQL/**  
  Berisi SQL script untuk membuat tabel staging, warehouse, data mart, serta stored procedure.

## 🐳 Docker Implementation

Proyek ini menggunakan **Docker** agar lingkungan pengembangan dan eksekusi pipeline menjadi konsisten, serta mempermudah setup tanpa harus menginstal dependency secara manual.

### **Apa yang dicontainerize?**
1. **Apache Airflow**  
   Airflow dijalankan dalam beberapa container:
   - webserver
   - scheduler
   - postgres (metadata database)
   - airflow-init

2. **Python ETL Environment**  
   Semua script ETL berjalan di dalam container Airflow sehingga dependency Python terisolasi dari OS lokal.

### **File Docker yang Digunakan**
- **docker-compose.yml**  
  Mengatur service Airflow (scheduler, webserver, worker, postgres).
  
## 📊 Data Pipeline Flow 

Sumber data pipeline berasal dari dua jenis data:

1. **Database `db.sample`**  
   → Diasumsikan sebagai database transaksi utama.  

2. **File CSV dan Excel**  
   → Sumber data pendukung yang turut diproses.

Flow ETL dibangun menggunakan Python dan dijalankan melalui Airflow.  
Alur lengkapnya sebagai berikut:

1. **Extract (Python → Airflow)**  
   - Mengambil data dari database `db.sample`.  
   - Membaca file CSV dan Excel dari folder `DATA SOURCE/`.

2. **Load ke STAGING**  
   - Data hasil extract dimuat ke tabel staging.  
   - Tahap ini hanya menyimpan data mentah yang sudah dibersihkan.

3. **Transform & Load ke CORE (Warehouse)**  
   - Python script memproses staging → core.  
   - Pada tahap ini, data dibentuk menjadi struktur dimension dan fact.

4. **Load ke MART**  
   - Data mart hanya berisi 2 tabel utama:
     - `mart.CustomerMart`
     - `mart.DailySummaryMart`
   - Tabel mart bersifat final dan siap dipakai untuk laporan.

5. **Stored Procedure (SP)**  
   - SP hanya mengambil data dari mart.  
   - Tidak melakukan join atau transformasi berat karena semua proses telah dilakukan sebelumnya.  

  

