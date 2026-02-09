Pada sesi kali ini, saya akan mendemonstrasikan acive data guard physical standby menggunakan utility broker sebagai management data guard pada server grid oracle restart. Demonstrasi ini bertujuan untuk mencapai High availibility dengan zero downtime.


Hal yang dipersiapkan:
- 1 Server database primary menggunakan grid oracle restart
- 1 Server standby menggunakan grid oracle restart
- Large storage capacity
- ASM disk groups
- Broker sebagai management data guard





1. Verifikasi cluster resource dan listener sudah berjalan (online) 

   <img width="1599" height="879" alt="Screenshot (1057)" src="https://github.com/user-attachments/assets/095f59e6-6ce4-4197-bd37-17cdc1cba728" />


2. Konfigurasi name resolution local server dengan mencantumkan IP dan hostname masing-masing server. Memastikan server primary mengenali standby di level OS sebelum inisialisasi data guard

   <img width="789" height="125" alt="Screenshot (1062)" src="https://github.com/user-attachments/assets/94685cd2-d752-4c52-b7de-2f39e68e27dd" />


3. Melakukan test connection via ping untuk memverifikasi jalur interconnect dan redo transport bebas dari blokade firewall

   <img width="849" height="318" alt="Screenshot (1063)" src="https://github.com/user-attachments/assets/0b6689e2-b7da-4916-99d1-5ac4859b03ea" />


4. Membuat direktori backup lokal di level OS pada kedua node. Menjamin keseragaman jalur restorasi RMAN untuk membuat duplikasi database dan sebagai jalur redo transport menuju standby
   
   <img width="529" height="61" alt="Screenshot (1064)" src="https://github.com/user-attachments/assets/3660f2e9-b07e-496e-be29-7445a4e0949b" />


5. Add direktori pada ASM diskgroups untuk alokasi storage file konfigurasi Data Guard Broker dan offline log (archivelog)

   <img width="508" height="62" alt="Screenshot (1065)" src="https://github.com/user-attachments/assets/08aae930-564a-4792-8224-61c8a4c66000" />


6. Inisialisasi struktur folder ASM pada server standby untuk menampung replica datafiles, archivelogs, dan komponen PDB.
   - menyimpan konfigurasi database standby
   - menyimpan file konfigurasi Data Guard Broker
   - menyimpan data milik pdb, apabila menggunakan fitur multitenant
   - menyimpan file Offline redo log (archivelog)

     <img width="599" height="142" alt="Screenshot (1066)" src="https://github.com/user-attachments/assets/c2ab0618-9824-4615-bfdf-d21d7244ed06" />


7. Memastikan database dalam mode ARCHIVELOG melalui perintah archive log list sebagai syarat mutlak redo shipping data guard

     <img width="818" height="803" alt="Screenshot (1069)" src="https://github.com/user-attachments/assets/8ddf10f1-de8a-41f2-8a65-97646e539622" />


8. Validasi status FORCE_LOGGING untuk menjamin semua transaksi dan perubahan data tercatat secara permanen tanpa kecuali
   - berfungsi sebagai data integrity antar database

     <img width="561" height="443" alt="Screenshot (1070)" src="https://github.com/user-attachments/assets/b49e5abe-70ad-41e7-bb43-e956c44a5416" />


9. Konfigurasi parameter LOG_ARCHIVE_CONFIG untuk mendefinisikan anggota/server Data Guard Cluster

   <img width="792" height="310" alt="Screenshot (1071)" src="https://github.com/user-attachments/assets/3d28be82-e09a-43f4-92a7-e2ddf23efc5c" />


10. Setup parameter FAL_SERVER dan FAL_CLIENT sebagai mekanisme otomatisasi recovery penanganan gap data antar database
    - fal_server : database yang mengirimkan atau menyediakan archive log
    - fal_client : identitas database yang meminta atau menerima data

      <img width="590" height="399" alt="Screenshot (1072)" src="https://github.com/user-attachments/assets/e8ca486f-8ed2-4ca6-b3f2-67f922d1b84c" />


11. Set parameter STANDBY_FILE_MANAGEMENT ke AUTO untuk sinkronisasi otomatis perubahan struktur fisik antar database
    - mencegah kegagalan sinkronisasi datafile yang dapat menyebabkan database standby menjadi crash

      <img width="573" height="425" alt="Screenshot (1073)" src="https://github.com/user-attachments/assets/1aa8117e-1942-42d7-879e-7b257ef035a4" />


12. Konfigurasi DB_FILE_NAME_CONVERT dan LOG_FILE_NAME_CONVERT untuk pemetaan otomatis antar diskgroup ASM yang berbeda
    - Mencegah gagalnya replikasi apabila ada penambahan datafile atau log file baru
    - Menerjemahkan secara otomatis name path dari server primary ke path server standby sehingga proses Redo Apply tidak terhenti karena kendala "File Not Found"

      <img width="836" height="850" alt="Screenshot (1074)" src="https://github.com/user-attachments/assets/b877a327-6ac6-4779-bc50-d443138fda79" />


13. Inisialisasi grup standby redo logs (SRL) dengan size memory yang identik dengan online redo log untuk mendukung fitur Real-Time Apply
    - Memastikan tidak ada data yang hilang apabila database standby menjadi primary

      <img width="1005" height="828" alt="Screenshot (1075)" src="https://github.com/user-attachments/assets/722b885f-5ef9-40cc-a6c5-90f47dca6662" />


14. Create password file (orapwd) untuk otentikasi remote koneksi antar database menggunakan Data Guard Broker

    <img width="1074" height="93" alt="Screenshot (1077)" src="https://github.com/user-attachments/assets/e96775e8-dbc7-47b9-bb87-31482ff6e876" />


15. Generate SPFILE menjadi PFILE sebagai basis konfigurasi awal duplikasi database untuk standby

    <img width="598" height="287" alt="Screenshot (1080)" src="https://github.com/user-attachments/assets/61d3bd64-bd73-4fd0-9247-61325b1a474d" />


16. Sinkronisasi password file (orapwd) ke direktori backup untuk memastikan kredensial user SYS identik di kedua server

    <img width="729" height="114" alt="Screenshot (1081)" src="https://github.com/user-attachments/assets/6e3bc82a-aaf3-40d9-8ea9-561178603110" />


17. Konfigurasi Parameter RMAN pada database primary untuk persiapan duplikasi database primary ke server standby

    <img width="879" height="559" alt="Screenshot (1082)" src="https://github.com/user-attachments/assets/453c8eb4-b16f-4eb7-8063-807e44e511f8" />


18. Melakukan full backup (Data, Archive, & Controlfile) sebagai sumber data utama membuat database standby

    <img width="837" height="344" alt="Screenshot (1083)" src="https://github.com/user-attachments/assets/1796d696-d9ae-4aa6-8231-3825866977fd" />


19. Pengarsipan seluruh komponen backup menggunakan utilitas tar untuk efisiensi distribusi data
    - Mempermudah dan mempercepat proses distribusi data dari server Primary ke server standby untuk membangun database standby yang identik dengan primary

    <img width="613" height="459" alt="Screenshot (1089)" src="https://github.com/user-attachments/assets/cbc6cbe8-2638-410a-a63f-27c10bdf2eaa" />


20. Melakukan transfer file arsip backup ke server standby menggunakan protokol aman SCP

    <img width="1554" height="205" alt="Screenshot (1090)" src="https://github.com/user-attachments/assets/95743173-64a8-46e5-818a-b4aa4e847f1a" />


21. Validasi eksistensi dan ukuran file backup di server standby pasca-transfer

    <img width="616" height="102" alt="Screenshot (1091)" src="https://github.com/user-attachments/assets/ad86d18d-adb7-4d6f-bad2-cb3898f2f3c5" />


22. Melakukan extract (un-tar) pada server standby guna mempersiapkan file untuk proses restorasi RMAN

    <img width="538" height="389" alt="Screenshot (1092)" src="https://github.com/user-attachments/assets/92cd68a1-160d-4dc4-8875-bccda7b10c2d" />


23. Modifikasi parameter spesifik standby (seperti db_unique_name dan fal_server) pada file konfigurasi fisik (PFILE) untuk menentukan konfigurasi database standby

    <img width="1258" height="872" alt="Screenshot (1100)" src="https://github.com/user-attachments/assets/483aece6-6e81-4bd6-b776-0024ded1b619" />


24. Inisialisasi instance standby pada level NOMOUNT untuk alokasi SGA dan background process sebelum melakukan cloning database standby

    <img width="890" height="420" alt="Screenshot (1101)" src="https://github.com/user-attachments/assets/a3ed2293-ca9a-432a-9d3d-a1fb48f83e50" />
    <img width="404" height="35" alt="Screenshot (1102)" src="https://github.com/user-attachments/assets/8db99d4b-e585-4180-ae0f-ed63f230005f" />
    <img width="817" height="467" alt="Screenshot (1103)" src="https://github.com/user-attachments/assets/27ba324c-9ee6-478d-8256-f2b9110b4275" />


25. Validasi ulang parameter instance pasca-startup untuk memastikan PFILE terbaca secara akurat

    <img width="804" height="765" alt="Screenshot (1104)" src="https://github.com/user-attachments/assets/6b167cea-f082-4c30-b9e2-c2b5088b9352" />


26. Verifikasi exsistensi file backup melalui RMAN standby. Memastikan semua backupset (full backup database, archivelog, dan controlfile) tersedia sebelum proses restorasi

    <img width="731" height="304" alt="Screenshot (1106)" src="https://github.com/user-attachments/assets/536a1216-9cf7-4aff-b37e-b6519361127b" />


27. Restorasi database standby menggunakan utilitas RMAN 

    <img width="784" height="876" alt="Screenshot (1108)" src="https://github.com/user-attachments/assets/305ed8fe-0d67-4deb-a74e-5fe5d62c702a" />


28. Eksekusi perintah RECOVER untuk sinkronisasi data hingga titik waktu konsisten data terakhir

    <img width="690" height="458" alt="Screenshot (1109)" src="https://github.com/user-attachments/assets/4fa00327-f620-414e-9d09-8cff554857b6" />


29. Mendaftarkan database standby ke dalam Grid Infrastructure menggunakan utilitas srvctl

    <img width="566" height="143" alt="Screenshot (1116)" src="https://github.com/user-attachments/assets/f2522ab2-bacd-409b-a720-d9c6a739b56e" />
    - db : nama unik database standby
    - o : lokasi direcroti oracle software
    - p : lokasi PFILE database standby
    - r : role database standby
    - s : start option
    - n : nama database


30. Validasi dan penyesuaian nama database instance agar selaras dengan konfigurasi Clusterware

    <img width="655" height="726" alt="Screenshot (1117)" src="https://github.com/user-attachments/assets/1fc55ecb-0cf7-4a02-8720-7b55428274cf" />


31. Memastikan database standby berada dalam mode MOUNT (active) dan proses PMON aktif di level OS

    <img width="726" height="180" alt="Screenshot (1120)" src="https://github.com/user-attachments/assets/7b408694-b2c3-44c1-bfde-ceef1692024c" />
    <img width="727" height="83" alt="Screenshot (1058)" src="https://github.com/user-attachments/assets/fd6f48e4-2c5c-4fa8-acd0-948c43cb4f44" />


32. Verifikasi konektivitas layer aplikasi antar server menggunakan utilitas tnsping

    <img width="1600" height="526" alt="Screenshot (1121)" src="https://github.com/user-attachments/assets/e81a62ca-7faf-43d7-9bd4-4af2b94cc96e" />


33. Melakukan duplikasi password file (orapwd) ke server standby untuk menjamin kelancaran komunikasi Broker

    <img width="1232" height="328" alt="Screenshot (1122)" src="https://github.com/user-attachments/assets/b7203687-7870-47da-900f-a9fd8c2de5df" />


34. Verifikasi status Listener menggunakan lsnrctl. Memastikan service database standby telah teregistrasi dan berjalan
    <img width="844" height="685" alt="Screenshot (1124)" src="https://github.com/user-attachments/assets/7f5188b8-7481-42e5-9295-e880d00547e5" />


35. Aktivasi pluggable database (PDB) pada sisi standby sebagai target testing replikasi

    <img width="635" height="501" alt="Screenshot (1125)" src="https://github.com/user-attachments/assets/5e613b87-b01b-4e18-b573-e22d31046a0e" />


36. Konfigurasi lokasi file DG_BROKER_CONFIG secara redundan pada diskgroup ASM yang berbeda. Memastikan diskgroup ASM dapat diakses secara terpusat oleh broker

    <img width="829" height="452" alt="Screenshot (1126)" src="https://github.com/user-attachments/assets/d7c0c227-e9ee-4b5d-a163-4b8a8d0aba32" />
    <img width="900" height="452" alt="Screenshot (1127)" src="https://github.com/user-attachments/assets/2ca1664f-eb61-451c-a693-9cfbc28ed868" />


37. Mengaktifkan Data Guard Broker dengan mengubah parameter DG_BROKER_START=TRUE. Memastikan proses background DMON berjalan untuk mulai mengelola cluster database

    <img width="1920" height="1080" alt="Screenshot (1129)" src="https://github.com/user-attachments/assets/d1b8d5b3-08c3-47a4-9e6f-2f560627b512" />\


38. Membuat objek konfigurasi dan aktivasi koneksi database primary dan stanby menggunakan utility DGMGRL

    <img width="1025" height="316" alt="Screenshot (1129)" src="https://github.com/user-attachments/assets/7268917e-d15b-4ba2-b7db-40c82cd32f27" />


39. Validasi status konfigurasi melalui SHOW CONFIGURATION dengan hasil akhir SUCCESS

    <img width="807" height="806" alt="Screenshot (1130)" src="https://github.com/user-attachments/assets/bcf875a8-9723-42ce-9418-1e987d72531d" />
    - protection mode : maxPerformance = data replikasi dikirim secara asyncronous 


40. Verifikasi status sinkronisasi log dan proses background pada standby
    - Memastikan database berada dalam mode READ ONLY WITH APPLY (Active Data Guard)
    - Database standby dapat dibaca, sementara proses MRP terus menerapkan (apply) perubahan dari Primary

    <img width="1166" height="423" alt="Screenshot (1131)" src="https://github.com/user-attachments/assets/9c063e6c-8477-4fb1-ade1-ed5c3444cd6e" />


41. Injeksi data (INSERT) pada database primary untuk memicu pengiriman redo data secara real-time melalui prosess NSS/NSA

    <img width="644" height="555" alt="Screenshot (1133)" src="https://github.com/user-attachments/assets/301ddda7-cc19-419e-9f32-28471cccfd06" />


42. Verifikasi data pada sisi Standby. Membuktikan keberhasilan replikasi data tanpa latency

    <img width="662" height="435" alt="Screenshot (1134)" src="https://github.com/user-attachments/assets/a5930ff5-a0e5-495f-b07d-16b022110153" />
    - Keberadaan tabel dan data yang identik dengan Primary membuktikan bahwa fitur Real-Time Apply telah berhasil mereplikasi perubahan secara instan






    







    





    


    




    






    









