pada sesi kali ini, saya akan melakukan instalasi dan konfigurasi Oracle Real Application Clusters (RAC) menggunakan dua node server sebagai infrastruktur Primary. Implementasi ini dirancang untuk menjamin 
Local High Availability (HA) dengan memanfaatkan arsitektur Shared Storage dan Oracle Automatic Storage Management (ASM)

kelebihan RAC:
- High availability & scalability
- Centralized data management
- Efisiensi penyimpanan storage menggunakan ASM

kekurangan RAC:
- Mempertimbangkan performa interconnect overhead karena keterbatasan penggunaan RAM server

hal yang dipersiapkan:
- 1 server untuk node 1
- 1 server untuk node 2 
- adapter network public dan private untuk mengkoneksikan antar server dan RAC antar server




1. Set konfigurasi memori server pada virtual machine

   
