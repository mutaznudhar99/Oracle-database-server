Pada sesi kali ini, saya akan mensimulasikan proses role transition (Switchover) dari Database primary ke standby tanpa menyebabkan diskoneksi proses transaksi pada sisi aplikasi dengan mengimplementasikan fitur transparent application continuity (TAC)


1. Melakukan validasi awal pada level infrastruktur untuk memastikan seluruh resource oracle restart berstatus ONLINE

   <img width="559" height="383" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/05b56535-62de-4ccd-a4e3-958b0556210c" />
   <img width="567" height="386" alt="Screenshot (1170)" src="https://github.com/user-attachments/assets/959b9f0b-05b6-49d3-8be4-46c60c7a3132" />


2. Melakukan konfigurasi database service khusus (TAC) yang dirancang untuk menangani failover transaksional secara transparan

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


3. Melakukan sinkronisasi file tnsnames.ora pada sisi client/aplikasi untuk mengarahkan koneksi ke service khusus database (TAC)

   <img width="567" height="414" alt="Screenshot (1204)" src="https://github.com/user-attachments/assets/4bc57b24-62cd-4249-a1eb-a097e2745e51" />


4. Melakukan query data views V$ACTIVE_SERVICES untuk memastikan parameter TAC telah aktif pada instance database
   
   <img width="1135" height="615" alt="Screenshot (1211)" src="https://github.com/user-attachments/assets/6b511b42-4d33-41c8-8611-5bd4dcb514fc" />

   
5. Verifikasi Status Role dan Open Mode Database untuk memastikan sinkronisasi berjalan dengan baik sebelum switchover dilakukan
   
   <img width="591" height="319" alt="Screenshot (1173)" src="https://github.com/user-attachments/assets/034c0662-0db5-46bf-89b4-4c2c5763a751" />
   <img width="597" height="304" alt="Screenshot (1174)" src="https://github.com/user-attachments/assets/a9639295-e789-4acd-bbf3-08d5b0bc6b56" />


6. Melakukan injeksi data (INSERT) tanpa eksekusi COMMIT. Mensimulasikan kondisi transaksi yang menggantung (in-flight transaction) saat sesi diputus secara paksa oleh proses switchover atau terjadi failover

   <img width="651" height="391" alt="Screenshot (1212)" src="https://github.com/user-attachments/assets/50cdc630-fd82-4640-ac79-7b241b1246ea" />


7. Validasi kesiapan switchover. Memastikan database standby berada dalam kondisi ideal (ready for switchover: YES) untuk takeover role primary tanpa ada risiko kehilangan data 

   <img width="816" height="950" alt="Screenshot (1213)" src="https://github.com/user-attachments/assets/cb7c5cbf-9ef6-43bb-92c7-c68724adff8b" />


8. Eksekusi switchover & transaction replay pada aplikasi. Memastikan sesi tertunda (pause) karena aplikasi sedang melakukan transaksi replay ke target database baru tanpa memutus sesi

   <img width="1567" height="461" alt="Screenshot (1214)" src="https://github.com/user-attachments/assets/3f8efcda-ef60-48c8-8e65-abf807d08581" />


9. Memverifikasi bahwa transaksi yang tertunda (pause) sebelumnya berhasil masuk ke database primary yang baru tanpa memicu error ORA-03113 atau Connection Losti

   <img width="1789" height="524" alt="Screenshot (1215)" src="https://github.com/user-attachments/assets/d446e86d-989b-46ce-a314-4fab632a6332" />


10. Melakukan audit akhir pasca-switchover. Memastikan role database telah bertukar dengan kondisi sesi tetap berjalan tanpa interupsi 

    <img width="1758" height="811" alt="Screenshot (1217)" src="https://github.com/user-attachments/assets/329a7e0f-b579-42f7-9f95-3ec13e14a97b" />

    

   

   

   

   


   




