<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> 

## List of Prerequisites
| Item                                    | Purpose                                      | Download Link |
|----------------------------------------|-----------------------------------------------|---------------|
| Microsoft Azure Free Account           | Host the virtual machine                      | [azure.microsoft.com/free](https://azure.microsoft.com/free) |
| PHP Manager for IIS                    | Manage PHP within IIS                         | PHPManagerForIIS_V1.5.0.msi |
| Microsoft Visual C++ Redistributable  | Required runtime libraries                    | VC_redist.x86.exe (or x64 depending on setup) |
| MySQL 5.5.62                                   | Database server                               | Official MySQL archive |
| osTicket Installation Files (v1.15.x or latest stable) | Core application files          | [github.com/osTicket/osTicket](https://github.com/osTicket/osTicket/releases) |
| HeidiSQL (optional but recommended)   | MySQL GUI client                              | [heidisql.com](https://www.heidisql.com/) |

<br/>



### Step 1: Create a Virtual Machine in Microsoft Azure

To run osTicket, we need a Windows host system. In this lab we will use **Microsoft Azure** with the free tier subscription (more than sufficient for this entire lab).

If you don’t already have one, sign up for a free Azure account here:  
https://azure.microsoft.com/free/
<p>

   

To run osTIcket we are going to need a host system, 
I have decided to use Microsoft Azure as my cloud hosting service for a client to run osTicket on.
to get setup create a Microsoft Azure account and select the free subscription provided. Then, follow these steps..

1. Log in to the [Azure Portal](https://portal.azure.com)
2. In the search bar at the top, type **Virtual Machines** → click it


<img width="1353" height="935" alt="image" src="https://github.com/user-attachments/assets/ed107ad8-b007-4f50-9b91-6acb39ce5bec" />

3. Click **+ Create**

<img width="2290" height="680" alt="image" src="https://github.com/user-attachments/assets/adc3acbc-fb2f-49b8-b793-ad57fc11ff83" />

4. Choose a windows 10 image from the dropdown selection, then make sure under vm size, you run atleast 2 virtual cpus
5. Create a memerable username and password for the vm
6. Allow imbound RDP port traffic on port 3389 ( so we can remote into the machine) 


<img width="781" height="1304" alt="image" src="https://github.com/user-attachments/assets/84bf7dab-5aa0-46be-b855-e42861e5287e" />

7. go ahead and proceed to review + create, make sure the license agreement is checked.

<img width="378" height="164" alt="image" src="https://github.com/user-attachments/assets/d0e81e48-3214-4dbc-89a7-44ffba4e3ac4" />

8. After the machine has been deployed go ahead and copy the public ip address and use rdp to connect.

<img width="2537" height="713" alt="image" src="https://github.com/user-attachments/assets/2ff8ef7b-4f0d-4abf-91d8-1c7c636a7090" />


<img width="399" height="247" alt="image" src="https://github.com/user-attachments/assets/9ea7e867-2e08-48ce-86c7-1780775faab2" />

9. Continue with your selected login credentials when you created the vm.

<img width="451" height="439" alt="image" src="https://github.com/user-attachments/assets/d9dcaf8c-fb04-4775-9cb5-18d64c895b4a" />




### Step 2: Gather and centralize all of the dependancies for osTicket
-You can refer to the links mentioned in the beginning of this walkthrough


1. Once logged into the vm, head to Microsoft edge and paste the google drive link in the search header:
https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0

<img width="1234" height="288" alt="image" src="https://github.com/user-attachments/assets/8f27739d-8133-42d1-9825-1b135b2d8bd8" />

( you may also refer to the offical https://osticket.com/download/ for an updated version, the google drive folder
simply has the collected dependancies in one location. )

2. Lets go ahead and extract these instillation programs into one uncompressed folder.

<img width="611" height="452" alt="image" src="https://github.com/user-attachments/assets/08c4df5e-46fc-4f67-a962-3ad26b81c023" />

### Step 3: Install/Enable IIS in Windows with CGI.

-Why Enabling IIS (With CGI) is critical to hosting osTicket on windows:

osTicket is a PHP based web application, enorder for it to work on a windows web server, the server must be able to
execute PHP code and return safe HTML to the visitor's machine.

-IIS will handle the incoming HTTP requests, and act as a gateway for blocking dangerous requests and handing raw PHP data over to osTickets' host PHP engine for execution.

-If IIS is not present or not configured with the proper PHP hanlder mapping. Any request for a index.php has the ability to:
download the raw PHP data form the osTicket's hosting webserver, exposing all of your stored passwords, code and database credentials.

We can go ahead and configure our loopback address (127.0.0.1) as the webserver to host osTicket on let's continue..


1. Navigate to "uninstall a program" under the programs tab in the control pannel
<img width="1099" height="471" alt="image" src="https://github.com/user-attachments/assets/03d1c133-e16a-4c42-b053-ec7f644a701d" />

2. On the left hand side access the "Turn Windows features on or off" button

<img width="815" height="355" alt="image" src="https://github.com/user-attachments/assets/f5ce5d51-e578-4532-bb6c-4b36f7c45817" />

3. Inside the Windows features is where we will find the IIS tab
   go ahead and enable the feature.

<img width="417" height="369" alt="image" src="https://github.com/user-attachments/assets/3ec7649e-3622-4033-8a0c-2216ff20bb48" />

###4. Now we install the CGI dependency.
go ahead and open the windows feature tab again and select th Internet Information Services folder we just enabled,
navigate to: 
Internet Information Services > 
World Wide Web Services > 
Application Development Features >
CGI

then check the enable box.

<img width="411" height="368" alt="image" src="https://github.com/user-attachments/assets/fb26f461-85e6-4a9c-bdbe-cf582b6beeef" />

great now we have all of our web server dependancies to host through localhost

### Step 5: Go ahead and put your loopback address "127.0.0.1" into your browser.


<img width="1239" height="759" alt="image" src="https://github.com/user-attachments/assets/219207f7-f774-405b-8351-4c92d32dcd93" />

Congratulations your now hosting a webserver, we will now configure this webserver to host osTicket.

### Step 6: Open osTicket-Instilation-Files
1. once inside select PHPManagerForIIS_V1.5.0

<img width="971" height="679" alt="image" src="https://github.com/user-attachments/assets/8e731cd6-2e05-4aaf-a3eb-f41e9462e5b5" />

2. Agree to all of the terms, then continue

3. do the same with the rewrite_amd64_en-US installer

<img width="912" height="663" alt="image" src="https://github.com/user-attachments/assets/3fe7cc5e-35c5-4583-9c35-f35058e3c50f" />

( this is a microsoft extention for IIS that will turn https://127.0.0.1/osticket/upload/scp/tickets.php?id=123
into a cleaner url like: https://127.0.0.1/osticket/upload/tickets/123 )


### Step 7. Create the directory C:\PHP

1. open file explorer select This PC > WIndows(C:)

2. Then create a new folder and name it "PHP"

<img width="1086" height="523" alt="image" src="https://github.com/user-attachments/assets/9c3e1582-10e6-4c5a-bb07-ed54d8969895" />

### Step 8: Unzip php-7.3.8-nts-Win32-VC15-x86 and extract it to our new PHP folder in our :C directory

1. extract the  php-7.3.8-nts-Win32-VC15-x86 zip file to: C:\PHP

( This zipped file contains all the binaries for php that osTicket will use )

<img width="2243" height="622" alt="image" src="https://github.com/user-attachments/assets/271976a8-017b-419b-a394-364c76b50b85" />

### Step 9: Install VC_redist.x86
(these are essentail DLL microsoft files that help osTicket run on windows)

1. You just have to agree to terms and click install

<img width="482" height="301" alt="image" src="https://github.com/user-attachments/assets/49a73ce4-2de5-4ef8-bbe1-4885348c4265" />

### Step 9: Install HeidiSQL_12.3.0.6589_Setup/ mysql-5.5.62-win32
(This tool will allow us to create and manage the MySQL/MariaDB database where 
osTicket will securely store all tickets, users, and configuration data. )

1. Accept the license agreement and it will create a new directory on our :C drive.

<img width="593" height="452" alt="image" src="https://github.com/user-attachments/assets/2aa35ac1-bc73-4518-9d7a-c3f1b431c68f" />

2. If propted to choose a type of installation choose "Typical".

<img width="605" height="473" alt="image" src="https://github.com/user-attachments/assets/db663837-2659-4f37-a953-94c5e666ec71" />


3. Continue with next if prompted through the SQL configuration wizard.

4. Then, choose "Standard" configuration.

<img width="608" height="466" alt="image" src="https://github.com/user-attachments/assets/b77c4a0a-1d82-4d78-968f-a575f56b5847" />

6. Hit next
   
7. When prompted for root password choose a strong password. (do not loose it)
<img width="607" height="461" alt="image" src="https://github.com/user-attachments/assets/393b3aa8-bb2a-4bb8-83a1-ba1c767b6e44" />

Great now we have an installed database to work with.
Now, we have to configure php through IIS.

### Step 10 configure PHP through IIS.
1. open Internet Information Services (IIS) Manager and run as admin.
   
<img width="824" height="679" alt="image" src="https://github.com/user-attachments/assets/66306cc5-dcf1-46df-9182-78c47175e1ea" />

2. Open the PHP manager Icon.

<img width="1495" height="408" alt="image" src="https://github.com/user-attachments/assets/44382d5b-0d0c-4000-b408-e8f45c64a962" />

3. Register a new PHP version.

   <img width="712" height="649" alt="image" src="https://github.com/user-attachments/assets/b645f61b-b41a-4faa-970f-1b2dbf3eb40f" />


4. When prompted to select a path
 we will use our previously created PHP folder on our C drive
 and select the php-cgi application.
   C:\PHP\php-cgi.exe

<img width="499" height="218" alt="image" src="https://github.com/user-attachments/assets/1a4cf69c-8ea9-42b8-9069-067a58933aba" />

5. Then stop and restart IIS.

<img width="1905" height="772" alt="image" src="https://github.com/user-attachments/assets/31518e15-2698-4cec-8fd9-edc55dbb07eb" />


### Step 11: Install osTicket

1. Go back to the osTicket.zip folder.
 
2. Then extract it to the specified folder.

<img width="1118" height="746" alt="image" src="https://github.com/user-attachments/assets/66f0d0a4-b822-4108-924d-e10046775638" />



Now we should have two folders that are inside the osTicket installation, the "upload"
file will be integeral to congiure our webserver to run osTicket, we simply need to change our configuration file for our webserver.

3. Copy the "upload" folder to the "c:\inetpub\wwwroot" directory.

<img width="745" height="591" alt="image" src="https://github.com/user-attachments/assets/4d1e9569-8cf9-493b-b232-04c1e0892bb5" />


(This c:\inetpub\wwwroot path is just the root for our webserver settings.)

4. For clarity sake, lets rename this "upload" folder to: osTicket.

<img width="832" height="343" alt="image" src="https://github.com/user-attachments/assets/739452a8-7c27-40b3-b582-5b437fbfdac3" />


5. Now becuase we made a change to the root directory, let's refresh IIS by restarting our webserver in the IIS menu.

<img width="1905" height="630" alt="image" src="https://github.com/user-attachments/assets/f8a98727-8001-4394-8929-b363966f149a" />

 Let's try to view the configuration by accessing:  sites > default > osTicket

6. click the dropdown in the left column for osTicket-vm (or whatever you choose to name your virtual machine.)

<img width="212" height="300" alt="image" src="https://github.com/user-attachments/assets/fa4417ac-b26e-496e-85b7-5cb4ad00063e" />

7. Click sites.

<img width="209" height="251" alt="image" src="https://github.com/user-attachments/assets/41137ba8-e4c8-49e9-ac92-f7c5e8a51362" />

8. Click Default Web Site.

<img width="203" height="279" alt="image" src="https://github.com/user-attachments/assets/0c6207a7-0de3-43bb-b580-98fa0218d26c" />

9. Click the osTicket folder.

<img width="1899" height="709" alt="image" src="https://github.com/user-attachments/assets/dd5ffbc5-aa93-4f92-ab65-a54b654dc927" />

10. Now in the right-side column under manage folder, click "Browse*:80(http)

<img width="1909" height="684" alt="image" src="https://github.com/user-attachments/assets/d6bbd3c9-ccee-4337-8eb5-2e7614c75359" />

congrats! we now can view our osTicket site!

<img width="1575" height="1034" alt="image" src="https://github.com/user-attachments/assets/51d0c442-186e-4210-972a-002ca176daa1" />

Now we got a lot of the heavy setup out of the way we just need to settup some of
osTicket's configuration settings for QoL and performance.



### Step 12: Configure IMAP Extension/Intl Extension for osTicket


<img width="856" height="789" alt="image" src="https://github.com/user-attachments/assets/ccb6b20e-273d-4930-86bd-48608d6f3a4c" />

1. Head to the IIS Manager then select osTicket > PHP Manager

<img width="1895" height="896" alt="image" src="https://github.com/user-attachments/assets/0364a5dc-d2c1-4a52-be70-f29b73d1ead9" />

2. Once inside, go to enable PHP Extensions.

<img width="1017" height="840" alt="image" src="https://github.com/user-attachments/assets/9d2e66bb-3f84-4d8e-9799-8ed518dd7310" />

3. Now lets enable the following:
   php_imap.dll
   php_intl.dll
   php_opache.dll
   
<img width="1590" height="1014" alt="image" src="https://github.com/user-attachments/assets/92be2fb8-2d09-4b70-85b9-d9317d19059b" />

4. Now, let's refresh osTicket.

<img width="828" height="780" alt="image" src="https://github.com/user-attachments/assets/9505747f-3260-4b0a-99e3-d79e430128c5" />

Now we just need to rename:  

ost-config.php
From: C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php
To: C:\inetpub\wwwroot\osTicket\include\ost-config.php

This is where osTicket stores user configuration settings and currently the path is not defined in osTicket's directory.
We will also need to grant higher privileges to osTicket for it to write to this file.

### Step 12 rename:  ost-config.php
1. Go to File Explorer, C: > Inetpub > wwwroot > osTicket > include

2. scroll down to find ost-sampleconfig.php

<img width="1120" height="654" alt="image" src="https://github.com/user-attachments/assets/e264ab76-3680-4b92-873c-b4bad96fbe3d" />

3. Right-click nename  "ost-sampleconfig.php" to "ost-config.php"

4. Rick click ost-oconfig.php

5. View properties, click the Security tab
   
<img width="356" height="504" alt="image" src="https://github.com/user-attachments/assets/cc09f43b-a8ea-4ab2-a23c-6b778718e769" />

6. Click advanced,

<img width="359" height="505" alt="image" src="https://github.com/user-attachments/assets/8d796bd1-df69-4ffd-8cd5-e71902dc6424" />


7. Disable inheritance.

<img width="762" height="517" alt="image" src="https://github.com/user-attachments/assets/68da4345-13c4-4eb6-85c2-9e927875fa5e" />

8. Click remove all inherited permissions

<img width="782" height="507" alt="image" src="https://github.com/user-attachments/assets/b0f04802-b171-4116-932d-3940ab9875cf" />

9.We can see now that we have cleared all permissions for this config php file, now we will add new permissions.

<img width="769" height="527" alt="image" src="https://github.com/user-attachments/assets/2109e46d-76a4-43c6-8b16-4371b5c23017" />

10. Select principal,

11. in this menu we will be able to select group control access to this file, for this example we will choose everyone, but this is not a reccommend default policy.

<img width="461" height="277" alt="image" src="https://github.com/user-attachments/assets/3016da98-7e3c-489b-a661-8e9891508886" />
    
12. Then select "OK"

13. From here, we will grant all permissions, then press "OK".

<img width="917" height="585" alt="image" src="https://github.com/user-attachments/assets/bdeeb51e-63e0-4aac-adfa-402153862c83" />

14. Now our permission entries have been updated, click "apply" and then select "ok"

<img width="760" height="516" alt="image" src="https://github.com/user-attachments/assets/2c3cb934-3da0-400f-b22a-308b5afe6ae4" />

15. On the properties menu select "ok"

<img width="359" height="505" alt="image" src="https://github.com/user-attachments/assets/b5250978-844f-4a7a-8fb3-a9f262cbef91" />


### Step 13 Configure osTicket settings:

1.Continue back to our osTicket page, then press "continue"

<img width="1666" height="1035" alt="image" src="https://github.com/user-attachments/assets/117f7483-320d-4938-a599-5f95d9a667a3" />


2. Now we will be prompted with fields for continuing the osTicket instillation

<img width="1583" height="897" alt="image" src="https://github.com/user-attachments/assets/b725a0cd-f2a8-4fe7-a72a-efd3fa91765d" />

3. Feel free to choose any credentials of our liking 

 We will now need to continue with our database settup


### Step 14 Configure Database settings:

1. Head to file explorer > Desktop > osTicket-Instillation-Files >  osTicket-Instillation-Files
C:\Users\azureuser\Desktop\osTicket-Installation-Files\osTicket-Installation-Files

2. Double click HeidiSQL_12.3.0.6589_Setup 

<img width="1130" height="341" alt="image" src="https://github.com/user-attachments/assets/d3734e1b-2bfb-4a6b-b68c-6770e0c2f965" />

3. Click "accept", then "Next" > "Install"

4. Now with HeidiSQL opened, we will connect our osTicket database we originaly created in step 9


5. Click New, 

<img width="687" height="485" alt="image" src="https://github.com/user-attachments/assets/7c27c0b5-67a2-4431-942b-de9e5491a9cd" />

6. Then, enter your username and password you created for the database in step 9.

<img width="685" height="480" alt="image" src="https://github.com/user-attachments/assets/e056f1d2-dae2-43d6-a238-872ab79aa99c" />

7. Then, click open,

**If you run into any issues getting HeidiSQL to find the database we created, I reccommend checking the services.mcs list and look to see if SQL is active,
<img width="413" height="224" alt="image" src="https://github.com/user-attachments/assets/54f2e177-b155-4841-8567-d8b252a0c05e" />

 also you can check if there is any activity on port 3306 buy using the command in cmd as admin:
 C:\Users\azureuser>netstat -ano | findstr :3306
C:\Users\azureuser>sc query type= service state= all | findstr /I "mysql mariadb"

<img width="887" height="462" alt="image" src="https://github.com/user-attachments/assets/e68710dc-b640-40ee-b4a0-37b698d821e5" />


### Step 14: Creating the osTicket database
1. Right click the un-named tag in the left column, 

<img width="984" height="542" alt="image" src="https://github.com/user-attachments/assets/6f457bd5-7a67-4074-8252-6ea636a558c9" />

2. Right click Un-named > Create New > Database
<img width="514" height="444" alt="image" src="https://github.com/user-attachments/assets/6e212048-9dd1-49bf-b444-4edfc305f842" />

3. Name the database "osTicket", then click "ok"

<img width="320" height="267" alt="image" src="https://github.com/user-attachments/assets/4904aae1-a224-438e-815a-3a2611ab54c4" />

now we have our osticket configured database ready,

<img width="981" height="546" alt="image" src="https://github.com/user-attachments/assets/c3c3c793-8f0b-4de3-be2c-b8125f5500e9" />

Let's head back to the osTicket installation and fill out the remaining SQL settings.


### Step 15: Finish SQL configuration, 
1. Enter in the MySQL database we just made named "osTicket"

2. Enter the SQL Username and Password from step 9

<img width="635" height="337" alt="image" src="https://github.com/user-attachments/assets/b2b66185-b6ca-45cb-a362-0d2f385c0e59" />

3. Then click "Install Now"

<img width="625" height="514" alt="image" src="https://github.com/user-attachments/assets/01e50e9f-7b24-46cb-aa62-bbe186fae23a" />

