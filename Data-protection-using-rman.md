Demoonstrate physical backup on Oracle database using native tool Recovery Manager (RMAN). In this simulation i will be implementing simulation loss of critical files database on Automatic storage managemenr (ASM) to testing resilience system and solve with automatic disaster recovery (ADR) RMAN to minimum downtime.

Pros RMAN:
- Support hot backup (Backup online)
- Support Increment and Compression backup
- Support validation backup
- Automatic restoration through RMAN respository
- Integrated with media management layer (OCI, TAPE, DISK)




1. Verify parameter db_recovery_file_dest and directory size to ensure storage backup location has been configuration on ASM

   <img width="734" height="373" alt="Screenshot (766)" src="https://github.com/user-attachments/assets/2e00ea77-6216-43d7-b3aa-b3ebcd599a3c" />


2. Verify all database resources, listener, ASM instance in online status through crsctl utility before take backup, cause database system can't take backup if       Oracle Cluster Resgitry (OCR) and ASM not running. 

   <img width="830" height="640" alt="Screenshot (765)" src="https://github.com/user-attachments/assets/297e6329-6c25-482a-9378-624e8b9ddee2" />


3. Validate directory structure and space availiability on ASM disk group through user grid before take physical backup file. Ensure size ASM Disko Group still       more space to saving pysical backup

   <img width="1534" height="257" alt="Screenshot (767)" src="https://github.com/user-attachments/assets/e0db52bd-cfa6-4bd8-9a99-defd2c84a6a4" />


4. Create data sample to validate last data before loss of critical files going happens

   <img width="821" height="472" alt="Screenshot (773)" src="https://github.com/user-attachments/assets/a5a2695c-1371-42b5-bff8-b0a3ec29d42a" />


5. Take Backup database plus acrhivelog to saving copy all datafiles, controlfiles, spfile, and archivelogs database system to Ensure database has copy data to
   Disaster Recovery if loss ritical files going happens 

   <img width="1148" height="683" alt="Screenshot (768)" src="https://github.com/user-attachments/assets/5f181ff5-96b3-41ca-964d-b629a574fd87" />


6. Validate metadata backup after take backup using list backup command line to ensure all component crucial files already saving on Disk Group name resgitry

   <img width="1002" height="575" alt="Screenshot (769)" src="https://github.com/user-attachments/assets/7fe5d098-70ae-4a93-a0be-c7344d671038" />


7. Verify ASM Disk Group to ensure all physical backup has been formed and resgitry on ASM Instance 

   <img width="1549" height="304" alt="Screenshot (771)" src="https://github.com/user-attachments/assets/705b4bb7-e99b-46a0-af71-2369a5c98c03" />


8. Drop critical files (Datafiles users) on ASM instance to ensure database system can't running database if one of the crucial files has gone

   <img width="1533" height="395" alt="Screenshot (776)" src="https://github.com/user-attachments/assets/ad2ccfc4-69d7-412a-84c0-6ff5509f840b" />


9. Read data using query statement on sqlplus to ensure database system can't read datafile, cause the backround process DBWR/LGWR try to write to missing files  

   <img width="760" height="622" alt="Screenshot (778)" src="https://github.com/user-attachments/assets/f3c657dc-d226-420c-b3b9-2e99bca1778f" />


10. Do recovery database using automatic disaster recocery to read system failure automate, to gave solution recovery database, and fix it database automate.   
    
    <img width="1153" height="796" alt="Screenshot (780)" src="https://github.com/user-attachments/assets/ed38f652-76a8-4ff8-84ac-258ed49f73b4" />
    
    <img width="1126" height="763" alt="Screenshot (781)" src="https://github.com/user-attachments/assets/2635dad9-0617-4d79-ba90-dcf3f4bd8e75" />
      
    <img width="1209" height="805" alt="Screenshot (782)" src="https://github.com/user-attachments/assets/69b9f35e-a4af-43cc-88c7-c0fbad1655f7" />
  

11. Verify datafile instance after recovery database to ensure all datafiles in a state online and database system in a state open
    
    <img width="834" height="500" alt="Screenshot (783)" src="https://github.com/user-attachments/assets/23e65d1a-729c-42d9-8ffa-02c34af97af4" />
    <img width="737" height="143" alt="Screenshot (785)" src="https://github.com/user-attachments/assets/015a44f6-7dbf-4693-865f-361352a86dcd" />


12. Do read data using query statement to ensure database system can read datafiles thourgh backround process like DBWR/LGWR after recovery database 

    <img width="600" height="149" alt="Screenshot (784)" src="https://github.com/user-attachments/assets/15f5fe4a-75f2-4260-bdeb-4796f73a482b" />


    
