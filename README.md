# timeTrack_app

The app was developed to collect the operators’ productivity in real time. The information is imputed manually by the users, but the time is calculated automatically by the app.
Currently this app has 6 interfaces/screens: Start, Input, View/Edit, Admin, Receive and Dispatch.
The app information (data base) is storage in Share Point list, it has 6 lists: Task, ChechInCheckOut, Project, Productivity, Users and Boxes. 4 temporally collections (BoxesDispacth, BoxesNotExist, BoxesExist and BoxesReceive), and 1 excel book with the same quantity of tables as a backup. The back up and some processes are automatized with Power Automate, with 10 flows.

## Start Screen.
When the app is opened this is the first screen. It has 3 buttons, Clock in, Clock Out, Admin.
![image](https://github.com/user-attachments/assets/279f97f4-ff21-4420-9e09-b15ec4b33e60)

### Clock In Button.
  ![image](https://github.com/user-attachments/assets/6d818964-6c1d-4eac-8542-da175e3117c8)
Is available only if the user has not clicked on it. 
On select the system is going to create a new row in the list ChechInCheckOut, adding the current date and time and status “IN”. Then will check the user department, if it is “GI” will take it to the input screen if it is “AVIVA” to the Input_Aviva screen of it is “ALL” it will give the options to select the department.
  ![image](https://github.com/user-attachments/assets/c98ebec8-4937-45b1-9348-4f53037b37d6)
	 
### Clock Out Button.
  ![image](https://github.com/user-attachments/assets/94970df1-71fa-492d-97cb-0a077c632235)
Become available is the user has not had any task open and the status in the ChechInCheckOut list is “IN”.
On select the system will look in the ChechInCheckOut list for the row when the same user is in status IN, and will add the current date and time, change the status to OUT and calculate the time between IN and OUT.

### Admin Button.
  ![image](https://github.com/user-attachments/assets/308a2b2e-f9e4-4369-b7ae-f0923bfa1839)
It is available only for users with an admin profile.
On select the user will be taken to the Admin screen.
#### Note: 
There was an issue that after click on CLOCK IN the user was took to the Input screen and the row was created in the CheckInCheckOut list, but the system did not refresh properly so the CLOCK IN button keep available and the user was not able to start a new task so had to press again the button until it become unavailable and the user can start a task. And for every click on the button a new row was created in the list so at the end of the day it looks like the user clock in 2, 3 or 4 times so had to press the CLOCK OUT button for the same time. 
To fix this was created a container4. In this container is another container (more for presentation) and on it is a text box, a timer (is not visible for the user) and a progress bar. Every time that the user clicks on CLOCK IN this container becomes available and the timer starts. Every timer cycle is 1 second, and on each cycle the system will look if the row was successfully created in the ChechInCheckOut list, if so, will navigate to the next screen and will close the container, if not, will update the variable progress (to give the user the progress sensation), will stop the time, refresh the list, and start the timer over. This will happen till the system finds the row created with the click on the CLOCK IN button and avoids the described issue.  
This is what the last process looks like.

![image](https://github.com/user-attachments/assets/bbc676f1-8d98-428e-90cd-59d8ea8aa281)

## Input Screen.
This has 7 buttons: Clock In, Clock Out, Start, End, Admin, R & D and View/Edit Daily Input. 3 combobox: ComboBoxTask, ComboBoxProject and ComboBoxBoxId. 4 TextInput: TextInputBoxIdAlter, TextInputFolders, TextInputImages, TextInputComments. 2 labels: LableSTime and LabelSTime_1. And the DataTableProductivity.
![image](https://github.com/user-attachments/assets/f5859267-c4f4-4c73-a311-d2cce9d4a52c)

### Clock In Button.
  ![image](https://github.com/user-attachments/assets/d4afcfb6-7fed-4765-9bde-04d570f50c96)
Is available only if the user has not clicked on it. 
On select the system is going to create a new row in the list ChechInCheckOut, adding the current date and time and status IN. 

### LableSTime_1.
 ![image](https://github.com/user-attachments/assets/82a3cf06-e990-42e8-a93e-fec6c3d7d626)
It shows the clock in time.

### Clock Out Button.
 ![image](https://github.com/user-attachments/assets/075e1381-7d68-44c7-b09c-74c3640fa0c0)
Become available if the user does not have any task open and the status in the ChechInCheckOut list is IN.
On select the system will look in the ChechInCheckOut list for the row when the same user is in status IN, and will add the current date and time, change the status to OUT and calculate the time between IN and OUT.

### ComboBox Task.
  ![image](https://github.com/user-attachments/assets/48812cbd-1fea-4bb0-bc79-21d5c94cad67)
It is connected to the Task list, becomes available if the user is in status IN and if there is not any other task OPEN for this user. It is filtering the task according to the task id assigned.
If the user selects Break, Calibration, Meeting, Admin, Miscellaneous, R & D or Training. Will be available to start the task right away, otherwise will have to select a project and box id. 

### ComboBox Project.
 ![image](https://github.com/user-attachments/assets/2d186e45-102b-43f3-81e1-a8bb23c08ec9)
Becomes available depending on the task selected. It is connected to the Share Point list Project. 

### ComboBox Id.
 ![image](https://github.com/user-attachments/assets/26f48b8d-494d-45bc-9e3d-4fd837ca3640)
Becomes available after the user selects the project. It is connected to the Share Point list Boxes. It has a filter that shows only the boxes that belong to the project selected.

### TextInput BoxIdAlter.
 ![image](https://github.com/user-attachments/assets/559a0143-a4b1-4c26-93b7-6de3882938fd)
It Becomes available only when the task selected has the value “OK” in the column M_BoxId, if the task selected does not have the value “OK” but the project selected does, so it also becomes available.
This is because some projects are not received through R&D process and for some tasks who do not have barcode available. 

### Start Button.
 ![image](https://github.com/user-attachments/assets/45ea9772-ca98-4c13-a97d-900ef369e4f2)
It becomes available If the user selects Break, Calibration, Meeting, Admin, Miscellaneous, R & D or Training. Otherwise, you must select a project and box id.
On select the system is going to create a new row in the Productivity list, adding the username, user email, start date, start date, start date, start time, project, task, box id and status (OPEN).

### LableSTime.
 ![image](https://github.com/user-attachments/assets/37a6244f-1899-4060-b1c4-9f86f5b7709a)
It shows the task started time.

### TextInput Folders.
 ![image](https://github.com/user-attachments/assets/9aeca23b-3259-4a5a-a4b1-ce1504918ec0)
It becomes available if the task started was Scan, Index, Prep or Reassembly. And it is mandatory to finish the task.

### TextInput Images.
 ![image](https://github.com/user-attachments/assets/d0355052-485d-44f7-890a-42bf0c6900d9)
It becomes available if the task started was Scan, QA, Microfiche, Microfilm, WF, Merging, Post-Scan QA, or Re-Scan. And it is mandatory to finish the task.

### TextInput Comments.
 ![image](https://github.com/user-attachments/assets/27028871-4a26-4988-9f73-26610019b986)
It is always available. And is optional when the user needs to notify an unusual event.

### End Button.
 ![image](https://github.com/user-attachments/assets/37b3b73c-b9a4-4a74-9c48-bba374676d0a)
It becomes available after the user click start and the task selected was Break, Calibration, Meeting, Admin, Miscellaneous, R & D or Training. Otherwise, the user must fill in the text input images or folders, according to the task selected, or both if the task selected was scanned.
On select the system is going to look for the task in status open for the same user in the list Productivity, and will update the missing columns: end date, end time, time, folders, images, comments, and update status to CLOSE. And will reset the text inputs and combo boxes.

### Admin Button.
 ![image](https://github.com/user-attachments/assets/bb34b645-0a63-4928-911c-36267c705343)
It is available only for users with an admin profile.
On select the system will take the user to the Admin screen.

### R&D Button.
 ![image](https://github.com/user-attachments/assets/271e38b9-0afc-4de3-a74f-35d72b88000b)
It becomes available when the R&D task is started.
On select the container6 become visible.

### Container6.
 ![image](https://github.com/user-attachments/assets/ecbb955a-25af-43c1-9aa6-acd3d6269c5d)
There is a container7 (user visual), receive button, dispatch button, and return button. Its visibility is managed by the variable varRD.
#### Receive Button.
 ![image](https://github.com/user-attachments/assets/a4ca39c5-ea9e-4fb3-996c-38e61e1407fe)
On select the system will navigate to Receive screen and will close the container6.
#### Dispatch Button.
 ![image](https://github.com/user-attachments/assets/89eafcf1-384a-4d2c-b44c-e86764e278c1)
On select the system will navigate to Dispatch screen and will close the container6.
#### Return Button.
 ![image](https://github.com/user-attachments/assets/adea3b62-15ca-4166-948a-a2c35062bd90)
On select the system will close the cointainer6.

### DataTable Productivity.
 ![image](https://github.com/user-attachments/assets/e0a62d62-4f7d-4753-8034-10051db8bc04)
This table shows all the tasks done by the user in the current day. It relates to the share point list Productivity. It has 6 columns (self-explanatory).

### View/Edit Daily Input
 ![image](https://github.com/user-attachments/assets/cd5ecbfa-b4c3-4694-a740-3d1cf970a15a)
On select the system will navigate to View/Edit screen.

## Input_Aviva Screen.
This has 5 buttons: Clock In, Clock Out, Start, End and Admin. 1 ComboBoxTask. 1 LableSTime. And the DataTableProductivity.
![image](https://github.com/user-attachments/assets/b4f57acd-aee4-4a27-8844-9ad9300e2f59)

### Clock In Button.
 ![image](https://github.com/user-attachments/assets/48b46e01-5db7-410c-b768-d277cba08a5c)
Is available only if the user has not clicked on it. 
On select the system is going to create a new row in the list ChechInCheckOut, adding the current date and time and status IN. 

### LableSTime.
 ![image](https://github.com/user-attachments/assets/e04fa618-473f-4b2a-a746-07a45618aef1)
It shows the clock in time.

### Clock Out Button.
  ![image](https://github.com/user-attachments/assets/beb4b65c-1b20-4af4-8573-6edca72397e6)
Become available if the user does not have any task open and the status in the ChechInCheckOut list is IN.
On select the system will look in the ChechInCheckOut list for the row when the same user is in status IN, and will add the current date and time, change the status to OUT and calculate the time between IN and OUT.

### ComboBox Task.
  ![image](https://github.com/user-attachments/assets/df26f139-f4fb-4d0e-bea9-f95e04f529ec)
It is connected to the Task list, becomes available if the user is in status IN and if there is not any other task OPEN for this user. It is filtering the task according to the task id assigned.

### Start Button.
 ![image](https://github.com/user-attachments/assets/7e2b8adb-e6ea-4592-8e54-03bb1d31745c)
It becomes available after the user selects the task.
On select the system is going to create a new row in the Productivity list, adding the username, user email, date, start date, start time, task, and status (OPEN).

### End Button.
 ![image](https://github.com/user-attachments/assets/d9f84c35-4684-42ef-8d0b-b1f181c29046)
It becomes available if there is any task open.
On select the system is going to look for the task in status open for the same user in the list Productivity, and will update the missing columns: end date, end time, time, and update status to CLOSE. And will reset the combo box.

### DataTable Productivity.
 ![image](https://github.com/user-attachments/assets/be0699e3-ded7-478a-b67e-ed78e3132050)
This table shows all the tasks done by the user in the current day. It relates to the share point list Productivity. It has 4 columns (self-explanatory).

### Admin Button.
 ![image](https://github.com/user-attachments/assets/51951d5b-4ca0-4f7b-8d1e-1b24189aee26)
It is available only for users with an admin profile.
On select the system will take the user to the admin screen.

## View/Edit Screen.
It has 2 buttons: Edit and Return. 8 text input: TextInputTask, TextInputProject, TextInputBoxId, TextInputFolders, TextInputImages, TextInputComments, TextInputStatus, and TextInputUser. And DataTableProductivity.
![image](https://github.com/user-attachments/assets/57662233-68f0-4129-9e28-75c2afdfe2ef)

### Text Inputs.
All text inputs are self-explanatory and show information by default according to the raw selected in the DataTableProductivity. The user is available to change any of those except for Status and User.

### DataTable Productivity.
 ![image](https://github.com/user-attachments/assets/dffa4011-b0ba-45e2-87dc-5e390ea20e65)
This table shows all the tasks done by the user in the current day. It relates to the share point list Productivity. It has 6 columns (self-explanatory).

### Edit Button.
 ![image](https://github.com/user-attachments/assets/f3de3d8a-d4ba-4b69-8082-d29bf3e8f960)
Becomes available only if the task selected in the DataTable Productivity is close.
On select the system will look for the row selected in the Productivity list and will update the columns when the user made any change (task, project, box id, folders, images, or comments).

### Return Button.
 ![image](https://github.com/user-attachments/assets/02a2efd0-c733-4727-be54-16cc84105f24)
On select the system will navigate to Input screen.

## Receive Screen.
It has 2 buttons, Load and Return. One ComboBox Project and 40 text inputs.
![image](https://github.com/user-attachments/assets/f7add4a5-cbc5-4498-887e-ca61fbba31bc)

### ComboBox ProjectLoad.
 ![image](https://github.com/user-attachments/assets/62b440be-b1bd-4a55-aa10-70b08f00271e)
It relates to the current projects in the Project list.

### Load Button.
 ![image](https://github.com/user-attachments/assets/ec3234ac-467a-40a4-a078-325df3994007)
It becomes available after the user selects any project from the ComboBox ProjectLoad.
On selection the system will activate the Container2 (controlled with the variable varLoader) and will deactivate the variable varStartTimer. Then it will go for every text input and if it is not blank will increase the variable CollectionQ in 1. After it will activate the variable varStartTimer. Then the system will create a temporary collection (BoxesReceive) with the text keyed in the text inputs (only if is not blank) and add this information in the BoxesList (for Box Id, the text keyed in the textInputBox. Project, the project selected in the ComboBoxProjectLoad. Date In, today. Status, In. And Id will concatenate, project and box id). Then will run the ReceiveFlow who sent a notification email with the boxes received. At the end will reset all fields.

### Container2.
 ![image](https://github.com/user-attachments/assets/306da34c-d585-4f6c-b42a-5ce557997dc7)
It becomes available when the button Load is selected. It has a progress bar, a timer (not visible) and a text box. It gives the user a progress impression while the system loads the boxes to the Boxes list.
The timer starts with the variable varStartTimer, every lap is for 1 second, in every lap will check if the variable Progress (which start in 0) is equal to the variable CollectionQ (which start according to the quantity of boxes inputted by the user) if so, will close the Container, reset the variables, and notify the user. If it does not, it will update the variable progress + 1 and reset the timer.
The progress bar is a visual who gives the user the progress impression where the maximum value is the variable CollectionQ and the current value is Progress.
And in the text box is a default text which shows the quantity of boxes (Progress variable) loaded of boxes to load (CollectionQ).
### Return Button.
 ![image](https://github.com/user-attachments/assets/ad06ced7-2c29-4483-a3e0-affea9942555)
On select will navigate to the Input screen.

## Dispatch Screen.
 ![image](https://github.com/user-attachments/assets/cb099fc1-8d4a-4465-8646-685bf9385fe0)
It has 2 buttons, DownLoad and Return. 2 ComboBox, Project and Status. And 40 text input.

### ComboBox ProjectDownLoad.
 ![image](https://github.com/user-attachments/assets/b06a5f31-edd4-4647-9588-88da65d24d0a)
It relates to the current projects in the Project list.

### ComboBox Status.
 ![image](https://github.com/user-attachments/assets/60f9f3f1-a6dc-4927-9fb7-ca032243256a)
It has a default list to select between Destroy or Return.

### Download Button.
 ![image](https://github.com/user-attachments/assets/495645d6-3e2d-404c-b350-6360a5419752)
It becomes available after the user selects the project to dispatch and the new boxes status.
On select the system will activate the Container2_1 (controlled with the variable varLoader) and will deactivate the variable varStartTimer. Then it will go for every text input and if it is not blank will increase the variable CollectionQ in 1. After it will activate the variable varStartTimer. Then the system will create a temporary collection (BoxesDispatch) with the text keyed in the text inputs (only if it is not blank). Then will run the DispatchFlow who sent a notification email with the dispatched boxes. Then for all boxes in the temporary collection (BoxesDispatch) will validate if all ids exist in the BoxesList, if so, it will update that row with the Date Out and the new status, and it will create a new collection (BoxesExist) with the same information. If it does not exist it will create a collection (BoxesNotExist) with the same information. Then will run BoxesExistFlow who sent a notification email with the updated boxes and if the collection BoxesNotExist is not empty will run the flow BoxesNotExistFlow who sent a notification email with the boxes not updated. At the end will reset all the collections, combo box and text input.

### Container2_1.
 ![image](https://github.com/user-attachments/assets/1f99e6fb-4a25-4aa3-ac3e-161ed992fa69)
It becomes available when the button DownLoad is selected. It has a progress bar, a timer (not visible) and a text box. It gives the user a progress impression while the system loads the boxes to the Boxes list.
The timer starts with the variable varStartTimer, every lap is for 1 second, in every lap will check if the variable Progress (which start in 0) is equal to the variable CollectionQ (which start according to the quantity of boxes inputted by the user) if so, will close the Container, reset the variables, and notify the user. If it does not it will update the variable progress + 1 and reset the timer.
The progress bar is a visual who gives the user the progress impression where the maximum value is the variable CollectionQ and the current value is Progress.
And in the text box is a default text which shows the quantity of boxes (Progress variable) loaded of boxes to load (CollectionQ).

### Return Button.
 ![image](https://github.com/user-attachments/assets/b2221615-7af3-40de-94ea-0bdaff3fbb94)
On select will navigate to the Input screen.

## Admin Screen.
There are 6 buttons: Users, Projects, Clock In, Productivity, BI Reports and Navigate.
![image](https://github.com/user-attachments/assets/5bbbe8dc-d9c6-494b-ac84-5f29f2ab1c5b)

### Users Button.
 ![image](https://github.com/user-attachments/assets/d8c2c3dc-dda2-4fe2-a9c4-98811a32a677)
On select it will show a container with:
#### Combobox Users-Admin.
 ![image](https://github.com/user-attachments/assets/5ee8e8c2-dcd8-4dca-94d2-ce220fdd50b7)
It relates to the Users list in share point and shows only the active users.
#### Table Users.
 
It relates to the Users list in share point, is showing by default all users or filter the user according to the user selected on the combobox user-admin. It has 6 columns self-explanatory.
#### New User Button.
 ![image](https://github.com/user-attachments/assets/3ad140f8-f76b-4e4e-8159-022a0330e56a)
On select it will show the follow form.

 ![image](https://github.com/user-attachments/assets/62a600b6-3ee1-465f-8456-ed9c9bbc977f)
It is necessary to fill all fields to create a new user.
  ##### Add User Button.
 ![image](https://github.com/user-attachments/assets/a586fb01-d4c9-4ee3-bbbc-b755994956ec)
  It becomes available when all fields are filled.
  On select the system will check if the user already exists if not, will create a new user adding the values imputed, caNumber, UserId, Status (Active), Profile (EndUser), Email and department. And it will create a JSON collection to send an email with the details of the new user created.
  ##### Cancel User Button.
 ![image](https://github.com/user-attachments/assets/4475c081-8ac6-4e4c-ac12-3d214a845c81)
  On select it will clean the fields and close the form.
#### Edit User Button.
 ![image](https://github.com/user-attachments/assets/4ae44823-64c8-4d68-a65a-42088b050db7)
On select it will show the follow form.

![image](https://github.com/user-attachments/assets/ccae4b4f-16cb-49c4-9b5e-562751dbaf74)
This form will show the details of the user selected in the users table or keying caNumber (Xerox ID) the other fields will fill automatically.
##### Edit User Button.
 ![image](https://github.com/user-attachments/assets/dd0b6020-1883-4967-ab86-415d9c3ad861)
On select the system will update all the fields for the caNumber keyed or selected.
##### Cancel User Button.
 ![image](https://github.com/user-attachments/assets/67ff4936-8a28-4c07-9881-fc368dbfc752)
On select it will clean the fields and close the form.
#### Inactive User Button.
 ![image](https://github.com/user-attachments/assets/32faadae-1e81-44e6-aadc-c7d57b3037d8)
On select it will show the follow form.

![image](https://github.com/user-attachments/assets/89c0a84d-cb03-4f56-afb5-5a5ca2d89b35)
This form will show the details of the user selected in the users table or keying caNumber (Xerox ID) the other fields will fill automatically.
##### Delete Button.
 ![image](https://github.com/user-attachments/assets/252b49d9-169e-4ea4-aa95-b4c02f6cd646)
On select the system will update the status to inactive for the user selected.
##### Cancel User Button.
 ![image](https://github.com/user-attachments/assets/6393004b-9953-4133-a82b-c997439c3c7e)
On select it will clean the fields and close the form.

### Projects Button.
 ![image](https://github.com/user-attachments/assets/2a3e87a4-04fd-45e1-b5f0-4291db72cdc8)
On select it will show the following lists.
#### ListBox Project.
 
It shows the current projects in the Project list.
#### New Project Button.
![image](https://github.com/user-attachments/assets/e218d68a-808a-4336-b3d3-59bc9a465249)
On select it will show the follow form.

![image](https://github.com/user-attachments/assets/47b697ec-ca44-4c52-8a57-9b8f7c1a2fd1)
##### Add Project Button.
![image](https://github.com/user-attachments/assets/2cb02421-b439-476f-993b-ac30ea28137f)
It becomes available only when the project name field is filled.
On select the system will create a new project on the Projects list with status active.
##### Cancel Button.
![image](https://github.com/user-attachments/assets/5e8cb37f-f54c-4a9f-ae9a-a5181866ef16)
On select it will clean the fields and close the form.
#### Edit Project Button.
![image](https://github.com/user-attachments/assets/9c9937f2-f98d-47a0-91c5-7cd95ea3fc02)
On select it will show the follow form.
##### Reactivate Project Button.
 ![image](https://github.com/user-attachments/assets/b4016285-7433-4f01-9b1f-30a734bf50ac)
It becomes available if the project selected in the project list or input in the Project Name field is inactive.
On select the system will update the status to active for the project selected.
##### Cancel Button.
![image](https://github.com/user-attachments/assets/a2d51684-bf1c-45cb-b405-d8ff893a47dd)
On select it will clean the fields and close the form.
#### Delete Project Button.
 ![image](https://github.com/user-attachments/assets/581a4f7c-7b06-465b-8999-e5ef7ec94e8a)
On select it will show the follow form.

![image](https://github.com/user-attachments/assets/c31c99d6-28cd-4be0-a0be-8a81f1a0b608)
##### Delete Project Button.
 ![image](https://github.com/user-attachments/assets/b93194b2-ca73-4d52-bb2b-3ca4bcfc71c8)
It becomes available if the project selected in the project list is active.
On select the system will change the project status to Inactive.
##### Cancel Button.
 ![image](https://github.com/user-attachments/assets/eedccab9-a917-4d68-b560-a4ec8f790560)
On select it will clean the fields and close the form.

### Clock In Button.
 ![image](https://github.com/user-attachments/assets/7666d9ea-2daf-4c4d-a90a-3886e2315b33)
On select the system will show the following table.
#### Table ClockIn.
 
It relates to the CheckInCheckOut list, and it shows the clock in time for all users for the current day.

### Productivity Button.
 ![image](https://github.com/user-attachments/assets/4ee55b25-02ae-426a-a52a-b82ea5793276)
On select the system will show the following table.
#### ComboBox Users.
 ![image](https://github.com/user-attachments/assets/b3d2b36a-9888-4a50-8a67-d8b4de3e5f47)
It relates to the Users list, on select any user will filter the table productivity.
#### Table Productivity.

It relates to the Productivity list, it shows the current task for all users, when a user is selected in the ComboBox Users it will show all the task done for that user in the current day. 

### BI Reports Button.
![image](https://github.com/user-attachments/assets/ed6645b6-9337-4cf1-bdbf-2687cc196ba2)
On select it will show the follow buttons.
#### GI Dashboard Button.
 ![image](https://github.com/user-attachments/assets/c0f29b9d-7a80-464b-9fb3-80512cbc2783)
On select it will open a Power Bi report, with the entire information of all projects, time, cost, and productivity by user. 
#### Comparatives Button.
 ![image](https://github.com/user-attachments/assets/68eb06b1-a17e-4787-af09-24a8705c340a)
On select it will open a Power Bi report, with the comparative in the time worked and productivity time by user, the difference between the SCAN and POST-SCAN QA images and the total time worked by user.

### Navigate Button.
 ![image](https://github.com/user-attachments/assets/a1a58a78-65af-4364-aade-3011c95c823d)
On select it will show the follow buttons.
#### GI -Input Button.
 ![image](https://github.com/user-attachments/assets/02fd1aac-740b-4b41-be5f-a088faa887f8)
On select the system will navigate to the Input screen.
#### AVIVA – Input Button.
 ![image](https://github.com/user-attachments/assets/afdefdd5-48c1-4c40-b8b6-42d31a542c35)
On select the system will navigate to the Input_Aviva screen.
#### Main Button.
 ![image](https://github.com/user-attachments/assets/f3c1cf99-a840-417c-b1f1-04728524cd00)
On select the system will navigate to the Start screen.




