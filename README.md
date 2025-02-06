<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller & Client-1 in Azure
- Install Active Directory
- Create a Domain Admin user within the domain In Active Directory 
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users
- Create a bunch of additional users and attempt to log into client-1
- Deal with Account Lockouts 

<h2>Deployment and Configuration Steps</h2>

<p>
(Image #1)
  
![image](https://github.com/user-attachments/assets/502d742d-6ff2-45fe-8b00-785abc3f9d12)
(Image #2)

![image](https://github.com/user-attachments/assets/bc85416f-c99a-4081-812c-ecd5d6d28226)
(Image #3)

![image](https://github.com/user-attachments/assets/2cf93d4f-9e2d-4bea-93f9-c189b29e3bc9)


</p>
<p>
#1 Setup Domain Controller & Client-1 in AzureCreate a Resource Group
Create a Virtual Network and Subnet Create the Domain Controller VM (Windows Server 2022) named “DC-1” Username: labuser Password: Cyberlab123! After VM is created, set Domain Controller’s NIC Private IP address to be static.Log into the VM and disable the Windows Firewall. Create the Client VM (Windows 10) named “Client-1”Username: labuser Password: Cyberlab123! Attach it to the same region and Virtual Network as DC-1 After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address From the Azure Portal. Set it from the virtual netwrok to the custom private ip which is now 10.0.0.4. restart Client-1 Login to Client-1 Attempt to ping DC-1’s private IP address Ensure the ping succeeded From Client-1, open PowerShell and run ipconfig /allThe output for the DNS settings should show DC-1’s private IP Address


</p>
<br />
(Image #1)

![image](https://github.com/user-attachments/assets/d93b7b4e-fd06-4be3-a0af-f446a5dbf8c8)
(Image #2)

![image](https://github.com/user-attachments/assets/49c30d4a-2eed-45bf-89bd-0f830e924e71)

</p>
<p>
#2 Install Active Directory  Login to DC-1 and install Active Directory Domain Services by going into server manager and click add roles and features Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />
(Image #1)

![image](https://github.com/user-attachments/assets/af8da180-acff-43e9-9e4e-f8c4e1bca6dc)

</p>
<p>
#3 Create a Domain Admin user within the domain In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” Then Create a new OU named “_ADMINS” Create a new employee named “Jane Doe” (same password) with the username of “phil_admin” / Cyberlab123! Add jane_admin to the “Domain Admins” Security Group Log out / close the connection to DC-1 and log back in as “mydomain.com\phil_admin” User phil_admin as your admin account from now on

</p>
<br />

<p>
(Image #1)
  
![image](https://github.com/user-attachments/assets/68b71b57-725a-44c3-afc3-bcc776cc7be0)
(Image #2)

![image](https://github.com/user-attachments/assets/56b4e548-60f2-4e5e-bd58-ce2a8a479ce8)
(Image #3)

 ![image](https://github.com/user-attachments/assets/b7b8f4e8-c092-43d9-9e58-a2954f94e03f)
 
</p>
<p>
#4 Join Client-1 to your domain and Setup Remote Desktop for non-administrative users (mydomain.com)Login to Client-1 as the original local  admin (labuser) and join it to the domain (computer will restart) Login to the Domain Controller and verify Client-1 shows up in ADUC Create a new OU named “_CLIENTS” and drag Client-1 into there on Client-1 Log into Client-1 as mydomain.com\jane_admin Open system properties Click “Remote Desktop” Allow “domain users” access to remote desktopYou can now log into Client-1 as a normal, non-administrative user nowNormally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

</p>
<br />
(Image #1)

![image](https://github.com/user-attachments/assets/1b7b0e2c-6e6e-4afb-a4b3-ab601c7f3b3d)
(Image #2)

![image](https://github.com/user-attachments/assets/8abc83bf-925a-4d00-ac21-2c4fff043106)


</p>
<p>
#5 Create a bunch of additional users and attempt to log into client-1 with one of the users Login to DC-1 as jane_admin Open PowerShell_ise as an administrator Create a new File and paste the contents of the script into it Run the script and observe the accounts being created. When finished, open ADUC and observe the accounts in the _EMPLOYEE section. I chose a random user named "bale.celu" and attempted to log into Client-1 with the account. 
</p>
<br />
(Image #1)

![image](https://github.com/user-attachments/assets/c5070dce-84ce-43ad-9167-98072bf7ccbe)
(Image #2)

![image](https://github.com/user-attachments/assets/d22a9b17-5ee0-4da1-a12e-0365c0524fb3)
(Image #3)

![image](https://github.com/user-attachments/assets/6a2e016a-6e0a-4476-b854-55e5209fc3e6)
(Image #4)

![image](https://github.com/user-attachments/assets/79b68485-5f67-465c-aeaf-906dc8b266ab)
(Image #5)

![image](https://github.com/user-attachments/assets/1e889d38-d3fb-4d6c-9103-6858fec2474d)

</p>
<p>
#6 Dealing with Account Lockouts Get logged into dc-1 Pick a random user account you created previously Attempt to log in with it 10 times with a bad password Configure Group Policy to Lockout the account after 5 attempts: How To Configure Account Lockout Threshold in Group Policy Attempt to log in with it 6 times with a bad password Observe that the account has been locked out within Active Directory Unlock the account Reset the password Attempt to login with it Enabling and Disabling Accounts Disable the same account in Active Directory Attempt to login with it, observe the error message Re-enable the account and attempt to login with it. Observing Logs Observe the logs in the Domain Controller Observe the logs on the client Machine


