Pada sesi ini, saya akan mengimplementasikan strategi Data Protection menggunakan utilitas Oracle RMAN. Fokus utama simulasi ini adalah pengujian Disaster Recovery melalui metode Point-In-Time Recovery (PITR) guna meminimalkan RPO (Recovery Point Objective) dan RTO (Recovery Time Objective) pada database mission-critical


hal yang dipersiapkan:
- Validasi alokasi dan kapasitas Flash Recovery Area (FRA)
- Pembuatan simulasi transactional dataset
- Penetapan database baseline dengan memastikan mode ARCHIVELOG dan melakukan Full Backup






1. Memastikan database berjalan dalam mode ARCHIVELOG

   <img width="732" height="474" alt="Screenshot (786)" src="https://github.com/user-attachments/assets/af650b48-882c-4c20-858b-38088dc3ebf3" />


2. Membuat data sampel dan melakukan COMMIT. Memastikan perubahan data ditulis secara permanen ke dalam Undo Segments dan informasi perubahannya dicatat ke dalam online redo log di level disk

   <img width="775" height="566" alt="Screenshot (787)" src="https://github.com/user-attachments/assets/df3002cb-7692-4922-823c-60afa0ccaec7" />
   - Commit : menyimpan perubahan secara permanan ke file online redo log di disk


3. Menjalankan perintah BACKUP DATABASE PLUS ARCHIVELOG. Proses ini menciptakan titik pemulihan awal (baseline) yang mencakup semua komponen fisik database (datafile, controlfile, archivelog)

   <img width="1143" height="524" alt="Screenshot (789)" src="https://github.com/user-attachments/assets/2085bc79-5173-4904-96cf-62edc1a8f080" />
   <img width="1165" height="400" alt="Screenshot (790)" src="https://github.com/user-attachments/assets/c78e2f35-1acc-4ba9-a0bd-91115acfaa68" />


4. Melakukan verifikasi metadata backup untuk memastikan semua komponen database datafiles, control files, SPFILE, dan archivelogs sudah terarsip dengan status 'AVAILABLE'

   <img width="987" height="782" alt="Screenshot (796)" src="https://github.com/user-attachments/assets/25645ac1-2f87-4ba5-8419-8c3ad487cfc4" />


5. Simulasi logical corruption atau human error dengan melakukan delete data 

   <img width="728" height="544" alt="Screenshot (792)" src="https://github.com/user-attachments/assets/9c9aefea-79d4-4dc9-b5c3-40a306cff2ec" />


6. Melakukan investigasi titik kesalahan menggunakan flashback 1uery untuk timestamp terakhir sebelum terjadinya insiden delete/drop data

   <img width="794" height="309" alt="Screenshot (797)" src="https://github.com/user-attachments/assets/55e82bc8-8f85-4d5e-bb43-01bc3b51aacc" />


7. Eksekusi Point-In-Time Recovery (PITR) melalui utilitas RMAN. Memastikan database ke kondisi konsisten sebelum kehilangan data

   <img width="771" height="570" alt="Screenshot (798)" src="https://github.com/user-attachments/assets/c6686aa3-0a28-4b03-a46f-7bce6ce2ebc0" />


8. Melakukan pengecekan akhir pada tabel transaksional. Memastikan seluruh record (data) yang terhapus pada poin 5 telah kembali ke kondisi semula sebelum insiden

   <img width="658" height="199" alt="Screenshot (799)" src="https://github.com/user-attachments/assets/284570a7-6296-4dd0-92f3-ef603400eee0" />




   

   


   

   

