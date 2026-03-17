Implemented Transaction Application Continuity (TAC) on an Oracle Active Data Guard environment. This configuration ensures that database transactions remain uninterrupted during a failover or switchover, providing a seamless experience for the application layer by automatically replaying in-flight transactions on the new primary database



Core Objectives:
- Zero Interruption: Prevent session termination during database role changes.
- Transaction Replay: Automatically replay uncommitted transactions on the new primary instance.
- High Availability: Maintain continuous application connectivity without manual intervention.



1. Verified the status of both Primary and Standby databases using Grid Infrastructure tools to ensure they are online and healthy

   <img width="559" height="383" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/05b56535-62de-4ccd-a4e3-958b0556210c" />
   <img width="567" height="386" alt="Screenshot (1170)" src="https://github.com/user-attachments/assets/959b9f0b-05b6-49d3-8be4-46c60c7a3132" />


2. Created and configured a dedicated database service with TAC parameters to handle transaction continuity and session draining

   <img width="693" height="305" alt="Screenshot (1177)" src="https://github.com/user-attachments/assets/ebb00e23-3e8d-4aea-a6ad-747b56328e59" />


3. Updated the tnsnames.ora file on the application side to use the new TAC-enabled service string for database connections

   <img width="567" height="414" alt="Screenshot (1204)" src="https://github.com/user-attachments/assets/4bc57b24-62cd-4249-a1eb-a097e2745e51" />


4. Verified TAC Parameters used SQL*Plus to query and confirm that the Transaction Application Continuity parameters are correctly registered within the database     instance
   
   <img width="1135" height="615" alt="Screenshot (1211)" src="https://github.com/user-attachments/assets/6b511b42-4d33-41c8-8611-5bd4dcb514fc" />


5. Verified that the Data Guard replication is synchronized and that both instances are ready for a role transition
   
   <img width="591" height="319" alt="Screenshot (1173)" src="https://github.com/user-attachments/assets/034c0662-0db5-46bf-89b4-4c2c5763a751" />
   <img width="597" height="304" alt="Screenshot (1174)" src="https://github.com/user-attachments/assets/a9639295-e789-4acd-bbf3-08d5b0bc6b56" />


6. Performed a data injection (INSERT) without a COMMIT. This puts the transaction in a "suspended" state to test the replay capability

   <img width="651" height="391" alt="Screenshot (1212)" src="https://github.com/user-attachments/assets/50cdc630-fd82-4640-ac79-7b241b1246ea" />


7. Initiated a database switchover. During this phase, the application session pauses as TAC begins replaying the uncommitted transaction to the new primary          database

   <img width="1567" height="461" alt="Screenshot (1214)" src="https://github.com/user-attachments/assets/3f8efcda-ef60-48c8-8e65-abf807d08581" />


8. Confirmed that the transaction was successfully committed after the switchover was completed, without any session errors or interruptions

   <img width="1789" height="524" alt="Screenshot (1215)" src="https://github.com/user-attachments/assets/d446e86d-989b-46ce-a314-4fab632a6332" />


9. Verified that the database roles have successfully swapped, while the application session remained active and connected throughout the process

    <img width="1758" height="811" alt="Screenshot (1217)" src="https://github.com/user-attachments/assets/329a7e0f-b579-42f7-9f95-3ec13e14a97b" />

    

   

   

   

   


   




