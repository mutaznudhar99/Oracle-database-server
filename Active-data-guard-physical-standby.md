Pada sesi kali ini, saya akan mendemonstrasikan acive data guard physical standby menggunakan utility broker sebagai management data guard pada server grid oracle restart. Demonstrasi ini bertujuan untuk mencapai
High availibility dengan zero downtime.


Hal yang dipersiapkan:
- 1 Server database primary menggunakan grid oracle restart
- 1 Server standby menggunakan grid oracle restart
- ASM disk
- Broker sebagai management data guard





1. Memastikan service database dan listener berjalan di server primary dan standby sebagai jalan utama untuk menjalankan active data guard

   <img width="1599" height="879" alt="Screenshot (1057)" src="https://github.com/user-attachments/assets/095f59e6-6ce4-4197-bd37-17cdc1cba728" />


2. Memastikan ip dan hostname server primary dan standby di masing-masing library server /etc/hosts untuk mengkoneksikan antar server

   <img width="789" height="125" alt="Screenshot (1062)" src="https://github.com/user-attachments/assets/94685cd2-d752-4c52-b7de-2f39e68e27dd" />


3. Testing ip hostname menggunakan tnsping/ping untuk memverifikasi server sudah saling mengenal

   <img width="849" height="318" alt="Screenshot (1063)" src="https://github.com/user-attachments/assets/0b6689e2-b7da-4916-99d1-5ac4859b03ea" />


4. Membuat direktori backup yang sama di kedua server sebagai storage oracle database untuk menjamin sinkronisasi database primary dan standby
   
   <img width="529" height="61" alt="Screenshot (1064)" src="https://github.com/user-attachments/assets/3660f2e9-b07e-496e-be29-7445a4e0949b" />


5. Menambahkan direktori pada disk ASM primary untuk menyimpan konfigurasi Data Guard Broker dan Offline redo log (archivelog)

   <img width="508" height="62" alt="Screenshot (1065)" src="https://github.com/user-attachments/assets/08aae930-564a-4792-8224-61c8a4c66000" />


6. Menambahkan direkori pada disk ASM standby untuk menyinkronisasikan database standby dengan primary
   - menyimpan konfigurasi database standby
   - menyimpan file konfigurasi Data Guard Broker
   - menyimpan data milik pdb, apabila menggunakan fitur multitenant
   - menyimpan file Offline redo log (archivelog)

     <img width="599" height="142" alt="Screenshot (1066)" src="https://github.com/user-attachments/assets/c2ab0618-9824-4615-bfdf-d21d7244ed06" />


7. Mengaktifkan Offline redo log (archivelog) dan pdb pada database primary, sebagai senjata utama dalam instalasi Active Data Guard. Set lokasi penyimpanan archivelog ke dalam disk ASM archive yang sebelumnya sudah dibuat, untuk memudahkan dalam management pengiriman redo/archivelog shipping dari database primary ke standby.

     <img width="818" height="803" alt="Screenshot (1069)" src="https://github.com/user-attachments/assets/8ddf10f1-de8a-41f2-8a65-97646e539622" />


8. Mengaktifkan force logging untuk memaksa menyimpan setiap perubahan yang terjadi pada database primary
   - berfungsi sebagai data integrity antar database

     <img width="561" height="443" alt="Screenshot (1070)" src="https://github.com/user-attachments/assets/b49e5abe-70ad-41e7-bb43-e956c44a5416" />


9. Set parameter log_acrhive_config pada database primary untuk mendefinisikan anggota/server yang terlibat dalam proses Data Guard ini

   <img width="792" height="310" alt="Screenshot (1071)" src="https://github.com/user-attachments/assets/3d28be82-e09a-43f4-92a7-e2ddf23efc5c" />


10. Set parameter fetch archive log (fal_server & fal_client) di database primary
    - berfungsi sebagai "automatic recovery mecahnism" jika terjadi Gap (celah) data apabila database priamry menjadi standby
    - fal_server : database yang mengirimkan atau menyediakan archive log
    - fal_client : identitas database yang meminta atau menerima data

      <img width="590" height="399" alt="Screenshot (1072)" src="https://github.com/user-attachments/assets/e8ca486f-8ed2-4ca6-b3f2-67f922d1b84c" />


11. Set standby_file_management to AUTO pada database primary untuk sinkronisasi perubahan file database primary ke standby secara otomatis
    - mencegah kegagalan sinkronisasi datafile yang dapat menyebabkan database standby menjadi crash

      <img width="573" height="425" alt="Screenshot (1073)" src="https://github.com/user-attachments/assets/1aa8117e-1942-42d7-879e-7b257ef035a4" />


12. Set LOG/DB/PDB_FILE_NAME_CONVERT pada database primary untuk sinkronisasi nama path folder apabila db primary menjadi standby
    - Mencegah kegagalan replikasi saat ada penambahan datafile atau log file baru
    - Menerjemahkan secara automatic name path dari server primary ke path server standby sehingga proses Redo Apply tidak terhenti karena kendala "File Not Found"

      <img width="836" height="850" alt="Screenshot (1074)" src="https://github.com/user-attachments/assets/b877a327-6ac6-4779-bc50-d443138fda79" />


13. Create Standby Redo Log files (SRL) di database primary sebagai Real-Time Apply pada setiap perubahan di primary ke standby melalui redo log files
    - Memastikan tidak ada yang hilang apabila db standby menjadi primary

      <img width="1005" height="828" alt="Screenshot (1075)" src="https://github.com/user-attachments/assets/722b885f-5ef9-40cc-a6c5-90f47dca6662" />


14. Create password file untuk Otentikasi Data Guard Broker sebagai managing dan monitoring antara database primary dan standby menggunakan user administratif seperti (SYS)

    <img width="1074" height="93" alt="Screenshot (1077)" src="https://github.com/user-attachments/assets/e96775e8-dbc7-47b9-bb87-31482ff6e876" />


15. Generate PFILE dari SPFILE pada database primary dan disimpan ke dalam direktori backup sebagai basis konfigurasi awal untuk database standby dan memastikan parameter krusial seperti db_file_name_convert atau fal_server) ikut ter-copy.

    <img width="598" height="287" alt="Screenshot (1080)" src="https://github.com/user-attachments/assets/61d3bd64-bd73-4fd0-9247-61325b1a474d" />


16. Generate password file pada server primary dan disimpan ke dalam direktori backup untuk sinkronisasi otentifikasi Data Guard Broker di server standby

    <img width="729" height="114" alt="Screenshot (1081)" src="https://github.com/user-attachments/assets/6e3bc82a-aaf3-40d9-8ea9-561178603110" />


17. Konfigurasi Parameter RMAN pada database primary untuk Persiapan Duplikasi database primary ke server standby

    <img width="879" height="559" alt="Screenshot (1082)" src="https://github.com/user-attachments/assets/453c8eb4-b16f-4eb7-8063-807e44e511f8" />


18. Melakukan full backup database, archivelog, dan controlfile sebagai sumber data utama (Backup Set) pada database primary yang akan dikirim ke server Standby untuk membangun database standby agar memiliki data yang identik dengan database primary

    <img width="837" height="344" alt="Screenshot (1083)" src="https://github.com/user-attachments/assets/1796d696-d9ae-4aa6-8231-3825866977fd" />


19. Pengarsipan direktori backup menggunakan utilitas TAR pada server primary untuk menyimpan seluruh komponen backup ke dalam file arsip TAR.
    - Mempermudah dan mempercepat proses distribusi data dari server Primary ke server standby untuk membangun database standby yang identik dengan primary

    <img width="613" height="459" alt="Screenshot (1089)" src="https://github.com/user-attachments/assets/cbc6cbe8-2638-410a-a63f-27c10bdf2eaa" />


20. Transfer arsip direktori backup ke server standby menggunakan protocol SCP

    <img width="1554" height="205" alt="Screenshot (1090)" src="https://github.com/user-attachments/assets/95743173-64a8-46e5-818a-b4aa4e847f1a" />


21. Verifikasi arsip direktori backup di server standby

    <img width="616" height="102" alt="Screenshot (1091)" src="https://github.com/user-attachments/assets/ad86d18d-adb7-4d6f-bad2-cb3898f2f3c5" />


22. Ektrak (Un-tar) direktori backup pada server standby

    <img width="538" height="389" alt="Screenshot (1092)" src="https://github.com/user-attachments/assets/92cd68a1-160d-4dc4-8875-bccda7b10c2d" />


23. Melakukan konfigurasi ulang Parameter File (PFILE) di server standby dengan menyesuaikan parameter spesifik (seperti db_unique_name, fal_server, dan db_file_name_convert) agar sesuai dengan konfigurasi database Primary sebelum melakukan proses instantiation database standby.

    <img width="1258" height="872" alt="Screenshot (1100)" src="https://github.com/user-attachments/assets/483aece6-6e81-4bd6-b776-0024ded1b619" />


24. Create standby instance dalam mode NOMOUNT menggunakan PFILE sebelum restore/cloning duplikasi database primary pada server standby

    <img width="890" height="420" alt="Screenshot (1101)" src="https://github.com/user-attachments/assets/a3ed2293-ca9a-432a-9d3d-a1fb48f83e50" />
    <img width="404" height="35" alt="Screenshot (1102)" src="https://github.com/user-attachments/assets/8db99d4b-e585-4180-ae0f-ed63f230005f" />
    <img width="817" height="467" alt="Screenshot (1103)" src="https://github.com/user-attachments/assets/27ba324c-9ee6-478d-8256-f2b9110b4275" />


25. Melakukan verifikasi parameter instance pada server standby untuk memastikan PFILE telah terbaca dengan benar

    <img width="804" height="765" alt="Screenshot (1104)" src="https://github.com/user-attachments/assets/6b167cea-f082-4c30-b9e2-c2b5088b9352" />


26. Melakukan verifikasi direktori backup di server standby untuk memastikan seluruh backupset yang diperlukan (full backup data, archivelog, dan controlfile) tersedia dan siap digunakan sebelum proses restorasi/duplikasi database

    <img width="731" height="304" alt="Screenshot (1106)" src="https://github.com/user-attachments/assets/536a1216-9cf7-4aff-b37e-b6519361127b" />


27. Restore dan duplikasi database standby menggunakan utilitas RMAN

    <img width="784" height="876" alt="Screenshot (1108)" src="https://github.com/user-attachments/assets/305ed8fe-0d67-4deb-a74e-5fe5d62c702a" />


28. Recover database untuk mengambil semua file backup yang ada di disk

    <img width="690" height="458" alt="Screenshot (1109)" src="https://github.com/user-attachments/assets/4fa00327-f620-414e-9d09-8cff554857b6" />


29. 




    


    




    






    









