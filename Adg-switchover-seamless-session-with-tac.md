Pada sesi kali ini, saya akan mendemonstrasikan switchover database primary menuju standby tanpa memutus sesi koneksi menggunakan fitur transparent application continuity (TAC). Hal ini bertujuan agar proses transaksi client tetap berjalan lancar tanpa terputus ditengah jalan apabila database mengalami failover otomatis maupun switchover secara manual.

hal yang dipersiapkan:
- Memastikan database primary dan standby berjalan di masing-masing server
- Apliksi swingbench sebagai tools untuk mensimulasikan dan mengirim workload transaksi secara real-time ke database

