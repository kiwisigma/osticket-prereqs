<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1 align="center">osTicket — Prerequisites & Installation</h1>

<p align="center">
  A full walkthrough for deploying the osTicket open-source helpdesk system on a Windows VM hosted in Microsoft Azure.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Microsoft%20Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white"/>
  <img src="https://img.shields.io/badge/OS-Windows%2010-0078D6?style=for-the-badge&logo=windows&logoColor=white"/>
  <img src="https://img.shields.io/badge/Web%20Server-IIS-5C2D91?style=for-the-badge&logo=microsoft&logoColor=white"/>
  <img src="https://img.shields.io/badge/Database-MySQL%205.5-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
</p>

---

## 🧰 Skills Demonstrated

- Provisioning and configuring a **cloud VM** in Microsoft Azure
- Installing and configuring **IIS** as a Windows web server
- Setting up **PHP + CGI** to execute server-side code on IIS
- Installing and configuring **MySQL** as a database backend
- Managing **Windows file permissions** and inheritance
- Deploying a production-grade **PHP web application** end-to-end
- Troubleshooting connectivity with `netstat` and Windows Services

---

## 🏗️ Architecture Overview

```
[Azure VM - Windows 10]
        │
        ▼
   IIS Web Server (port 80)
        │
        ├── PHP 7.3 via CGI
        │        │
        │        └── osTicket Application (C:\inetpub\wwwroot\osTicket)
        │
        └── MySQL 5.5 Database (port 3306)
                 │
                 └── osTicket Database (managed via HeidiSQL)
```

---

## 📋 Prerequisites

| Component | Purpose |
|-----------|---------|
| Microsoft Azure Free Account | Host the virtual machine |
| PHP Manager for IIS | Register and manage PHP within IIS |
| Microsoft Visual C++ Redistributable | Required runtime DLLs for PHP |
| PHP 7.3.8 (NTS x86) | Server-side scripting engine for osTicket |
| MySQL 5.5.62 | Database backend |
| osTicket v1.15.x | The helpdesk application |
| HeidiSQL | GUI client for managing the MySQL database |
| URL Rewrite Module (IIS) | Converts query-string URLs to clean URLs |

---

## 🚀 Installation Walkthrough

### Step 1 — Create a Virtual Machine in Azure

osTicket needs a Windows host. We'll use a **Microsoft Azure free-tier VM** — more than sufficient for this lab.

1. Log in to the [Azure Portal](https://portal.azure.com)
2. Search for **Virtual Machines** in the top search bar and click it

<img width="1353" height="935" alt="image" src="https://github.com/user-attachments/assets/ed107ad8-b007-4f50-9b91-6acb39ce5bec" />

3. Click **+ Create**

<img width="2290" height="680" alt="image" src="https://github.com/user-attachments/assets/adc3acbc-fb2f-49b8-b793-ad57fc11ff83" />

4. Select a **Windows 10** image and choose a VM size with at least **2 vCPUs**
5. Set a username and password for the VM
6. Allow inbound **RDP traffic on port 3389** so we can remote in

<img width="781" height="1304" alt="image" src="https://github.com/user-attachments/assets/84bf7dab-5aa0-46be-b855-e42861e5287e" />

7. Proceed to **Review + Create** and check the license agreement, then deploy
8. Once deployed, copy the **public IP address** and connect via RDP

<img width="2537" height="713" alt="image" src="https://github.com/user-attachments/assets/2ff8ef7b-4f0d-4abf-91d8-1c7c636a7090" />

<img width="399" height="247" alt="image" src="https://github.com/user-attachments/assets/9ea7e867-2e08-48ce-86c7-1780775faab2" />

9. Log in with the credentials you created

<img width="451" height="439" alt="image" src="https://github.com/user-attachments/assets/9ea7e867-2e08-48ce-86c7-1780775faab2" />

---

### Step 2 — Download Installation Dependencies

Inside the VM, open Microsoft Edge and download the pre-collected dependency bundle:

```
https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0
```

<img width="1234" height="288" alt="image" src="https://github.com/user-attachments/assets/8f27739d-8133-42d1-9825-1b135b2d8bd8" />

> You can also download components individually from [osticket.com/download](https://osticket.com/download/). The Drive folder simply has everything in one place.

Extract all contents into a single uncompressed folder on the Desktop.

<img width="611" height="452" alt="image" src="https://github.com/user-attachments/assets/08c4df5e-46fc-4f67-a962-3ad26b81c023" />

---

### Step 3 — Install and Enable IIS with CGI

**Why IIS + CGI is required:**

osTicket is a PHP web application. Without IIS configured with CGI support, any request for a `.php` file could expose raw PHP source code to the browser — including database credentials and passwords. IIS acts as the gateway, handing requests to the PHP engine safely.

1. Open **Control Panel → Programs → Uninstall a program**

<img width="1099" height="471" alt="image" src="https://github.com/user-attachments/assets/03d1c133-e16a-4c42-b053-ec7f644a701d" />

2. Click **Turn Windows features on or off** in the left panel

<img width="815" height="355" alt="image" src="https://github.com/user-attachments/assets/f5ce5d51-e578-4532-bb6c-4b36f7c45817" />

3. Enable **Internet Information Services**

<img width="417" height="369" alt="image" src="https://github.com/user-attachments/assets/3ec7649e-3622-4033-8a0c-2216ff20bb48" />

4. Expand IIS and enable CGI:
   `Internet Information Services → World Wide Web Services → Application Development Features → CGI`

<img width="411" height="368" alt="image" src="https://github.com/user-attachments/assets/fb26f461-85e6-4a9c-bdbe-cf582b6beeef" />

---

### Step 4 — Verify IIS is Running

Enter `127.0.0.1` in the browser. If you see the IIS default page, the web server is live.

<img width="1239" height="759" alt="image" src="https://github.com/user-attachments/assets/219207f7-f774-405b-8351-4c92d32dcd93" />

---

### Step 5 — Install PHP Manager and URL Rewrite Module

1. From the installation files, run **PHPManagerForIIS_V1.5.0.msi** and accept all prompts

<img width="971" height="679" alt="image" src="https://github.com/user-attachments/assets/8e731cd6-2e05-4aaf-a3eb-f41e9462e5b5" />

2. Install **rewrite_amd64_en-US.msi**

<img width="912" height="663" alt="image" src="https://github.com/user-attachments/assets/3fe7cc5e-35c5-4583-9c35-f35058e3c50f" />

> The URL Rewrite module converts messy query-string URLs like `/tickets.php?id=123` into clean ones like `/tickets/123`.

---

### Step 6 — Set Up the PHP Directory

1. Open File Explorer → **This PC → Windows (C:)**
2. Create a new folder named `PHP`

<img width="1086" height="523" alt="image" src="https://github.com/user-attachments/assets/9c3e1582-10e6-4c5a-bb07-ed54d8969895" />

3. Extract **php-7.3.8-nts-Win32-VC15-x86.zip** into `C:\PHP`

<img width="2243" height="622" alt="image" src="https://github.com/user-attachments/assets/271976a8-017b-419b-a394-364c76b50b85" />

---

### Step 7 — Install Visual C++ Redistributable

Run **VC_redist.x86.exe** and accept the license. These are essential Microsoft runtime DLLs that PHP depends on.

<img width="482" height="301" alt="image" src="https://github.com/user-attachments/assets/49a73ce4-2de5-4ef8-bbe1-4885348c4265" />

---

### Step 8 — Install MySQL 5.5

MySQL is the database where osTicket stores all tickets, users, and configuration.

1. Run **mysql-5.5.62-win32.msi** and accept the license agreement
2. Choose **Typical** installation

<img width="605" height="473" alt="image" src="https://github.com/user-attachments/assets/2aa35ac1-bc73-4518-9d7a-c3f1b431c68f" />

3. In the configuration wizard, choose **Standard Configuration**

<img width="608" height="466" alt="image" src="https://github.com/user-attachments/assets/b77c4a0a-1d82-4d78-968f-a575f56b5847" />

4. Set a strong root password — **do not lose this**

<img width="607" height="461" alt="image" src="https://github.com/user-attachments/assets/393b3aa8-bb2a-4bb8-83a1-ba1c767b6e44" />

---

### Step 9 — Register PHP with IIS

1. Open **IIS Manager** as Administrator

<img width="824" height="679" alt="image" src="https://github.com/user-attachments/assets/66306cc5-dcf1-46df-9182-78c47175e1ea" />

2. Open **PHP Manager**

<img width="1495" height="408" alt="image" src="https://github.com/user-attachments/assets/44382d5b-0d0c-4000-b408-e8f45c64a962" />

3. Click **Register new PHP version** and point it to `C:\PHP\php-cgi.exe`

<img width="499" height="218" alt="image" src="https://github.com/user-attachments/assets/1a4cf69c-8ea9-42b8-9069-067a58933aba" />

4. Stop and restart IIS to apply the changes

<img width="1905" height="772" alt="image" src="https://github.com/user-attachments/assets/31518e15-2698-4cec-8fd9-edc55dbb07eb" />

---

### Step 10 — Deploy osTicket

1. Extract the osTicket zip file — it will contain an `upload` folder and a `scripts` folder
2. Copy the `upload` folder to `C:\inetpub\wwwroot\`

<img width="745" height="591" alt="image" src="https://github.com/user-attachments/assets/4d1e9569-8cf9-493b-b232-04c1e0892bb5" />

3. Rename the `upload` folder to `osTicket`

<img width="832" height="343" alt="image" src="https://github.com/user-attachments/assets/739452a8-7c27-40b3-b582-5b437fbfdac3" />

4. Restart IIS, then navigate in IIS Manager to:
   **Sites → Default Web Site → osTicket → Browse \*:80 (http)**

<img width="1909" height="684" alt="image" src="https://github.com/user-attachments/assets/d6bbd3c9-ccee-4337-8eb5-2e7614c75359" />

The osTicket installer page should now load in the browser.

<img width="1575" height="1034" alt="image" src="https://github.com/user-attachments/assets/51d0c442-186e-4210-972a-002ca176daa1" />

---

### Step 11 — Enable Required PHP Extensions

Some osTicket features require PHP extensions that are disabled by default.

1. In IIS Manager, navigate to **osTicket → PHP Manager → Enable or disable an extension**

<img width="1017" height="840" alt="image" src="https://github.com/user-attachments/assets/9d2e66bb-3f84-4d8e-9799-8ed518dd7310" />

2. Enable the following:
   - `php_imap.dll` — email integration
   - `php_intl.dll` — internationalization support
   - `php_opcache.dll` — performance caching

<img width="1590" height="1014" alt="image" src="https://github.com/user-attachments/assets/92be2fb8-2d09-4b70-85b9-d9317d19059b" />

3. Refresh the osTicket installer page — the previously red items should now show green

<img width="828" height="780" alt="image" src="https://github.com/user-attachments/assets/9505747f-3260-4b0a-99e3-d79e430128c5" />

---

### Step 12 — Configure ost-config.php

osTicket requires a writable config file. The sample config needs to be renamed and its permissions set correctly.

1. Navigate to `C:\inetpub\wwwroot\osTicket\include\`
2. Rename `ost-sampleconfig.php` → `ost-config.php`

<img width="1120" height="654" alt="image" src="https://github.com/user-attachments/assets/e264ab76-3680-4b92-873c-b4bad96fbe3d" />

3. Right-click `ost-config.php` → **Properties → Security → Advanced**
4. Click **Disable inheritance** → **Remove all inherited permissions**

<img width="782" height="507" alt="image" src="https://github.com/user-attachments/assets/b0f04802-b171-4116-932d-3940ab9875cf" />

5. Add a new principal — select **Everyone** and grant **Full Control**

> ⚠️ This is appropriate for a lab environment only. In production, permissions should be scoped to the IIS application pool identity.

<img width="917" height="585" alt="image" src="https://github.com/user-attachments/assets/bdeeb51e-63e0-4aac-adfa-402153862c83" />

---

### Step 13 — Complete the osTicket Web Installer

1. Back in the browser, click **Continue** on the osTicket installer
2. Fill in your helpdesk name, admin email, and admin credentials

<img width="1583" height="897" alt="image" src="https://github.com/user-attachments/assets/b725a0cd-f2a8-4fe7-a72a-efd3fa91765d" />

---

### Step 14 — Create the Database with HeidiSQL

1. From the installation files, run **HeidiSQL_12.3.0.6589_Setup** and install it
2. Open HeidiSQL and click **New** to create a connection

<img width="687" height="485" alt="image" src="https://github.com/user-attachments/assets/7c27c0b5-67a2-4431-942b-de9e5491a9cd" />

3. Enter the MySQL root credentials from Step 8 and click **Open**

> **Troubleshooting:** If HeidiSQL can't connect, verify MySQL is running:
> ```cmd
> netstat -ano | findstr :3306
> sc query type= service state= all | findstr /I "mysql"
> ```

<img width="887" height="462" alt="image" src="https://github.com/user-attachments/assets/e68710dc-b640-40ee-b4a0-37b698d821e5" />

4. Right-click the connection → **Create New → Database** → name it `osTicket`

<img width="320" height="267" alt="image" src="https://github.com/user-attachments/assets/4904aae1-a224-438e-815a-3a2611ab54c4" />

---

### Step 15 — Finish Installation

1. Back in the osTicket web installer, enter the MySQL database name (`osTicket`) and root credentials
2. Click **Install Now**

<img width="625" height="514" alt="image" src="https://github.com/user-attachments/assets/01e50e9f-7b24-46cb-aa62-bbe186fae23a" />

✅ **Installation complete.** osTicket will display links to your staff control panel and end-user portal.

<img width="622" height="517" alt="image" src="https://github.com/user-attachments/assets/70ec8ece-01f0-4732-8a92-f14586c7def8" />

---

## 🔗 Related Projects

- **[osticket-prereqs](https://github.com/kiwisigma/osticket-prereqs)** — this repo (prerequisites & installation)
- Post-installation configuration and ticketing lifecycle walkthrough *(coming soon)*
