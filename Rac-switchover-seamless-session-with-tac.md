Pada sesi kali ini, saya akan mengimplementasikan transparent application continuity (TAC) untuk mempresentasikan seamless session saat melakukan switcohver atau terjadi failover. Berujuan memastikan proses transaksi pada level aplikasi tetap berjalan tanpa error walau berpindah node

hal yang dipersiakan:
- Rac database minimal 2 node server
- Konfigurasi service TAC 
- Connection string untuk mengkoneksikan database dengan client (aplikasi)


1. Memastikan seluruh stack Clusterware di semua node berstatus Online.

   <img width="563" height="89" alt="Screenshot (1014)" src="https://github.com/user-attachments/assets/a5767f24-4ff1-4aff-a0e2-ffda421327df" />
   <img width="560" height="150" alt="Screenshot (1030)" src="https://github.com/user-attachments/assets/2d7c1771-aecb-4847-91ed-9ef14854768b" />

2. Create service database khusus dengan konfigurasi Transparent Application Continuity (TAC).

   <img width="734" height="387" alt="Screenshot (1031)" src="https://github.com/user-attachments/assets/039b62d0-394a-4403-940f-b299e9a311a6" />
   - db        : Menentukan nama database service
   - preferred : Mendaftarkan node active ke dalam database service khusus tac
   - failoverty: Auto, konfigurasi utama menngaktifkan fitur TAC
   - famethod  : Basic, metode failover standar untuk membangun kembali koneksi setelah terjadi switchover atau failover
   - commitout : True, menyimpan status transaksi terakhir di database
   - failoverre: Auto, otomatisasi recovery sesi aktif ke new database primary
   - replay_ini: Batas waktu sistem diperbolehkan replay(eksekusi ulang) transaksi yang gagal akibat putus koneksi
   - notificati: True, Mengaktifkan Fast Application Notification (FAN). Mengirim sinyal ke driver aplikasi (JDBC/OCI) segera melakukan failover ke node rac yang available


3. Melakukan validasi TAC pada level instance. Memastikan service sudah aktif pada database

   <img width="1480" height="490" alt="Screenshot (1221)" src="https://github.com/user-attachments/assets/97a8c85a-7f2e-4536-a565-7c246ef6f658" />


4. Konfigurasi connection string menggunakan SCAN IP. Memungkinkan client mencoba mengkoneksikan kembali ke cluster rac secara otomatis jika salah satu IP terputus.

   <img width="982" height="208" alt="Screenshot (1032)" src="https://github.com/user-attachments/assets/df3d481b-28d7-4c3f-9d10-e1361c1628ea" />

5. Input connection string TAC ke dalam Swingbench untuk mengkoneksikan client (Swingbench) ke dalam cluster

   <img width="643" height="334" alt="Screenshot (1033)" src="https://github.com/user-attachments/assets/9190a560-4221-45c2-9bad-11a9c82624ba" />

6. Menjalankan transaksi sebagai testing control data. Bertujuan memvalidasi sistem dapat menjamin Commit Outcome, sehingga aplikasi tahu pasti apakah transaksi berhasil masuk ke disk atau perlu replay saat terjadi perpindahan node.

   <img width="552" height="287" alt="Screenshot (1222)" src="https://github.com/user-attachments/assets/feb25828-abe7-45b0-a196-be607003a123" />

       
7. Melakukan switchover dari node 1 ke node 2. Memverifikasi keberhasilan replay transaksi aplikasi (Swingbench) tidak mengalami disconnect atau error, hanya mengalami sedikit jeda (latency) saat transaksi dipindahkan dan di-replay di node available

   <img width="1501" height="546" alt="Screenshot (1223)" src="https://github.com/user-attachments/assets/b14d5782-3d40-4a9f-8cb5-df80b75c2cc3" />


8. Melakukan uji Komparasi pada koneksi database standar. Hasilnya menunjukkan perbedaan signifikan: tanpa TAC, aplikasi akan langsung menerima pesan error (seperti ORA-03113 atau Connection Lost)

   <img width="1723" height="476" alt="Screenshot (1224)" src="https://github.com/user-attachments/assets/85ec82da-d5b4-4308-a679-757bb0bbcc2e" />



    
