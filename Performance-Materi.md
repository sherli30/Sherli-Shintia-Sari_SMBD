# PERFORMANCE

# LATAR BELAKANG PERMASALAHAN

Dalam pengelolaan basis data, performa eksekusi query menjadi faktor penting yang mempengaruhi efisiensi sistem. Salah satu cara meningkatkan performa adalah dengan penggunaan teknik optimasi seperti indeks, partisi tabel, dan strategi eksekusi query yang efisien. Indeks memungkinkan database untuk mengambil data lebih cepat dibandingkan dengan pemindaian tabel penuh, sementara partisi tabel dapat mengurangi beban pemrosesan dengan membagi data ke dalam segmen yang lebih kecil.

Laporan ini membahas berbagai teknik optimasi performa database, termasuk penggunaan indeks dan partisi tabel, serta analisis terhadap dampaknya terhadap kecepatan eksekusi query. Dengan implementasi yang tepat, optimasi ini dapat meningkatkan efisiensi pengelolaan data dan mengurangi waktu respons sistem, sehingga mendukung kinerja aplikasi yang lebih baik.

----------------------------------------------------------------------------

## PROBLEM YANG DIANGKAT

Berdasarkan masalah utama yang dapat diangkat dalam laporan ini:  

1. Bagaimana proses impor data dalam skala besar ke dalam MySQL?
2. Bagaimana cara menganalisis performa eksekusi query dengan dan tanpa teknik optimasi?
3. Bagaimana pengaruh indeks terhadap kecepatan eksekusi query dalam basis data besar? 
4. Bagaimana penggunaan indeks composite dan foreign key dapat membantu optimalisasi pencarian data?

----------------------------------------------------------------------------

# SOLUSI / SKENARIO AKTIVITAS
**1. Import data**

Lakukan import data dengan Langkah sebagai berikut:

- a.	Download dan extrak dataset employe pada folder
- b.	Masuk konsol dos, sehingga tampil seperti berikut:

**Hasil :**
![image](https://github.com/user-attachments/assets/fc3e6bf5-05f7-4a36-b79c-43251cbb54a7)

**Penjelasan:**

Masuk ke Direktori XAMPP

- Awalnya, pengguna berada di direktori c:\xampp, yang merupakan folder tempat XAMPP terinstal.

**Berpindah ke Folder Dataset**

- 1.	Perintah cd "C:\Users\useri\Downloads\dataset_emp\dataset_full" digunakan untuk masuk ke folder tempat file dataset berada.
- 2.	Setelah menjalankan perintah ini, pengguna sekarang berada di dalam folder dataset_full, siap untuk melakukan langkah berikutnya, seperti mengimpor data ke dalam MySQL.
   
**Berpindah ke Folder Dataset**

- 1.	Perintah cd "C:\Users\useri\Downloads\dataset_emp\dataset_full" digunakan untuk masuk ke folder tempat file dataset berada.
- 2.	Setelah menjalankan perintah ini, pengguna sekarang berada di dalam folder dataset_full, siap untuk melakukan langkah berikutnya, seperti mengimpor data ke dalam MySQL.
   
**Informasi yang Ditampilkan**

- 1.	"Welcome to the MariaDB monitor" → Berarti koneksi ke database berhasil.
- 2.	Versi server MariaDB yang digunakan adalah 10.4.32.
- 3.	Pengguna dapat mulai menjalankan perintah SQL untuk mengelola database.

**Jangan lupa, posisi cursor pada tempat menyimpan file dataset.**

- c.	Masuk ke database mysql
- d.	Lakukan import data dengan menggunakan script berikut:
Source employee.sql seperti berikut:

**Hasil :**
![image](https://github.com/user-attachments/assets/3fbd72c3-7c9d-4b97-8e58-c11a0a58e3c2)

**Penjelasan Proses Import:**

   - File employee.sql dijalankan

Server mulai mengeksekusi skrip SQL yang ada dalam file tersebut.

**Kesimpulan :**

Pengguna berhasil mengimpor database employee ke dalam MySQL/MariaDB. Struktur database telah dibuat dan tabel sudah siap digunakan.

- e.	Tunggu sampai selesai.
- f.	Setelah selesai, maka nanti pada database, akan tampak sebagai berikut:

**Hasil :**
![image](https://github.com/user-attachments/assets/55ebd177-f9f6-481f-910f-29237e6ac1a2)

----------------------------------------------------------------------------

**1.	Jalankan query :**

```
SELECT * FROM employee
```

**Hasil :**
![image](https://github.com/user-attachments/assets/53530713-eb80-426b-8a94-7b59962d0da2)

**Penjelasan:**

Fungsinya untuk mengambil seluruh data dari tabel `employee`, yang berisi informasi karyawan seperti nomor pegawai (`emp_no`), tanggal lahir (`birth_date`), nama depan (`first_name`), nama belakang (`last_name`), jenis kelamin (`gender`), dan tanggal perekrutan (`hire_date`). Pada hasil eksekusi, sistem menampilkan 1.000 baris data dalam waktu **0.006 detik**, dengan waktu eksekusi query **0.001 detik**.

**2.	Jalankan query explain :**
   
```
EXPLAIN SELECT * FROM employee 
WHERE first_name = 'Georgi'
```

**Hasil :**
![image](https://github.com/user-attachments/assets/68242c0c-f5c9-4414-93b8-57d8d1d40d86)

**Penjelasan:**

Fungsinya untuk menganalisis cara MySQL mengeksekusi perintah pencarian data berdasarkan nama depan. Hasilnya menunjukkan bahwa MySQL melakukan **full table scan** (`type: ALL`), karena tidak ada indeks yang digunakan (`possible_keys: NULL`, `key: NULL`). Akibatnya, sistem harus memindai **299.423 baris** untuk menemukan data yang sesuai, yang dapat memperlambat proses pencarian terutama pada tabel berukuran besar. Selain itu, pada kolom `Extra` terdapat keterangan `Using where`, yang berarti MySQL memfilter hasil berdasarkan kondisi dalam klausa `WHERE`. Untuk meningkatkan efisiensi query ini, disarankan untuk membuat **indeks pada kolom `first_name`**, sehingga proses pencarian dapat dilakukan lebih cepat tanpa harus membaca seluruh isi tabel.

**3.	Tambahkan index**

```
ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name)
```

**Hasil :**
![image](https://github.com/user-attachments/assets/7cf969f7-1a76-4de8-b3dd-665f7b2952ce)

**Penjelasan:**

Fungsinya untuk menambahkan indeks komposit (idx_full_name) pada kolom first_name dan last_name dalam tabel employee. Indeks ini akan mempercepat pencarian data berdasarkan kombinasi kedua kolom tersebut, terutama untuk query dengan kondisi WHERE first_name = '...' AND last_name = '...'. Proses eksekusi memakan waktu 1.255 detik, menunjukkan bahwa indeks berhasil dibuat tanpa error atau peringatan.

**4.	Lakukan query exlain lagi untuk query diatas :**
   
```
EXPLAIN SELECT * FROM employee
WHERE first_name = 'Georgi'
AND last_name = 'Bahr'
```

**Hasil :**
![image](https://github.com/user-attachments/assets/a36721aa-31f5-463d-86c8-820c4046c72a)

**Penjelasan:**

Fungsinya untuk menganalisis performa pencarian data setelah penambahan indeks komposit. Sebelumnya, pencarian dilakukan dengan **full table scan**, yang menyebabkan pemrosesan ratusan ribu baris. Setelah menambahkan indeks komposit `idx_full_name` pada kolom `first_name` dan `last_name` menggunakan query `ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);`, MySQL kini dapat menggunakan indeks untuk pencarian yang lebih efisien. Hasil `EXPLAIN` menunjukkan bahwa MySQL menggunakan indeks (`type: ref`), dengan jumlah baris yang diproses turun drastis menjadi **1 baris**, dibandingkan dengan pencarian sebelumnya. Kolom `Extra` menyatakan **Using index condition**, yang berarti MySQL memanfaatkan indeks dalam penyaringan data sebelum mengambilnya dari tabel. 

# Sekarang bisa kita lihat bahwa query yang kita jalankan tidak lagi scaning penuh ketabel, melainkan akses pada index. Ini terlihat pada nilai kolom key dan possible_keys.

----------------------------------------------------------------------------

# LATIHAN SOAL

1. Lakukan semua tahapan di atas, dan jalankan pada database Anda. Tambahkan screenshot hasil dari setiap langkah yang Anda lakukan.

2. Tambahkan kolom nama departemen pada tabel `dept_manager` dan lakukan update terhadap kolom tersebut. Tambahkan screenshot hasilnya.

# JAWABAN
**Query 1: Tambahkan Kolom Baru**
```sql
ALTER TABLE dept_manager ADD COLUMN dept_name VARCHAR(255);
```

**Hasil 1:**
![image](https://github.com/user-attachments/assets/db311d99-7b81-42e8-8dd7-a48c19934405)

**Penjelasan 1:**
Perintah `ALTER TABLE` digunakan untuk menambahkan kolom baru `dept_name` ke dalam tabel `dept_manager`. Kolom ini bertipe data `VARCHAR(255)`, yang berarti dapat menyimpan teks hingga 255 karakter. Operasi ini berhasil tanpa error atau peringatan, dengan waktu eksekusi 0.011 detik.

**Query 2: Perbarui Data dalam Kolom Baru**
```sql
UPDATE dept_manager dm
JOIN department d ON dm.dept_no = d.dept_no
SET dm.dept_name = d.dept_name;
```

**Hasil 2:**
![image](https://github.com/user-attachments/assets/30541f00-7d0d-4960-b886-efdebad7bb28)

**Penjelasan 2:**
Query ini memperbarui kolom `dept_name` dalam tabel `dept_manager` dengan mengisi nilai dari tabel `department`. Prosesnya dilakukan dengan `JOIN` berdasarkan `dept_no`, sehingga setiap manager akan memiliki nama departemen yang sesuai. Sebanyak 24 baris diperbarui dalam 0.016 detik tanpa error.

**Query 3: Tampilkan Hasilnya**
```sql
SELECT * FROM dept_manager;
```

**Hasil 3:**
![image](https://github.com/user-attachments/assets/a1080370-6b48-4484-b4fe-ddfe024072b5)

**Penjelasan 3:**
Query ini digunakan untuk memverifikasi apakah kolom `dept_name` telah berhasil ditambahkan dan diperbarui. Data yang ditampilkan menunjukkan bahwa setiap manager kini memiliki nama departemen yang sesuai dengan `dept_no`.

---

3. Tambahkan kolom nama departemen pada tabel `dept_emp`. Dan lakukan update terhadap kolom tersebut. Tambahkan screenshot hasilnya.

# JAWABAN
**Query 1: Tambahkan Kolom `nama_departemen`**
```sql
ALTER TABLE dept_emp ADD COLUMN nama_departemen VARCHAR(100);
```

**Hasil 1:**
![image](https://github.com/user-attachments/assets/46c5f4f1-165b-4a5e-ab6b-a13e59a9fe4a)

**Penjelasan 1:**
Query ini menambahkan kolom baru bernama `nama_departemen` ke dalam tabel `dept_emp`. Tipe data `VARCHAR(100)` dipilih karena cukup untuk menyimpan nama departemen dengan panjang yang wajar. Proses ini berhasil tanpa error atau peringatan, dengan waktu eksekusi 0.022 detik.

**Query 2: Perbarui Data `nama_departemen`**
```sql
UPDATE dept_emp
JOIN department ON dept_emp.dept_no = department.dept_no
SET dept_emp.nama_departemen = department.dept_name;
```

**Hasil 2:**
![image](https://github.com/user-attachments/assets/53efa3c8-ca91-4816-b2df-764390cf6d09)

**Penjelasan 2:**
Query ini memperbarui kolom `nama_departemen` dengan mengisi nilainya dari tabel `department`, menggunakan `dept_no` sebagai kunci pencocokan. Dengan cara ini, setiap karyawan dalam tabel `dept_emp` akan memiliki informasi nama departemen yang sesuai.

**Query 3: Tampilkan Semua Data**
```sql
SELECT * FROM dept_emp;
SELECT * FROM dept_emp LIMIT 10;
```

**Hasil 3:**
![image](https://github.com/user-attachments/assets/287cf5b8-c635-4065-9903-7494c4626856)

---

4. Buat query untuk menampilkan gaji yang tertinggi pada departemen `d006`. Siapa Namanya?

# JAWABAN
**Query:**
```sql
SELECT e.first_name, e.last_name, s.amount
FROM employee e
JOIN salary s ON e.emp_no = s.emp_no
JOIN dept_emp d ON e.emp_no = d.emp_no
WHERE d.dept_no = 'd006'
ORDER BY s.amount DESC
LIMIT 1;
```

**Hasil:**
![image](https://github.com/user-attachments/assets/56d079f5-006a-42e0-b034-c9aa86a12c2a)

**Penjelasan:**
Query ini mencari karyawan dengan gaji tertinggi di departemen `d006`. Data diambil dengan melakukan `JOIN` antara tabel `employee`, `salary`, dan `dept_emp`, lalu difilter berdasarkan `dept_no` dan diurutkan secara descending berdasarkan jumlah gaji.

---

5. Dari database employee yang sudah diimport, tambahkan kolom umur pada tabel `employee`.

# JAWABAN
**Query 1: Menambahkan Kolom `umur`**
```sql
ALTER TABLE employee ADD COLUMN umur INT;
```

**Penjelasan 1 :**
Untuk menambahkan kolom umur bertipe INT ke dalam tabel employee menggunakan perintah ALTER TABLE.


**Hasil:**
![image](https://github.com/user-attachments/assets/0fd4e2e8-9700-4fa5-8b4e-1b7c75b23594)

**Query 2: Mengupdate Nilai `umur`**
```sql
UPDATE employee SET umur = YEAR(CURDATE()) - YEAR(birth_date);
```

**Penjelasan 2 :**
Untuk memperbarui kolom umur di tabel employee berdasarkan perhitungan selisih tahun antara tanggal saat ini (CURDATE()) dan birth_date.

**Hasil:**
![image](https://github.com/user-attachments/assets/88d2f487-e893-447f-b581-b25aef6d40c5)

---

6. Tambahkan masing-masing jenis index composite dan foreign key index pada tabel `employee`.

# JAWABAN
**Query 1: Tambahkan Composite Index**
```sql
ALTER TABLE employee ADD INDEX idx_nama (first_name, last_name);
```

**Penjelasan 1 :**
Untuk menambahkan indeks idx_nama pada kolom first_name dan last_name di tabel employee. Indeks berhasil ditambahkan tanpa error, tetapi ada 1 peringatan (warning). Waktu eksekusi sekitar 1.101 detik.

**Hasil:**
![image](https://github.com/user-attachments/assets/bfdbc4f0-2f9a-44bf-8c6b-053852163c0f)

**Query 2: Tambahkan Foreign Key Index**
```sql
ALTER TABLE dept_emp ADD CONSTRAINT fk_emp FOREIGN KEY (emp_no) REFERENCES employee(emp_no);
```

**Penjelasan 2 :**
Kolom emp_no di tabel dept_emp kini memiliki foreign key yang merujuk ke kolom emp_no di tabel employee, memastikan integritas data antar tabel.

**Hasil:**
![image](https://github.com/user-attachments/assets/53ce1a12-bede-4407-a010-e2f3d8415760)

---

7. Lakukan pengujian terhadap query berikut, apakah sudah mengakses index atau belum.

# JAWABAN
**Query:**
```sql
EXPLAIN SELECT * FROM employee WHERE first_name = 'Georgi';
```

**Penjelasan :**
Query menggunakan indeks idx_full_name untuk pencarian, dengan panjang kunci 58 dan memfilter sekitar 253 baris, sehingga optimasi indeks digunakan untuk mempercepat pencarian.

**Hasil:**
![image](https://github.com/user-attachments/assets/2d1b26c7-5ea2-4ba1-b195-88c165eeebc6)

---

8. Lakukan pengujian dari query berikut. Apakah ada perbedaan sebelum dan sesudah ditambahkan index.

# JAWABAN
**Query Uji:**
```sql
SELECT * FROM employee WHERE first_name = 'Georgi' AND last_name = 'Bahr';
```

**Penjelasan :**
query SQL pada database yang mencari karyawan dengan nama depan "Georgi" dan nama belakang "Bahr" dalam tabel employee. Hasilnya menampilkan satu baris data dengan informasi berikut:
•	emp_no: 36825
•	birth_date: 13 Oktober 1963
•	first_name: Georgi
•	last_name: Bahr
•	gender: M (Male/Laki-laki)
•	hire_date: 30 Oktober 1988
•	umur: 62 tahun

**Hasil:**
![image](https://github.com/user-attachments/assets/8c659078-26ae-4600-b6a5-6ad8577958cb)

---

**Cara menguji**
•	Jalankan query diatas sebanyak 10x. catet waktunya setiap kali dijalankan
•	Buat index composite yang bersesuaian dengan query diatas.
•	Ambil rata2 sebelum dan sesudah
•	Tulis kesimpulanya.

**Query Index:**
```sql
ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);
```

**Hasil:**
![image](https://github.com/user-attachments/assets/f3bb41bc-1905-437c-9311-512296dc4622)

**Hasil Pengujian:**

| No | Waktu Sebelum | Waktu Sesudah |
|----|--------------|--------------|
| 1  | 0.001 sec    | 0.004 sec    |
| 2  | 0.001 sec    | 0.003 sec    |
| 3  | 0.001 sec    | 0.001 sec    |
| 4  | 0.001 sec    | 0.002 sec    |
| 5  | 0.002 sec    | 0.002 sec    |
| 6  | 0.001 sec    | 0.002 sec    |
| 7  | 0.001 sec    | 0.002 sec    |
| 8  | 0.001 sec    | 0.002 sec    |
| 9  | 0.001 sec    | 0.002 sec    |
| 10 | 0.002 sec    | 0.002 sec    |
| **Rata-rata** | **0.0011 sec** | **0.0022 sec** |

---

**Kesimpulan Pengujian Index pada Query SQL**
- Data menunjukkan rata-rata waktu eksekusi sebelum indeks (0.0011 detik) memang lebih cepat dibandingkan setelah indeks (0.0022 detik)
- Kesimpulan bahwa penambahan indeks justru membuat query sedikit lebih lambat adalah akurat berdasarkan hasil pengujian yang dilakukan

**Analisis Penyebab**
•	Ukuran tabel masih kecil → Jika tabel employee belum memiliki banyak data, MySQL dapat dengan cepat mencari data tanpa perlu menggunakan indeks. Indeks lebih efektif jika tabel berisi jutaan baris data.
•	MySQL Query Optimizer → MySQL mungkin sudah cukup efisien dalam mencari data tanpa indeks dalam kasus ini, sehingga menambahkan indeks justru menambah sedikit overhead dalam pencarian.
•	Overhead dari indeks → Saat menambahkan indeks baru, MySQL perlu mengelola struktur tambahan, yang dapat menyebabkan sedikit penurunan performa pada tabel yang kecil.

**Kesimpulan Akhir Pengujian**
- Kesimpulan bahwa indeks tidak selalu meningkatkan performa pada tabel kecil sesuai dengan hasil yang diperoleh
- Pernyataan bahwa indeks lebih berguna untuk tabel besar juga benar dan merupakan prinsip dasar dalam optimasi database
- Saran untuk menggunakan EXPLAIN juga tepat sebagai praktik terbaik dalam analisis query
- Semua kesimpulan tersebut sudah sejalan dengan data yang ditampilkan dalam tabel hasil pengujian dan mencerminkan pemahaman yang benar tentang perilaku indeks dalam database MySQL/MariaDB.

----------------------------------------------------------------------------
## **PEMBAHASAN**

1. **Penambahan Kolom**

   - Kolom `dept_name` ditambahkan pada tabel `dept_manager` dan `dept_emp` untuk mencantumkan nama departemen.  
   - Kolom `umur` ditambahkan pada tabel `employee` untuk menyimpan usia karyawan, yang dihitung berdasarkan selisih antara tahun saat ini dan tahun lahir.  

3. **Update Data**
   
   - Kolom `dept_name` dan `nama_departemen` diisi dengan data dari tabel `department` menggunakan operasi `JOIN`.  
   - Kolom `umur` diperbarui dengan selisih tahun antara tanggal lahir (`birth_date`) dan tanggal saat ini (`CURDATE()`).  

5. **Pembuatan dan Pengujian Indeks**
   
   - **Composite Index** ditambahkan pada tabel `employee` dengan indeks `idx_full_name` untuk kolom `first_name` dan `last_name`.  
   - **Foreign Key Index** dibuat di tabel `dept_emp`, menghubungkan `emp_no` ke tabel `employee`.  
   - **Pengujian Query dengan EXPLAIN** menunjukkan bahwa indeks digunakan dalam pencarian karyawan berdasarkan nama depan dan belakang.  

7. **Uji Performa Query**
   
   - Query pencarian data karyawan sebelum dan sesudah indeks dibuat dibandingkan.  
   - Hasilnya menunjukkan bahwa rata-rata waktu eksekusi sebelum indeks (0.0011 detik) lebih cepat dibandingkan setelah indeks (0.0022 detik), karena tabel masih kecil dan overhead dari indeks menambah sedikit waktu pencarian.  

----------------------------------------------------------------------------
# KESIMPULAN SECARA KESELURUHAN

1. **Penggunaan Indeks dalam Database:**
   - Indeks mempercepat pencarian data dengan mengurangi jumlah baris yang perlu diproses, terutama pada tabel berukuran besar
   - Tanpa indeks, MySQL melakukan full table scan yang memproses seluruh isi tabel
   - Indeks komposit pada kolom first_name dan last_name meningkatkan efisiensi untuk pencarian berdasarkan kedua kolom tersebut
   - Pada tabel kecil, penambahan indeks tidak selalu meningkatkan performa dan kadang justru menambah overhead

2. **Manipulasi Struktur Tabel:**
   - Kolom baru dapat ditambahkan menggunakan perintah ALTER TABLE
   - Relasi antar tabel dapat dibuat dengan JOIN dan foreign key constraint untuk menjaga integritas data
   - Operasi UPDATE dengan JOIN memungkinkan pembaruan data berdasarkan nilai dari tabel lain

3. **Analisis Performa Query:**
   - Perintah EXPLAIN berguna untuk menganalisis cara MySQL mengeksekusi query
   - Perbedaan antara full table scan (type: ALL) dan penggunaan indeks (type: ref) signifikan dalam jumlah baris yang diproses
   - Pengoptimalan query penting untuk meningkatkan kinerja database, terutama saat data bertambah besar

4. **Pengujian Empiris:**
   - Pengujian performa query sebelum dan sesudah penambahan indeks penting untuk mengevaluasi efektivitas indeks
   - Pada kasus tertentu dengan data kecil, overhead dari indeks dapat mengurangi performa keseluruhan

---

# REFERENSI

1. MySQL Documentation - Indexing: https://dev.mysql.com/doc/refman/8.0/en/optimization-indexes.html
2. MariaDB Documentation: https://mariadb.com/kb/en/getting-started-with-indexes/
3. Database Performance Tuning Guide: https://use-the-index-luke.com/
4. SQL Performance Explained: Understanding and Using Indexes - Markus Winand
5. MySQL: Composite Indexes vs. Multiple Single Indexes: https://www.percona.com/blog/mysql-indexes-part-1-the-basics/
6. Understanding EXPLAIN in MySQL: https://dev.mysql.com/doc/refman/8.0/en/explain-output.html
7. Database Indexing Strategies: https://www.dataversity.net/database-indexing-strategies/
