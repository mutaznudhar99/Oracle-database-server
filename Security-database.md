Pada sesi kali ini, saya akan mensimulasikan pembuatan user operasional beserta database profile user sebagai control access, security, dan performa database.

tujuannya:
- Mengatur keamanan
- Mengatur pengunaan resources
- Mencegah access user mencurigakan



1. Konfigurasi database profile. bertujuan untuk Resource Management dan Password Policy Enforcement 

   <img width="541" height="219" alt="Screenshot (832)" src="https://github.com/user-attachments/assets/85ff9972-cbda-423e-bd0f-59531cd85ebb" />
   - FAILED_LOGIN_ATTEMPTS 3     : membatasi jumlah gagal login sebelum akun dikunci
   - SESSIONS_PER_USER 1         : membatasi jumlah sesi login per user
   - IDLE_TIME 10                : memutus sesi jika user idle (menganggur)
   - CONNECT_TIME 120            : total batas waktu koneksi user
   - PASSWORD_LIFE_TIME 30       : menentukan berapa lama password berlaku
   - PASSWORD_LOCK_TIME 1/24     : durasi akun terkunci setelah login gagal
   - PASSWORD_VERIFY_FUNCTION    : verifikasi penggunaan password untuk menjaga keamanan database


2. Verifikasi user profile aktif

   <img width="731" height="461" alt="Screenshot (833)" src="https://github.com/user-attachments/assets/fc3b83fa-7e80-4ecc-bc1f-12b563f0f128" />


3. Verifikasi kebijakan password mengikuti standar system security database

   <img width="381" height="98" alt="Screenshot (834)" src="https://github.com/user-attachments/assets/61bb4ced-2618-4839-8bb0-ae256121d6ff" />
   <img width="770" height="67" alt="Screenshot (835)" src="https://github.com/user-attachments/assets/00df5e3a-5298-44e5-832b-9e3e70e9beaa" />
   - Char => 8    : minimal 8 karakter 
   - Letter => 1  : minimal 1 karakter huruf besar
   - Digit => 1   : minimal 1 karakter angka
   - Special => 1 : minimal 1 karakter spesial

   
4. Membuat Role-Based Access Control (RBAC) sebagai kontainer operational role mengikuti prinsip least privilige

   <img width="619" height="69" alt="Screenshot (836)" src="https://github.com/user-attachments/assets/bedc143d-0e4a-40f0-b8cf-6414b3bc6a01" />
   <img width="1518" height="232" alt="Screenshot (837)" src="https://github.com/user-attachments/assets/2e53e1d4-0393-402f-9d0f-e8dac6ce08d0" />

   

5. Simulasi kegagalan manajemen user & authentifikasi

   <img width="676" height="214" alt="Screenshot (838)" src="https://github.com/user-attachments/assets/43b3941d-20aa-4056-89c1-a1514dd74325" />


6. Membuat user sesuai dengan authentifikasi password

   <img width="554" height="158" alt="Screenshot (839)" src="https://github.com/user-attachments/assets/584e81c5-b922-412e-a281-d18f7edfbc05" />


7. Memberikan hak akses operationel role kepada user  

   <img width="567" height="158" alt="Screenshot (842)" src="https://github.com/user-attachments/assets/a562d6c3-da79-49c2-a1b9-a7bb95def334" />


8. Validasi eksistensi user 

   <img width="1312" height="140" alt="Screenshot (840)" src="https://github.com/user-attachments/assets/731f2f6a-818e-4ac7-8c05-c6b076247e45" />
   - Expiry_date : terkonfigurasi dengan PASSWORD_LIFE_TIME 30 

10. Simulasi penggunaan hak akses kepada user

   <img width="815" height="304" alt="Screenshot (845)" src="https://github.com/user-attachments/assets/93d1c125-2e98-4ca8-adb1-a77e316ef182" />
   - Insufficient Privileges : melakukan eksekusi yang tidak diizinkan oleh system keamanan database.


11. Simulasi penggunaan user dalam aksesibilitas control profile

    <img width="535" height="406" alt="Screenshot (846)" src="https://github.com/user-attachments/assets/1ae8df2b-46f4-48d0-9e9d-f083becc9650" />
    - menjawah konfigurasi profile dari LOGIN FAILED ATTEMPS = 3 

12. Unlock user untuk mengaktifkan operational user

    <img width="425" height="191" alt="Screenshot (847)" src="https://github.com/user-attachments/assets/23161599-45ac-495d-9c25-35edcd7972e2" />


   
