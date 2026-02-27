Pada sesi kali ini, saya akan melakukan instalasi dan konfigurasi Oracle 19c Grid Infrastructure Standalone Server (oracle restart) pada lingkungan RHEL. Bertujuan untuk mengimplementasi dependency management untuk mengotomatisasi resource lifecycle seperti Oracle ASM, Listener, dan Database. Memastikan ketersediaan layanan yang optimal pada server tunggal


hal yang dipersiapkan:
- Infrastructure     : Virtual machine (RHEL compatible)
- Storage management : Oracle ASM (Automatic Storage Management)
- Software           : Oracle Grid Infrastructure 19c & Oracle Database 19c




1. Melakukan verifikasi spesifikasi dan versi OS. Memastikan compatable dengan Oracle Database 19c

   <img width="930" height="409" alt="Screenshot (633)" src="https://github.com/user-attachments/assets/523bd327-dcfa-4f5b-90af-57350ba85049" />


2. Melakukan konfigurasi Name Resolution lokal ke dalam file /etc/hosts. Bertujuan agar saat instalasi Oracle Grid Infrastructure, sistem dapat mengenali nama server (hostname) database

   <img width="815" height="173" alt="Screenshot (631)" src="https://github.com/user-attachments/assets/e73763c6-a820-4ae4-b200-4a0b1d494f0b" />


3. Mengubah hostname sistem agar sesuai dengan identitas server yang telah didaftarkan pada file /etc/hosts. Memastikan konsistensi identitas server saat proses validasi Oracle Grid Infrastructure.

   <img width="758" height="139" alt="Screenshot (746)" src="https://github.com/user-attachments/assets/75de8953-146e-43c8-adbd-be73206726d9" />

   
4. Verifikasi loopback networking menggunakan ping. Memastikan layer network siap merespons permintaan layanan Oracle

   <img width="942" height="215" alt="Screenshot (747)" src="https://github.com/user-attachments/assets/16c3550d-c896-4ad6-8072-298daa6d0c6e" />


5. Menginstalasi paket oracle-database-preinstall-19c untuk mengotomatisasi konfigurasi kernel, user, dan group sesuai best practice Oracle

   <img width="1694" height="284" alt="Screenshot (632)" src="https://github.com/user-attachments/assets/e5197dd4-fc90-44b1-ab22-b9d318fd2faa" />


6. Verifikasi parameter kernel sistem pasca-instalasi paket pre-install. Memastikan kepatuhan terhadap threshold minimum Oracle 19c

   <img width="583" height="320" alt="Screenshot (634)" src="https://github.com/user-attachments/assets/7d362920-d5f3-4304-b8a8-abdae41a20fc" />
   - parameter kernel secara default sudah sesuai dengan persyataran minimum instalasi oracle 19c


7. Membuat grup sistem tambahan untuk memisahkan hak akses administratif antara manajemen storage (ASM) dan manajemen database

   <img width="723" height="87" alt="Screenshot (635)" src="https://github.com/user-attachments/assets/cc22f08b-ca1c-423c-adc2-7976ee03196c" />


8. update hak akses user oracle dan grid agar memiliki wewenang yang tepat dalam mengelola infrastucture resources

   <img width="1162" height="100" alt="Screenshot (637)" src="https://github.com/user-attachments/assets/b298793a-704a-4b98-bc92-96541a8574a2" />


9. Melakukan konfigurasi security hardening untuk akun sistem yang akan menjalankan infrastructure resources grid dan oracle database

   <img width="910" height="307" alt="Screenshot (639)" src="https://github.com/user-attachments/assets/1bcad034-2bef-4d61-a938-0a26cffeda99" />


10. Konfigurasi batasan proses (soft/hard limits) pada /etc/security/limits.conf membatasi penggunaan sumber daya sistem (resource limits) bagi setiap user atau grup

    <img width="436" height="344" alt="Screenshot (748)" src="https://github.com/user-attachments/assets/39edce24-4d78-4813-86a6-136c14ba9e4d" />


11. Membangun struktur direktori berdasarkan standar Optimal Flexible Architecture (OFA) untuk memisahkan biner software, konfigurasi, dan log inventory

    <img width="1558" height="147" alt="Screenshot (641)" src="https://github.com/user-attachments/assets/94a966f6-a78d-42af-bf8f-4115d2308d2d" />
    - oracle_base : merupakan Direktori "induk" yang menampung file log, file konfigurasi, dan berbagai versi perangkat lunak Oracle
    - oracle_home : merupakan direktori spesifik tempat biner (program/aplikasi) Oracle Grid atau Database terinstal
    - inventory   : folder yang mencatat semua software Oracle terinstal di server tersebut. penting untuk proses patching dan upgrade


12. Konfigurasi environment variables pada file .bash_profile untuk masing-masing user grid dan oracle. Memudahkan eksekusi perintah administratif Oracle tanpa perlu mengetik lokasi folder secara manual

      <img width="806" height="259" alt="Screenshot (642)" src="https://github.com/user-attachments/assets/a7c4303c-dce4-41d9-af65-784ffefba99e" />
      <img width="827" height="259" alt="Screenshot (645)" src="https://github.com/user-attachments/assets/2aa3d6e4-1984-450a-b026-c14e5e95dc49" />


13. Stop dan disabled service firewall atau melakukan filtering port untuk menjamin komunikasi tanpa hambatan antara Listener, ASM, dan Database

    <img width="1683" height="337" alt="Screenshot (647)" src="https://github.com/user-attachments/assets/ce6828cf-4b54-45ca-b163-54b4a04f2ca0" />


14. Menambahkan virtual disk tambahan sebagai raw storage yang digunakan sebagai manage storage unit oleh Oracle ASM

    <img width="695" height="235" alt="Screenshot (749)" src="https://github.com/user-attachments/assets/7d9455a9-89d0-401a-a3c0-97cbb8f4b968" />
    <img width="863" height="248" alt="Screenshot (750)" src="https://github.com/user-attachments/assets/fc820a92-c6fb-4713-90b2-cd4f3ae43f96" />


15 Mengidentifikasi new virtual disk menggunakan utilitas lsblk sebelum dilakukan proses partisi

   <img width="499" height="244" alt="Screenshot (752)" src="https://github.com/user-attachments/assets/57f5a7d7-036d-4338-893c-d2c9705336ad" />


16. Membuat partisi pada additional disk untuk didaftarkan sebagai anggota ASM Disk Group

    <img width="781" height="541" alt="Screenshot (648)" src="https://github.com/user-attachments/assets/c6e1a2de-4286-4323-b9b1-3b92c4176c4d" />
    <img width="792" height="541" alt="Screenshot (649)" src="https://github.com/user-attachments/assets/f6f48b83-69a6-4466-b5ca-8e193fb29532" />
    <img width="560" height="290" alt="Screenshot (650)" src="https://github.com/user-attachments/assets/c62d611b-eb81-42a2-859a-5f7c490bee67" />

    
17. Membuat direktori temporer untuk penempatan file software installer sebelum proses extract file dilakukan

    <img width="648" height="56" alt="Screenshot (755)" src="https://github.com/user-attachments/assets/b733cd09-4f1e-4de1-a340-ce8578799951" />


18. Konfigurasi UDEV RULES. Memastikan identitas, ownership, dan hak akses disk ASM tetap konsisten (persisten) setelah sistem dijalankan ulang

    <img width="1501" height="129" alt="Screenshot (656)" src="https://github.com/user-attachments/assets/68096996-b500-4f49-9a8e-14b07499c94f" />


19. Menerapkan aturan UDEV dan memverifikasi pembuatan symbolic link untuk memastikan aksesibilitas disk oleh user grid

    <img width="714" height="141" alt="Screenshot (657)" src="https://github.com/user-attachments/assets/6f74b4fc-8295-439c-b48e-918c75758765" />


20. Mengubah ownership perangkat disk ke user grid sebagai syarat mutlak instalasi Grid Infrastructure

    <img width="808" height="150" alt="Screenshot (754)" src="https://github.com/user-attachments/assets/6a35a8ae-29db-4d3a-88f5-db36b40f8d9c" />


21. Install file biner Oracle 19c Grid & Database ke dalam server

    <img width="1709" height="601" alt="Screenshot (756)" src="https://github.com/user-attachments/assets/1600a482-60a1-4674-8062-0aca9a3448ab" />


22. Mengaktifkan fitur shared folder pada VM untuk transfer data antara host (desktop) dan guest (server OS)

    <img width="852" height="318" alt="Screenshot (757)" src="https://github.com/user-attachments/assets/e1d970fd-3f1d-4682-b09a-19f09d39ea70" />


23. Memindahkan file installer dari direktori mount ke server local storage untuk mempercepat proses ekstraksi

    <img width="816" height="218" alt="Screenshot (758)" src="https://github.com/user-attachments/assets/1f278ce4-29b3-43de-836c-9a5106118f53" />


24. Melakukan proses mount pada media shared. Memastikan aksesibilitas file biner oleh sistem

    <img width="1097" height="188" alt="Screenshot (660)" src="https://github.com/user-attachments/assets/5823f8b9-56fd-40ce-86c1-49a65854c20f" />


25. Copy file installer ke masing-masing direktori $ORACLE_HOME grid & oracle home untuk menjaga isolasi antar software

    <img width="1086" height="106" alt="Screenshot (661)" src="https://github.com/user-attachments/assets/19bdd2fe-0a85-4380-b824-cb785d26b40e" />


26. Validasi eksistensi file installer di direktori $ORACLE_HOME. Memastikan proses file copy berhasil sebelum dilakukan ekstraksi

    <img width="806" height="155" alt="Screenshot (661) - Copy" src="https://github.com/user-attachments/assets/69bf36e5-ab9e-4950-8dca-9e1e95415ff8" />


27. Melakukan extract paket instalasi Grid Infrastructure dan Database menggunakan user yang relevan

    <img width="815" height="337" alt="Screenshot (663)" src="https://github.com/user-attachments/assets/740d4f43-2c9a-4665-994a-848c1d64714f" />
    <img width="787" height="202" alt="Screenshot (707)" src="https://github.com/user-attachments/assets/c5a2df44-d277-42f6-a4cc-7e4b257446ba" />



28. Menghapus file arsip (.zip) installer setelah proses extract selesai untuk mengoptimalkan penggunaan disk storage system

    <img width="741" height="174" alt="Screenshot (664)" src="https://github.com/user-attachments/assets/0988e19a-a3c4-4bd4-b3b0-9bf623b2b0ca" />
    <img width="723" height="199" alt="Screenshot (708)" src="https://github.com/user-attachments/assets/3cda205f-0fc2-488a-9c8a-435f01b301ca" />


29. Inisialisasi Grid Setup dengan menjalankan skrip gridSetup.sh dengan parameter cv_assume_distid untuk memanipulasi identifikasi OS agar compatable dengan installer
    
    <img width="690" height="137" alt="Screenshot (667)" src="https://github.com/user-attachments/assets/2b6935e8-d49b-44bb-ad07-a77daf3f1a36" />
    


30. Konfigurasi Oracle Restart dengan opsi Standalone Server untuk manajemen high availibility

    <img width="991" height="773" alt="Screenshot (670)" src="https://github.com/user-attachments/assets/1000968f-3c0a-40d3-bece-2b1df0e9b164" />
    <img width="985" height="782" alt="Screenshot (671)" src="https://github.com/user-attachments/assets/d3d99536-0a0e-44f2-a23e-b6d35b7544e0" />
    <img width="992" height="771" alt="Screenshot (673)" src="https://github.com/user-attachments/assets/506d96f4-4589-44fb-9547-e489cbcc2429" />
    <img width="998" height="785" alt="Screenshot (674)" src="https://github.com/user-attachments/assets/61612c4c-df7a-460a-ad26-a28ac127b1c0" />
    <img width="996" height="775" alt="Screenshot (675)" src="https://github.com/user-attachments/assets/8d2b6f66-afba-408f-8cd2-1e2d49038313" />
    <img width="992" height="773" alt="Screenshot (676)" src="https://github.com/user-attachments/assets/3a71ff81-20ce-4fce-a577-e3b741f119d1" />
    <img width="990" height="774" alt="Screenshot (677)" src="https://github.com/user-attachments/assets/355f3aee-f7ff-48b6-b021-e5ab82c05b35" />
    <img width="985" height="779" alt="Screenshot (678)" src="https://github.com/user-attachments/assets/88a9a0f9-09c5-4bf9-a544-dbbfb500986e" />


31. Melakukan konfigurasi ASM Disk Group menggunakan ASMCA (ASM Configuration Assistant) untuk membangun Disk Group DATA (datafiles) dan ARCHIVE (recovery/backup) pada layer ASM
    
    <img width="1183" height="800" alt="Screenshot (705)" src="https://github.com/user-attachments/assets/e1503fc5-60d3-4c41-acee-c0103d533989" />



32. Melakukan verifikasi awal terhadap status operasional resource Oracle Grid Infrastructure (seperti ASM, Listener, dan Disk Group). Memastikan komponen sudah terdaftar dan berjalan (ONLINE)

     <img width="826" height="523" alt="Screenshot (706)" src="https://github.com/user-attachments/assets/668b9557-69c8-4396-9664-4eb61fe0c180" />


33. Inisialisasi Database Installer dengan menjalankan perintah runInstaller untuk memulai proses instalasi database engine Oracle 19c

    <img width="542" height="134" alt="Screenshot (759)" src="https://github.com/user-attachments/assets/ce6065ce-12e0-488e-8d78-b790cf525874" />


34. Melakukan konfigurasi Instalasi Software Oracle Database 19c Enterprise Edition utnuk menentukan lokasi $ORACLE_BASE, $ORACLE_HOME, dan pemetaan group sistem
    
    <img width="1005" height="780" alt="Screenshot (709)" src="https://github.com/user-attachments/assets/cbff8bca-05d5-4c75-8696-f74b70572e05" />
    <img width="998" height="793" alt="Screenshot (710)" src="https://github.com/user-attachments/assets/d53538d0-55b2-4fa2-a802-3cefd49c7dae" />
    <img width="996" height="774" alt="Screenshot (711)" src="https://github.com/user-attachments/assets/c0adcc74-c311-4de6-a283-c688ed0ea7bd" />
    <img width="1001" height="792" alt="Screenshot (712)" src="https://github.com/user-attachments/assets/678c02f8-3c88-481c-b778-4b6987a0f075" />
    <img width="994" height="785" alt="Screenshot (713)" src="https://github.com/user-attachments/assets/0ddb9a87-d070-4c6e-bc4a-d5fa3cee7882" />
    <img width="997" height="788" alt="Screenshot (714)" src="https://github.com/user-attachments/assets/96377c3f-d1f3-40ca-a3a7-3bd0c4287704" />
    <img width="1003" height="783" alt="Screenshot (715)" src="https://github.com/user-attachments/assets/cf470743-ddc5-478c-b6ff-294369b375b1" />


35. Validasi Aksesibilitas Utilitas SQL*Plus dan Konfigurasi Environment dapat diakses setelah melakukan instalasi software oracle database 19c

    <img width="612" height="100" alt="Screenshot (717)" src="https://github.com/user-attachments/assets/c37dc139-bc97-4657-b8a3-bef68a9dda83" />


36. Create database menggunakan utility database configuration assistent (DBCA) untuk membuat instance database pada storage ASM secara otomatis

    <img width="995" height="787" alt="Screenshot (718)" src="https://github.com/user-attachments/assets/77b68168-928a-4dc2-8bb4-d3afc1915006" />
    <img width="999" height="779" alt="Screenshot (721)" src="https://github.com/user-attachments/assets/3d69fc8c-1afc-4a0a-9c6d-1bd808695f4e" />
    <img width="999" height="777" alt="Screenshot (722)" src="https://github.com/user-attachments/assets/8578d7b8-8231-44dc-84f4-044d5da4e7d9" />
    <img width="992" height="780" alt="Screenshot (723)" src="https://github.com/user-attachments/assets/9bc0235a-4d27-4996-bd06-b96ec2450a65" />
    <img width="993" height="769" alt="Screenshot (724)" src="https://github.com/user-attachments/assets/d93e4a2f-9e70-43d8-8dcc-17db1c4a7a8f" />
    <img width="991" height="785" alt="Screenshot (725)" src="https://github.com/user-attachments/assets/80be1a88-973d-4325-a29a-22b47885bed5" />
    <img width="1002" height="782" alt="Screenshot (726)" src="https://github.com/user-attachments/assets/c58a4c37-3ae2-4365-93fc-b20aca215d66" />
    <img width="1004" height="793" alt="Screenshot (727)" src="https://github.com/user-attachments/assets/0e7cde58-98de-4b7b-a8d7-c474e65f4afb" />
    <img width="988" height="775" alt="Screenshot (729)" src="https://github.com/user-attachments/assets/aa3f1a12-d211-417a-b50d-78a6b92d717f" />
    <img width="999" height="790" alt="Screenshot (730)" src="https://github.com/user-attachments/assets/144224b3-2594-4d0d-bee1-bfc14021c34a" />
    <img width="997" height="791" alt="Screenshot (731)" src="https://github.com/user-attachments/assets/0ed9ba28-e41d-402d-a633-117e23f49d98" />
    <img width="992" height="790" alt="Screenshot (732)" src="https://github.com/user-attachments/assets/4e6df0cf-df19-4951-ae0f-0c04d5a1f435" />
    <img width="995" height="781" alt="Screenshot (735)" src="https://github.com/user-attachments/assets/418c2743-7104-4e69-8ee8-d16571249abf" />


37. Melakukan verifikasi lokasi Alert Log, Diagnostic Destination, dan file parameter (SPFILE). Memastikan pengelolaan metadata database berjalan benar

    <img width="870" height="412" alt="Screenshot (762)" src="https://github.com/user-attachments/assets/e1b8bda3-3b17-4033-b3c7-8e72b2a29246" />
    <img width="872" height="496" alt="Screenshot (760)" src="https://github.com/user-attachments/assets/541cb599-9152-4283-aaa8-8be9ce66d9e9" />
    <img width="783" height="148" alt="Screenshot (761)" src="https://github.com/user-attachments/assets/07d794e4-8670-4031-b70b-c5b06d347ae7" />
    - diagnostic_dest merupakan "pintu masuk" utama untuk semua aktivitas monitoring dan troubleshooting database Oracle



38. Melakukan verifikasi akhir pada status instance database. Memastikan database dalam keadaan OPEN
        
    <img width="780" height="682" alt="Screenshot (763)" src="https://github.com/user-attachments/assets/828e08c1-f1c2-4ea0-afa6-81afa18a900d" />



39. Verifikasi resource database. Memastikan database terdaftar di dalam grid infrastucture resources

    <img width="851" height="619" alt="Screenshot (738)" src="https://github.com/user-attachments/assets/36e6248b-86d3-4903-aa32-8eafd910d8d4" />


40. Verifikasi komprehensif dengan melakukan audit terhadap status operasional komponen (Listener, ASM, Database). Memastikan sistem siap digunakan di lingkungan produksi
    
    <img width="674" height="369" alt="Screenshot (740)" src="https://github.com/user-attachments/assets/d094abbe-236e-4a3d-831f-06f5efa696ee" />
    <img width="959" height="676" alt="Screenshot (741)" src="https://github.com/user-attachments/assets/bef250f1-049b-4e44-b951-3d78da998a6f" />
    <img width="1179" height="353" alt="Screenshot (764)" src="https://github.com/user-attachments/assets/973c80dd-45d6-4b25-b1e7-c7c4d96feb73" />




 




    


















    

    


















    





   






   


   

   
