**PERFORMANCE**

**LATAR BELAKANG**

Dalam pengelolaan basis data, performa eksekusi query menjadi faktor penting yang mempengaruhi efisiensi sistem. Salah satu cara meningkatkan performa adalah dengan penggunaan teknik optimasi seperti indeks, partisi tabel, dan strategi eksekusi query yang efisien. Indeks memungkinkan database untuk mengambil data lebih cepat dibandingkan dengan pemindaian tabel penuh, sementara partisi tabel dapat mengurangi beban pemrosesan dengan membagi data ke dalam segmen yang lebih kecil.

Laporan ini membahas berbagai teknik optimasi performa database, termasuk penggunaan indeks dan partisi tabel, serta analisis terhadap dampaknya terhadap kecepatan eksekusi query. Dengan implementasi yang tepat, optimasi ini dapat meningkatkan efisiensi pengelolaan data dan mengurangi waktu respons sistem, sehingga mendukung kinerja aplikasi yang lebih baik.

**PROBLEM YANG DIANGKAT**

Berdasarkan masalah utama yang dapat diangkat dalam laporan ini:  

1. Bagaimana proses impor data dalam skala besar ke dalam MySQL?

2. Bagaimana cara menganalisis performa eksekusi query dengan dan tanpa teknik optimasi?

3. Bagaimana pengaruh indeks terhadap kecepatan eksekusi query dalam basis data besar? 

4. Bagaimana penggunaan indeks composite dan foreign key dapat membantu optimalisasi pencarian data?

**SOLUSI / SKENARIO AKTIVITAS**
**1. 
Import data**

• Lakukan import data dengan Langkah sebagai berikut:

a.	Download dan extrak dataset employe pada folder

b.	Masuk konsol dos, sehingga tampil seperti berikut:

![image](https://github.com/user-attachments/assets/fc3e6bf5-05f7-4a36-b79c-43251cbb54a7)

**Penjelasan:**

•	Masuk ke Direktori XAMPP

Awalnya, pengguna berada di direktori c:\xampp, yang merupakan folder tempat XAMPP terinstal.

•	Berpindah ke Folder Dataset

1.	Perintah cd "C:\Users\useri\Downloads\dataset_emp\dataset_full" digunakan untuk masuk ke folder tempat file dataset berada.

2.	Setelah menjalankan perintah ini, pengguna sekarang berada di dalam folder dataset_full, siap untuk melakukan langkah berikutnya, seperti mengimpor data ke dalam MySQL.
   
•	Berpindah ke Folder Dataset

1.	Perintah cd "C:\Users\useri\Downloads\dataset_emp\dataset_full" digunakan untuk masuk ke folder tempat file dataset berada.

2.	Setelah menjalankan perintah ini, pengguna sekarang berada di dalam folder dataset_full, siap untuk melakukan langkah berikutnya, seperti mengimpor data ke dalam MySQL.
   
•	Informasi yang Ditampilkan

1.	"Welcome to the MariaDB monitor" → Berarti koneksi ke database berhasil.
2.	Versi server MariaDB yang digunakan adalah 10.4.32.
3.	Pengguna dapat mulai menjalankan perintah SQL untuk mengelola database.

• Jangan lupa, posisi cursor pada tempat menyimpan file dataset.

c.	Masuk ke database mysql

d.	Lakukan import data dengan menggunakan script berikut:
Source employee.sql seperti berikut:

![image](https://github.com/user-attachments/assets/3fbd72c3-7c9d-4b97-8e58-c11a0a58e3c2)

**Penjelasan Proses Import:**

•	File employee.sql dijalankan

Server mulai mengeksekusi skrip SQL yang ada dalam file tersebut.

**Kesimpulan :**

Pengguna berhasil mengimpor database employee ke dalam MySQL/MariaDB. Struktur database telah dibuat dan tabel sudah siap digunakan.

e.	Tunggu sampai selesai.

f.	Setelah selesai, maka nanti pada database, akan tampak sebagai berikut:

![image](https://github.com/user-attachments/assets/55ebd177-f9f6-481f-910f-29237e6ac1a2)

1.	Jalankan query : 

SELECT * FROM employee

![image](https://github.com/user-attachments/assets/53530713-eb80-426b-8a94-7b59962d0da2)

**Penjelasan:**

Fungsinya untuk mengambil seluruh data dari tabel `employee`, yang berisi informasi karyawan seperti nomor pegawai (`emp_no`), tanggal lahir (`birth_date`), nama depan (`first_name`), nama belakang (`last_name`), jenis kelamin (`gender`), dan tanggal perekrutan (`hire_date`). Pada hasil eksekusi, sistem menampilkan 1.000 baris data dalam waktu **0.006 detik**, dengan waktu eksekusi query **0.001 detik**.

2.	Jalankan query explain :
   
EXPLAIN SELECT * FROM employee
WHERE first_name = 'Georgi'

![image](https://github.com/user-attachments/assets/68242c0c-f5c9-4414-93b8-57d8d1d40d86)

**Penjelasan:**

Fungsinya untuk menganalisis cara MySQL mengeksekusi perintah pencarian data berdasarkan nama depan. Hasilnya menunjukkan bahwa MySQL melakukan **full table scan** (`type: ALL`), karena tidak ada indeks yang digunakan (`possible_keys: NULL`, `key: NULL`). Akibatnya, sistem harus memindai **299.423 baris** untuk menemukan data yang sesuai, yang dapat memperlambat proses pencarian terutama pada tabel berukuran besar. Selain itu, pada kolom `Extra` terdapat keterangan `Using where`, yang berarti MySQL memfilter hasil berdasarkan kondisi dalam klausa `WHERE`. Untuk meningkatkan efisiensi query ini, disarankan untuk membuat **indeks pada kolom `first_name`**, sehingga proses pencarian dapat dilakukan lebih cepat tanpa harus membaca seluruh isi tabel.

3.	Tambahkan index

ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name)

![image](https://github.com/user-attachments/assets/7cf969f7-1a76-4de8-b3dd-665f7b2952ce)

**Penjelasan:**

FUngsinya untuk menambahkan indeks komposit (idx_full_name) pada kolom first_name dan last_name dalam tabel employee. Indeks ini akan mempercepat pencarian data berdasarkan kombinasi kedua kolom tersebut, terutama untuk query dengan kondisi WHERE first_name = '...' AND last_name = '...'. Proses eksekusi memakan waktu 1.255 detik, menunjukkan bahwa indeks berhasil dibuat tanpa error atau peringatan.

4.	Lakukan query exlain lagi untuk query diatas :
   
EXPLAIN SELECT * FROM employee
WHERE first_name = 'Georgi'
AND last_name = 'Bahr'


![image](https://github.com/user-attachments/assets/a36721aa-31f5-463d-86c8-820c4046c72a)

**Penjelasan:**

Fungsinya untuk menganalisis performa pencarian data setelah penambahan indeks komposit. Sebelumnya, pencarian dilakukan dengan **full table scan**, yang menyebabkan pemrosesan ratusan ribu baris. Setelah menambahkan indeks komposit `idx_full_name` pada kolom `first_name` dan `last_name` menggunakan query `ALTER TABLE employee ADD INDEX idx_full_name (first_name, last_name);`, MySQL kini dapat menggunakan indeks untuk pencarian yang lebih efisien. Hasil `EXPLAIN` menunjukkan bahwa MySQL menggunakan indeks (`type: ref`), dengan jumlah baris yang diproses turun drastis menjadi **1 baris**, dibandingkan dengan pencarian sebelumnya. Kolom `Extra` menyatakan **Using index condition**, yang berarti MySQL memanfaatkan indeks dalam penyaringan data sebelum mengambilnya dari tabel. 

Sekarang bisa kita lihat bahwa query yang kita jalankan tidak lagi scaning penuh ketabel, melainkan akses pada index. Ini terlihat pada nilai kolom key dan possible_keys.











