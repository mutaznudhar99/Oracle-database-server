Pada sesi kali ini, saya akan mendemonstrasikan switchover database primary menuju standby tanpa memutus sesi koneksi menggunakan fitur transparent application continuity (TAC). Hal ini bertujuan agar proses transaksi client tetap berjalan lancar tanpa terputus ditengah jalan apabila database mengalami failover otomatis maupun switchover secara manual.

hal yang dipersiapkan:
- Memastikan database primary dan standby berjalan di masing-masing server
- Apliksi swingbench sebagai tools untuk mensimulasikan dan mengirim workload transaksi secara real-time ke database




1. Memastikan resource database primary dan standby berjalan di masing-masing host

   <img width="559" height="383" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/05b56535-62de-4ccd-a4e3-958b0556210c" />
   <img width="567" height="386" alt="Screenshot (1170)" src="https://github.com/user-attachments/assets/959b9f0b-05b6-49d3-8be4-46c60c7a3132" />


2. Create service pada database primary dan standby untuk mengkonfigurasi transparent application continuity pada database apabila terjadi kegagalan atau switchover secara manual

   <img width="693" height="305" alt="Screenshot (1177)" src="https://github.com/user-attachments/assets/ebb00e23-3e8d-4aea-a6ad-747b56328e59" />
   - db        : Menentukan nama database service
   - pdb       : Menentukan spesifik pdb untuk menjalankan proses tac seamless session
   - service   : Nama service yang digunakan oleh client
   - role      : Primary, Menentukan service ini berjalan apabila database dalam keadaan primary
   - failover  : Auto, konfigurasi utama menngaktifkan fitur TAC
   - famethod  : Basic, metode failover standar untuk membangun kembali koneksi setelah terjadi switchover atau failover
   - commitout : True, menyimpan status transaksi terakhir di database
   - failoverre: Level1/Auto, set pemulihan sesi aktif ke new database primary
   - replay_ini: Batas waktu sistem diperbolehkan replay(eksekusi ulang) transaksi yang gagal akibat putus koneksi
   - notificati: True, Mengaktifkan Fast Application Notification (FAN). Mengirimkan sinyal "cepat" ke driver aplikasi (JDBC/OCI) agar segera melakukan failover ke database yang available


3. Konfigurasi tnsnames.ora pada database primary dan standby untuk mengirim connection string kepada client
   - Memastikan client bisa mengakses database tac hanya dengan konfigurasi ipadress:port:nameservice 

     <img width="730" height="255" alt="Screenshot (1151)" src="https://github.com/user-attachments/assets/fddb72e2-0ff5-4bfb-90b6-c5763124fd03" />


4. Create/Copy konfigurasi service tnsnames.ora ke aplikasi swingbench untuk menjalankan proses data workload

   <img width="552" height="485" alt="Screenshot (1154)" src="https://github.com/user-attachments/assets/9a57281e-9457-4143-8213-21a53cc25adb" />


5. Konfigurasi simulasi load balancing pada aplikasi swingbench

   <img width="562" height="436" alt="Screenshot (1155)" src="https://github.com/user-attachments/assets/2be6ca03-4438-431c-872c-9f8aaceac177" />


6. Monitoring load balancing dan status database pada database primary dan standby sebelum menjalankan simulasi data workload ke dalam database primary

   <img width="622" height="385" alt="Screenshot (1153)" src="https://github.com/user-attachments/assets/c3f17502-415d-4de5-bceb-84843fe97a02" />
   <img width="591" height="319" alt="Screenshot (1173)" src="https://github.com/user-attachments/assets/034c0662-0db5-46bf-89b4-4c2c5763a751" />
   <img width="597" height="304" alt="Screenshot (1174)" src="https://github.com/user-attachments/assets/a9639295-e789-4acd-bbf3-08d5b0bc6b56" />


7. Running simulasi data workload pada aplikasi swingbench dan Monitoring secara real-time load balancing pada database primary

   <img width="1633" height="938" alt="Screenshot (1185)" src="https://github.com/user-attachments/assets/0d30c9af-1076-44d9-984b-b338ae35aba7" />
   - hostname                : primary
   - total_session           : 1 > 52 = Swingbench berhasil memberikan data workload ke database primary
   - trsancations per minute : 73

8. Validasi database menggunakan data guard command line interface (DGMGRL) untuk mengetahui status kesiapan database primary dan standby sebelum melakukan switchover database
   - Memastikan database primary dan standby siap untuk melakukan switchover database

     <img width="839" height="854" alt="Screenshot (1161)" src="https://github.com/user-attachments/assets/e45e7ca5-be25-4341-8d59-8383f2f6ebd5" />
     - ready for switcover : yes    = kedua database siap untuk melakukan perpindahan operasional (Switchover) database


9. Melakukan eksekusi switchover menggunakan DGMGRL dan verifikasi proses switchover menggunakan SHOW CONFIGURATION. melihat status database yang baru

   <img width="1580" height="934" alt="Screenshot (1186)" src="https://github.com/user-attachments/assets/2b2f7d69-f573-4d8c-a583-9c517ba20818" />
   - trasancations per minute : 73 > 0
   - prod_stby                : primary
   - prod                     : physical standby database


10. Monitoring proses load balancing dan status database pada database old primary setelah melakukan switchover

    <img width="1634" height="941" alt="Screenshot (1188)" src="https://github.com/user-attachments/assets/1f29b1cd-7731-4fcf-a0cf-a830fba96cff" />
    - hostname               : primary > standby
    - transaction per minute : 73 > 0 > 24
    - total session          : 1 > 52 > 51
    <img width="602" height="359" alt="Screenshot (1175)" src="https://github.com/user-attachments/assets/6822c5d9-0814-491a-a578-5c99d4ed820d" />
    <img width="601" height="310" alt="Screenshot (1176)" src="https://github.com/user-attachments/assets/0396c4c8-e825-4dae-b91c-33b32eea6823" />


    

      

    









   




