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

## Installation Steps

### Step 1: Create a Virtual Machine in Microsoft Azure

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Azure VM Creation"/>
</p>

-to run osTIcket we are going to need a host system, 
I have decided to use Microsoft Azure as my cloud hosting service for a client to run osTicket on.
to get setup create a Microsoft Azure account and select the free subscription provided. Then, follow these steps..

1. Log in to the [Microsoft Azure Portal](https://portal.azure.com).
2. Click **Create a resource** → **Virtual machine**.
3. Configure the following basic settings:
   - **Subscription**: Free Trial (or your existing subscription)
   - **Resource Group**: Create new (e.g., `osTicket-Lab-RG`)
   - **Virtual machine name**: `VM-osTicket`
   - **Region**: Choose the closest to you (e.g., East US)
   - **Image**: **Windows 10 Pro, Version 21H2** (or latest available)
   - **Size**: **Standard_B1s** or **B1ms** (eligible for free tier)
   - **Administrator account**: Set a username and strong password
4. Under **Networking**, ensure port **3389 (RDP)** and **80 (HTTP)** are allowed in the NSG.
5. Review + Create → **Create**.
  
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
