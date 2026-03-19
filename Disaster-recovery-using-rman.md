Implemented a data protection strategy using the RMAN (Recovery Manager) utility, including Point-in-Time Recovery (PITR). This disaster recovery method possible the database to return to a specific situation before a system failure or human error, effectively minimizing the Recovery Point Objective (RPO) and Recovery Time Objective (RTO) for mission-critical databases

Key Objectives:
- Minimize Data Loss: Restore data to the exact second before incident.
- Database Integrity: Ensure all datafiles and logs are synchronized during recovery.
- Flashback Investigation: Use metadata to identify the exact moment of failure.



1. Verified the database in ARCHIVELOG mode. This is important for disaster recovery because it saves the offline redo logs, which record all data changes

   <img width="732" height="474" alt="Screenshot (786)" src="https://github.com/user-attachments/assets/af650b48-882c-4c20-858b-38088dc3ebf3" />


2. Created test data and issued a COMMIT to ensure the changes permanently written to the redo segments and recorded in the online redo logs

   <img width="775" height="566" alt="Screenshot (787)" src="https://github.com/user-attachments/assets/df3002cb-7692-4922-823c-60afa0ccaec7" />


3. Executed the BACKUP DATABASE PLUS ARCHIVELOG command to save all critical components, including datafiles, control files, and archivelogs

   <img width="1143" height="524" alt="Screenshot (789)" src="https://github.com/user-attachments/assets/2085bc79-5173-4904-96cf-62edc1a8f080" />
   <img width="1165" height="400" alt="Screenshot (790)" src="https://github.com/user-attachments/assets/c78e2f35-1acc-4ba9-a0bd-91115acfaa68" />


4. Verified the backup status using the RMAN repository to ensure all crucial files are 'AVAILABLE'

   <img width="987" height="782" alt="Screenshot (796)" src="https://github.com/user-attachments/assets/25645ac1-2f87-4ba5-8419-8c3ad487cfc4" />


5. Performed a logical corruption with data delete to accident data loss

   <img width="728" height="544" alt="Screenshot (792)" src="https://github.com/user-attachments/assets/9c9aefea-79d4-4dc9-b5c3-40a306cff2ec" />


6. Investigated failure point used a Flashback Query to identify the exact timestamp before the data loss. This timestamp is critical for accurate PITR recovery

   <img width="794" height="309" alt="Screenshot (797)" src="https://github.com/user-attachments/assets/55e82bc8-8f85-4d5e-bb43-01bc3b51aacc" />


7. Executed PITR used the RMAN utility to perform a recovery to the specific timestamp identified. This process rolls the database back to before the incident

   <img width="771" height="570" alt="Screenshot (798)" src="https://github.com/user-attachments/assets/c6686aa3-0a28-4b03-a46f-7bce6ce2ebc0" />


8. Final verified the transaction tables to confirm the deleted data has been restored and the database has returned to its normal condition
   
   <img width="658" height="199" alt="Screenshot (799)" src="https://github.com/user-attachments/assets/284570a7-6296-4dd0-92f3-ef603400eee0" />




   

   


   

   

