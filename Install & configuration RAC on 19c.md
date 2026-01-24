pada sesi kali ini, saya akan melakukan instalasi dan konfigurasi Oracle Real Application Clusters (RAC) menggunakan dua node server sebagai infrastruktur Primary. Implementasi ini dirancang untuk menjamin Local High Availability (HA) dengan memanfaatkan arsitektur Shared Storage dan Oracle Automatic Storage Management (ASM)

kelebihan RAC:
- High availability & scalability
- Centralized data management
- Efisiensi penyimpanan storage menggunakan ASM

kekurangan RAC:
- Performa interconnect overhead karena keterbatasan penggunaan RAM server

hal yang dipersiapkan:
- 1 server untuk node 1
- 1 server untuk node 2 
- adapter network public dan private untuk mengkoneksikan antar server dan RAC antar server




1. Set konfigurasi memori kedua server pada virtual machine, memastikan konfigurasi antar node sama dan sesuai.

   <img width="383" height="514" alt="Screenshot (881)" src="https://github.com/user-attachments/assets/cea52359-5be1-4bdf-a2c3-414fca033a4b" />
   - 4 GB : untuk CPU cores
   - 100 GB : storage untuk instalasi software grid and oracle db
   - network adapter : connector antara node server untuk menjalankan RAC


2. Validasi ip di kedua server berjalan dan berada di subnet yang sama

   <img width="1044" height="413" alt="Screenshot (882)" src="https://github.com/user-attachments/assets/6507d04e-6fe8-4b11-bbfd-f53f510e4074" />
   - ens33 : sebagai ip public untuk menghubungkan aplikasi/user dengan database
   - ens34 : sebagai ip private untuk menghubungkan konfigurasi RAC di kedua server


3. Set librari hosts di level kedua server sebagai connector

   <img width="806" height="405" alt="Screenshot (883)" src="https://github.com/user-attachments/assets/13b7cd0b-d5c1-4ac9-830f-7f8e1f9c3e2b" />
   - Virtual IP : sebagai
   - SCAN IP : sebagai


4. Validasi aksesibilitas antar server berjalan menggunakan IP atau DNS yang sudah di set di dalam hosts librari

   <img width="915" height="348" alt="Screenshot (884)" src="https://github.com/user-attachments/assets/b08bde9b-30eb-4421-8ed0-37dc2fcfbc5c" />


5. Install repositori oracle resmi di kedua server node

   <img width="1594" height="371" alt="Screenshot (885)" src="https://github.com/user-attachments/assets/244e2330-b93a-4f98-b233-fcf6f954c9bb" />


6. Validasi kernel repositori oracle

   <img width="428" height="330" alt="Screenshot (887)" src="https://github.com/user-attachments/assets/1d5095c7-ef86-44b4-8735-cd72eaa6cd37" />


7. Add group untuk instalasi grid and oracle

   <img width="516" height="533" alt="Screenshot (889)" src="https://github.com/user-attachments/assets/062be088-0c83-43ee-bb7c-a7ccbcd7be09" />


8. Add user ke dalam group grid and oracle

   <img width="1414" height="123" alt="Screenshot (890)" src="https://github.com/user-attachments/assets/5ad61fc3-81c9-49e3-b651-f7c9a31b1d0e" />
   <img width="799" height="289" alt="Screenshot (892)" src="https://github.com/user-attachments/assets/386454bd-1674-4a67-9b93-114761961e63" />


9. Create password untuk user grid dan oracle

   <img width="936" height="259" alt="Screenshot (893)" src="https://github.com/user-attachments/assets/8ab30240-7e57-4297-85f7-82d8e90f6da7" />


10. Konfigurasi security limits pada librari limits.conf
    - vi /etc/security/limits.conf

      <img width="398" height="380" alt="Screenshot (894)" src="https://github.com/user-attachments/assets/c94b8d17-9537-4eb3-bf72-1c32e3396133" />


11. Membuat direktori untuk menyimpan file grid dan oracle

    <img width="1464" height="210" alt="Screenshot (895)" src="https://github.com/user-attachments/assets/a2a7c48b-d6a9-492b-b42d-c4dd36f55443" />


12. Set permission user grid dan oracle untuk access direktori

    <img width="759" height="284" alt="Screenshot (896)" src="https://github.com/user-attachments/assets/872db33d-8cc8-4bfb-bf9a-67e1202427a8" />


13. Set .bash_profile grid dan oracle
    - vi .bash_profile

      <img width="785" height="416" alt="Screenshot (897)" src="https://github.com/user-attachments/assets/c6ac2928-cc9c-4674-aca6-8c7b36e6d5f6" />
      <img width="767" height="419" alt="Screenshot (898)" src="https://github.com/user-attachments/assets/41778db8-5c3c-4840-afb5-a8c0c7d8db07" />


14. Disable Firewall dan Selinux

    <img width="1580" height="354" alt="Screenshot (900)" src="https://github.com/user-attachments/assets/e8299ca8-fddf-43e5-a9cd-cecc1e7d2a1b" />
    
    - vi /etc/selinux/config
    <img width="888" height="184" alt="Screenshot (901)" src="https://github.com/user-attachments/assets/2e34248e-a51b-408a-a57d-07b91e234a1f" />


15. 

















   
