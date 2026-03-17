Performed a database upgrade from version 12c to 19c on a standalone server using Database Upgrade Assistant (DBUA). This process included rigorous pre-upgrade checks and post-upgrade validations to ensure system stability and data integrity.


Pros and Cons of DBUA
Pros:
- User-Friendly: Easy to use with a graphical user interface (GUI).
- Visual Progress: Provides real-time visual feedback during the upgrade.

Cons:
- Limited Scope: Can only upgrade one database at a time.
- Performance: Slower compared to the modern AutoUpgrade utility.
- Architecture Restriction: Cannot convert a non-CDB architecture to a PDB (Multitenant) during the upgrade.



1. Verified that the existing 12c database is running and healthy

   <img width="729" height="83" alt="Screenshot (1337)" src="https://github.com/user-attachments/assets/6ba71df0-cdcc-4280-aa41-c52ba7581a1b" />


2. Inspected the oratab file to ensure only the target old database version is active on the server

   <img width="1044" height="522" alt="Screenshot (1338)" src="https://github.com/user-attachments/assets/e9a65562-7f8d-414a-8a57-2c36f2a281fa" />


3. Created dedicated directories for pre-upgrade fixup logs, following Optimal Flexible Architecture (OFA) standards

    <img width="620" height="139" alt="Screenshot (1317)" src="https://github.com/user-attachments/assets/e22f12ba-decf-4a80-b1e8-25d41a5bfe5d" />


4. Confirmed the database is in ARCHIVELOG mode. This is critical for creating a fallback point if the upgrade fails

   <img width="774" height="551" alt="Screenshot (1318)" src="https://github.com/user-attachments/assets/5edc99f0-77a7-4925-a5c6-5f5ca65fbad7" />


5. Inserted test data to verify Zero Data Loss after the upgrade is complete

   <img width="777" height="637" alt="Screenshot (1319)" src="https://github.com/user-attachments/assets/571cd0bb-bae3-4ca7-a2e8-773022ba62ee" />


6. Purged the recycle bin and created a Guaranteed Restore Point with sql statement. This allows the database to flash back to the 12c state if a disaster occurs     during the upgrade

   <img width="792" height="776" alt="Screenshot (1320)" src="https://github.com/user-attachments/assets/43084c08-e99c-4655-9104-4a4e26df1fb9" />


7. Executed the pre-upgrade tool to check for compatibility and database readiness

   <img width="1432" height="456" alt="Screenshot (1321)" src="https://github.com/user-attachments/assets/787aef97-2970-42be-8ec9-3204138e96bd" />


8. Reviewed the pre-upgrade SQL and logs to identify any errors or mandatory manual fixups required

   <img width="791" height="757" alt="Screenshot (1322)" src="https://github.com/user-attachments/assets/28aadf32-84ed-40ce-a442-875f6a0101a5" />
   <img width="828" height="547" alt="Screenshot (1323)" src="https://github.com/user-attachments/assets/eda4f51a-58a2-49ee-aab2-b6da889e6354" />


9. Solved all identified issues to prevent the DBUA process from failing midway

   <img width="828" height="867" alt="Screenshot (1324)" src="https://github.com/user-attachments/assets/7ca4eaa1-6de8-4226-9a70-a79312b6d1bb" />


10. Ran the pre-check again to confirm the database is 100% ready for the upgrade

    <img width="1456" height="462" alt="Screenshot (1325)" src="https://github.com/user-attachments/assets/3c1f4d32-7424-4808-a067-cfe8afedff2e" />
    <img width="774" height="821" alt="Screenshot (1328)" src="https://github.com/user-attachments/assets/52beb748-a7ab-47c2-8c51-e4f0d290d68b" />


11. Execute DBUA to launched the DBUA graphical tool and followed the wizard to start the upgrade process

    <img width="789" height="197" alt="Screenshot (1339)" src="https://github.com/user-attachments/assets/790a386f-967f-4ad9-84c6-ed5915209d39" />
    <img width="998" height="781" alt="Screenshot (1340)" src="https://github.com/user-attachments/assets/b5099642-37d1-442a-9ef6-c7dcb7324224" />
    <img width="989" height="790" alt="Screenshot (1341)" src="https://github.com/user-attachments/assets/513279ed-800a-425c-900e-b68fdb2873c3" />
    <img width="986" height="776" alt="Screenshot (1342)" src="https://github.com/user-attachments/assets/1f9eca14-f6f6-44af-bfaa-c52064dc8731" />
    <img width="1000" height="780" alt="Screenshot (1343)" src="https://github.com/user-attachments/assets/85a7ad16-438b-4557-b327-cd6fcb925ae3" />
    <img width="992" height="790" alt="Screenshot (1344)" src="https://github.com/user-attachments/assets/a19ebccf-489e-4326-bed6-a9de9bef23e0" />
    <img width="995" height="783" alt="Screenshot (1345)" src="https://github.com/user-attachments/assets/9f58264e-a3c7-4e58-804c-78d76d7a65cd" />
    <img width="989" height="785" alt="Screenshot (1346)" src="https://github.com/user-attachments/assets/eab0740d-930f-4aa0-94c2-65324adb7f09" />
    

12. Monitored the upgrade until it reached 100% completion without errors

    <img width="991" height="780" alt="Screenshot (1348)" src="https://github.com/user-attachments/assets/f9adf4db-9360-427c-ad3e-a8cd9e99c984" />


13. Validated that the oratab library has been automatically updated to point to the new 19c Oracle Home

    <img width="1050" height="598" alt="Screenshot (1349)" src="https://github.com/user-attachments/assets/f035dbd9-542f-4a77-ade4-92ab58aaf593" />


14. Verified that the sample data created in step 5 is still present and intact

    <img width="723" height="664" alt="Screenshot (1350)" src="https://github.com/user-attachments/assets/58d3a20f-f3f2-4c74-ba64-ff3ac750880c" />


15. Ran a query to ensure no critical database objects became invalid during the upgrade

    <img width="708" height="508" alt="Screenshot (1354)" src="https://github.com/user-attachments/assets/3eb67505-eeb0-4195-bc51-079ebba1844a" />


16. Once the upgrade was confirmed successful, I dropped the restore point to reclaim disk space

    <img width="720" height="591" alt="Screenshot (1352)" src="https://github.com/user-attachments/assets/d8a648d6-eb78-41ad-bbe0-eab8c3d00537" />    


17. Updated the COMPATIBLE initialization parameter to 19.0.0. This enables new 19c features that were restricted during the upgrade

    <img width="775" height="844" alt="Screenshot (1353)" src="https://github.com/user-attachments/assets/0f7bd7c5-a45d-4639-a790-813d58b3c560" />


18. Executed PL/SQL scripts to update the data dictionary and object statistics for optimal performance

    <img width="716" height="443" alt="Screenshot (1355)" src="https://github.com/user-attachments/assets/2dfd3e35-648a-4974-8a96-35ce1f409066" />


19. Verified the database is correctly registered and running under the cluster/standalone service

    <img width="555" height="51" alt="Screenshot (1356)" src="https://github.com/user-attachments/assets/20f92666-23a4-4b7c-b711-77958cbfe465" />



    


    




















