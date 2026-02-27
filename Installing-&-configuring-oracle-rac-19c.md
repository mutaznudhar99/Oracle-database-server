pada sesi kali ini, saya akan melakukan instalasi dan konfigurasi Oracle Real Application Clusters (RAC) menggunakan dua node server sebagai infrastruktur Primary. Implementasi ini dirancang untuk menjamin Local High Availability (HA) dengan memanfaatkan arsitektur Shared Storage dan Oracle Automatic Storage Management (ASM)

kelebihan RAC:
- High availability & scalability
- Centralized data management
- Efisiensi penyimpanan storage menggunakan ASM

kekurangan RAC:
- Performa interconnect overhead apabila keterbatasan penggunaan RAM server dan jaringan tidak stabil pada internet local

hal yang dipersiapkan:
- Virtual machine
- Large storage capacity
- 2 Server untuk 2 Node rac database
- adapter network public dan private untuk mengkoneksikan antar server dan RAC antar server




1. Melakukan konfigurasi resource virtual machine untuk kedua server rac. Memastikan konfigurasi resource identik

   <img width="383" height="514" alt="Screenshot (881)" src="https://github.com/user-attachments/assets/cea52359-5be1-4bdf-a2c3-414fca033a4b" />
   - 4 GB   : untuk CPU core
   - 100 GB : storage untuk instalasi software grid and oracle db
   - network adapter : connector antar node server untuk menjalankan RAC


2. Validasi interface network. Memastikan ip public dan private berjalan di atas operasi system

   <img width="1044" height="413" alt="Screenshot (882)" src="https://github.com/user-attachments/assets/6507d04e-6fe8-4b11-bbfd-f53f510e4074" />
   - ens33 : sebagai ip public untuk menghubungkan aplikasi/user dengan database
   - ens34 : sebagai ip private untuk heartbeat antar node dan sinkronisasi cache data


3. Melakukan konfigurasi Name Resolution lokal /etc/hosts. Memastikan saat instalasi grid infrastucture dan oracle database dapat mengenali hostname server

   <img width="806" height="405" alt="Screenshot (883)" src="https://github.com/user-attachments/assets/13b7cd0b-d5c1-4ac9-830f-7f8e1f9c3e2b" />
   - Virtual IP : sebagai IP failover, apabila node 1 mati, IP vip akan berpindah ke node yang available
   - SCAN IP    : sebagai side client access name. client cukup mengakses satu nama hostname SCAN IP untuk bisa mengakses seluruh instance di dalam cluster rac


4. Melakukan testing connection menggunakan perintah ping ke seluruh hostname (Public, Private, dan VIP) untuk memastikan tidak ada blokade firewall antar node

   <img width="915" height="348" alt="Screenshot (884)" src="https://github.com/user-attachments/assets/b08bde9b-30eb-4421-8ed0-37dc2fcfbc5c" />


5. Instalasi paket oracle-database-preinstall-19c untuk mengotomatisasi konfigurasi kernel, user, dan group sesuai best practice Oracle

   <img width="1594" height="371" alt="Screenshot (885)" src="https://github.com/user-attachments/assets/244e2330-b93a-4f98-b233-fcf6f954c9bb" />


6. Validasi parameter kernel sistem pasca-instalasi paket pre-install. Memastikan kepatuhan terhadap threshold minimum shared memory Oracle 19c

   <img width="428" height="330" alt="Screenshot (887)" src="https://github.com/user-attachments/assets/1d5095c7-ef86-44b4-8735-cd72eaa6cd37" />


7. Create spesific group sistem tambahan untuk memisahkan hak akses administratif antara manajemen storage (ASM) dan manajemen database

   <img width="516" height="533" alt="Screenshot (889)" src="https://github.com/user-attachments/assets/062be088-0c83-43ee-bb7c-a7ccbcd7be09" />
   - asmoper, asmadmin, asmdba : sebagai user manage clsuterware dan disk group ASM


8. Update hak akses user oracle dan grid agar memiliki wewenang yang tepat dalam mengelola infrastucture resources

   <img width="1414" height="123" alt="Screenshot (890)" src="https://github.com/user-attachments/assets/5ad61fc3-81c9-49e3-b651-f7c9a31b1d0e" />
   <img width="799" height="289" alt="Screenshot (892)" src="https://github.com/user-attachments/assets/386454bd-1674-4a67-9b93-114761961e63" />


9. Melakukan konfigurasi security hardening untuk akun sistem yang akan menjalankan infrastructure resources grid dan oracle database

   <img width="936" height="259" alt="Screenshot (893)" src="https://github.com/user-attachments/assets/8ab30240-7e57-4297-85f7-82d8e90f6da7" />


10. Konfigurasi batasan proses (soft/hard limits) pada /etc/security/limits.conf untuk membatasi penggunaan sumber daya sistem (resource limits) bagi setiap user atau grup.

      <img width="398" height="380" alt="Screenshot (894)" src="https://github.com/user-attachments/assets/c94b8d17-9537-4eb3-bf72-1c32e3396133" />


11. Membangun struktur direktori berdasarkan standar Optimal Flexible Architecture (OFA) untuk memisahkan biner software, konfigurasi, dan log inventory
    <img width="1464" height="210" alt="Screenshot (895)" src="https://github.com/user-attachments/assets/a2a7c48b-d6a9-492b-b42d-c4dd36f55443" />


12. Set ownership dan access permissions agar tidak terjadi error saat instalasi

    <img width="759" height="284" alt="Screenshot (896)" src="https://github.com/user-attachments/assets/872db33d-8cc8-4bfb-bf9a-67e1202427a8" />
    - User grid: sebagai pemilik direktori untuk grid infrastucture dan inventory
    - User oracle : sebagai pemilik direktori untuk oracle db home


13. Set variable environment pada file .bash_profile untuk masing-masing user grid dan oracle. Memudahkan eksekusi perintah administratif Oracle tanpa perlu mengetik lokasi folder secara manual

      <img width="785" height="416" alt="Screenshot (897)" src="https://github.com/user-attachments/assets/c6ac2928-cc9c-4674-aca6-8c7b36e6d5f6" />
      <img width="767" height="419" alt="Screenshot (898)" src="https://github.com/user-attachments/assets/41778db8-5c3c-4840-afb5-a8c0c7d8db07" />
      - User grid : untuk mengelola clusterware dan disk group ASM
      - User oracle : untuk mengelola database 


14. Stop dan disabled service firewall, selinux, atau melakukan filtering port untuk menjamin komunikasi tanpa hambatan antara Listener, ASM, dan Database

    <img width="1580" height="354" alt="Screenshot (900)" src="https://github.com/user-attachments/assets/e8299ca8-fddf-43e5-a9cd-cecc1e7d2a1b" />  
    <img width="888" height="184" alt="Screenshot (901)" src="https://github.com/user-attachments/assets/2e34248e-a51b-408a-a57d-07b91e234a1f" />


15. Disable konfigurasi nameserver dan set konfigurasi nsswitch.conf ke hostname. Memastikan system mendahulukan name resolution local /etc/hosts daripada DNS agar tidak terjadi latency/kegagalan saat setup instalasi grid infrastructure 
    
    <img width="379" height="64" alt="Screenshot (995)" src="https://github.com/user-attachments/assets/856d4315-714e-4623-b7b2-abfa053685c6" />
    <img width="377" height="75" alt="Screenshot (902)" src="https://github.com/user-attachments/assets/06e53072-fd5f-4b84-b7e5-a199b395036c" />


16. Konfigurasi Chrony service untuk menjaga sinkronisasi waktu antar node 

    <img width="1241" height="208" alt="Screenshot (925)" src="https://github.com/user-attachments/assets/b977fbbe-904c-4160-b89a-34678ec3c701" />


17. Add shared virtual disk sebagai shared storage ASM yang dapat diakses oleh kedua node secara bersamaan

    <img width="391" height="592" alt="Screenshot (905)" src="https://github.com/user-attachments/assets/83e9445a-467b-4362-9445-3293cc2a0ceb" />
    <img width="370" height="582" alt="Screenshot (906)" src="https://github.com/user-attachments/assets/3005aab8-bddb-4dd4-aa3e-02e33bb07433" />


18. Validasi shared virtual disk menggunakan utilitas lsblk sebelum dilakukan proses partisi

    <img width="518" height="266" alt="Screenshot (907)" src="https://github.com/user-attachments/assets/b85425db-0bab-4c74-b4ff-2726498aafc4" />


19. Melakukan partisi disk pada server Node 1 untuk didaftarkan sebagai anggota ASM Disk Group. Kemudian validasi table partisi di server Node 2

    <img width="703" height="538" alt="Screenshot (908)" src="https://github.com/user-attachments/assets/e5f17da5-f320-4bf1-9a9b-1af14c04a139" />
    <img width="523" height="346" alt="Screenshot (909)" src="https://github.com/user-attachments/assets/9c7ca8bb-f864-4adc-bf32-614bad929ad4" />


20. Mencari SCSI ID (UUID) dari setiap shared disk. Memastikan persistensi penamaan device menggunakan UDEV Rules di kedua server

    <img width="592" height="160" alt="Screenshot (910)" src="https://github.com/user-attachments/assets/907417dc-7ed2-4be0-b73b-173450db1e70" />


21. Konfigurasi UDEV RULES pada kedua server. Memastikan identitas, ownership, dan hak akses disk ASM tetap konsisten (persisten) setelah sistem dijalankan ulang

      <img width="1607" height="251" alt="Screenshot (911)" src="https://github.com/user-attachments/assets/073fdfae-5ddf-4c8f-b457-13c6aee0724b" />


22. Load dan trigger konfigurasi udev rules agar konfigurasi diterapkan di kedua server

    <img width="604" height="70" alt="Screenshot (912)" src="https://github.com/user-attachments/assets/731474f5-70ed-4bea-8316-f098f9d33011" />


23. Validasi hasil udev rules. Memastikan user grid memiliki ownership shared virtual disk (storage) 

    <img width="656" height="272" alt="Screenshot (913)" src="https://github.com/user-attachments/assets/d8c3532e-446d-4d60-b11c-3d6e7ef260c9" />


24. Mengatur kapasitas temporary files pada direktori /dev/shm melalui file /etc/fstab untuk mendukung penggunaan shared memory (SGA) oracle database. Menghindari kegagalan size SGA saat instalasi database

      <img width="941" height="297" alt="Screenshot (914)" src="https://github.com/user-attachments/assets/56a40822-38b5-4545-a20e-52959006cf32" />


25. Copy file installer (zip) grid infrastructure dan oracle database ke masing-masing direktori $ORACLE_HOME pada server Node 1

    <img width="987" height="201" alt="Screenshot (916)" src="https://github.com/user-attachments/assets/1cfc1d8f-0b32-43bb-9b0d-d90ad6bba197" />


26. Validasi eksistensi file installer grid infrastucture dan oracle database 

    <img width="734" height="225" alt="Screenshot (918)" src="https://github.com/user-attachments/assets/90d3f0f1-fcb4-4cdc-990a-faf080dfd6e6" />


27. Melakukan extract paket instalasi Grid Infrastructure dan Database menggunakan user grid, dan melakukan validasi struktur direktori

    <img width="728" height="180" alt="Screenshot (919)" src="https://github.com/user-attachments/assets/f28e5d68-a522-400d-bf65-484fdb4088e0" />
    <img width="603" height="214" alt="Screenshot (920)" src="https://github.com/user-attachments/assets/04dabdc2-00b5-4c61-ab99-6bd32439af99" />


28. Instalasi paket cvuqdisk pada server node 1 agar cluster verification utility (CVU) dapat mengidentifikasi dan memvalidasi shared disk yang akan digunakan oleh oracle ASM

    <img width="791" height="129" alt="Screenshot (921)" src="https://github.com/user-attachments/assets/1cf3f179-4f9f-4cff-ad30-fdb572d86d65" />


29. Konfigurasi password SSH connectivity user grid dan user oracle di kedua node untuk komunikasi antar-node, replikasi software, dan manajemen cluster secara otomatis

    <img width="610" height="160" alt="Screenshot (922)" src="https://github.com/user-attachments/assets/f652c1c0-c390-4abe-a41d-45535a58c090" />


30. Validasi konektivitas SSH. Memastikan user dapat melakukan login antar node tanpa password

    <img width="952" height="498" alt="Screenshot (923)" src="https://github.com/user-attachments/assets/a9afdf8d-c08f-407b-a47c-946a18902fab" />


31. Menjalankan cluster verification utility (CVU) untuk melakukan validasi prerequisite check. memastikan hardware, software, network, dan shared storage tidak ada masalah saat memulai instalasi Grid Infrastructure

    <img width="917" height="96" alt="Screenshot (924)" src="https://github.com/user-attachments/assets/31ae688d-0306-46af-81c2-89adfaa20369" />


32. Menjalankan dan mengkonfigurasi oracle grid infrastructure for a New Cluster menggunakan wizard gridSetup.sh untuk membentuk klaster RAC (Real Application Clusters)

    <img width="627" height="112" alt="Screenshot (928)" src="https://github.com/user-attachments/assets/95ca65bd-0499-49dd-ae0d-f4199ae69215" />
    <img width="985" height="910" alt="Screenshot (929)" src="https://github.com/user-attachments/assets/fe3fe3d7-e508-4570-a56a-88e9c5c4cfb1" />
    <img width="991" height="800" alt="Screenshot (930)" src="https://github.com/user-attachments/assets/64413599-defb-4dd0-ad01-1acc7c4f89cc" />
    <img width="1003" height="785" alt="Screenshot (931)" src="https://github.com/user-attachments/assets/db2e9e59-6f5b-4478-8dd7-ded7a875f2b5" />
    <img width="1005" height="767" alt="Screenshot (932)" src="https://github.com/user-attachments/assets/0ed149c5-55e7-4d02-9426-81695d23fb59" />
    <img width="997" height="763" alt="Screenshot (933)" src="https://github.com/user-attachments/assets/6ef8ae99-fe9b-40fd-86e2-7196935bd262" />
    <img width="1003" height="780" alt="Screenshot (934)" src="https://github.com/user-attachments/assets/836745cd-7cd9-48d6-b392-3a2a1e72f4f6" />
    <img width="1008" height="774" alt="Screenshot (935)" src="https://github.com/user-attachments/assets/d628e841-21b6-456d-907c-f9c717c67d47" />
    <img width="989" height="779" alt="Screenshot (936)" src="https://github.com/user-attachments/assets/d66becbf-df88-4a21-8d6b-884ce4279031" />
    <img width="997" height="787" alt="Screenshot (937)" src="https://github.com/user-attachments/assets/deabf063-7d8c-4286-a69b-2abf033ef38f" />
    <img width="996" height="775" alt="Screenshot (938)" src="https://github.com/user-attachments/assets/9c1e3698-31d3-4f44-a7d0-f96f5329889b" />
    <img width="990" height="777" alt="Screenshot (939)" src="https://github.com/user-attachments/assets/ce46c9cb-6f98-4273-ac7c-642a919ce60b" />
    <img width="1001" height="785" alt="Screenshot (940)" src="https://github.com/user-attachments/assets/b547d992-a1ff-402c-896d-dd655cef072e" />
    <img width="989" height="759" alt="Screenshot (941)" src="https://github.com/user-attachments/assets/a43c039d-9781-4d04-a511-7b7da29c1670" />
    <img width="993" height="772" alt="Screenshot (942)" src="https://github.com/user-attachments/assets/7f5ec0fa-b86b-401a-bade-10fb84af7bc1" />
    <img width="1003" height="779" alt="Screenshot (943)" src="https://github.com/user-attachments/assets/45a05257-5f9a-4751-8cc3-c3f168532ffb" />
    <img width="984" height="787" alt="Screenshot (944)" src="https://github.com/user-attachments/assets/fa163d4d-70f9-4a6f-b086-7f9650d90066" />
    <img width="713" height="388" alt="Screenshot (945)" src="https://github.com/user-attachments/assets/b4c82c9b-e404-444d-8b2c-db77c6dc007f" />
    <img width="998" height="793" alt="Screenshot (946)" src="https://github.com/user-attachments/assets/4a5c1704-61d0-4ca0-a1c5-e3c76846c4ae" />


33. Melakukan verifikasi status operasional oracle Clusterware dan ASM Instance. Memastikan semua resource cluster berjalan (online)

    <img width="815" height="445" alt="Screenshot (947)" src="https://github.com/user-attachments/assets/97658b7b-a397-4882-b56d-efd0cba6d2bc" />
    <img width="783" height="776" alt="Screenshot (948)" src="https://github.com/user-attachments/assets/c2bea8a2-884c-4dcf-80a7-6e6295a1bdd9" />


34. Konfigurasi ASM disk groups menggunakan ASMCA (Oracle ASM Configuration Assistant) untuk menyiapkan ruang storage bagi database files (DATA) dan recovery area (FRA)

    <img width="614" height="78" alt="Screenshot (950)" src="https://github.com/user-attachments/assets/65769836-ad5d-4971-99ce-e78a123c4ff9" />
    <img width="1188" height="782" alt="Screenshot (951)" src="https://github.com/user-attachments/assets/2c673618-f1cb-4f67-986f-4a474efd3cca" />


35. Melakukan verifikasi oracle ASM (Automatic Storage Management) menggunakan utility asmcmd. Memastikan seluruh disk groups telah berjalan dan dapat diakses oleh semua node cluster

    <img width="1554" height="252" alt="Screenshot (983)" src="https://github.com/user-attachments/assets/4c169865-a70a-487a-9355-b327ecd96765" />


36. Melakukan extract paket software oracle database 19c ke dalam direktori ORACLE_HOME menggunakan user oracle

    <img width="699" height="201" alt="Screenshot (952)" src="https://github.com/user-attachments/assets/ce023635-b080-49b5-acec-fb171eb37e39" />
    <img width="638" height="242" alt="Screenshot (953)" src="https://github.com/user-attachments/assets/c488ec86-3ba8-4604-9db0-3d5506b13a1a" />


37. Menjalankan wizard ./runInstaller untuk melakukan instalasi binari oracle database 19c software sebelum create database

    <img width="616" height="110" alt="Screenshot (954)" src="https://github.com/user-attachments/assets/020ed036-1184-4c19-823f-ef9ef3cd2a95" />
    <img width="996" height="786" alt="Screenshot (955)" src="https://github.com/user-attachments/assets/b06bb43f-bebe-4192-8e41-1425db3347be" />
    <img width="993" height="783" alt="Screenshot (956)" src="https://github.com/user-attachments/assets/5ea60f96-1a72-4b61-84ab-e09e1ac532a6" />
    <img width="991" height="775" alt="Screenshot (957)" src="https://github.com/user-attachments/assets/1d857fa5-6b83-4591-b9e5-7bbcfc681ef9" />
    <img width="999" height="794" alt="Screenshot (958)" src="https://github.com/user-attachments/assets/9c137937-548b-4fed-994a-bb43a7afbffc" />
    <img width="993" height="781" alt="Screenshot (959)" src="https://github.com/user-attachments/assets/6aa7cb5e-e38c-427f-8a1f-bd074df96380" />
    <img width="996" height="795" alt="Screenshot (960)" src="https://github.com/user-attachments/assets/53de1c9c-1612-4cea-92b5-906dd719b5c9" />
    <img width="993" height="789" alt="Screenshot (961)" src="https://github.com/user-attachments/assets/0c4b4c63-746f-45fd-8222-64d3878e678e" />
    <img width="1003" height="789" alt="Screenshot (962)" src="https://github.com/user-attachments/assets/d1a0a1f3-1fcd-4f88-9d60-0dd69f8d06ed" />
    <img width="699" height="369" alt="Screenshot (963)" src="https://github.com/user-attachments/assets/6e91d7d6-6e13-43f9-855e-6df12810e186" />
    <img width="1004" height="772" alt="Screenshot (964)" src="https://github.com/user-attachments/assets/1b1e6e3f-ee53-42ea-a9b2-c153b29b6f2f" />


38. Melakukan konfigurasi dan create Database RAC menggunakan DBCA (Database Configuration Assistant) untuk mendistribusikan instance database di semua node cluster

    <img width="604" height="69" alt="Screenshot (965)" src="https://github.com/user-attachments/assets/b9c4403c-05fd-4681-a987-b073195f0a76" />
    <img width="1017" height="782" alt="Screenshot (966)" src="https://github.com/user-attachments/assets/6fa934b9-554c-412b-8aae-96824bb72da5" />
    <img width="994" height="798" alt="Screenshot (967)" src="https://github.com/user-attachments/assets/107645f5-c66b-46ab-a1a5-41c063475e25" />
    <img width="1005" height="787" alt="Screenshot (968)" src="https://github.com/user-attachments/assets/adf0521b-1e7d-4b7a-847b-ddaf79fbc09d" />
    <img width="983" height="787" alt="Screenshot (969)" src="https://github.com/user-attachments/assets/e93e361f-2787-4ade-ad6e-33635048d920" />
    <img width="998" height="780" alt="Screenshot (971)" src="https://github.com/user-attachments/assets/ff060100-5a78-48b8-8f47-cf70b5642d12" />
    <img width="992" height="776" alt="Screenshot (972)" src="https://github.com/user-attachments/assets/0bee0116-551c-4d16-8bcb-fcadcac2f65e" />
    <img width="996" height="777" alt="Screenshot (973)" src="https://github.com/user-attachments/assets/a579353d-fdfd-4466-a935-64d3bc3636e6" />
    <img width="984" height="774" alt="Screenshot (974)" src="https://github.com/user-attachments/assets/1759460e-bdf4-41c8-b6f7-e3b0e827318b" />
    <img width="998" height="778" alt="Screenshot (975)" src="https://github.com/user-attachments/assets/a4ec4e25-fb8e-4095-b7d6-c3fc2e10c578" />
    <img width="986" height="782" alt="Screenshot (976)" src="https://github.com/user-attachments/assets/9cf14b1b-a020-47ac-91e3-35f47446c2d8" />
    <img width="991" height="766" alt="Screenshot (977)" src="https://github.com/user-attachments/assets/07c57a38-8f41-491e-8e73-1ec6c9426148" />
    <img width="992" height="780" alt="Screenshot (978)" src="https://github.com/user-attachments/assets/67f89e35-743c-48e5-b73e-c5c75113adbe" />


39. Melakukan verifikasi dan konfigurasi database RAC menggunakan utility srvctl untuk memastikan seluruh instance berjalan di masing-masing node

    <img width="611" height="606" alt="Screenshot (992)" src="https://github.com/user-attachments/assets/49b3895f-8f8f-4111-8435-470931709dfb" />


40. Melakukan verifikasi oracle clusterware berjalan pada setiap node menggunakan perintah crsctl check cluster. Memastikan sinkronisasi cluster antar-node berjalan (online)

    <img width="545" height="174" alt="Screenshot (986)" src="https://github.com/user-attachments/assets/02428f78-c132-4c02-a27a-b159ed464a33" />
    <img width="571" height="128" alt="Screenshot (987)" src="https://github.com/user-attachments/assets/cf980ca8-8b83-4e7f-b714-b6053893d032" />


41. Melakukan verifikasi operasional global database instance melalui SQL*Plus. Kemudian memastikan semua backround process oracle RAC telah aktif di kedua node server

    <img width="1267" height="293" alt="Screenshot (979)" src="https://github.com/user-attachments/assets/959a823e-4df4-44e1-9a0a-b5d3884f9b53" />
    <img width="1571" height="544" alt="Screenshot (980)" src="https://github.com/user-attachments/assets/f221abd8-36b0-4da1-9475-6c81f927186e" />


42. Melakukan verifikasi lokasi dan status voting files menggunakan utility crsctl. Memastikan kedua node terkoneksi pada shared storage yang sama

    <img width="658" height="153" alt="Screenshot (984)" src="https://github.com/user-attachments/assets/f717b0f0-0a7a-4cf7-b45b-8d2f50ac7e38" />
    <img width="664" height="122" alt="Screenshot (993)" src="https://github.com/user-attachments/assets/31f4fe98-9d12-44e5-9c05-2e92888c77d8" />


43. Melakukan verifikasi cluster resources berjalan (online) menggunakan perintah crsctl stat res -t. Memastikan semua komponen (Database, ASM, Network, dan VIP) berada pada status TARGET: ONLINE dan STATE: ONLINE di kedua node

    <img width="802" height="433" alt="Screenshot (988)" src="https://github.com/user-attachments/assets/9cde0190-f401-4447-9ed7-6ae5fda4f90f" />
    <img width="822" height="882" alt="Screenshot (989)" src="https://github.com/user-attachments/assets/e19e529e-eb08-44b1-8829-5eca390bad4a" />



44. Melakukan testing consistency data antar-instance dengan membuat objek tabel pada Node 1 dan melakukan query pada Node 2 untuk memvalidasi semua instance mengakses database storage yang sama secara real-time

    <img width="813" height="389" alt="Screenshot (981)" src="https://github.com/user-attachments/assets/9edf872d-62c3-4ba6-a5af-bbeb579df3be" />
    <img width="790" height="303" alt="Screenshot (982)" src="https://github.com/user-attachments/assets/9fa6b5d4-71e1-4919-ac19-9bd0d2e51476" />






    
    
    




















    

    




























   
