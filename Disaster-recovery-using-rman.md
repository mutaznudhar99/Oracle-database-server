pada sesi kali ini, saya akan melakukan strategi Data Protection menggunakan utilitas Oracle RMAN. Fokus utama simulasi ini adalah pengujian Disaster Recovery melalui metode Point-In-Time Recovery (PITR) 
guna meminimalkan RPO dan RTO pada database kritikal.


hal yang dipersiapkan:
- Alokasi kapasitas storage backup
- Simulasi dataset sebagai bahan disaster recovery pitr
- Database baseline dengan mengaktifkan archivelog dan melakukan full backup






1. Verifikasi mode archivelog pada database server

   <img width="732" height="474" alt="Screenshot (786)" src="https://github.com/user-attachments/assets/af650b48-882c-4c20-858b-38088dc3ebf3" />


2. Membuat sampel data atau simulasi dataset sebagai bahan disaster recovery PITR

   <img width="775" height="566" alt="Screenshot (787)" src="https://github.com/user-attachments/assets/df3002cb-7692-4922-823c-60afa0ccaec7" />
   - Commit : menyimpan perubahan secara permanan ke file online redo log di disk


3. Melakukan Full Backup Database beserta Archived Redo Logs sebagai baseline recovery PITR

   <img width="1143" height="524" alt="Screenshot (789)" src="https://github.com/user-attachments/assets/2085bc79-5173-4904-96cf-62edc1a8f080" />
   <img width="1165" height="400" alt="Screenshot (790)" src="https://github.com/user-attachments/assets/c78e2f35-1acc-4ba9-a0bd-91115acfaa68" />


4. Melakukan verifikasi metadata backup untuk memastikan seluruh komponen database (Datafiles, Control Files, SPFILE, dan Archived Logs) telah terarsip dengan status 'AVAILABLE'

   <img width="987" height="782" alt="Screenshot (796)" src="https://github.com/user-attachments/assets/25645ac1-2f87-4ba5-8419-8c3ad487cfc4" />


5. Mensimulasikan insiden kehilangan data melalui penghapusan sebagian data pada tabel transaksional. untuk menguji skenario recovery karena logical corruption atau human error

   <img width="728" height="544" alt="Screenshot (792)" src="https://github.com/user-attachments/assets/9c9aefea-79d4-4dc9-b5c3-40a306cff2ec" />


6. Melakukan investigasi menggunakan Flashback Version Query untuk Timestamp terakhir sebelum terjadinya insiden (Delete/Drop).

   <img width="794" height="309" alt="Screenshot (797)" src="https://github.com/user-attachments/assets/55e82bc8-8f85-4d5e-bb43-01bc3b51aacc" />


7. Eksekusi Point-In-Time Recovery (PITR) melalui utilitas RMAN dengan skenario Cross-Incarnation. Memastikan database ke kondisi konsisten sebelum kehilangan data

   <img width="771" height="570" alt="Screenshot (798)" src="https://github.com/user-attachments/assets/c6686aa3-0a28-4b03-a46f-7bce6ce2ebc0" />


8. Verifikasi integritas data pasca-recovery. memastikan tabel dan seluruh record yang hilang telah berhasil dipulihkan secara utuh

   <img width="658" height="199" alt="Screenshot (799)" src="https://github.com/user-attachments/assets/284570a7-6296-4dd0-92f3-ef603400eee0" />




   

   


   

   

