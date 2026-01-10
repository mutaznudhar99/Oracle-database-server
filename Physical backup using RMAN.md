Pada sesi kali ini, saya akan melakukan proses physical backup pada Oracle Database menggunakan native tool yaitu Recovery Manager (RMAN). Simulasi ini mencakup Loss of Critical files pada arsitektur ASM untuk menguji ketahanan sistem, dilanjutkan dengan proses automasi disaster recovery menggunakan fitur Data Recovery Advisor untuk meminimalkan Downtime dan menjamin Data Integrity.

kelebihan RMAN:
- support hot backup
- support incremental dan compression backup
- support deteksi blok corrupt saat melakukan backup
- support validasi file backup sebelum melakukan restore. mencegah terjadinya restore file corrupt.
- otomatisasi restore tanpa perlu melacak lokasi backup secara manual
- support backup dengan media manager seperti (cloud, tape, dsb)





1. Melakukan validasi parameter konfigurasi lokasi penyimpanan backup 

   <img width="734" height="373" alt="Screenshot (766)" src="https://github.com/user-attachments/assets/2e00ea77-6216-43d7-b3aa-b3ebcd599a3c" />


2. Verifikasi Service Availability: Memastikan seluruh resources Database dan ASM dalam status Online melalui Grid Infrastructure sebelum memulai prosedur backup. 

   <img width="830" height="640" alt="Screenshot (765)" src="https://github.com/user-attachments/assets/297e6329-6c25-482a-9378-624e8b9ddee2" />


3. Review inventori backup melalui grid infrastructure sebelum melakukan prosedur backup fisik.

   <img width="1534" height="257" alt="Screenshot (767)" src="https://github.com/user-attachments/assets/e0db52bd-cfa6-4bd8-9a99-defd2c84a6a4" />


4. Membuat sampel data sebagai bahan untuk recovery loss critical file

   <img width="821" height="472" alt="Screenshot (773)" src="https://github.com/user-attachments/assets/a5a2695c-1371-42b5-bff8-b0a3ec29d42a" />

   
5. Eksekusi Full Physical Backup database menggunakan utilitas Oracle RMAN.

   <img width="1148" height="683" alt="Screenshot (768)" src="https://github.com/user-attachments/assets/5f181ff5-96b3-41ca-964d-b629a574fd87" />


6. Validasi Metadata Backup. Memastikan komponen database (Datafiles, Control Files, & Archivelogs) telah terdaftar dalam backupset

   <img width="1002" height="575" alt="Screenshot (769)" src="https://github.com/user-attachments/assets/7fe5d098-70ae-4a93-a0be-c7344d671038" />
   - piece_name : lokasi backup RMAN
   - list of datafiles : file apa saja yang dibackup
   - spfile included : spfile terbackup
   - controlfile included : controlfile terbackup
   - backupset : menyimpan datafile backup
   - autobackup : menyimpan spfile dan controlfile backup

7. Review inventory backup melalui grid infrastcuture setelah melakukan prosedur backup fisik

   <img width="1549" height="304" alt="Screenshot (771)" src="https://github.com/user-attachments/assets/705b4bb7-e99b-46a0-af71-2369a5c98c03" />


8. Melakukan penghapusan Critical File (seperti Data File users) pada direktori aktif untuk mendeteksi kegagalan system dan menguji redundansi database.

   <img width="1533" height="395" alt="Screenshot (776)" src="https://github.com/user-attachments/assets/ad2ccfc4-69d7-412a-84c0-6ff5509f840b" />


9. login ke dalam oracle db untuk mengetahui perilaku database saat kondisi Critical File database hilang secara manual.

   <img width="760" height="622" alt="Screenshot (778)" src="https://github.com/user-attachments/assets/f3c657dc-d226-420c-b3b9-2e99bca1778f" />

   
10. Melakukan recovery database secara otomatis menggunakan Data Recovery Advisor (DRA) untuk mengembalikan status database ke kondisi normal.
    
    <img width="1153" height="796" alt="Screenshot (780)" src="https://github.com/user-attachments/assets/ed38f652-76a8-4ff8-84ac-258ed49f73b4" />
    - list failure : menampilkan kegagalan system yang terjadi karena loss of critical file
    
    <img width="1126" height="763" alt="Screenshot (781)" src="https://github.com/user-attachments/assets/2635dad9-0617-4d79-ba90-dcf3f4bd8e75" />
    - advise failure : memberikan solusi yang direkomendasikan oleh Oracle kepada user
      
    <img width="1209" height="805" alt="Screenshot (782)" src="https://github.com/user-attachments/assets/69b9f35e-a4af-43cc-88c7-c0fbad1655f7" />
    - repair failure : Menjalankan Solusi yang diberikan Oracle ke User

    

11. verifikasi menyeluruh terhadap Data File yang telah dipulihkan untuk memastikan status operasional kembali normal.
    
    <img width="834" height="500" alt="Screenshot (783)" src="https://github.com/user-attachments/assets/23e65d1a-729c-42d9-8ffa-02c34af97af4" />
    <img width="737" height="143" alt="Screenshot (785)" src="https://github.com/user-attachments/assets/015a44f6-7dbf-4693-865f-361352a86dcd" />



12. Melakukan test akhir. memastikan seluruh komponen sistem telah kembali ke status operasional normal pasca recovery datafiles.

    <img width="600" height="149" alt="Screenshot (784)" src="https://github.com/user-attachments/assets/15f5fe4a-75f2-4260-bdeb-4796f73a482b" />


    
