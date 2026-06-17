# Enterprise Help Desk Lab with Active Directory

## Objective

The goal of this project was to build and manage a simulated enterprise IT support environment integrated with Active Directory services.

This lab was designed to replicate real-world Help Desk and Systems Administration tasks by deploying a ticketing system (UVDesk) within a Windows Server domain environment and resolving common IT support incidents.

The environment was built to strengthen practical IT support, system administration, and troubleshooting skills commonly used in enterprise environments.

This was using a previously built infrastrucutre in other labs:

- <b>Linux Virtualization Host Setup (QEMU-KVM)</b>
  - [Linux Virtualization Host Setup](https://github.com/AllFullPower/Linux-Virtualization-Host-Setup-QEMU-KVM-)

- <b>Enterprise Lab: Windows Server & AD DS Deployment</b>
  - [Enterprise Lab: Windows Server & AD DS Deployment](https://github.com/AllFullPower/Enterprise-Lab-Windows-Server-AD-DS-Deployment)


## Skills Learned

- Active Directory user and group management
- Password reset and account unlock procedures
- DNS configuration and troubleshooting
- DHCP scope management and lease troubleshooting
- Linux server administration (Ubuntu Server)
- Docker and Docker Compose deployment
- Apache web server configuration
- Help Desk ticket management workflows
- Network troubleshooting and service validation
- User permission and access control management
- Enterprise IT documentation practices
- Virtualized lab environment deployment

## Tools Used

- **Infrastructure & Virtualization**: Oracle VirtualBox / VMware, Ubuntu Server, Windows Server
- **Directory Services & Networking**: Active Directory Domain Services (AD DS), DNS, DHCP
- **Ticketing System & Web Services**: UVDesk, Apache2, Docker, Docker Compose
- **Administration & Troubleshooting**: PowerShell, Windows Administrative Tools, Linux Terminal, Command Prompt, Event Viewer


# Steps Taken
This section documents how I deployed UVDesk and integrated it into my Active Directory lab environment.

## Step 1 - Created an Ubuntu Server VM to host the Ticketing Software

Created an Ubuntu Server virtual machine to host the ticketing platform within the internal network:
- Configured the server to obtain network settings through **DHCP** and use the Windows Server as its gateway.
- Connected through **SSH** to perform server administration and deploy the application remotely.


<img width="910" height="560" alt="Pasted image 20260505211815" src="https://github.com/user-attachments/assets/59760f93-85e3-4683-a4dc-e521d3a85c10" />
<br/>
<br/>

Installed UVDesk using **Docker Compose** and verified all required containers started successfully:

<img width="951" height="576" alt="Pasted image 20260505221848" src="https://github.com/user-attachments/assets/4a51721a-76a8-477f-bd32-f94d9cb4aa93" />
<br/>
<br/>

Published the service through Apache, allowing internal users to access the ticketing platform:

<img width="955" height="678" alt="Pasted image 20260505223431" src="https://github.com/user-attachments/assets/13359ddf-cd7c-4b5d-9163-42d2a4f7b095" />
<br/>
<br/>

Assigned a static IP address before creating DNS records for the application server:
- Created an A record that mapped the server IP to a user-friendly hostname

> Accessing the Web Application Using DNS:
<img width="938" height="453" alt="Pasted image 20260506194026" src="https://github.com/user-attachments/assets/92e7fe2c-573d-4e59-a5b5-a9929088163d" />
<br/>
<br/>

> Configuration made on the server through SSH:
<img width="893" height="425" alt="Pasted image 20260506200440" src="https://github.com/user-attachments/assets/c48ea036-e998-4db6-a10a-6bf71c9c0bd4" />
<br/>
<br/>

> SSH Access Using the DNS Hostname:
<img width="731" height="49" alt="Pasted image 20260506200444" src="https://github.com/user-attachments/assets/ee233699-a981-4357-9fe3-bcf8ce5179ff" />
<br/>
<br/>

# Resolved Tickets
The following tickets simulate common incidents handled by a Help Desk Level 1 technician.

## Ticket 01: (DHCP Scope Exaustion)

**Situation:** A user reported no internet connectivity while other employees remained connected and working normally.

<img width="882" height="422" alt="Pasted image 20260506221328" src="https://github.com/user-attachments/assets/9b89badb-2e12-41d1-aa06-06f95994699c" />
<br/>
<br/>


**Followed Steps:**

Reviewed the DHCP scope and discovered no available addresses remained for workstation assignment:

<img width="545" height="374" alt="Pasted image 20260506221948" src="https://github.com/user-attachments/assets/2207bf62-4bf9-4035-8a01-84b0fa104fb6" />
<br/>
<br/>

Expanded the DHCP scope to provide additional addresses for workstation leases:
- We can see how the DHCP lease duration is just for 8 days.
<img width="765" height="598" alt="Pasted image 20260506222231" src="https://github.com/user-attachments/assets/e29c1ac9-54ea-4db6-91ab-25f69b7fdda6" />
<br/>
<br/>

**Root Cause**
- While she was on her vacations the lease for the IP of her workstation got expired because it just lasts 8 days and she didn't log in for more than that time.
- The limit of DHCP leases was reached while she wasn't in the office, so when she came back couldn't log in. 
- She lost internet connection since the workstation couldn't lease an IP address from the DHCP server.

**Solution:** 
- Expanded the DHCP scope by 90 additional IP addresses.
- Asked the user to restart the workstation.

Verified successful DHCP lease assignment after reboot

> Confirmed the workstation received a valid lease from the DHCP server:

<img width="955" height="319" alt="Pasted image 20260506223038" src="https://github.com/user-attachments/assets/9476ed96-44c8-4fed-89df-13fb16d212a7" />
<br/>
<br/>

> Confirmed internet connectivity was restored on the user's workstation:
<img width="949" height="752" alt="Pasted image 20260506223155" src="https://github.com/user-attachments/assets/b2aba7e3-c7f9-428c-9bbd-f026c2eb2633" />
<br/>
<br/>

Closed the ticket after validating service restoration with the user:
<img width="942" height="557" alt="Pasted image 20260506225655" src="https://github.com/user-attachments/assets/0d91f3f7-64e2-4a5d-a028-311345ee0f1b" />
<br/>
<br/>


## Ticket 02: "Leaked" Employee (Active Directory/IAM)
**Situation:** The agent Ma Robinett recently got promoted and could not access resources assigned to the HR department.
<img width="943" height="670" alt="Pasted image 20260506231103" src="https://github.com/user-attachments/assets/14909e46-54aa-43e6-9738-342ecf255b2e" />
<br/>
<img width="937" height="664" alt="Pasted image 20260506231123" src="https://github.com/user-attachments/assets/c3cb4db8-d7a6-4186-a648-37e2b4265390" />

<br/>
<br/>

**Followed steps:**

Reviewed the user's account within Active Directory Users and Computers:
<img width="937" height="664" alt="Pasted image 20260506231228" src="https://github.com/user-attachments/assets/1c36b5fc-a327-4119-9373-c995c81fb0bc" />
<br/>
<br/>

Discovered that the account still belonged to the previous department security group:
<img width="937" height="664" alt="Pasted image 20260506231243" src="https://github.com/user-attachments/assets/b2a229dc-4e13-4601-8152-b62df418699f" />
<br/>
<br/>

Added the user to the right group and removed her from the old one:
<img width="537" height="567" alt="Pasted image 20260506231306" src="https://github.com/user-attachments/assets/502b19f9-93f6-46f8-9f09-c796e5b6311d" />
<br/>
<br/>


Answering the ticket:
<img width="902" height="610" alt="Pasted image 20260506232001" src="https://github.com/user-attachments/assets/41d3606e-54ff-4f11-8c6a-8c2289247660" />
<br/>
<br/>

Finally, the user was able to access the HR folder:
<img width="902" height="610" alt="Pasted image 20260506231930" src="https://github.com/user-attachments/assets/cd692b81-9c89-49e8-b8a6-476e96021fcf" />
<br/>
<br/>

**Root Cause:** User got promoted from Customer Service Agent to the HR Department a few days ago, but the updated wasn't made on AD and they still on the Customer Service group and didn't have permissions to access the HR folder.

**Solution:** 
- Removed the user from the previous department and assigned the correct HR security group.
- Advised the user to sign in again, otherwise the change won't take effect. 
<br/>
<br/>

## Ticket 03: Account Lockout
**Situation:** IT Agent came from lunch and by accident failed his password 3 times and his account got locked out.
<img width="795" height="488" alt="Pasted image 20260507110826" src="https://github.com/user-attachments/assets/9e9b43ad-b883-461a-9eb0-0fa50fed4af9" />
<br/>
<img width="889" height="565" alt="Pasted image 20260507111014" src="https://github.com/user-attachments/assets/14dafa10-95dd-4dee-baf8-fdb566c5db06" />
<br/>
<br/>

**Followed steps:**

Unlocked the account and required a password change at the next sign-in:
<img width="691" height="556" alt="Pasted image 20260507111211" src="https://github.com/user-attachments/assets/6dcf638f-dde1-42ed-b63c-8817fde703b7" />
<br/>
<br/>

Assigned a temporary password for the user that they must change once they log in:
<img width="691" height="556" alt="Pasted image 20260507111316" src="https://github.com/user-attachments/assets/1ad0a77e-4273-4409-829c-b0794e5f4417" />
<br/>
<br/>

Replying to the user:
<img width="882" height="521" alt="Pasted image 20260507111653" src="https://github.com/user-attachments/assets/e773ef93-faed-4a18-a5d2-0f04f6f0e786" />
<br/>
<br/>


User successfully changed the password and signed into his workstation:
<img width="946" height="683" alt="Pasted image 20260507112315" src="https://github.com/user-attachments/assets/149c8ebf-4c3d-425d-80bf-9348d9f099b6" />
<br/>
<br/>

**Cause:** The account lockout policy was configured to trigger after three failed authentication attempts.

**Solution:** 
- Unlocking the user account, resetting the password, and assigning a temporal password that they must change on the next sign in.
  
<br/>
<br/>
<br/>
<br/>

