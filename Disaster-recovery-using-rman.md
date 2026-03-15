Implementation Data protection using utility RMAN including Point in Time Recovery (PITR) as a Disaster Recovery to back on data before failure system or human error which cause loss data to minimum RPO or RTO on database mission critical



1. Verify database instance on mode Archivelog. Archivelog is important as offline redo log to saving data changer on database instance to disaster recovery.

   <img width="732" height="474" alt="Screenshot (786)" src="https://github.com/user-attachments/assets/af650b48-882c-4c20-858b-38088dc3ebf3" />


2. Create sample data and doing COMMIT to ensure data changer writing permanently into Redo segemnts and recording into online redo log on disk level before take     to archive log

   <img width="775" height="566" alt="Screenshot (787)" src="https://github.com/user-attachments/assets/df3002cb-7692-4922-823c-60afa0ccaec7" />


3. Execute BACKUP DATABASE PLUS ARCHIVELOG to saving all datafies like (Users, Control Files, Datafiles) and Archivelog as the primary component to disaster          recovery

   <img width="1143" height="524" alt="Screenshot (789)" src="https://github.com/user-attachments/assets/2085bc79-5173-4904-96cf-62edc1a8f080" />
   <img width="1165" height="400" alt="Screenshot (790)" src="https://github.com/user-attachments/assets/c78e2f35-1acc-4ba9-a0bd-91115acfaa68" />


4. Verify metadata backup to ensure all components database crutial files like datafiles, controlfiles, and archivelogs has been saving and validate status 'AVAILABLE'

   <img width="987" height="782" alt="Screenshot (796)" src="https://github.com/user-attachments/assets/25645ac1-2f87-4ba5-8419-8c3ad487cfc4" />


5. Doing logical corruption with do data delete

   <img width="728" height="544" alt="Screenshot (792)" src="https://github.com/user-attachments/assets/9c9aefea-79d4-4dc9-b5c3-40a306cff2ec" />


6. Investigate point failure using flashback query statement with last timestamp before insident data loss happens. This important to know last time before data      loss to input on PITR recovery

   <img width="794" height="309" alt="Screenshot (797)" src="https://github.com/user-attachments/assets/55e82bc8-8f85-4d5e-bb43-01bc3b51aacc" />


7. Execute PITR with utility RMAN by timestamp last time before data loss to return data before data loss
   
   <img width="771" height="570" alt="Screenshot (798)" src="https://github.com/user-attachments/assets/c6686aa3-0a28-4b03-a46f-7bce6ce2ebc0" />


8. Last verify on transaction table to ensure data is back has been delete and return to the first condition before data loss
   
   <img width="658" height="199" alt="Screenshot (799)" src="https://github.com/user-attachments/assets/284570a7-6296-4dd0-92f3-ef603400eee0" />




   

   


   

   

