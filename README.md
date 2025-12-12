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
















