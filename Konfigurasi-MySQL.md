# KONFIGURASI MYSQL

# LATAR BELAKANG PEMBAHASAN
MySQL dan MariaDB adalah sistem manajemen basis data relasional (RDBMS) yang populer digunakan dalam berbagai aplikasi, mulai dari website hingga sistem informasi skala besar. Dalam pengelolaan data, pemahaman tentang instalasi dan konfigurasi server database menjadi hal yang penting untuk memastikan kinerja, keamanan, dan efisiensi sistem. Selain itu, perbedaan antara database relasional dan non-relasional serta antara MySQL dan MariaDB perlu dipahami agar dapat memilih teknologi yang sesuai dengan kebutuhan.

---

# PROBLEM YANG DIANGKAT
- 1 Bagaimana langkah-langkah yang benar dalam menginstal dan mengonfigurasi server database MySQL atau MariaDB?
- 2 Bagaimana cara mengelola user dan hak akses dalam MySQL atau MariaDB untuk memastikan keamanan data?
- 3 Bagaimana cara melakukan konfigurasi dasar MySQL atau MariaDB untuk meningkatkan performa dan efisiensi sistem?
- 4 Bagaimana cara membuat dan mengelola database?

---

# SOLUSI/SKENARIO AKTIVITAS 
1. Jelaskan tentang database relational, database unrelational dan berikan contoh produknya masing-masing 3.
   
**> 1. Database Relational**

Database relational menyimpan data dalam bentuk tabel yang saling berelasi melalui primary key dan foreign key. Data diakses menggunakan SQL (Structured Query Language).

**Contoh produk:**
- MySQL
- PostgreSQL
- Microsoft SQL Server

**> 2. Database Unrelational**

Database unrelational (NoSQL) menyimpan data tanpa struktur tabel tetap, cocok untuk big data dan aplikasi real-time. Tipe penyimpanan bisa berbasis dokumen, keyvalue, column-family, atau graph.

**Contoh produk:**
- MongoDB (berbasis dokumen)
- Redis (key-value store)
- Cassandra (column-family)

---
  
2. Jelaskan kapan harus menggunakan database relational dan kapan harus menggunakan database unrelational

**> Database Relasional (SQL)**
- Data terstruktur dengan jelas (misalnya tabel dengan kolom tetap)
- Ada hubungan antar data yang kompleks (relasi antar tabel)
- Butuh transaksi yang aman dan konsisten (ACID, seperti pada perbankan)
- Perlu query yang kuat dan fleksibel (misalnya dengan SQL)

**> Database Non-Relasional (Nosql)**
- Data tidak terstruktur atau sering berubah (misalnya JSON, dokumen, graf)
- Butuh kecepatan tinggi dan skalabilitas besar (misalnya media sosial, big data)
- Tidak terlalu membutuhkan transaksi yang ketat
- Harus menangani data dalam jumlah sangat besar dengan cepat

---

3. Lakukan instalasi database mysql dari awal sampai akhir.
4. Pastikan dalam setiap tahapan instalasi, didokumentasikan dalam bentuk screenshot untuk bahan Menyusun laporan

- Buka website MySQL.com
![image](https://github.com/user-attachments/assets/9442bba5-5537-4f22-b25f-a4555cf64a43)

- Pilih navbar “Downloads” pada MySQL
![image](https://github.com/user-attachments/assets/c9b693e1-1211-434e-9b54-802fa034880f)

- Scroll kebawah, klik link “MySQL Community (GPL) Downloads >>” 
![image](https://github.com/user-attachments/assets/bd7e413f-ae5a-449d-a08f-b8e059df5f02)

-  Klik link “MySQL Installer for Windows”
![image](https://github.com/user-attachments/assets/72f11d99-9373-4439-a349-3909fec4b20c)

- Klik tanda “Download” pada “Windows (x86, 32-bit), MSI Installer”, untuk version apk menyesuaikan versi terbaru
![image](https://github.com/user-attachments/assets/72951a5b-6b19-47bc-8740-155f8e0bd74f)

- Klik link "No thanks, just start my download"
![image](https://github.com/user-attachments/assets/4c27abe9-c5a7-4dc0-be9d-720de1ec822e)

- Secara otomatis akan langsung terdownload dan berada di Folder "Download"
![image](https://github.com/user-attachments/assets/c3a9b9e8-cca0-4149-ac8b-17f45bf639c7)




---

5. Lakukan perubahan perubahan pada variable berikut (dokumentasikan before dan after nya)
- a. port dari default 3306 menjadi 3309.
- b. innodb_buffer_pool_size dr default 16M (menjadi 25% dari RAM )

---

7. lakukan perubahan terhadap password root.

---

8. Buat database dengan nama: kelompok_AB_nama_mhs

---

9. Semua Langkah-langkah konfigurasi diatas, lakukan dengan command prompt.

---

# PEMBAHASAN

# KESIMPULAN SECARA KESELURUHAN 

# REFERENSI
