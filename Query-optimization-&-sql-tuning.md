Pada sesi kali ini, saya akan mensimulasikan analisis Execution Plan untuk mengidentifikasi efisiensi query. Proses ini mencakup implementasi SQL Tuning (logika dan fisik/indeks) yang divalidasi 
melalui laporan AWR untuk mengukur peningkatan performa secara sistemik.


1. Inisiasi Baseline AWR untuk menangkap metrik performa sistem (Workload Repository) sebelum pengujian beban (Workload Testing) dilakukan.

   <img width="712" height="344" alt="Screenshot (801)" src="https://github.com/user-attachments/assets/0fdcc34c-302d-47ea-a281-c649574c6039" />


2. Membuat dataset skala besar (Large Dataset) Untuk mengevaluasi performa skalibilitas query dan memvalidasi perilaku Optimizer terhadap volume data.

   <img width="842" height="731" alt="Screenshot (804)" src="https://github.com/user-attachments/assets/828535dd-203a-47b5-99b0-3ed302d7d7cc" />


3. Update statistik untuk memastikan Optimizer memiliki metadata yang akurat pasca operasi DML

   <img width="704" height="94" alt="Screenshot (805)" src="https://github.com/user-attachments/assets/0c8ca92b-6d31-4b62-8610-5f79a5dc7bb5" />


4. Simulasi eksekusi infficient query

   <img width="554" height="131" alt="Screenshot (815)" src="https://github.com/user-attachments/assets/ac8fda62-27ad-44e2-b4bd-b54a257426ba" />
   <img width="361" height="67" alt="Screenshot (814)" src="https://github.com/user-attachments/assets/1cf90139-c0c1-4b9b-af42-582a7da6115d" />


5. Analisis inefficient query menggunakan DBMS_XPLAN.DISPLAY_CURSOR

   <img width="985" height="551" alt="Screenshot (810)" src="https://github.com/user-attachments/assets/75f8dbf7-015d-43d6-9942-e93990e1b236" />
   - Rows : Estimasi jumlah baris yang akan dihasilkan oleh operasi query
   - Bytes : Estimasi volume data
   - Cost : Biaya penggunaan I/O dan CPU
   - CPU : Seberapa besar penggunaan resource
   - Table_Access_Full : Pemindaian seluruh blok data (tabel
   - Predict information : Xplan mendiagnosis penyebab buruknya performa query
     


6. Mengidentifikasi dominasi Logical Reads (CPU & Memory Bottleneck) yang berkorelasi dengan aktivitas Full Table Scan sebelumnya melalui laporan AWR

   <img width="995" height="150" alt="Screenshot (820)" src="https://github.com/user-attachments/assets/d06670b4-c3b6-4328-af12-8fd559839856" />
   <img width="781" height="156" alt="Screenshot (822)" src="https://github.com/user-attachments/assets/cc673ba0-a330-4a66-b51b-29d8b8121de2" />



7. Melakukan Optimasi Query pada level aplikasi (Syntax Refactoring/Logic Tuning) tanpa penambahan index

   <img width="594" height="147" alt="Screenshot (812)" src="https://github.com/user-attachments/assets/f4bf33a9-b0b0-4a54-81c7-d4b99788ad8e" />
   <img width="361" height="67" alt="Screenshot (814)" src="https://github.com/user-attachments/assets/bfec39cb-de46-4009-8a0e-d29317e146ca" />

   

8. Validasi efisiensi query setelah mengoptimalkan query tanpa penambahan index.

   <img width="904" height="545" alt="Screenshot (813)" src="https://github.com/user-attachments/assets/d30dee97-8790-4f78-a5fc-332e68682453" />
   - Rows : 20000 > 5480 turun 72%
   - Bytes : 839k > 230k turun 72%
   - Cost : 4855 > 4746 turun 2%
   - CPU  : 4  > 2 turun 50%
   - Table acces full > table access full  

9. Mengidentifikasi laporan AWR setelah melakukan optimasi query (syntax refactoring)

    <img width="982" height="155" alt="Screenshot (821)" src="https://github.com/user-attachments/assets/65b1e338-42ce-4b54-9248-11fa6684ba07" />
    <img width="818" height="164" alt="Screenshot (823)" src="https://github.com/user-attachments/assets/9bd6a68f-c04f-4add-b04b-83d637d8e2e4" />


10. Implementasi Physical Schema Tuning melalui pembuatan Index
    
    <img width="936" height="94" alt="Screenshot (818)" src="https://github.com/user-attachments/assets/3955b41d-6d97-44cf-819e-4d0665ea13b8" />


11. Validasi Execution Plan setelah membuat index
    
    <img width="911" height="543" alt="Screenshot (819)" src="https://github.com/user-attachments/assets/ffb049c2-b7b5-4599-9fd3-f56632a667f8" />
    - Rows : 20000 > 5480 > 5480
    - Bytes : 839k > 230k > 230k
    - Cost : 4855 > 4746 > 42 turun 99%
    - CPU  : 4  > 2 > 0 turun 100%
    - Table acces full > table access full > index range scan
    
12. Final, Identifikasi performa query dan database melalui laporan AWR. Melihat efektifas optimasi query plus index terhadap performa CPU & I/0
     
    <img width="1044" height="148" alt="Screenshot (825)" src="https://github.com/user-attachments/assets/feac9d50-a074-4762-bb59-72029088ab05" />
    <img width="799" height="185" alt="Screenshot (824)" src="https://github.com/user-attachments/assets/7a6fc8f7-9c65-4d7e-9d7b-bb5cf72cf7a1" />







   


   
