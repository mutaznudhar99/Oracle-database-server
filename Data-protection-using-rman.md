Demonstrated a physical backup and recovery process using Oracle’s native tool, Recovery Manager (RMAN). In this simulation, I tested system resilience by deleting critical database files from Automatic Storage Management (ASM) and resolving the failure using RMAN’s Data Repair Advisor to minimize downtime

Advantages od RMAN:
- Hot Backup: Supports online backups while the database is active.
- Efficiency: Supports incremental backups and data compression.
- Validation: Built-in features to verify backup integrity.
- Automation: Simplifies restoration through the RMAN repository.
- Integration: Works seamlessly with media management layers (Cloud, Tape, Disk).




1. Verified the db_recovery_file_dest parameter and storage size to ensure the backup location is correctly configured on ASM

   <img width="734" height="373" alt="Screenshot (766)" src="https://github.com/user-attachments/assets/2e00ea77-6216-43d7-b3aa-b3ebcd599a3c" />


2. Verified all cluster resources used the crsctl utility were online. RMAN need Oracle Cluster Registry (OCR) and ASM to be running to function properly

   <img width="830" height="640" alt="Screenshot (765)" src="https://github.com/user-attachments/assets/297e6329-6c25-482a-9378-624e8b9ddee2" />


3. Inspected the ASM Disk Group structure and available space through the Grid user to ensure enough capacity for the physical backup

   <img width="1534" height="257" alt="Screenshot (767)" src="https://github.com/user-attachments/assets/e0db52bd-cfa6-4bd8-9a99-defd2c84a6a4" />


4. Inserted test data to provide a baseline for validating data integrity after the recovery process

   <img width="821" height="472" alt="Screenshot (773)" src="https://github.com/user-attachments/assets/a5a2695c-1371-42b5-bff8-b0a3ec29d42a" />


5. Executed a full database backup, including Archivelogs, to create a complete copy of the datafiles, control files, and SPFILE

   <img width="1148" height="683" alt="Screenshot (768)" src="https://github.com/user-attachments/assets/5f181ff5-96b3-41ca-964d-b629a574fd87" />


6. Verified backup metadata used the list backup command to ensure all crucial components successfully saved to the Disk Group

   <img width="1002" height="575" alt="Screenshot (769)" src="https://github.com/user-attachments/assets/7fe5d098-70ae-4a93-a0be-c7344d671038" />


7. Verified the ASM instance to confirm that the physical backup files correctly created and registered

   <img width="1549" height="304" alt="Screenshot (771)" src="https://github.com/user-attachments/assets/705b4bb7-e99b-46a0-af71-2369a5c98c03" />


8. Drop a critical datafile (the USERS tablespace) from the ASM instance to trigger a system failure

   <img width="1533" height="395" alt="Screenshot (776)" src="https://github.com/user-attachments/assets/ad2ccfc4-69d7-412a-84c0-6ff5509f840b" />


9. Performed query the database in SQL*Plus. The query failed because the background processes (DBWR/LGWR) could not find the missing datafile

   <img width="760" height="622" alt="Screenshot (778)" src="https://github.com/user-attachments/assets/f3c657dc-d226-420c-b3b9-2e99bca1778f" />


10. Recovery database used the Data Repair Advisor (list failure, advise failure, repair failure) to automatically diagnose the problem and restore the                missing files  
    
    <img width="1153" height="796" alt="Screenshot (780)" src="https://github.com/user-attachments/assets/ed38f652-76a8-4ff8-84ac-258ed49f73b4" />
    
    <img width="1126" height="763" alt="Screenshot (781)" src="https://github.com/user-attachments/assets/2635dad9-0617-4d79-ba90-dcf3f4bd8e75" />
      
    <img width="1209" height="805" alt="Screenshot (782)" src="https://github.com/user-attachments/assets/69b9f35e-a4af-43cc-88c7-c0fbad1655f7" />
  

11. Verified the datafiles again to ensure they back ONLINE and the database instance in OPEN stat
    
    <img width="834" height="500" alt="Screenshot (783)" src="https://github.com/user-attachments/assets/23e65d1a-729c-42d9-8ffa-02c34af97af4" />
    <img width="737" height="143" alt="Screenshot (785)" src="https://github.com/user-attachments/assets/015a44f6-7dbf-4693-865f-361352a86dcd" />


12. Successfully ran a query statement to confirm the database could read the restored datafiles and the system was fully recovered

    <img width="600" height="149" alt="Screenshot (784)" src="https://github.com/user-attachments/assets/15f5fe4a-75f2-4260-bdeb-4796f73a482b" />


    
