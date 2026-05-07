# Ticketing-System-integration-with-AD-Enviroment


# Taken Steps
Here's a quick review of how I installed the ticketing software on an Ubuntu Server. 

## Step 1 - Created an Ubuntu Server VM to host the Ticketing Software

Created another VM running Ubuntu Server that will get its IP through DHCP and it will  use our Windows Server machine as its default gateway.

Finally, I shh the Ubuntu Server in order to install docker and the ticketing software (UVDesk):

<img width="910" height="560" alt="Pasted image 20260505211815" src="https://github.com/user-attachments/assets/59760f93-85e3-4683-a4dc-e521d3a85c10" />
<br/>
<br/>
Installed the ticketing software on a docker container using docker compose:

<img width="951" height="576" alt="Pasted image 20260505221848" src="https://github.com/user-attachments/assets/4a51721a-76a8-477f-bd32-f94d9cb4aa93" />
<br/>
<br/>

The software was hosted on a web page using HTTP and now our Ubuntu Server is a internal web server running Apache while hosting this service:
<img width="955" height="678" alt="Pasted image 20260505223431" src="https://github.com/user-attachments/assets/13359ddf-cd7c-4b5d-9163-42d2a4f7b095" />
<br/>
<br/>

Finally I created a A DNS record that mapps the Ticketing server's IP with a user friendly name (UVdesk):
- Before adding the DNS record, I set a static IP address to the Ubuntu Server machine hosting the UVdesk after configuration.

> Connecting to the web server using the domain name of the server:
<img width="938" height="453" alt="Pasted image 20260506194026" src="https://github.com/user-attachments/assets/92e7fe2c-573d-4e59-a5b5-a9929088163d" />
<br/>
<br/>

> Configuration made on the server through SSH:
<img width="893" height="425" alt="Pasted image 20260506200440" src="https://github.com/user-attachments/assets/c48ea036-e998-4db6-a10a-6bf71c9c0bd4" />
<br/>
<br/>

> Reconnecting through SSH using the domain name of the server:
<img width="731" height="49" alt="Pasted image 20260506200444" src="https://github.com/user-attachments/assets/ee233699-a981-4357-9fe3-bcf8ce5179ff" />
<br/>
<br/>

# Resolved Tickets
Now I simulated some common issues that  a Help Desk L1 Agent would've resolved.

## Ticket 01: (DHCP Scope Exaustion)

**Situation:** Customer called IT due to she didn't have internet access on her workstation, but other coworkers did have access.

<img width="882" height="422" alt="Pasted image 20260506221328" src="https://github.com/user-attachments/assets/9b89badb-2e12-41d1-aa06-06f95994699c" />
<br/>
<br/>


**Followed Steps:**

Checked the IP Address pool on the DHCP configurations of the server, and there was no available addresses for the workstation to take since the range was the same as the ones that were reserved.

<img width="545" height="374" alt="Pasted image 20260506221948" src="https://github.com/user-attachments/assets/2207bf62-4bf9-4035-8a01-84b0fa104fb6" />
<br/>
<br/>

I changed the address range to have 90 IP addresses more for workstations:
- We can see how the DHCP lease duration is just for 8 days.
<img width="765" height="598" alt="Pasted image 20260506222231" src="https://github.com/user-attachments/assets/e29c1ac9-54ea-4db6-91ab-25f69b7fdda6" />
<br/>
<br/>

**Cause:** Agent Alva Cosey from financial department didn't have internet for two reasons:
- She was on a vacations of 15 days and the lease for the IP of her workstation got expired because it just lasts 8 days.
- Then the scope was too little and limited for the reserved statics IPs. 
- She lost her internet connection since the workstation couldn't lease an IP address from the DHCP server.

**Solution:** 
- Increased the DHCP Scope with 90 more IPs.
- Send an email to the customer telling them to restart the computer.

After contacting the Agent and told us she restarted the computer we confirmed it leased and IP address successfully. 

> Screenshot of the server to confirm the lease:

<img width="955" height="319" alt="Pasted image 20260506223038" src="https://github.com/user-attachments/assets/9476ed96-44c8-4fed-89df-13fb16d212a7" />
<br/>
<br/>

> Screenshot from the workstation, confirming it had internet access:
<img width="949" height="752" alt="Pasted image 20260506223155" src="https://github.com/user-attachments/assets/b2aba7e3-c7f9-428c-9bbd-f026c2eb2633" />
<br/>
<br/>

Finally, I resolved the ticket:
<img width="942" height="557" alt="Pasted image 20260506225655" src="https://github.com/user-attachments/assets/0d91f3f7-64e2-4a5d-a028-311345ee0f1b" />
<br/>
<br/>


## Ticket 02: "Leaked" Employee (Active Directory/IAM)
**Situation:** The agent Ma Robinett got promoted from Customer Service to HR Department, but she cannot access the shared folder.
<img width="943" height="670" alt="Pasted image 20260506231103" src="https://github.com/user-attachments/assets/14909e46-54aa-43e6-9738-342ecf255b2e" />
<br/>
<img width="937" height="664" alt="Pasted image 20260506231123" src="https://github.com/user-attachments/assets/c3cb4db8-d7a6-4186-a648-37e2b4265390" />

<br/>
<br/>
**Followed steps:**

Searched the user on Active Directory Users and Computers:
<img width="937" height="664" alt="Pasted image 20260506231228" src="https://github.com/user-attachments/assets/1c36b5fc-a327-4119-9373-c995c81fb0bc" />
<br/>
<br/>

When getting into the user properties I confirmed that she was not in the right group:
<img width="937" height="664" alt="Pasted image 20260506231243" src="https://github.com/user-attachments/assets/b2a229dc-4e13-4601-8152-b62df418699f" />
<br/>
<br/>

Added the user to the right group and removed the old one:
<img width="537" height="567" alt="Pasted image 20260506231306" src="https://github.com/user-attachments/assets/502b19f9-93f6-46f8-9f09-c796e5b6311d" />
<br/>
<br/>


Answer the ticket:
<img width="902" height="610" alt="Pasted image 20260506232001" src="https://github.com/user-attachments/assets/41d3606e-54ff-4f11-8c6a-8c2289247660" />
<br/>
<br/>

Finally, user was able to access the HR folder:
<img width="902" height="610" alt="Pasted image 20260506231930" src="https://github.com/user-attachments/assets/cd692b81-9c89-49e8-b8a6-476e96021fcf" />
<br/>
<br/>

**Cause:** User got promoted from Customer Service Agent to the HR Department a few days ago, but the updated wasn't made on AD and they still on the Customer Service group and didn't have permissions to access the folder of HR.

**Solution:** 
- Removed the Customer Service agents group on the user properties in AD and add them to HR Department group.
- Advised the customer to sign in again, otherwise the change won't take effect. 
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

Unlocked the user's account and made him to change his password on the next login:
<img width="691" height="556" alt="Pasted image 20260507111211" src="https://github.com/user-attachments/assets/6dcf638f-dde1-42ed-b63c-8817fde703b7" />
<br/>
<br/>

Set up a generic temporal password for the user that they will need to change once log in:
<img width="691" height="556" alt="Pasted image 20260507111316" src="https://github.com/user-attachments/assets/1ad0a77e-4273-4409-829c-b0794e5f4417" />
<br/>
<br/>

Replying to the user:
<img width="882" height="521" alt="Pasted image 20260507111653" src="https://github.com/user-attachments/assets/e773ef93-faed-4a18-a5d2-0f04f6f0e786" />
<br/>
<br/>


User successfully changed the password and log into his workstation:
<img width="946" height="683" alt="Pasted image 20260507112315" src="https://github.com/user-attachments/assets/149c8ebf-4c3d-425d-80bf-9348d9f099b6" />
<br/>
<br/>

**Cause:** User typed the wrong password several times and the Group Policy of Account Lockout has a threshold of 3 times, if you fail that amount your account gets locked out.

**Solution:** 
- Unlocking the user account and resetting his password for a temporal one that he would need to change once logs in again.

