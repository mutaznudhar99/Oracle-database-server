Pada sesi kali ini, saya akan mendemonstrasikan switchover database primary menuju standby tanpa memutus sesi koneksi menggunakan fitur transparent application continuity (TAC). Hal ini bertujuan agar proses transaksi client tetap berjalan lancar tanpa terputus ditengah jalan apabila database mengalami failover otomatis maupun switchover secara manual.

hal yang dipersiapkan:
- Service database primary dan standby berjalan (running) di masing-masing server
- Mengaktifkan listener database primary dan standby




1. Verifikasi status resource high availability untuk memastikan resource database primary dan standby berjalan di masing-masing host

   <img width="559" height="383" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/05b56535-62de-4ccd-a4e3-958b0556210c" />
   <img width="567" height="386" alt="Screenshot (1170)" src="https://github.com/user-attachments/assets/959b9f0b-05b6-49d3-8be4-46c60c7a3132" />


2. Create service transparent application continuity (TAC) pada database primary untuk support transaksi tanpa memutus sesi saat melakukan switchover atau terjadi failover

   <img width="693" height="305" alt="Screenshot (1177)" src="https://github.com/user-attachments/assets/ebb00e23-3e8d-4aea-a6ad-747b56328e59" />
   - db        : Menentukan nama database service
   - pdb       : Menentukan spesifik pdb untuk menjalankan proses tac seamless session
   - service   : Nama service yang digunakan oleh client
   - role      : Primary, Menentukan service ini berjalan apabila database dalam keadaan primary
   - failoverty: Auto, konfigurasi utama menngaktifkan fitur TAC
   - famethod  : Basic, metode failover standar untuk membangun kembali koneksi setelah terjadi switchover atau failover
   - commitout : True, menyimpan status transaksi terakhir di database
   - failoverre: Auto, otomatisasi recovery sesi aktif ke new database primary
   - replay_ini: Batas waktu sistem diperbolehkan replay(eksekusi ulang) transaksi yang gagal akibat putus koneksi
   - notificati: True, Mengaktifkan Fast Application Notification (FAN). Mengirimkan sinyal "cepat" ke driver aplikasi (JDBC/OCI) agar segera melakukan failover ke database yang available


3. Copy client connectivity (tnsnames.ora) untuk mengintegrasikan konfigurasi service prod_tac pada sisi aplikasi client

   <img width="567" height="414" alt="Screenshot (1204)" src="https://github.com/user-attachments/assets/4bc57b24-62cd-4249-a1eb-a097e2745e51" />


4. Melakukan verifikasi v$active_services untuk memastikan parameter TAC benar-benar aktif secara runtime pada level PDB
   
   <img width="1135" height="615" alt="Screenshot (1211)" src="https://github.com/user-attachments/assets/6b511b42-4d33-41c8-8611-5bd4dcb514fc" />

   
5. Verifikasi Status Role dan Open Mode Database untuk memastikan sinkronisasi berjalan sempurna sebelum switchover dilakukan
   
   <img width="591" height="319" alt="Screenshot (1173)" src="https://github.com/user-attachments/assets/034c0662-0db5-46bf-89b4-4c2c5763a751" />
   <img width="597" height="304" alt="Screenshot (1174)" src="https://github.com/user-attachments/assets/a9639295-e789-4acd-bbf3-08d5b0bc6b56" />


6. Melakukan testing dengan menginput data tanpa melakukan COMMIT. Bertujuan untuk mensimulasikan kondisi di mana transaksi masih menggantung 

   <img width="651" height="391" alt="Screenshot (1212)" src="https://github.com/user-attachments/assets/50cdc630-fd82-4640-ac79-7b241b1246ea" />


7. Memastikan database standby berada dalam kondisi ideal (ready for switchover: YES) untuk takeover role primary tanpa ada risiko kehilangan data 

   <img width="816" height="950" alt="Screenshot (1213)" src="https://github.com/user-attachments/assets/cb7c5cbf-9ef6-43bb-92c7-c68724adff8b" />


8. Menjalankan perintah switchover sekaligus COMMIT pada aplikasi. Memastikan sesi tertunda (pause) karena aplikasi sedang melakukan transaksi replay ke target database baru tanpa memutus sesi

   <img width="1567" height="461" alt="Screenshot (1214)" src="https://github.com/user-attachments/assets/3f8efcda-ef60-48c8-8e65-abf807d08581" />


9. Memastikan transaksi yang tertunda berhasil di replay pada database primary baru tanpa terjadi error/memutus sesi

   <img width="1789" height="524" alt="Screenshot (1215)" src="https://github.com/user-attachments/assets/d446e86d-989b-46ce-a314-4fab632a6332" />


10. Melakukan monitoring akhir untuk memastikan role database telah bertukar dengan kondisi sesi tetap berjalan tanpa interupsi 

    <img width="1758" height="811" alt="Screenshot (1217)" src="https://github.com/user-attachments/assets/329a7e0f-b579-42f7-9f95-3ec13e14a97b" />

    

   

   

   

   


   




