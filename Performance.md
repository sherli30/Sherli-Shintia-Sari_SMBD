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

Penjelasan Proses Import :

•	File employee.sql dijalankan

Server mulai mengeksekusi skrip SQL yang ada dalam file tersebut.

**Kesimpulan :**

Pengguna berhasil mengimpor database employee ke dalam MySQL/MariaDB. Struktur database telah dibuat dan tabel sudah siap digunakan.

e.	Tunggu sampai selesai.

f.	Setelah selesai, maka nanti pada database, akan tampak sebagai berikut:

![image](https://github.com/user-attachments/assets/55ebd177-f9f6-481f-910f-29237e6ac1a2)

1.	Jalankan query : 
SELECT * FROM employee






