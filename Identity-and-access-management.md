Demonstrated a security strategy using User Profiles and Role-Based Access Control (RBAC). This serves as an instrument for access control, security policy, and performance optimization through effective resource management


Purpose:
- Security Hardening: Secure database security through strict password policies.
- Resource Management: Control and limit system resource consumption by user.
- Risk Mitigation: Minimize the risk of unauthorized access by following the Principle of Least Privilege.


1. Set profile parameters to manage user behavior and enforce password security policy

   <img width="541" height="219" alt="Screenshot (832)" src="https://github.com/user-attachments/assets/85ff9972-cbda-423e-bd0f-59531cd85ebb" />


2. Verified Profile Configuration used query statements to ensure the new profiles correctly added to the database instance

   <img width="731" height="461" alt="Screenshot (833)" src="https://github.com/user-attachments/assets/fc3b83fa-7e80-4ecc-bc1f-12b563f0f128" />


3. Verified the password verification functions meet minimum security standards using administrative queries

   <img width="381" height="98" alt="Screenshot (834)" src="https://github.com/user-attachments/assets/61bb4ced-2618-4839-8bb0-ae256121d6ff" />
   <img width="770" height="67" alt="Screenshot (835)" src="https://github.com/user-attachments/assets/00df5e3a-5298-44e5-832b-9e3e70e9beaa" />
   

4. Created roles to act as containers for operational privileges. This allows for easier collective maintenance of permissions based on grant option

   <img width="619" height="69" alt="Screenshot (836)" src="https://github.com/user-attachments/assets/bedc143d-0e4a-40f0-b8cf-6414b3bc6a01" />
   <img width="1518" height="232" alt="Screenshot (837)" src="https://github.com/user-attachments/assets/2e53e1d4-0393-402f-9d0f-e8dac6ce08d0" />
   

5. Created a user with a password does not meet the complex criteria to ensure exists error after passing password verification

   <img width="676" height="214" alt="Screenshot (838)" src="https://github.com/user-attachments/assets/43b3941d-20aa-4056-89c1-a1514dd74325" />


6. recreated a user account with a password meet the complex criteria security profile and setting disk resource limits (tablespace quotas) to ensure validation
   password after passing password verification

   <img width="554" height="158" alt="Screenshot (839)" src="https://github.com/user-attachments/assets/584e81c5-b922-412e-a281-d18f7edfbc05" />


7. Set the RBAC roles to the user to provide the access control for their specific tasks

   <img width="567" height="158" alt="Screenshot (842)" src="https://github.com/user-attachments/assets/a562d6c3-da79-49c2-a1b9-a7bb95def334" />


8. Validated the existence and status of the new user account through metadata queries

   <img width="1312" height="140" alt="Screenshot (840)" src="https://github.com/user-attachments/assets/731f2f6a-818e-4ac7-8c05-c6b076247e45" />


9. Performed a "penetration test" with try to execute commands outside of the privileges to confirm unauthorized actions

    <img width="815" height="304" alt="Screenshot (845)" src="https://github.com/user-attachments/assets/93d1c125-2e98-4ca8-adb1-a77e316ef182" />


11. Tested multiple failed login attempts to trigger the FAILED_LOGIN_ATTEMPTS limit until the account status changed to LOCKED

    <img width="535" height="406" alt="Screenshot (846)" src="https://github.com/user-attachments/assets/1ae8df2b-46f4-48d0-9e9d-f083becc9650" />
    

12. Unlocked the user and restore operational accessibility used a superuser (DBA)

    <img width="425" height="191" alt="Screenshot (847)" src="https://github.com/user-attachments/assets/23161599-45ac-495d-9c25-35edcd7972e2" />


   
