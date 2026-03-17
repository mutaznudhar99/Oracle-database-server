Demonstrated Transparent Application Continuity (TAC) on an Oracle Real Application Clusters (RAC) environment. The goal is to ensure high availability and a seamless user session during a node failure or switchover. TAC enables the database to automatically replay in-flight transactions on an available node without the application experiencing a timeout or connection error

Core Objectives:
- Seamless Failover: Prevent session termination when a specific RAC node goes down.
- Transaction Replay: Automatically resubmit uncommitted transactions to a surviving node.
- High Availability: Maintain 24/7 database operations without manual application restarts.



1. Confirmed the Oracle Clusterware (Grid Infrastructure) and listeners are online and healthy across all RAC nodes

   <img width="563" height="89" alt="Screenshot (1014)" src="https://github.com/user-attachments/assets/a5767f24-4ff1-4aff-a0e2-ffda421327df" />
   <img width="560" height="150" alt="Screenshot (1030)" src="https://github.com/user-attachments/assets/2d7c1771-aecb-4847-91ed-9ef14854768b" />


2. Created a specific database service using srvctl with TAC parameters enabled. This is the foundation for enabling seamless session failover in a cluster

   <img width="734" height="387" alt="Screenshot (1031)" src="https://github.com/user-attachments/assets/039b62d0-394a-4403-940f-b299e9a311a6" />


3. Validate TAC Parameters used SQL*Plus to query the database metadata, ensuring that the TAC parameters (such as FAILOVER_TYPE=AUTO) are active on the RAC          instance

   <img width="1480" height="490" alt="Screenshot (1221)" src="https://github.com/user-attachments/assets/97a8c85a-7f2e-4536-a565-7c246ef6f658" />


4. Configure Connection String (TNS) using the SCAN IP and the TAC-enabled service name to interconnect the application with the RAC clusterware

   <img width="982" height="208" alt="Screenshot (1032)" src="https://github.com/user-attachments/assets/df3d481b-28d7-4c3f-9d10-e1361c1628ea" />


5. Configured the application (Swingbench) with the TAC connection string to test real-world workload continuity

   <img width="643" height="334" alt="Screenshot (1033)" src="https://github.com/user-attachments/assets/9190a560-4221-45c2-9bad-11a9c82624ba" />


6. Executed a DML operation (INSERT) without issuing a COMMIT. This allows us to test if the database can replay this "in-flight" transaction if a failure occurs

   <img width="552" height="287" alt="Screenshot (1222)" src="https://github.com/user-attachments/assets/feb25828-abe7-45b0-a196-be607003a123" />


7. Shut down one RAC node while simultaneously attempting to commit the transaction from the application. Verified that the transaction successfully replayed and     committed on the surviving node without error   

   <img width="1501" height="546" alt="Screenshot (1223)" src="https://github.com/user-attachments/assets/b14d5782-3d40-4a9f-8cb5-df80b75c2cc3" />


8. Tested a standard connection string without TAC features. Confirmed that when a node fails in this scenario, the application immediately receives a "Connection    Lost" or "Communication Channel" error.

   <img width="1723" height="476" alt="Screenshot (1224)" src="https://github.com/user-attachments/assets/85ec82da-d5b4-4308-a679-757bb0bbcc2e" />



    
