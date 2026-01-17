<p align="center"><img width="448" height="168" alt="image" src="https://github.com/user-attachments/assets/67a313d2-8c83-4886-a2fe-c5ce2f13a00a" /></p>

# osTicket IT Support Lab: End-User Troubleshooting & Escalation
*Completed: January 17, 2026*

# Project Overview
This project demonstrates hands-on **IT helpdesk** and **technical support skills** using **osTicket**, an open-source ticketing and support system widely used in enterprise IT environments. I walk through how to setup your environment with **Virtualbox virtual machines**, installing **XAMPP** for local web server hosting, deploying **osTicket** for technical support, and simulating real helpdesk scenarios such as ticket intake, troubleshooting, and escalation.

## Technologies & Tools Used
| Technology/Tool | Purpose | Link |
|------------------|-----------------|-----------------|
| osTicket | Open-source IT helpdesk and ticketing system to simulate support workflows | [https://osticket.com/](https://osticket.com/) |
| XAMPP | Local web server environment supporting Apache, MySQL, PHP, and more for hosting osTicket | [https://www.apachefriends.org/index.html](https://www.apachefriends.org/index.html) |
| MySQL/MariaDB | Database backend for storing tickets, users, and configurations | [https://www.mysql.com/](https://www.mysql.com/) |
| phpMyAdmin | Web-based MySQL database management and administration | [https://www.phpmyadmin.net/](https://www.phpmyadmin.net/) |
| Virtual Machines | Optional Windows 10 virtualization for sandboxed environments| [https://www.virtualbox.org/](https://www.virtualbox.org/) |

- - - 
# 🔢 Step-by-Step Walkthrough 🔢
## 1️⃣ Virtual Machine & XAMPP Installation
1. Install [Virtualbox](https://www.virtualbox.org/) and create a Windows 10 or 11 virtual machine (VM). Alternatively, you can utilize a cloud-based VM service such as [Vultr](https://www.vultr.com/).
   > NOTE: This step is optional; its just good practice to use a Virtual Machine in testing environments.

2. Inside the VM, go to the official [XAMPP website](https://www.apachefriends.org/index.html) and click **Download for Windows**.
   - Choose a **PHP 8.x version** (recommended for newer osTicket versions)
   - During the installation process, make sure **Apache**, **MySQL**, **PHP**, and **phpMyAdmin** are **checked**. Everything else can remain unchecked.
  
3. Start **Apache and **MySQL** within the XAMPP Control Panel app. If successful, Apache and MySQL should turn green.

4. Click **Start** next to MySQL. The status change dialog should list "**Running**".

<p align="center"> <img width="663" height="432" alt="Screenshot 2026-01-17 112859" src="https://github.com/user-attachments/assets/331bd5f2-c0f1-4f84-9336-b11d1bf99148"/></p>

5. Verify MySQL and phpMyAdmin by visiting ```http://localhost/phpmyadmin``` on a web browser.

<p align="center"> <img width="1834" height="884" alt="Screenshot 2026-01-17 112959" src="https://github.com/user-attachments/assets/08e2eb27-6b59-4e42-a75b-c0be159ee12d"/></p>

6. From the admin dashboard, navigate to the **Databases** tab and create an osticket database.

<p align="center"><img width="772" height="552" alt="Screenshot 2026-01-17 113839" src="https://github.com/user-attachments/assets/f637c49e-a444-4e54-9039-153a2d41dcae" /></p>

7. Then, navigate to the **Users** tab and create a user named "**ostuser**" with all privileges granted to it. This will serve as the admin account for the MySQL database.

- - - 

## 2️⃣ osTicket Installation
1. Go to the official [osTicket website](https://osticket.com/) and click download on the latest stable version. Extract the downloaded ```.zip``` file.
2. From the extracted ```.zip``` file, rename the folder titled "**upload**" to "**osticket**" and copy and paste it into the ```C:\xampp\htdocs``` directory.
   - If you changed default download directory when installing osTicket, navigate to that directory instead.
   
<p align="center"><img width="1055" height="515" alt="Screenshot 2026-01-17 113606" src="https://github.com/user-attachments/assets/dc64805f-eaf9-4b88-876b-05dcd34d332e"/></p>

3. Navigate to the ```C:\xampp\htdocs\osticket\include``` directory and rename the file titled "**ost-sampleconfig.php**" to "**ost-config.php**".

4. Then, set the **User file permissions** within the ```ost-config.php``` file's properties settings to **Full Control**. Make sure you apply your configuration.

<p align="center"><img width="1086" height="802" alt="Screenshot 2026-01-17 113710" src="https://github.com/user-attachments/assets/bf9f529f-7700-461a-91ca-e8ce4be1a608" /></p>

5. Open your browser and go to ```http://localhost/osticket/setup``` to set up basic installation settings for osTicket. Make sure you remember your admin account's login credentials.
    - Since this is a simulation of a helpdesk environment, you can use a fake email for the default email settings.

<p align="center"><img width="811" height="1033" alt="Screenshot 2026-01-17 153144" src="https://github.com/user-attachments/assets/08fd6c67-bc33-466e-a848-85fcd2db7286" /></p>

6. Your osTicket installation is complete! Now that you have finished the setup, it is recommended to **disable** the **Full Control** you granted **Users** on the ```ost-config.php``` file. You should also delete the setup folder located in ```C:\xampp\htdocs\osticket```.

- - - 

## 3️⃣ osTicket Configuration
Before we can begin ingesting ticket requests, we must configure the default settings of osTicket. 
We'll need to create agents & roles, departments, teams, ticket priorities & SLAs, and lastly a user portal for access. 
This will mirror a real Tier 1 / Tier 2 desk environment.

1. Sign into the admin account you created during installation at ```http://localhost/osticket/scp```.

osTicket has two main modes for configuring and navigating its settings: the **Admin Panel** and the **Agent Panel**. You alternate between these modes by clicking their respective option in the top-right of webpage.
   - The **Admin panel** is designed for the system administrators who manage the overall configuration of the helpdesk system.
   - The **Agent panel** is reserved for the actual support staff and agents who handle customer tickets.

2. Under the **Admin panel**, click on the Agent tabs and create an **IT Support**, **Networking**, and **Hardware department**. For each department, assign the admin as manager and leave everything else default.

<p align="center"><img width="960" height="513" alt="Screenshot 2026-01-17 120048" src="https://github.com/user-attachments/assets/b1196e0b-57fc-452d-b85c-dfae09247016"/></p>

3. Now that we have our departments, we'll need agents to utilize them. Let's create two agents, one for **Tier 1 Helpdesk**, and another for **Tier 2 Support**.
   - The distinctive agent tiers will help us simulate ticket escalation when conducting ticket scenarios later.

4. Add the **Tier 1 Helpdesk** to the **IT Support department** and set its role to **Limited Access**. For the **Tier 2 Support**, add it to the **Networking department** and assign **Expanded Access** privileges.

<p align="center"><img width="958" height="395" alt="Screenshot 2026-01-17 120620" src="https://github.com/user-attachments/assets/b11e906b-b0f3-4784-be68-3ed02325307f"/></p>

5. Click on the `Roles` tab and create a Tier 1 Support role with the ability to view, reply, and add internal notes to tickets. Create another role for Tier 2 Support that has full ticket access.

<p align="center"><img width="953" height="470" alt="Screenshot 2026-01-17 121134" src="https://github.com/user-attachments/assets/ab1540a3-2ac3-41a7-b483-698d4233a723"/></p>

6. Click on the `Teams` tab and create an Escalation team that is assigned only to the Tier 2 Support agent. If a Tier 1 agent is unable to resolve a ticket, they will send the ticket to the escalation team composed of Tier 2 Support agents.

<p align="center"><img width="956" height="261" alt="Screenshot 2026-01-17 121252" src="https://github.com/user-attachments/assets/5b1bdd3f-caec-4d5b-9376-99b375452a2b"/></p>

7. Let's configure the priorities of tickets themselves. We need to categorize them according to their severity. Create **Low**, **Normal**, **High**, and **Emergency** ticket priorities under the ```Manage``` tab.

8. On the same ```Manage``` tab, SLA plans of increasing severity should also be created. **Service Level Agreements (SLAs)** indicate the timeframe that a ticket must be resolved. Create three:
   1. **Sev-A (Critical)**: 1 Hour Grace Period, scheduled 24/7
   2. **Sev-B (Standard)**: 4 Hour Grace Period, scheduled for Business hours
   3. **Sev-C (Low)**: 24 Hour Grace Period, scheduled for Business hours
  
9. Lastly, let's configure the User Portal itself. Under the settings tab in the Admin Panel, set all users to require registration when they want to submit tickets.

**Your osTicket setup is finally complete! Now we can simulate realistic helpdesk ticket scenarios as practice.**

- - - 
# Ticket Scenarios & Resolutions
## Scenario #1: Password Reset
Let's start with a simple ticket scenario. A user, John Smith, submits a ticket using our osTicket portal and states that he has been locked out of his company email.

<p align="center"><img width="833" height="864" alt="Screenshot 2026-01-17 122652" src="https://github.com/user-attachments/assets/ccf5790f-51a3-4ece-a84a-8eb10d94a8b3"/></p>
<p align="center"><img width="822" height="351" alt="Screenshot 2026-01-17 122710" src="https://github.com/user-attachments/assets/e5c72d7a-1f69-4fab-9c37-38e9e988601a"/></p>

On the **Agent panel** of osTicket, we see the ticket appear with an assigned priority level of **High**. As the Admin user, I assign this ticket to the Tier 1 Helpdesk agent.

<p align="center"><img width="963" height="430" alt="Screenshot 2026-01-17 122741" src="https://github.com/user-attachments/assets/305d87a7-c6f8-45d8-b4ea-4168f0e486ae" /></p>
<p align="center"><img width="981" height="859" alt="Screenshot 2026-01-17 123048" src="https://github.com/user-attachments/assets/c12331de-117a-4281-bb70-51643478e89f" /></p>

The Tier 1 Helpdesk agent quickly resolves the case, informing the user that they should reset their password when prompted on the login page. The agent finalizes the ticket by adding internal notes and setting the status to **Resolved**.

- - - 
## Scenario #2: Wi-Fi Not Connecting (Tier 1 -> Tier 2 Escalation)
In this scenario, the Tier 1 Helpdesk agent will need to escalate a ticket to the escalation team. The escalation team, composed of Tier 2 Support, will resolve the issue and close the case.

<p align="center"><img width="961" height="166" alt="Screenshot 2026-01-17 123319" src="https://github.com/user-attachments/assets/6001de33-21fb-4320-aefb-f0e31f14c32e" /></p>

Jane Doe from HR submits a ticket stating she cannot reach the internet. A Tier 1 Agent attempts to assist her, but to **no avail**. The agent then **escalates** the case to the **escalation team** for further assistance.

<p align="center"><img width="955" height="895" alt="Screenshot 2026-01-17 124827" src="https://github.com/user-attachments/assets/5453afc6-8902-4d77-ab7c-7d878ad70f9e" /></p>

The Tier 2 Support responds to the escalated ticket and fixes Jane Doe's internet problem. Her DNS was misconfigured, preventing her from accessing to the internet. The Tier 2 Support resolves this and closes the case.

- - - 
## Scenario #3: Blue Screen Error (Critical / SLA)
The last scenario involves a ticket with a critical SLA status: repeated computer blue screening. This means the client cannot access their computer, disrupting business continuity.

<p align="center"><img width="1066" height="214" alt="Screenshot 2026-01-17 164740" src="https://github.com/user-attachments/assets/6b2b7fc2-5c6a-40b6-96c3-7c3ed2e95b66" /></p>
<p align="center"><img width="335" height="273" alt="Screenshot 2026-01-17 125143" src="https://github.com/user-attachments/assets/084072ab-5865-40c0-9203-d78707f245e7" /></p>

The ticket is assigned to the Senior Tier 2 Support, as opposed to the Tier 1 Helpdesk, and has a due date within an hour because of the Sev-A status (critical). The Senior support team quickly discovers a solution and resolves the case.

<img width="970" height="732" alt="Screenshot 2026-01-17 125216" src="https://github.com/user-attachments/assets/cd1fe69b-c482-44d3-bec7-5a87110ab882" />

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
This osTicket IT support lab successfully replicates a real-world helpdesk environment by demonstrating a full support ticket lifecycle. Through the configuration process, I developed practical skills such as web server setup, database configuration, file permission management, and basic security hardening. The creation of agents, roles, departments, and teams strengthened my understanding of role-based access control, especially in enforcing least privilege, as well as general security administrative concepts. By utilizing ticket priorities and SLAs, the efficiency of ticket escalation, assessing issue severity, and managing response deadlines greatly increased. This project has made me significantly more comfortable and proficient in IT service management and general structured problem resolution.
