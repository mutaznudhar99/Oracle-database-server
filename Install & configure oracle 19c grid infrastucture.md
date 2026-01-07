Pada sesi kali ini, saya akan melakukan instalasi dan konfigurasi Oracle 19c Grid Infrastructure standalone server (Oracle Restart). Tujuannya adalah untuk menjalankan database pada server tunggal sekaligus mempelajari dependency management, khususnya dalam mengelola siklus hidup resource seperti Oracle ASM, Listener, dan Database.



hal yang dipersiapkan:
- virtual machine
- iso rhel versi kompatibel dengan 19c
- file 19c grid infrastucture
- file 19c database
- tambahan disks untuk storage Data dan Archive.




1. cek informasi mengenai identitas server OS

   <img width="930" height="409" alt="Screenshot (633)" src="https://github.com/user-attachments/assets/523bd327-dcfa-4f5b-90af-57350ba85049" />


2. Melakukan konfigurasi Name Resolution lokal dengan menginput IP server ke dalam file /etc/hosts. Hal ini bertujuan agar saat instalasi Oracle Grid Infrastructure, sistem dapat mengenali nama server (hostname) dan memetakannya ke alamat IP secara tepat.

   <img width="815" height="173" alt="Screenshot (631)" src="https://github.com/user-attachments/assets/e73763c6-a820-4ae4-b200-4a0b1d494f0b" />


3. Mengubah hostname sistem agar sesuai dengan identitas server yang telah didaftarkan pada file /etc/hosts. memastikan konsistensi identitas server saat proses validasi Oracle Grid Infrastructure.

   <img width="758" height="139" alt="Screenshot (746)" src="https://github.com/user-attachments/assets/75de8953-146e-43c8-adbd-be73206726d9" />

   
4. Melakukan verifikasi konfigurasi file /etc/hosts untuk memastikan bahwa mekanisme name resolution lokal sudah berfungsi.

   <img width="942" height="215" alt="Screenshot (747)" src="https://github.com/user-attachments/assets/16c3550d-c896-4ad6-8072-298daa6d0c6e" />
   - icmp_seq, ttl, time : mekanisme name resolution local berfungsi dengan baik


5. Menginstal paket oracle-database-preinstall-19c dari repository resmi.

   <img width="1694" height="284" alt="Screenshot (632)" src="https://github.com/user-attachments/assets/e5197dd4-fc90-44b1-ab22-b9d318fd2faa" />


6. verifikasi parameter kernel setelah menginstall paket oracle-database-preinstall.

   <img width="583" height="320" alt="Screenshot (634)" src="https://github.com/user-attachments/assets/7d362920-d5f3-4304-b8a8-abdae41a20fc" />
   - parameter kernel secara default sudah sesuai dengan persyataran minimum instalasi oracle 19c


7. add grup sistem baru yang diperlukan oleh Oracle

   <img width="723" height="87" alt="Screenshot (635)" src="https://github.com/user-attachments/assets/cc22f08b-ca1c-423c-adc2-7976ee03196c" />

8. update anggota user yang dimiliki oleh oracle untuk mengakses layanan grid atau oracle database

   <img width="1162" height="100" alt="Screenshot (637)" src="https://github.com/user-attachments/assets/b298793a-704a-4b98-bc92-96541a8574a2" />


9. setting password untuk user sistem grid dan oracle database

   <img width="910" height="307" alt="Screenshot (639)" src="https://github.com/user-attachments/assets/1bcad034-2bef-4d61-a938-0a26cffeda99" />


10. konifgurasi shell limits pada file /etc/security/limits.conf untuk membatasi jumlah proses yang dijalankan oleh user grid atau oracle agar database tetap berjalan sesuai dengan standar performa oracle 19c.

    <img width="436" height="344" alt="Screenshot (748)" src="https://github.com/user-attachments/assets/39edce24-4d78-4813-86a6-136c14ba9e4d" />


11. Membuat struktur direktori instalasi Oracle Base, Oracle Home, dan Oracle Inventory, kemudian mengatur hak kepemilikan (Ownership) agar dapat diakses oleh user grid dan oracle

    <img width="1558" height="147" alt="Screenshot (641)" src="https://github.com/user-attachments/assets/94a966f6-a78d-42af-bf8f-4115d2308d2d" />
    - oracle_base : merupakan Direktori "induk" yang menampung file log, file konfigurasi, dan berbagai versi perangkat lunak Oracle.
    - oracle_home : merupakan direktori spesifik tempat biner (program/aplikasi) Oracle Grid atau Database terinstal.
    - inventory   : folder yang mencatat semua software Oracle yang terinstal di server tersebut. penting untuk proses patching dan upgrade.


12. Konfigurasi Environment Variables pada file .bash_profile untuk masing-masing user (grid dan oracle). bertujuan untuk menetapkan parameter penting seperti ORACLE_HOME, ORACLE_SID, dan PATH secara otomatis, sehingga memudahkan eksekusi perintah administratif Oracle tanpa perlu mengetik lokasi folder secara manual.
    - su grid/oracle
    - sudo vim .bash_profile

      <img width="806" height="259" alt="Screenshot (642)" src="https://github.com/user-attachments/assets/a7c4303c-dce4-41d9-af65-784ffefba99e" />

      <img width="827" height="259" alt="Screenshot (645)" src="https://github.com/user-attachments/assets/2aa3d6e4-1984-450a-b026-c14e5e95dc49" />


13. stop dan disable firewall, untuk memastikan tidak ada pemblokiran komunikasi antar node atau antar layanan (seperti Listener dan ASM) selama proses instalasi dan operasional Oracle.

    <img width="1683" height="337" alt="Screenshot (647)" src="https://github.com/user-attachments/assets/ce6828cf-4b54-45ca-b163-54b4a04f2ca0" />


14. Menambahkan Virtual Disk tambahan pada VM sebagai Raw Device untuk konfigurasi Disk Group ASM (DATA dan FRA).

    <img width="695" height="235" alt="Screenshot (749)" src="https://github.com/user-attachments/assets/7d9455a9-89d0-401a-a3c0-97cbb8f4b968" />
    <img width="863" height="248" alt="Screenshot (750)" src="https://github.com/user-attachments/assets/fc820a92-c6fb-4713-90b2-cd4f3ae43f96" />


15. restart os server, untuk memastikan penambahan virtual disk sudah diterapkan sepenuhnya pada server OS.
    - sudo reboot now


16 verifikasi disk group baru menggunakan perintah lsblk

   <img width="499" height="244" alt="Screenshot (752)" src="https://github.com/user-attachments/assets/57f5a7d7-036d-4338-893c-d2c9705336ad" />


17. partisi disks group, yang kemudian akan didaftarkan sebagai anggota disk group ASM

    <img width="781" height="541" alt="Screenshot (648)" src="https://github.com/user-attachments/assets/c6e1a2de-4286-4323-b9b1-3b92c4176c4d" />
    <img width="792" height="541" alt="Screenshot (649)" src="https://github.com/user-attachments/assets/f6f48b83-69a6-4466-b5ca-8e193fb29532" />
    <img width="560" height="290" alt="Screenshot (650)" src="https://github.com/user-attachments/assets/c62d611b-eb81-42a2-859a-5f7c490bee67" />

    
18. membuat direktori tambahan untuk manajemen file database

    <img width="648" height="56" alt="Screenshot (755)" src="https://github.com/user-attachments/assets/b733cd09-4f1e-4de1-a340-ce8578799951" />


19. Konfigurasi UDEV Rules untuk persistensi hak akses dan penamaan disk ASM. Memastikan disk tersebut tetap dimiliki oleh user grid secara permanen.

    <img width="1501" height="129" alt="Screenshot (656)" src="https://github.com/user-attachments/assets/68096996-b500-4f49-9a8e-14b07499c94f" />


20. Menerapkan aturan UDEV dan memverifikasi pembuatan symbolic link untuk disk ASM. 

    <img width="714" height="141" alt="Screenshot (657)" src="https://github.com/user-attachments/assets/6f74b4fc-8295-439c-b48e-918c75758765" />


21. ubah kepemilikan (ownership) disks menjadi milik user grid

    <img width="808" height="150" alt="Screenshot (754)" src="https://github.com/user-attachments/assets/6a35a8ae-29db-4d3a-88f5-db36b40f8d9c" />


22. 











    





   






   


   

   
