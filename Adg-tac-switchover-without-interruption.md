Impmented Transaction Application Continuity (TAC) on Active Data Guard to ensure transaction stay running without interruption on side application during database system failover or swithover



1. Validate beginning on grid infratructure to ensure database primary and standby online status

   <img width="559" height="383" alt="Screenshot (1169)" src="https://github.com/user-attachments/assets/05b56535-62de-4ccd-a4e3-958b0556210c" />
   <img width="567" height="386" alt="Screenshot (1170)" src="https://github.com/user-attachments/assets/959b9f0b-05b6-49d3-8be4-46c60c7a3132" />


2. Do configuring parameter database service TAC to registry and handled database system transaction contiunuity 

   <img width="693" height="305" alt="Screenshot (1177)" src="https://github.com/user-attachments/assets/ebb00e23-3e8d-4aea-a6ad-747b56328e59" />


3. Do sincronize library tnsnames.ora on side application to direct connection between application and database system TAC mode

   <img width="567" height="414" alt="Screenshot (1204)" src="https://github.com/user-attachments/assets/4bc57b24-62cd-4249-a1eb-a097e2745e51" />


4. Verify parameter TAC running on database system using query statement on sqlplus 
   
   <img width="1135" height="615" alt="Screenshot (1211)" src="https://github.com/user-attachments/assets/6b511b42-4d33-41c8-8611-5bd4dcb514fc" />


5. Verify database instance status to ensure sincronyze running good way before failover or switchver happens
   
   <img width="591" height="319" alt="Screenshot (1173)" src="https://github.com/user-attachments/assets/034c0662-0db5-46bf-89b4-4c2c5763a751" />
   <img width="597" height="304" alt="Screenshot (1174)" src="https://github.com/user-attachments/assets/a9639295-e789-4acd-bbf3-08d5b0bc6b56" />


6. Do inject data (Inert) without Commit to ensure transaction in a suspended state while session terminated during database system failure or switchover 

   <img width="651" height="391" alt="Screenshot (1212)" src="https://github.com/user-attachments/assets/50cdc630-fd82-4640-ac79-7b241b1246ea" />


7. Do switchover and make sure transaction will pause because side application currently doing transaction replay to target new database primary without session      terminated

   <img width="1567" height="461" alt="Screenshot (1214)" src="https://github.com/user-attachments/assets/3f8efcda-ef60-48c8-8e65-abf807d08581" />


8. Verify commit transaction success after switchover without intterupted or error issues. This is function of TAC to transaction replay to new database              primary/server without error issues

   <img width="1789" height="524" alt="Screenshot (1215)" src="https://github.com/user-attachments/assets/d446e86d-989b-46ce-a314-4fab632a6332" />


9. Verify role database instance has change to new database primary with session stay online without interruption

    <img width="1758" height="811" alt="Screenshot (1217)" src="https://github.com/user-attachments/assets/329a7e0f-b579-42f7-9f95-3ec13e14a97b" />

    

   

   

   

   


   




