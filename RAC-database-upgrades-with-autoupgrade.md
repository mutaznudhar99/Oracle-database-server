I will be implementing Oracle RAC database upgrade from non-cdb (12c) to pdb (19c) using autoupgrade. Why using autoupgrade?

Pros autoupgrade:
- Fixups incompatible dofferent version
- Automatic fixups, analyze, deploy, and upgrade database
- Modern method than DBUA and manual upgrade
- Resume upgrade if disaster happens during upgrade
- Automatic shutdown previous database during autoupgrade running


Cons autoupgrade:
- 



1. Verify backround process Oracle Database. Ensure bot database system running on same server

   <img width="709" height="99" alt="Screenshot (1359)" src="https://github.com/user-attachments/assets/59d58597-6ef0-4c8e-8fe6-89ceddf34c20" />


2. Verify version grid Infrastructure has upgrade to top version than old database version. This is so important because can't upgrade database 12c to 19c if grid
   still old version

   <img width="838" height="269" alt="Screenshot (1363)" src="https://github.com/user-attachments/assets/2d691502-06d7-4551-9ee5-1d3980dd7227" />
   <img width="863" height="241" alt="Screenshot (1364)" src="https://github.com/user-attachments/assets/12fdebe4-802c-464d-b320-63fecd5cc939" />


3. Verify instance old database running on both server

   <img width="610" height="665" alt="Screenshot (1365)" src="https://github.com/user-attachments/assets/a674153f-5675-4985-9940-e1d8bc6b63a6" />


4. Verify parameter compatible old database version and new database version. Ensure after autoupgrade success, parameter compatible chanage from old compatible
   to new compatible

   <img width="767" height="669" alt="Screenshot (1366)" src="https://github.com/user-attachments/assets/fa54e153-315c-4171-9210-ba171be061a7" />
   <img width="723" height="845" alt="Screenshot (1367)" src="https://github.com/user-attachments/assets/5401795f-dc92-483b-89e7-042caf4adb02" />


5. Verify existing data to make data there still (zero data loss) after autoupgrade success

   <img width="809" height="770" alt="Screenshot (1368)" src="https://github.com/user-attachments/assets/db285e52-f15d-47e8-bd85-573a19fafe95" />


6. Execute purge rececylbin to refuse trace file or death file/data give more space disk before autouprgade

   <img width="767" height="394" alt="Screenshot (1369)" src="https://github.com/user-attachments/assets/d8043dd4-7bf3-4a3b-8cf7-afe0fce9982e" />


7. Create direktori to saving logs or processing fixups, analyze, deploy, and upgrade

   <img width="615" height="64" alt="Screenshot (1370)" src="https://github.com/user-attachments/assets/ef289872-1223-4bb0-ad8c-69dfd1bbf971" />


8. Create configuration autoupgrade on new database version to upgrade non-cdb to pdb

   <img width="550" height="287" alt="Screenshot (1371)" src="https://github.com/user-attachments/assets/41074cca-7115-451b-a6ac-3e97f23bf25a" />
   <img width="709" height="123" alt="Screenshot (1372)" src="https://github.com/user-attachments/assets/c46c0f31-40ab-4af7-9302-2ac1cdf2e07a" />


9. Verify autoupgrade version on new database version. Ensure autoupgrade version has to upgrade to latest version. This is important to autoupgrade can readness
   configuration autoupgrade. Sometimes, if autoupgrade old version theres incompatible with parameter on configuration autoupgrade

   <img width="974" height="249" alt="Screenshot (1373)" src="https://github.com/user-attachments/assets/deae971d-628f-4297-b108-32e10afb5dc9" />


10. 




   

   






   
