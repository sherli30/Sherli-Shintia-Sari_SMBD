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

---

- Pilih navbar “Downloads” pada MySQL
![image](https://github.com/user-attachments/assets/c9b693e1-1211-434e-9b54-802fa034880f)

--

- Scroll kebawah, klik link “MySQL Community (GPL) Downloads >>” 
![image](https://github.com/user-attachments/assets/bd7e413f-ae5a-449d-a08f-b8e059df5f02)

---

-  Klik link “MySQL Installer for Windows”
![image](https://github.com/user-attachments/assets/72f11d99-9373-4439-a349-3909fec4b20c)

--

- Klik tanda “Download” pada “Windows (x86, 32-bit), MSI Installer”, untuk version apk menyesuaikan versi terbaru
![image](https://github.com/user-attachments/assets/72951a5b-6b19-47bc-8740-155f8e0bd74f)

--

-  instalasi “mysql” dengan versi yang sudah di setting sudah selesai

- Klik link "No thanks, just start my download"
![image](https://github.com/user-attachments/assets/4c27abe9-c5a7-4dc0-be9d-720de1ec822e)

--

- Secara otomatis akan langsung terdownload dan berada di Folder "Download"
![image](https://github.com/user-attachments/assets/c3a9b9e8-cca0-4149-ac8b-17f45bf639c7)

--

-  Buka Folder dengan apk MySQL yang sudah di download, lalu klik “Open”
  ![image](https://github.com/user-attachments/assets/6ca9ed93-a3e9-4104-b00b-8aa504765acf)

--

-  Notifikasi untuk menunggu Configures MySQL Installer pada windows
![image](https://github.com/user-attachments/assets/83ea38c9-2c43-4eae-9940-1258078e1493)

--

-  Klik “Yes” untuk installer apk mysql community version 8.0.41 pada “User
Account Control”
![image](https://github.com/user-attachments/assets/4dade162-5ca3-488c-84ce-019af39393ee)

--

- Notifikasi proses untuk Installer MySQL
![image](https://github.com/user-attachments/assets/30e597c4-42c9-45b1-bea4-4e1d388bb615)

--

- Klik “Yes” untuk Installer Launcher MySQL pada “User Account Control”
![image](https://github.com/user-attachments/assets/0469b4a5-2bb8-4918-a02a-0636656927f7)

--

- Untuk memvalidasi internet untuk connection pada “MySQL.Installer 1.6”
![image](https://github.com/user-attachments/assets/3a5aff57-9f5c-492c-86ad-1a95c35f16e5)

--

- Halaman awal pada apk MySQL. Installer, yaitu memiih “Setup Type pada use case” dengan pilihan default yaitu “Server Only”, lalu klik tombol “next”
![Screenshot 2025-03-29 111112](https://github.com/user-attachments/assets/0fe9d0b0-3aa1-4559-83ee-6de0396ae370)

--

- MySQL Installer pada tahap "Path Conflicts", yang mengindikasikan adanya konflik instalasi karena direktori tujuan sudah digunakan oleh versi MySQL Server 8.0.41.
   - Pengguna dapat memilih untuk tetap menggunakan direktori yang sama atau mengubahnya untuk menghindari konflik. Lalu klik tombol "next"
![image](https://github.com/user-attachments/assets/790f9a91-5f58-4c86-ae4a-0bd93c46e053)

--

- Tampilan instalasi MySQL Server 8.0.41 menggunakan MySQL Installer.
   - Statusnya "Ready to Install", dan pengguna dapat menekan tombol **Execute** untuk memulai proses instalasi.
![Screenshot 2025-03-29 111525](https://github.com/user-attachments/assets/29c5cee8-0009-4c64-9de8-dca3399c79bd)

--

- Instalasi **MySQL Server 8.0.41** telah selesai dengan status **Complete**.
   - Pengguna dapat melanjutkan ke langkah berikutnya dengan menekan tombol **Next**.
![Screenshot 2025-03-29 111617](https://github.com/user-attachments/assets/69a9f24f-12d9-4f59-ba9a-9f2cabb27ebe)

--

- **Product Configuration** dalam instalasi **MySQL Server 8.0.41**. Statusnya **Ready to configure**, dan pengguna dapat menekan tombol **Next** untuk memulai proses konfigurasi.
![Screenshot 2025-03-29 111638](https://github.com/user-attachments/assets/0c94dc04-cedd-4ff7-a72f-8b76f6cdd88c)

--

- Konfigurasi **Type and Networking** untuk **MySQL Server 8.0.41**, dengan mode **Development Computer** dan **TCP/IP (Port 3306)** diaktifkan.
   - Pengguna dapat melanjutkan dengan menekan **Next**.
![Screenshot 2025-03-29 111724](https://github.com/user-attachments/assets/469566e6-fc08-4da9-8217-da4c2ca3f742)

--

- **Authentication Method** dalam konfigurasi **MySQL Server 8.0.41**. Opsi yang direkomendasikan adalah **Strong Password Encryption (SHA256)** untuk keamanan lebih baik.
   - Pengguna dapat melanjutkan dengan menekan **Next**.
![Screenshot 2025-03-29 115250](https://github.com/user-attachments/assets/bc952333-6ecf-4382-be76-5aef28a0feb2)

--

- Root Account Password telah diatur, tetapi kekuatannya masih Weak (lemah). Sebaiknya gunakan kata sandi yang lebih kuat. Belum ada MySQL User Accounts yang ditambahkan.
   - Pengguna dapat menambahkan akun dengan mengklik Add User. Untuk melanjutkan, tekan Next.
![Screenshot 2025-03-29 115325](https://github.com/user-attachments/assets/abc4ce96-8162-4a5c-8e9d-63091ed471e8)

--

- **MySQL Server dikonfigurasi sebagai Windows Service** dengan nama **MySQL80**. **Opsi "Start the MySQL Server at System Startup" dicentang**, yang berarti MySQL akan otomatis berjalan saat Windows dinyalakan.
   - **Menggunakan "Standard System Account"**, yang direkomendasikan untuk sebagian besar skenario. Untuk melanjutkan, tekan **Next**.
![Screenshot 2025-03-29 115349](https://github.com/user-attachments/assets/4a1558c2-7662-4e01-a59d-07fb11e81879)

--

- Direktori data MySQL terletak di **C:\ProgramData\MySQL\MySQL Server 8.0\Data**. Opsi yang dipilih: **Memberikan akses penuh hanya kepada pengguna Windows Service dan administrator** untuk meningkatkan keamanan.
   - Opsi lain memungkinkan pengaturan akses manual, tetapi tidak dipilih. Untuk melanjutkan, tekan **Next**.
![Screenshot 2025-03-29 115403](https://github.com/user-attachments/assets/2b0ef302-91e5-4bb3-8161-fa335188ebf0)

--

- Apply Configuration dalam instalasi MySQL Server 8.0.41.
- Beberapa konfigurasi akan diterapkan, seperti menulis file konfigurasi, memperbarui firewall, menginisialisasi database, dan memulai server.
- Klik Execute untuk menjalankan proses konfigurasi.
![Screenshot 2025-03-29 115415](https://github.com/user-attachments/assets/efca388f-367b-49aa-92e7-87b358e9f8b5)

--

- Konfigurasi MySQL Server 8.0.41 telah berhasil diselesaikan.
   - Semua langkah, termasuk menulis file konfigurasi, memperbarui firewall, dan memulai server, telah selesai. Klik Finish untuk menyelesaikan proses instalasi.
![Screenshot 2025-03-29 115441](https://github.com/user-attachments/assets/96cb9f32-9b26-4b7e-ac64-6cc3d39539bb)

--

- konfigurasi MySQL Server 8.0.41 telah selesai.
   - Pengguna dapat melanjutkan dengan mengklik Next untuk menyelesaikan proses instalasi.
![Screenshot 2025-03-29 111936](https://github.com/user-attachments/assets/02e69bf2-6120-4dbe-a41d-528611202740)

--

- Proses instalasi MySQL Server telah selesai.
   - Pengguna dapat menyalin log instalasi dengan mengklik Copy Log to Clipboard atau menutup installer dengan mengklik Finish.
![Screenshot 2025-03-29 111946](https://github.com/user-attachments/assets/4c911d0d-f7f8-4129-9685-818f026113e1)

--

- MySQL Installer setelah instalasi selesai. Tampilan ini menampilkan MySQL Server versi 8.0.41 (arsitektur X64) yang telah terpasang.
   - Pengguna dapat Reconfigure server, menambah, mengubah, memperbarui, atau menghapus instalasi MySQL melalui opsi di sebelah kanan.
![Screenshot 2025-03-29 112004](https://github.com/user-attachments/assets/b8d356c1-048f-48fe-bffc-1f30962ded9f)

--

- Hasil pencarian di Windows untuk MySQL 8.0 Command Line Client.
   - Pengguna dapat membuka aplikasi ini untuk mengakses MySQL melalui command line. Klik opsi seperti Run as administrator,
![image](https://github.com/user-attachments/assets/1f5cb059-43e7-4330-93cc-a0a2b67818e9)

--

- Tampilan MySQL 8.0 Command Line Client yang meminta pengguna untuk memasukkan password.
   - Password ini adalah yang telah dibuat saat instalasi MySQL. Jika benar, pengguna akan masuk ke MySQL shell untuk menjalankan perintah SQL.
![Screenshot 2025-03-29 112403](https://github.com/user-attachments/assets/29763eb1-3e72-466c-aa2a-e37098d07d8b)

--

- MySQL telah berhasil diinstal dan pengguna berhasil masuk ke MySQL monitor setelah memasukkan password.
![Screenshot 2025-03-29 112413](https://github.com/user-attachments/assets/e1ab843d-818d-4926-9a84-247e101dd371)

---

**5. Lakukan perubahan perubahan pada variable berikut (dokumentasikan before dan after nya)**
- a. port dari default 3306 menjadi 3309.
  - Ketik perintah SHOW VARIABLES LIKE ‘port’; di command prompt mysql untuk mengecek port yang sedang digunakan lalu tekan enter dan hasilnya akan menunjukkan port saat ini default 3306. Lalu tutup mysql dengan perintah EXIT; dan enter
![Screenshot 2025-03-29 112518](https://github.com/user-attachments/assets/a1d35afd-cf43-404a-9525-1402bf7fc04f)

--

- Mengedit file konfigurasi my.ini untuk mengubah port MySQL dari 3306 ke 3309 menggunakan vscode.
   - Pastikan Anda menyimpan file my.ini setelah mengeditnya.
![Screenshot 2025-03-29 113706](https://github.com/user-attachments/assets/6e0ad505-aac9-4bc6-acaa-89f8cfb10337)

--

- Membuka Command Prompt di Windows dengan klik Kanan "Run as administrator", ketik "net stop mysql80" dan "net start mysql80" 
![Screenshot 2025-03-29 113937](https://github.com/user-attachments/assets/34688b88-bca2-485d-8bed-49e8b20ed580)

--

- Sesudah port MySQL telah berhasil diubah menjadi 3309 
![image](https://github.com/user-attachments/assets/60ea2a0a-170f-4dbb-9893-f7f5092c39ea)

--

- Membuka Command Prompt di Windows. Jika Anda ingin menjalankan perintah dengan hak administrator, pilih "Run as administrator" agar memiliki izin penuh untuk mengelola sistem, termasuk mengubah layanan seperti MySQL.
![Screenshot 2025-03-29 114012](https://github.com/user-attachments/assets/9e0476a3-2800-48ea-b519-e778da349799)

--

- b. innodb_buffer_pool_size dr default 16M (menjadi 25% dari RAM )
   - Cek size RAM Fisik yaitu 16,108 MB (sekitar 16GB) dengan perintah dibawah ini.
![Screenshot 2025-03-29 120612](https://github.com/user-attachments/assets/00efa91f-1872-4da3-967b-569d7e4c8770)

--

- Buka File "C:\ProgramData\MySQL\MySQL Server 8.0\my.ini" dan rubah innodb_buffer_pool_size menjadi 25% dari RAM, yang awalnya 128M menjadi 3G dari 16GB .
![image](https://github.com/user-attachments/assets/d862dd6f-e033-4509-9585-8c655235efa6)

--

- Cek nilai innodb_buffer_pool_size adalah 3221225472 byte, yang setara dengan 3 MB.
![ChatGPT Image Mar 29, 2025, 02_17_05 PM](https://github.com/user-attachments/assets/547eb80a-bb6e-4912-a800-8fcad42faed0)

--

- Perintah mengubah kata sandi pengguna `root` di MySQL menjadi **"sherli"**. Gantilah dengan kata sandi yang lebih aman.
![image](https://github.com/user-attachments/assets/30fc65a1-bb77-4506-867a-c168078d5b11)

---

**6. lakukan perubahan terhadap password root.**
![image](https://github.com/user-attachments/assets/2838b24f-6748-4b6d-9936-ab95b54e8066)

---

7. Buat database dengan nama: kelompok_AB_nama_mhs
![Screenshot 2025-03-29 122100](https://github.com/user-attachments/assets/01360c47-ef85-4cad-ac84-409166ccafdc)

8. Semua Langkah-langkah konfigurasi diatas, lakukan dengan command prompt.

---

# PEMBAHASAN

**1. Port Konfigurasi**  
Mengubah port default **3306 → 3309** untuk:  
- **Menghindari konflik** dengan aplikasi lain  
- **Meningkatkan keamanan** dari serangan umum  
- **Menjalankan multiple instance** MySQL di satu server  

**2. Buffer Pool Size**  
Menyesuaikan **`innodb_buffer_pool_size`** (128M → 3G) untuk:  
- **Caching lebih efisien** dan mengurangi I/O disk  
- **Meningkatkan kecepatan query** dan throughput  
- **Optimalisasi penggunaan RAM** (25% dari total 16GB)  

**3. Keamanan & Manajemen User**  
- **Mengganti password root** untuk mencegah akses ilegal  
- **Menggunakan enkripsi SHA256** untuk keamanan login  
- **Firewall & akses direktori** dikonfigurasi untuk perlindungan tambahan  

**4. Pemilihan Tipe Database**  
- **SQL (MySQL, PostgreSQL)** untuk data berstruktur & relasi kompleks  
- **NoSQL (MongoDB, Redis)** untuk skalabilitas tinggi & skema fleksibel  

**5. Pemantauan Status Server**  
- **`SHOW VARIABLES`** untuk verifikasi konfigurasi & troubleshooting  
- **Dokumentasi status server** untuk audit dan pemeliharaan  

**6. Manajemen Database**  
Pembuatan database baru menunjukkan dasar pengelolaan MySQL dalam pengembangan aplikasi.  

# KESIMPULAN SECARA KESELURUHAN 
Instalasi dan konfigurasi MySQL adalah aspek fundamental dalam membangun sistem basis data yang **efisien, aman, dan handal**. Berikut beberapa kesimpulan penting:  

**1. Pemilihan Teknologi Database**  
- **MySQL (SQL)**: Cocok untuk data terstruktur dengan relasi kompleks.  
- **NoSQL**: Lebih fleksibel untuk data tidak terstruktur dan skalabilitas tinggi.  

**2. Instalasi dan Konfigurasi Dasar**  
- Menyesuaikan parameter seperti **autentikasi**, **Windows Service**, dan **port** selama instalasi.  
- Memastikan MySQL berjalan sesuai kebutuhan sistem.  

**3. Optimasi Performa**  
- Mengatur **`innodb_buffer_pool_size`** (128M → 3G) untuk **meningkatkan efisiensi cache & mengurangi I/O disk**.  
- Menyesuaikan alokasi sumber daya berdasarkan spesifikasi hardware.  

**4. Keamanan Database**  
- **Mengubah password root**, membatasi akses direktori data, dan mengaktifkan firewall.  
- **Menggunakan enkripsi SHA256** untuk autentikasi yang lebih aman.  

**5. Fleksibilitas Konfigurasi**  
- **Mengubah port (3306 → 3309)** untuk menghindari konflik & meningkatkan keamanan.  
- Kemampuan menyesuaikan MySQL sesuai dengan kebutuhan sistem.  

**6. Manajemen dan Pemantauan**  
- **`SHOW VARIABLES`** digunakan untuk memantau dan memverifikasi konfigurasi.  
- Dokumentasi dan audit konfigurasi memastikan keandalan sistem.  

---

# REFERENSI
- MySQL Documentation. (2024). "MySQL 8.0 Reference Manual." Oracle Corporation. https://dev.mysql.com/doc/refman/8.0/en/
- Schwartz, B., Zaitsev, P., & Tkachenko, V. (2012). "High Performance MySQL: Optimization, Backups, and Replication." O'Reilly Media.
- Bell, C. (2022). "MySQL for the Internet of Things." Apress.
- DuBois, P. (2021). "MySQL Cookbook: Solutions for Database Developers and Administrators." O'Reilly Media.
- Vaswani, V. (2019). "MySQL Database Usage & Administration." McGraw-Hill Education.
- Salahaldin, A., & Razzaq, A. (2023). "Optimizing MySQL Database Performance: A Comprehensive Guide to Buffer Pool Configuration." International Journal of Database Management Systems, 15(2), 45-58.
- MySQL Security Team. (2024). "MySQL Security Best Practices Guide." Oracle Corporation. https://dev.mysql.com/doc/refman/8.0/en/security.html
- Kofler, M. (2020). "The Definitive Guide to MySQL 8." Apress.
- Reddy, S. M., & Kumar, P. (2023). "Comparative Analysis of SQL and NoSQL Databases for Modern Applications." Journal of Database Management, 34(3), 112-127.
- MySQL Performance Team. (2024). "InnoDB Buffer Pool Configuration Guide." Oracle Corporation. https://dev.mysql.com/doc/refman/8.0/en/innodb-buffer-pool.html
