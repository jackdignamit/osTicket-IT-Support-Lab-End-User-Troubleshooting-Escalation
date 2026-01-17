# (WIP) osTicket IT Support Lab: End-User Troubleshooting & Escalation
*Completed: January 17, 2026*

# Project Overview

This project demonstrates hands-on **IT helpdesk** and **service desk skills** using **osTicket**, an open-source ticketing and support system widely used in enterprise IT environments. I walkthrough how to setup your environment with **Virtualbox virtual machines**, installing **XAMPP** for local web server hosting, deploying **osTicket** for technical support, and simulating real helpdesk scenarios such as ticket intake, troubleshooting, and escalation.

## Technologies & Tools Used
| Technology/Tool | Purpose | Link |
|------------------|-----------------|-----------------|
| osTicket | Open-source IT Helpdesk and ticketing system to simulate support workflows | [https://osticket.com/](https://osticket.com/) |
| XAMPP | Local web server environment supporting Apache, MySQL, PHP, and more for hosting osTicket | [https://www.apachefriends.org/index.html](https://www.apachefriends.org/index.html) |
| MySQL/MariaDB | Database backend for storing tickets, users, and configurations | [https://www.mysql.com/](https://www.mysql.com/) |
| phpMyAdmin | Web-based MySQL database management and administration | [https://www.phpmyadmin.net/](https://www.phpmyadmin.net/) |
| Virtual Machines | Optional Windows 10 virtualization for sandboxed environments| [https://www.virtualbox.org/](https://www.virtualbox.org/) |

- - - 
# 🔢 Step-by-Step Walkthrough 🔢
## 1️⃣ Virtual Machine & XAMPP Installation
1. Install [Virtualbox](https://www.virtualbox.org/) and create a Windows 10 or 11 virtual machine (VM). Alternatively, you can utilize a cloud-based VM service such as [Vultr](https://www.vultr.com/).
   > NOTE: This step is optional, its just good practice to use a Virtual Machine in testing environments.

2. Inside the VM, go to the official [XAMPP website](https://www.apachefriends.org/index.html) and click **Download for Windows**.
   - Choose a **PHP 8.x version** (recommended for newer osTicket versions)
   - During the installation process, make sure **Apache**, **MySQL**, **PHP**, and **phpMyAdmin** are **checked**. Everything else can remained unchecked.
  
3. Start **Apache and **MySQL** within the XAMPP Control Panel app. If successful, Apache and MySQL should turn green.

4. Click start next to MySQL. The status change dialogue should list "**running**".

<p align="center"> <img width="663" height="432" alt="Screenshot 2026-01-17 112859" src="https://github.com/user-attachments/assets/331bd5f2-c0f1-4f84-9336-b11d1bf99148"/></p>

5. Verify MySQL and phpMyAdmin by visiting ```http://localhost/phpmyadmin``` on a web browser.

<p align="center"> <img width="1834" height="884" alt="Screenshot 2026-01-17 112959" src="https://github.com/user-attachments/assets/08e2eb27-6b59-4e42-a75b-c0be159ee12d"/></p>

6. From the admin dashboard, navigate to the **Databases** tab and create an osticket database.

<p align="center"><img width="772" height="552" alt="Screenshot 2026-01-17 113839" src="https://github.com/user-attachments/assets/f637c49e-a444-4e54-9039-153a2d41dcae" /></p>

7. Then, navigate to the **Users** tab and create a user named "**ostuser**" with all privelages granted to it. This will serve as the admin account for the MySQL database.

- - - 

## 2️⃣ osTicket Installation
1. Go to the official [osTicket website](https://osticket.com/) and click download on the latest stable version. Extract the downloaded ```.zip``` file.
2. From the extracted ```.zip``` file, rename the folder titled "**upload**" to "**osticket**" and copy and paste it into the ```C:\xampp\htdocs``` directory.
   - If you changed default download directory when installing osTicket, navigate to that directory instead.
   
<p align="center"><img width="1055" height="515" alt="Screenshot 2026-01-17 113606" src="https://github.com/user-attachments/assets/dc64805f-eaf9-4b88-876b-05dcd34d332e"/></p>

3. Navigate to the ```C:\xampp\htdocs\osticket\include``` directory and rename the file titled "**ost-sampleconfig.php**" to "**ost-config.php**".

4. Then, set the **User file permissions** within the ```ost-config.php``` file's properties settings to **Full Control**. Make sure you apply your configuration.

<p align="center"><img width="1086" height="802" alt="Screenshot 2026-01-17 113710" src="https://github.com/user-attachments/assets/bf9f529f-7700-461a-91ca-e8ce4be1a608" /></p>

5. Open your browser and go to ```http://localhost/osticket/setup``` to setup basic installation settings for osTicket. Make sure you remember your admin account's login credentials.
    - Since this is a simulation of a helpdesk environment, you can use a fake email for the default email settings.

<p align="center"><img width="811" height="1033" alt="Screenshot 2026-01-17 153144" src="https://github.com/user-attachments/assets/08fd6c67-bc33-466e-a848-85fcd2db7286" /></p>

6. Your osTicket installation is complete! Now that you have finished the setup, it is recommended to **disable** the **Full Control** you permitted **Users** on the ```ost-config.php``` file. You should also delete the setup folder located in ```C:\xampp\htdocs\osticket```.

- - - 

## 3️⃣ osTicket Configuration
Before we can begin ingesting ticket requests, we must configure the default settings of osTicket. 
We'll need to create agents & roles, departments, teams, ticket priorities & SLAs, and lastly a user portal for access. 
This will mirror a real Tier 1 / Tier 2 desk environment.

1. Sign into the admin account you created during installation at ```http://localhost/osticket/scp```.

osTickets has two main modes for configuring and navigating its settings: the **Admin Panel** and the **Agent Panel**. You alternate between these modes by clicking their respective option in the top right of webpage.
   - The **Admin panel** is designed for the system administrators who manage the overall configuration of the helpdesk system.
   - The **Agent panel** is reserved for the actual support staff and agents who handle customer tickets.

2. Under the **Admin panel**, click on the agent tabs and create an **IT Support**, **Networking**, and **Hardware department**. For each department, assign the admin as manager and leave everything else default.

<p align="center"><img width="960" height="513" alt="Screenshot 2026-01-17 120048" src="https://github.com/user-attachments/assets/b1196e0b-57fc-452d-b85c-dfae09247016"/></p>

3. Now that we have our departments, we'll need agents to utilize them. Let's create two agents, one for **Tier 1 Helpdesk**, and another for **Tier 2 Support**.
   - The distinctive agent tiers will help us simulate ticket escalation when conducting ticket scenarios later.

4. Add the **Tier 1 Helpdesk** to the **IT Support department** and set its role to **Limited Access**. For the **Tier 2 Support**, add it to the **Networking department** and assign **Expanded Access** privelages.

<p align="center"><img width="958" height="395" alt="Screenshot 2026-01-17 120620" src="https://github.com/user-attachments/assets/b11e906b-b0f3-4784-be68-3ed02325307f"/></p>

5. Click on the `Roles` tab and create a Tier 1 Support role with the ability to view, reply, and add internal notes to tickets. Create another role for Tier 2 Support that has full ticket access.

<p align="center"><img width="953" height="470" alt="Screenshot 2026-01-17 121134" src="https://github.com/user-attachments/assets/ab1540a3-2ac3-41a7-b483-698d4233a723"/></p>

6. Click on the `Teams` tab and create an Escalation team that is assigned only to the Tier 2 Support agent. Whenver a Tier 1 agent cannot resolve a ticket, they will escalate the ticket to the escalation team composed of Tier 2 Support agents.

<p align="center"><img width="956" height="261" alt="Screenshot 2026-01-17 121252" src="https://github.com/user-attachments/assets/5b1bdd3f-caec-4d5b-9376-99b375452a2b"/></p>

- - - 
# Ticket Scenarios & Resolutions

- - - 
# Future Enhancements
- Active Directory
- Email-to-ticket functionality
- Reporting and analytics dashboard
- Knowledge base articles

- - - 
# Key Skills Demonstrated
- IT Helpdesk ticket lifecycle management
- Tier 1 and Tier 2 technical troubleshooting
- Ticket escalation and reassignment
- SLA, priority, and service request management
- User support and professional communication
- System administration basics
- IT documentation and reporting

- - - 
# Conclusion
