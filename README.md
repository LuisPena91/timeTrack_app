# timeTrack_app

The app was developed to collect the operators’ productivity in real time. The information is imputed manually by the users, but the time is calculated automatically by the app.
Currently this app has 6 interfaces/screens: Start, Input, View/Edit, Admin, Receive and Dispatch.
The app information (data base) is storage in Share Point list, it has 6 lists: Task, ChechInCheckOut, Project, Productivity, Users and Boxes. 4 temporally collections (BoxesDispacth, BoxesNotExist, BoxesExist and BoxesReceive), and 1 excel book with the same quantity of tables as a backup. The back up and some processes are automatized with Power Automate, with 10 flows.

## Start Screen.
When the app is opened this is the first screen
![image](https://github.com/user-attachments/assets/0b70cf88-db34-4832-8643-d8c013e2ef88)
* Click on CLOCK IN when shift start.
* Click on CLOCK OUT when shift end. 

## Input Screen.
According the user profile this is the screen to input their productivity or the Input Aviva Screen.
![image](https://github.com/user-attachments/assets/afaf1383-b5f9-44d2-89f5-6e4873edc798)
* Enter or choose from the drop-down menus the following.
  - TASK - Index, Prepping, Scanning, QA, R&D, Break, Meeting, etc.
  - PROJECT – Name of the project
  - BOX ID – Xerox Barcode id as illustrated below.
* Missing BOX ID – Manually entered the box id if missing from the drop-down menu.
* Then press START button.  The app will begin tracking time spent with the task.
* Enter volume once task is completed.
* Press END button once completed.
* Repeat the same process until all the work assigned is completed.

## Input Aviva Screen.
According the user profile this is the screen to input their productivity or the Input Screen.
![image](https://github.com/user-attachments/assets/41389cd9-ca14-4e71-b04c-9d08ba4199e7)
* Select a task from the dropdown menu.
* Click on START.
* Press END button once completed.  
* Repeat the same process until all the work assigned is completed.

## View/Edit Screen.
To access click on View/Edit Daily Input button in main screen. To edit any task, it must be CLOSE.
![image](https://github.com/user-attachments/assets/07cb3ac2-3cbb-48a6-a00b-15ef6a3e0cff)
* Select the task on the table.
* The system will automatically load all fields.
* Make the necessary changes.
* Click on Edit.

## Receive Screen.
To access you must select R & D task then click in the button R & D and click on Receive.
![image](https://github.com/user-attachments/assets/4879466f-9c9f-4cba-8fe6-109e88f3a5a9)
* Select the project received.
* Scan de barcode in the box label.
* Click on Load to finish.

## Dispatch Screen.
To access you must select R & D task then click in the button R & D and click on Dispatch.
![image](https://github.com/user-attachments/assets/add990d9-9d27-4b6b-a44e-dd692bdd0daa)
* Select the project to dispatch.
* Select the new boxes status (Destroy, Return), according to the customer instructions.
* Scan the barcodes in the box label.
* Click on Download to finish.

## Admin Screen.
To access on it you must be an administrator profile, through the button Admin in the main screen.
![image](https://github.com/user-attachments/assets/f6456911-866e-4024-b43a-7b77155a3d62)



