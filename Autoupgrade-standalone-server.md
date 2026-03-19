Implemented a database upgrade from version 12c to 19c on a standalone server with Oracle Restart and Grid Infrastructure 19c. Upgrading the database is a critical DBA task to ensure the system benefits from the latest security patches, modern architecture, and new features.


Advantages of Autoupgrade:
- Automated Detection: Automatically detects errors and performs fixups.
- Scalability: Can upgrade multiple databases simultaneously.
- Architecture Conversion: Supports seamless conversion from Non-CDB to PDB.
- Speed & Modernity: Faster and is the currently recommended method by Oracle.



1. Ensured the Grid Infrastructure and both database versions (12c and 19c) are running on the same server

   <img width="737" height="125" alt="Screenshot (1257)" src="https://github.com/user-attachments/assets/d8bae02a-fe3c-4d7e-bfd0-0c1ac936e22a" />


2. Created an AutoUpgrade directory for logs and fixup files, following the Optimal Flexible Architecture (OFA)

   <img width="644" height="144" alt="Screenshot (1258)" src="https://github.com/user-attachments/assets/a989597c-60e7-4181-9a74-5e92eed3ca59" />


3. Verified the current user environment is pointing to the old (12c) database

   <img width="509" height="139" alt="Screenshot (1259)" src="https://github.com/user-attachments/assets/2345b462-58b8-4ac0-baec-f043f2ff6ca3" />


4. Inserted test data into the 12c database to verify Zero Data Loss after the upgrade

   <img width="836" height="808" alt="Screenshot (1305)" src="https://github.com/user-attachments/assets/902a83cd-e030-451d-ab2e-ac420066b9ab" />


5. Switched the environment using (. oraenv) to the new 19c version to manage the upgrade process

   <img width="615" height="190" alt="Screenshot (1263)" src="https://github.com/user-attachments/assets/a25159ef-09e4-44db-aba5-2e36fcd0074e" />


6. Confirmed the baseline data is not yet present in the new version before the upgrade starts

   <img width="721" height="150" alt="Screenshot (1264)" src="https://github.com/user-attachments/assets/15456435-425e-4195-aa1f-89cce7470933" />


7. Verified autoupgrade.jar is the latest version to ensure compatibility with 19c and future versions

   <img width="1525" height="214" alt="Screenshot (1290)" src="https://github.com/user-attachments/assets/5328fb6c-a661-4198-80ba-c5162ac58f91" />


8. Created configuration file to move the data from Non-CDB to a Pluggable Database (PDB)

   <img width="648" height="269" alt="Screenshot (1294)" src="https://github.com/user-attachments/assets/a137d9c1-b4d2-44b3-907a-43acd856271b" />


9. Verified the configuration file exists and has the correct oracle user ownership

   <img width="716" height="156" alt="Screenshot (1295)" src="https://github.com/user-attachments/assets/e831b04b-0666-4895-843d-9781dfdbc45d" />


10. Ran AutoUpgrade in ANALYZE mode to checks compatibility and ensures the environment is ready for the upgrade

    <img width="1401" height="824" alt="Screenshot (1296)" src="https://github.com/user-attachments/assets/b9b22803-6bdf-49e9-87d2-2ef4df67a078" />


11. Reviewed Analyze report the generated HTML documentation to ensure no errors found during the pre-checks

    <img width="1843" height="977" alt="Screenshot (1297)" src="https://github.com/user-attachments/assets/f9f8e5b2-3a3f-4d3a-ba12-2b5c8dec7172" />


12. Executed the DEPLOY mode to start the actual automated upgrade process

    <img width="1398" height="697" alt="Screenshot (1301)" src="https://github.com/user-attachments/assets/d83621ae-9a2a-4b7c-af7d-04fb1afbd4b3" />


13. Monitored the log files in real-time to ensure no critical errors or bottlenecks

    <img width="1012" height="876" alt="Screenshot (1302)" src="https://github.com/user-attachments/assets/d2e263ce-cce1-4830-ab3d-f82e35206f78" />
    <img width="1665" height="594" alt="Screenshot (1303)" src="https://github.com/user-attachments/assets/522d15aa-c426-44df-9386-ea6255869bd4" />


14. Reviewed the success HTML report. Confirmed all stages including drain, postupgrade, and noncdbtopdb—were successful

    <img width="1841" height="974" alt="Screenshot (1308)" src="https://github.com/user-attachments/assets/c6352a2f-9939-494c-8e3d-4bf091da3927" />


15. Verified the 12c background processes no longer running and have been replaced by the 19c processes

    <img width="721" height="154" alt="Screenshot (1309)" src="https://github.com/user-attachments/assets/e014291d-91c9-450d-9f53-de35784d2203" />
    <img width="1054" height="582" alt="Screenshot (1312)" src="https://github.com/user-attachments/assets/796c793e-8386-403f-946e-169853cd012b" />


16. Verified that the new PDB is correctly configured and registered within the 19c instance

    <img width="726" height="415" alt="Screenshot (1310)" src="https://github.com/user-attachments/assets/23c504c6-08e2-4708-8838-d8707bd20344" />


17. Validated the baseline data in the new PDB to ensure all data was migrated successfully with Zero Data Loss

    <img width="842" height="875" alt="Screenshot (1311)" src="https://github.com/user-attachments/assets/c75805cd-f46b-464a-9309-e9ac7992ad6c" />








1. 
