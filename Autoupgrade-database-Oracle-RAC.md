Implemented a database upgrade for an Oracle RAC environment, moving from a non-CDB (12c) architecture to a Pluggable Database (PDB) in 19c. I used the AutoUpgrade utility to automate the process, ensuring a smooth transition with minimal downtime.


Advantages of Autoupgrade:
- Automated Fixups: Automatically identifies and resolves incompatibilities between versions.
- Full Automation: Handles the entire workflow: Analyze, Fixups, Deploy, and Upgrade.
- Modern Standard: Efficient and reliable than DBUA or manual scripts.
- Clean Transition: Automatically shuts down the old database version during the process.

Cons:
- Java Dependency: Needs a specific Java version available on the server.
- Complex Configuration: Configuration files must be customized for a RAC environment.





1. Confirmed the Oracle database processes are running correctly on both RAC nodes

   <img width="709" height="99" alt="Screenshot (1359)" src="https://github.com/user-attachments/assets/59d58597-6ef0-4c8e-8fe6-89ceddf34c20" />


2. Verified that the Grid Infrastructure (GI) has been upgraded to a version equal to or higher than the target database version (19c)

   <img width="838" height="269" alt="Screenshot (1363)" src="https://github.com/user-attachments/assets/2d691502-06d7-4551-9ee5-1d3980dd7227" />
   <img width="863" height="241" alt="Screenshot (1364)" src="https://github.com/user-attachments/assets/12fdebe4-802c-464d-b320-63fecd5cc939" />


3. Confirmed the 12c instances are active on all nodes before starting the upgrade

   <img width="610" height="665" alt="Screenshot (1365)" src="https://github.com/user-attachments/assets/a674153f-5675-4985-9940-e1d8bc6b63a6" />


4. Verified the COMPATIBLE parameter for both versions to ensure the transition from 12c to 19c is supported

   <img width="767" height="669" alt="Screenshot (1366)" src="https://github.com/user-attachments/assets/fa54e153-315c-4171-9210-ba171be061a7" />
   <img width="723" height="845" alt="Screenshot (1367)" src="https://github.com/user-attachments/assets/5401795f-dc92-483b-89e7-042caf4adb02" />


5. Performed a final check on existing data to ensure Zero Data Loss after the upgrade

   <img width="809" height="770" alt="Screenshot (1368)" src="https://github.com/user-attachments/assets/db285e52-f15d-47e8-bd85-573a19fafe95" />


6. Performed purged the RECYCLEBIN used sql statements to free up disk space before the upgrade

   <img width="767" height="394" alt="Screenshot (1369)" src="https://github.com/user-attachments/assets/d8043dd4-7bf3-4a3b-8cf7-afe0fce9982e" />


7. Created specific directories for logging and storing AutoUpgrade fixup data

   <img width="615" height="64" alt="Screenshot (1370)" src="https://github.com/user-attachments/assets/ef289872-1223-4bb0-ad8c-69dfd1bbf971" />


8. Created the configuration file (config.cfg) to transform the non-CDB 12c database into a 19c PDB

   <img width="550" height="287" alt="Screenshot (1371)" src="https://github.com/user-attachments/assets/41074cca-7115-451b-a6ac-3e97f23bf25a" />
   <img width="709" height="123" alt="Screenshot (1372)" src="https://github.com/user-attachments/assets/c46c0f31-40ab-4af7-9302-2ac1cdf2e07a" />


9. Verified autoupgrade version to ensured the autoupgrade.jar utility is updated to the latest version to avoid parameter incompatibilities

   <img width="974" height="249" alt="Screenshot (1373)" src="https://github.com/user-attachments/assets/deae971d-628f-4297-b108-32e10afb5dc9" />


10. Executed AutoUpgrade in ANALYZE mode to perform pre-checks and ensure the environment is ready for the upgrade

    <img width="1345" height="853" alt="Screenshot (1374)" src="https://github.com/user-attachments/assets/13455e45-3ca8-41ca-9501-be3c3a8e22e7" />
    <img width="1001" height="436" alt="Screenshot (1375)" src="https://github.com/user-attachments/assets/79956440-7d3d-42ce-8d75-07c164cb89c3" />


11. Executed the DEPLOY mode, to ran automates the pre-fixups, database upgrade, and post-fixups in one go

    <img width="1328" height="515" alt="Screenshot (1386)" src="https://github.com/user-attachments/assets/d910cf62-1c33-41ec-ade6-6cceddc73801" />
    <img width="911" height="444" alt="Screenshot (1387)" src="https://github.com/user-attachments/assets/65f1d8cf-fb46-4331-845a-542f7078a91d" />


12. Reviewed the status.html documentation to confirm all stages (Pre-checks, Upgrade, and Non-CDB to PDB conversion) were successful

    <img width="1594" height="962" alt="Screenshot (1388)" src="https://github.com/user-attachments/assets/3471bd67-7bf9-477b-9cb8-001f01413aa9" />


13. Verified the OS level on both nodes to ensure only the 19c database services are running

    <img width="710" height="78" alt="Screenshot (1389)" src="https://github.com/user-attachments/assets/0a59c328-b563-42d1-add2-a04440f6a1ba" />
    <img width="704" height="78" alt="Screenshot (1390)" src="https://github.com/user-attachments/assets/ece11303-0f72-4e85-9bfc-51a6e409b3b5" />


14. Validated the success of the non-CDB to PDB conversion using SQL*Plus. Confirmed the data accessible within the new PDB

    <img width="733" height="873" alt="Screenshot (1392)" src="https://github.com/user-attachments/assets/22e99794-3b05-4857-ae17-aa1850dc354b" />
    <img width="821" height="876" alt="Screenshot (1393)" src="https://github.com/user-attachments/assets/1352e456-a106-438a-8834-926e196d4b10" />


15. Verified the database services are active and that all datafiles and homes have been correctly updated to the 19c environment

    <img width="630" height="657" alt="Screenshot (1394)" src="https://github.com/user-attachments/assets/020ced87-a12a-47f0-ac34-a12ac5bf9ad9" />

    








   

   






   
