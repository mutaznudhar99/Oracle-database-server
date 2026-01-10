Pada sesi kali ini, saya akan melakukan proses physical backup pada Oracle Database menggunakan native tool yaitu Recovery Manager (RMAN).

kelebihan RMAN:
- support hot backup
- support incremental dan compression backup
- support deteksi blok corrupt saat melakukan backup
- support validasi file backup sebelum melakukan restore. mencegah terjadinya restore file corrupt.
- otomatisasi restore tanpa perlu melacak lokasi backup secara manual
- support backup dengan media manager seperti (cloud, tape, dsb)





1. Verifikasi Service Availability: Memastikan seluruh resources Database dan ASM dalam status Online melalui Grid Infrastructure sebelum memulai prosedur backup.


2. Melakukan validasi parameter konfigurasi lokasi penyimpanan backup


3. Review inventori backup melalui grid infrastructure sebelum melakukan prosedur backup fisik.


4. Eksekusi Full Physical Backup database menggunakan utilitas Oracle RMAN.


5. Validasi Metadata Backup. Memastikan komponen database (Datafiles, Control Files, & Archivelogs) telah terdaftar dalam backupset


6. Review inventory backup melalui grid infrastcuture setelah melakukan prosedur backup fisik


7. Melakukan penghapusan Critical File (seperti Control File) pada direktori aktif untuk mensimulasikan kegagalan system.


8. login ke dalam oracle db untuk mengetahui perilaku database saat kondisi Critical File database hilang. 


9. Melakukan recovery database secara otomatis menggunakan RMAN untuk mengembalikan status database ke kondisi normal.


10. login ke dalam oracle db untuk memvalidasi system operasional database pasca reovery.
