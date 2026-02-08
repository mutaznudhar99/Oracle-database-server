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


3. Verifikasi service TAC menggunakan utilitas sqlplus. Memastikan konfigurasi berjalan pada level database

   <img width="1480" height="490" alt="Screenshot (1221)" src="https://github.com/user-attachments/assets/97a8c85a-7f2e-4536-a565-7c246ef6f658" />


3. Create Connection String (TNS) pada Sisi Server (Node 1)

   <img width="982" height="208" alt="Screenshot (1032)" src="https://github.com/user-attachments/assets/df3d481b-28d7-4c3f-9d10-e1361c1628ea" />

4. Konfigurasi connection string pada client (Swingbench)

   <img width="643" height="334" alt="Screenshot (1033)" src="https://github.com/user-attachments/assets/9190a560-4221-45c2-9bad-11a9c82624ba" />

5. Melakukan input data (INSERT) sebagai bahan testing untuk memvalidasi keberhasilan proses replay transaksi saat terjadi switcover atau failover

   <img width="552" height="287" alt="Screenshot (1222)" src="https://github.com/user-attachments/assets/feb25828-abe7-45b0-a196-be607003a123" />

       
6. Simulasi switchover manual pada node 1 dan verifikasi keberhasilan replay transaksi. Memastikan proses transaksi tetap berjalan, dan peran database berpindah host

   <img width="1501" height="546" alt="Screenshot (1223)" src="https://github.com/user-attachments/assets/b14d5782-3d40-4a9f-8cb5-df80b75c2cc3" />


7. Melakukan uji Komparasi: simulasi failover tanpa fitur TAC (default service)

   
    
    <img width="1731" height="654" alt="Screenshot (1048)" src="https://github.com/user-attachments/assets/e50295e7-1c63-4aa2-b105-e223d6b8b047" />
    <img width="1711" height="774" alt="Screenshot (1049)" src="https://github.com/user-attachments/assets/248f24ad-11ea-4d30-902c-8ffeedd7960d" />


    
