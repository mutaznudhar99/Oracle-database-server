Pada sesi kali ini, saya akan mengimplementasikan Application Continuity untuk zero downtime selama proses maintenance atau failover. Tujuannya memastikan sesi user  pada level aplikasi tetap berjalan tanpa error dengan memanfaatkan kapabilitas Oracle Real Application Clusters (RAC)

hal yang dipersiakan:
- Instalasi grid & RAC database
- Swingbench sebagai tool testing untuk mensimulasikan data workload secara real-time
- Connection string untuk support fitur application continuity



1. Swingbench terinstall di side server atau host server


2. Create application continuity di server node 1 RAC


3. Create connection string di sisi server node 1 RAC


4. Copy connection string ke swingbench


5. Konfigurasi swingbench sebelum menjalankan data workload secara real-time


6. Masuk ke dalam database menggunakan sqlplus dan monitoring sesi user yang berjalan


7. Eksekusi swingbench untuk menjalankan data workload kepada database


8. Verifikasi sesi user yang berjalan. memastikan load balancing seimbang di antara server RAC


9. Simulasi maintenance atau failover pada database server node 1 RAC serta cek status atau logging swingbench untuk memastikan sesi user tetap berjalan


10. Validasi sesi user tetap berjalan di another server RAC

