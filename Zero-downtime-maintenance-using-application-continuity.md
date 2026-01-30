Pada sesi kali ini, saya akan mengimplementasikan Application Continuity untuk zero downtime selama proses maintenance atau failover. Tujuannya memastikan sesi user pada level aplikasi tetap berjalan tanpa error dengan memanfaatkan kapabilitas Oracle Real Application Clusters (RAC)

hal yang dipersiakan:
- Instalasi grid & RAC database
- Swingbench sebagai tool testing atau client untuk mensimulasikan data workload secara real-time
- Connection string untuk support fitur application continuity



1. Verifikasi status grid infrastructure (RAC Node) dan SCAN listener

   <img width="563" height="89" alt="Screenshot (1014)" src="https://github.com/user-attachments/assets/a5767f24-4ff1-4aff-a0e2-ffda421327df" />
   <img width="560" height="150" alt="Screenshot (1030)" src="https://github.com/user-attachments/assets/2d7c1771-aecb-4847-91ed-9ef14854768b" />

2. Konfigurasi Database Service dengan Fitur Application Continuity (AC)

   <img width="734" height="387" alt="Screenshot (1031)" src="https://github.com/user-attachments/assets/039b62d0-394a-4403-940f-b299e9a311a6" />

3. Create Connection String (TNS) pada Sisi Server (Node 1)

   <img width="982" height="208" alt="Screenshot (1032)" src="https://github.com/user-attachments/assets/df3d481b-28d7-4c3f-9d10-e1361c1628ea" />

4. Konfigurasi connection string pada client (Swingbench)

   <img width="643" height="334" alt="Screenshot (1033)" src="https://github.com/user-attachments/assets/9190a560-4221-45c2-9bad-11a9c82624ba" />

5. Setting connection pooling pada swingbench

   <img width="653" height="430" alt="Screenshot (1043)" src="https://github.com/user-attachments/assets/e9522414-ebe4-4d48-9765-fb982f71acdd" />

6. Monitoring sesi user awal melalui SQL*Plus

   <img width="711" height="512" alt="Screenshot (1034)" src="https://github.com/user-attachments/assets/4626a412-b9e7-481a-b056-646954efdd89" />

7. Inisialisasi data workload menggunakan swingbench

   <img width="953" height="653" alt="Screenshot (1035) - Copy" src="https://github.com/user-attachments/assets/5921a0e8-5e33-4359-8631-1676bb0df70d" />

8. Verifikasi Load Balancing antar Node RAC

   <img width="759" height="607" alt="Screenshot (1035)" src="https://github.com/user-attachments/assets/a500e2f4-2706-4b2f-bf59-67835969eb41" />
       
9. Simulasi failover (Instance Shutdown) pada node 1 dan monitoring transparansi sesi

   <img width="1771" height="649" alt="Screenshot (1036)" src="https://github.com/user-attachments/assets/12debec9-e453-455e-ba0a-715ac73ba700" />

10. Validasi perpindahan sesi (Replay) ke node RAC yang lainnya

    <img width="734" height="744" alt="Screenshot (1037)" src="https://github.com/user-attachments/assets/12e83293-c311-4c3a-b542-41234ddcc361" />

11. Uji Komparasi: simulasi failover tanpa fitur TAC (default service)
    
    <img width="1731" height="654" alt="Screenshot (1048)" src="https://github.com/user-attachments/assets/e50295e7-1c63-4aa2-b105-e223d6b8b047" />
    <img width="1711" height="774" alt="Screenshot (1049)" src="https://github.com/user-attachments/assets/248f24ad-11ea-4d30-902c-8ffeedd7960d" />


    
