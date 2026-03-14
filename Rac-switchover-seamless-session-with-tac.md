Demonstrate transparent application continuity (TAC) on Real Application Cluster (RAC) server to ensure Database system stay High Availibity and seamless session while doing switchiver or failure happens. Purpose the transaction process can be replay transaction if swithcover or failure happens. 



1. Verify all stack clusterware and listener stay online on all node database instance

   <img width="563" height="89" alt="Screenshot (1014)" src="https://github.com/user-attachments/assets/a5767f24-4ff1-4aff-a0e2-ffda421327df" />
   <img width="560" height="150" alt="Screenshot (1030)" src="https://github.com/user-attachments/assets/2d7c1771-aecb-4847-91ed-9ef14854768b" />


2. Create configuration database service with parameter Transparent Application Continuity (TAC). This is important to enable database cluster (RAC) to seamless      session

   <img width="734" height="387" alt="Screenshot (1031)" src="https://github.com/user-attachments/assets/039b62d0-394a-4403-940f-b299e9a311a6" />


3. Validate configuration TAC on database instance using query statement on sqlplus to ensure TAC parameter ready on database instance RAC

   <img width="1480" height="490" alt="Screenshot (1221)" src="https://github.com/user-attachments/assets/97a8c85a-7f2e-4536-a565-7c246ef6f658" />


4. Configuration parameter connection string database instance with TAC and SCAN IP to interconnect side application to database instance clusterware (RAC) with      fitur (TAC) 

   <img width="982" height="208" alt="Screenshot (1032)" src="https://github.com/user-attachments/assets/df3d481b-28d7-4c3f-9d10-e1361c1628ea" />


5. Copy connection string TAC to side application (Swingbench) to interconnect side application to clusterware (RAC) with fitur (TAC)

   <img width="643" height="334" alt="Screenshot (1033)" src="https://github.com/user-attachments/assets/9190a560-4221-45c2-9bad-11a9c82624ba" />


6. Execute transaction on database instance without commit to ensure the trancation can be replay to another node during switchover or failure happens without        error

   <img width="552" height="287" alt="Screenshot (1222)" src="https://github.com/user-attachments/assets/feb25828-abe7-45b0-a196-be607003a123" />


7. Shutdown one of all node RAC with execute commit on another session at the same time and verify transaction replay running smoothly to available node       

   <img width="1501" height="546" alt="Screenshot (1223)" src="https://github.com/user-attachments/assets/b14d5782-3d40-4a9f-8cb5-df80b75c2cc3" />


8. Compare connection database clusterware (RAC) without fitur (TAC) to ensure database instance getting error issues (Connection Lost)

   <img width="1723" height="476" alt="Screenshot (1224)" src="https://github.com/user-attachments/assets/85ec82da-d5b4-4308-a679-757bb0bbcc2e" />



    
