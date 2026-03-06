in this session, i will implementing Oracle database upgrade from 12c to 19c in server standalone/oracle restart with grid infrastructure 19c and autoupgrades. Oracle upgrades its one of most important as a tasks of dba
because the database system needs to configure or switch to new compatabale version for upgrade security, architecture, new features, and etc. 

why choosing autoupgrades for upgrade database system?
- automatic detection debug, error, and etc
- upgrade more 1 database
- could upgrade from non-cdb to pdb
- very faster
- modern method
   
what do i need?
- Server standalone/oracle restart
- grid infrastructure 19c
- oracle database 12c
- oracle database 19c
- autoupgrades tool




1. Ensure the grid and both database 12c and 19c running on same server

   <img width="737" height="125" alt="Screenshot (1257)" src="https://github.com/user-attachments/assets/d8bae02a-fe3c-4d7e-bfd0-0c1ac936e22a" />


2. Create direktory autoupgrade for saving file log or anything during autoupgrade based on Optimal Flexible Architecture (OFA)

   <img width="644" height="144" alt="Screenshot (1258)" src="https://github.com/user-attachments/assets/a989597c-60e7-4181-9a74-5e92eed3ca59" />


3. Check environtment with user oracle. ensure the env running by old version database

   <img width="509" height="139" alt="Screenshot (1259)" src="https://github.com/user-attachments/assets/2345b462-58b8-4ac0-baec-f043f2ff6ca3" />


4. Create baseline data on old database for testing existing data. Ensure zero data loss if autoupgrade successfully.

   <img width="836" height="808" alt="Screenshot (1305)" src="https://github.com/user-attachments/assets/902a83cd-e030-451d-ab2e-ac420066b9ab" />


5. Create new environtment with . oraenv for running new version database as a moderator processing autoupgrade

   <img width="615" height="190" alt="Screenshot (1263)" src="https://github.com/user-attachments/assets/a25159ef-09e4-44db-aba5-2e36fcd0074e" />


6. Ensure no one baseline data from old version database on new version database

   <img width="721" height="150" alt="Screenshot (1264)" src="https://github.com/user-attachments/assets/15456435-425e-4195-aa1f-89cce7470933" />


7. Validation tool autoupgrade.jar running for latest version. Ensure the latest version autoupgrade.jar as a tool has compatibility better than old version for processing autoupgrade from 12c to new version like 18, 19, 21, 26.

   <img width="1525" height="214" alt="Screenshot (1290)" src="https://github.com/user-attachments/assets/5328fb6c-a661-4198-80ba-c5162ac58f91" />


8. Create own configuration for processing autoupgrade from old version database non-cdb to new version database pdb

   <img width="648" height="269" alt="Screenshot (1294)" src="https://github.com/user-attachments/assets/a137d9c1-b4d2-44b3-907a-43acd856271b" />


9. Verify existing the configuration and ensure the config has ownership oracle db

   <img width="716" height="156" alt="Screenshot (1295)" src="https://github.com/user-attachments/assets/e831b04b-0666-4895-843d-9781dfdbc45d" />


10. Precheck configuration with tool autoupgrade.jar by mode ANALYZE. Mode Anlyze can precheck the all compatibility between old version database and new version database and Ensure its compatible and keep running autoupgrade

    <img width="1401" height="824" alt="Screenshot (1296)" src="https://github.com/user-attachments/assets/b9b22803-6bdf-49e9-87d2-2ef4df67a078" />


11. Validate precheck with documentation (html). Ensure no one error issues during precheck running.

    <img width="1843" height="977" alt="Screenshot (1297)" src="https://github.com/user-attachments/assets/f9f8e5b2-3a3f-4d3a-ba12-2b5c8dec7172" />


12. Deploy autoupgrade for processing automatic upgrade from old version database to new version database.

    <img width="1398" height="697" alt="Screenshot (1301)" src="https://github.com/user-attachments/assets/d83621ae-9a2a-4b7c-af7d-04fb1afbd4b3" />


13. Monitoring process autoupgrade with log file. Ensure no one error critical issues during autoupgrade running that can cause processing bootleneck, shutdown, and corrupt file environtment

    <img width="1012" height="876" alt="Screenshot (1302)" src="https://github.com/user-attachments/assets/d2e263ce-cce1-4830-ab3d-f82e35206f78" />
    <img width="1665" height="594" alt="Screenshot (1303)" src="https://github.com/user-attachments/assets/522d15aa-c426-44df-9386-ea6255869bd4" />


14. Validate autoupgrade process successfully with documentation (html). Ensure all process like prefixups, prechecks, drain, postupgrade, noncdbtopdb, and sysupdates success

    <img width="1841" height="974" alt="Screenshot (1308)" src="https://github.com/user-attachments/assets/c6352a2f-9939-494c-8e3d-4bf091da3927" />


15. Ensure the old version database (12c) there no on processing backround monitor

    <img width="721" height="154" alt="Screenshot (1309)" src="https://github.com/user-attachments/assets/e014291d-91c9-450d-9f53-de35784d2203" />
    <img width="1054" height="582" alt="Screenshot (1312)" src="https://github.com/user-attachments/assets/796c793e-8386-403f-946e-169853cd012b" />


16. Verify existing new pdb has been configuration

    <img width="726" height="415" alt="Screenshot (1310)" src="https://github.com/user-attachments/assets/23c504c6-08e2-4708-8838-d8707bd20344" />


17. Validate baseline data from old version database on new version database and ensure zero data loss on pdb

    <img width="842" height="875" alt="Screenshot (1311)" src="https://github.com/user-attachments/assets/c75805cd-f46b-464a-9309-e9ac7992ad6c" />








1. 
