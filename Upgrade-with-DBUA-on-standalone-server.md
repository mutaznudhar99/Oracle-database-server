In this session, i will be implementing upgrade database system from 12c version to 19c version with Database Upgrade Assistant (DBUA) on standalone server. 



pros DBUA:
- easily to use
- using grafik user

cons DBUA:
- upgrade without multiple database
- very slow between autoupgrade
- can't upgrade from non-cdb to pdb





1. Ensure old database system running

   <img width="729" height="83" alt="Screenshot (1337)" src="https://github.com/user-attachments/assets/6ba71df0-cdcc-4280-aa41-c52ba7581a1b" />


2. Validate oratab library, and ensure only old database running on the server

   <img width="1044" height="522" alt="Screenshot (1338)" src="https://github.com/user-attachments/assets/e9a65562-7f8d-414a-8a57-2c36f2a281fa" />


3. Create direktory for saving log during preupgrade fixups based on Optimal Flexible Architecture (OFA)

    <img width="620" height="139" alt="Screenshot (1317)" src="https://github.com/user-attachments/assets/e22f12ba-decf-4a80-b1e8-25d41a5bfe5d" />


4. Ensure database in archivelog mode. this is important to saving data for replace/flashback to old database if upgrade failed

   <img width="774" height="551" alt="Screenshot (1318)" src="https://github.com/user-attachments/assets/5edc99f0-77a7-4925-a5c6-5f5ca65fbad7" />


5. Create baseline data for testing during upgrade. Ensure processing upgrade zero data loss

   <img width="777" height="637" alt="Screenshot (1319)" src="https://github.com/user-attachments/assets/571cd0bb-bae3-4ca7-a2e8-773022ba62ee" />


6. Set parameter purge recyclebin to ensure disk/storage clean up and create restore point to return to old database if disaster happens during upgrade

   <img width="792" height="776" alt="Screenshot (1320)" src="https://github.com/user-attachments/assets/43084c08-e99c-4655-9104-4a4e26df1fb9" />


7. run preupgrade to check compatibility or readiness old database before upgrade

   <img width="1432" height="456" alt="Screenshot (1321)" src="https://github.com/user-attachments/assets/787aef97-2970-42be-8ec9-3204138e96bd" />


8. Verify preupgrade fixups log/sql to check incompatibility, error, or anything before upgrade

   <img width="791" height="757" alt="Screenshot (1322)" src="https://github.com/user-attachments/assets/28aadf32-84ed-40ce-a442-875f6a0101a5" />
   <img width="828" height="547" alt="Screenshot (1323)" src="https://github.com/user-attachments/assets/eda4f51a-58a2-49ee-aab2-b6da889e6354" />


9. Solve incompatibility to ensure database no error issues or failed during running upgrade with dbua

   <img width="828" height="867" alt="Screenshot (1324)" src="https://github.com/user-attachments/assets/7ca4eaa1-6de8-4226-9a70-a79312b6d1bb" />


10. re-verirified after solve incompatibility. Ensure the database already to processing upgrade

    <img width="1456" height="462" alt="Screenshot (1325)" src="https://github.com/user-attachments/assets/3c1f4d32-7424-4808-a067-cfe8afedff2e" />
    <img width="774" height="821" alt="Screenshot (1328)" src="https://github.com/user-attachments/assets/52beb748-a7ab-47c2-8c51-e4f0d290d68b" />


11. execute DBUA to upgrade

    <img width="789" height="197" alt="Screenshot (1339)" src="https://github.com/user-attachments/assets/790a386f-967f-4ad9-84c6-ed5915209d39" />
    <img width="998" height="781" alt="Screenshot (1340)" src="https://github.com/user-attachments/assets/b5099642-37d1-442a-9ef6-c7dcb7324224" />
    <img width="989" height="790" alt="Screenshot (1341)" src="https://github.com/user-attachments/assets/513279ed-800a-425c-900e-b68fdb2873c3" />
    <img width="986" height="776" alt="Screenshot (1342)" src="https://github.com/user-attachments/assets/1f9eca14-f6f6-44af-bfaa-c52064dc8731" />
    <img width="1000" height="780" alt="Screenshot (1343)" src="https://github.com/user-attachments/assets/85a7ad16-438b-4557-b327-cd6fcb925ae3" />
    <img width="992" height="790" alt="Screenshot (1344)" src="https://github.com/user-attachments/assets/a19ebccf-489e-4326-bed6-a9de9bef23e0" />
    <img width="995" height="783" alt="Screenshot (1345)" src="https://github.com/user-attachments/assets/9f58264e-a3c7-4e58-804c-78d76d7a65cd" />
    <img width="989" height="785" alt="Screenshot (1346)" src="https://github.com/user-attachments/assets/eab0740d-930f-4aa0-94c2-65324adb7f09" />
    

12. Ensure upgrade process 100% running smoothly no on error issues and etc

    <img width="991" height="780" alt="Screenshot (1348)" src="https://github.com/user-attachments/assets/f9adf4db-9360-427c-ad3e-a8cd9e99c984" />


13. Validate library oratab. Ensure the path home change to new version database

    <img width="1050" height="598" alt="Screenshot (1349)" src="https://github.com/user-attachments/assets/f035dbd9-542f-4a77-ade4-92ab58aaf593" />


14. Validate baseline data for testing before it. Ensure process upgrade success and zero data loss

    <img width="723" height="664" alt="Screenshot (1350)" src="https://github.com/user-attachments/assets/58d3a20f-f3f2-4c74-ba64-ff3ac750880c" />


15. Validate no one database objects invalid during process upgrade

    <img width="708" height="508" alt="Screenshot (1354)" src="https://github.com/user-attachments/assets/3eb67505-eeb0-4195-bc51-079ebba1844a" />


16. Drop restore point if the process upgrade look succesfully

    <img width="720" height="591" alt="Screenshot (1352)" src="https://github.com/user-attachments/assets/d8a648d6-eb78-41ad-bbe0-eab8c3d00537" />    


17. Verify parameter compatibile database. Ensure the parameter can change to new version compatible after 100% look succesfully, cause its enable to new features or change everything from old version to new version.

    <img width="775" height="844" alt="Screenshot (1353)" src="https://github.com/user-attachments/assets/0f7bd7c5-a45d-4639-a790-813d58b3c560" />


18. Execute PL/SQL to upgrade statistic dictionary and objects to making good performance database

    <img width="716" height="443" alt="Screenshot (1355)" src="https://github.com/user-attachments/assets/2dfd3e35-648a-4974-8a96-35ce1f409066" />


19. Ensure database running on service cluster

    <img width="555" height="51" alt="Screenshot (1356)" src="https://github.com/user-attachments/assets/20f92666-23a4-4b7c-b711-77958cbfe465" />



    


    




















