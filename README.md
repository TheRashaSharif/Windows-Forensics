# Windows-Forensics PART I

<h1> This is an exercise provided by the TryHackMe DFIR course </h1>

 
 #### All images and copyrights are protected for TryHackMe

<h2>Description</h2>
Project: Windows artifact in several registry hives.
Objective: Identify where different files for the relevant registry hives are located and load them into the tools of our choice. 



<h2> Scenario </h2>

One of the Desktops in the research lab at Organization X is suspected to have been accessed by someone unauthorized. Although they generally have only one user account per Desktop, there were multiple user accounts observed on this system. It is also suspected that the system was connected to some network drive, and a USB device was connected to the system. The triage data from the system was collected and placed on the attached VM. 

<h2>Tools/ Utilities Used</h2>

- <b> Windows Virtual Machine</b> 
- <b> Eric Zimmerman's (EZ)  registry explorer Tool</b>

Tasks:
- Explore SAM hive for user information. 
- Find information about specific files and user accounts.
- Find USB information in the SYSTEM and SOFTWARE hives.

The artifacts location: a folder in the desktop called triage has a collection collected through Kape ( a forensics tool), this collection has the same directory structure as the parent. 

![DFIR desktop](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/4a9cd9ba-b08d-4cc4-9d02-d93d8ff941f4)


We will open the artifacts in Eric Zimmerman's registry explorer tool as the following: 



![DFIR EZ tool](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/63ce8449-365a-434b-895c-7d48d66cc723)


![DFIR regestry tool2](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/3fc5864e-5ecd-4f8d-a840-1b827a46a80e)


 1- SAM users hives: 

The location of the artifacts as we mentioned is in the Triage folder on the desktop. It has the same path /location as any registry hive. 
To open a hive from the file menu in the upper left corner, we select File--> Load Hive

![DFIR file load hive](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/77559ec2-dab7-4915-9363-1d69f17cd8fd)

We will navigate and open the following location to find users information:  PC\Desktop\triage\c\windows\system32\config\SAM

![DFIR artifacts location](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/4aef0ab2-376b-4eb9-8831-8411a28f834f)


The artifacts are loaded on the left side there is a tree view of the registry hive(s). Expand: 
Root\SAM\Domains\Users to view the users' accounts in the left side viewer.

There are three (3) user accounts created in this case.

![DFIR user accounts 3](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/c5aaee57-8a49-4680-9710-5f171303daea)

In the view pane on the left side, we can find helpful information about the users. For example, the User name, and log-in times ( thm-user2 logged 0 times for instant while thm-4n6 logged 19 times)
Password hint for each user for example for user thm-406 it is count, and it is null for the other two users ( so they did not use a hint).

 2- Users infrormation : 

  If we want to find more information about a specific user activity or the admin activities, we can load artifacts from the user-created account as the following: 
PC\Desktop\triage\c\users\THM-4n6\NTUSER.DAT

![DFIR NTUSER artifacts loading](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/968f587d-49f9-4072-abe6-8bd91eeb4004)

We might get questions about the "log" not being clean and we have to load the. LOG 1 & 2 to save a clean copy.
Usually, we will be looking at the actual registry data to compare.
  

To locate a file .txt we navigate to the location
SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs

Finding when the file was launched is displayed in the right-side viewing pane
![DFIR recent doc txt](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/86806c78-e75f-4ee3-b37f-bc83e775480f)


To locate when a Python setup we go to the location: in this case, we found the one that has a Python set up.
SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\ then we look in each of the folders inside the count
![DFRI python setup](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/5563d97a-4769-4b3c-8553-888d96f8335e)


z:\setups\python-3.8.3.exe

  3- USB information in SYSTEM and SOFTWARE hives 
Find the USB named Kingstone (the friendly name is USB)  in the SOFTWARE &  SYSTEM hives: to locate USB information like the last opened. 

To load the SYSTEM hives in the EZ's tool we go to the File menu and select load hives. Hives are in the following location:
PC\Desktop\triage\c\windows\system32\config\SYSTEM 

![DFIR SYSTEM hives USB Kings](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/6fe79284-18e9-45e6-be33-90ba24a7913d)


Usually, USB s are saved in the following location: ROOT\ControlSet001\Enum\USB or USBSTORE

![DFIR SYSTEM hive kingston](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/f1338307-0747-4938-af9e-10207bebe8a7)

We can locate two USBs one of them is the Kingston but we have to find the friendly name USB ( in the SOFTWARE hives).

Now let's load the SOFTWARE hives to find the clue. Following the location PC\Desktop\triage\c\windows\system32\config\SOFTWARE

![DFIR software hives](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/3951c27d-3275-4318-8b9a-1b7b81399b7e)

To locate the USB in the SOFTWARE, we can navigate to the location Microsoft\Windows Portable Devices\Devices
Here we found a USB with a friendly name DATA USB and the Disk ID is {E251921F-4DA2-11EC-A783-001A7DD7110} this is the desk ID we should match in the SYSTEM hives.

Back to the SYSTEM hives ( all loaded hives will remain in the left side tree view until closed:
Location COntrolSet001\Enum\USBSTOR

Note: the Kingston USB has the same Desk ID ( Serial number) {E251921F-4DA2-11EC-A783-001A7DD7110}
![DFIR SYSTEM hives USB Kings](https://github.com/TheRashaSharif/Windows-Forensics/assets/98124961/62a8eaba-a1b8-4fbe-a34c-1a6c69a175ba)

Therefore it is the USB we need the Last connect info.



That was an intro to Windows artifacts.
   See you in part 2.
   
