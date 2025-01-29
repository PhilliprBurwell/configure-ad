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

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#1 Setup Domain Controller & Client-1 in AzureCreate a Resource Group
Create a Virtual Network and Subnet Create the Domain Controller VM (Windows Server 2022) named “DC-1” Username: labuser Password: Cyberlab123! After VM is created, set Domain Controller’s NIC Private IP address to be static Log into the VM and disable the Windows Firewall (for testing connectivity)Create the Client VM (Windows 10) named “Client-1”Username: labuser Password: Cyberlab123! Attach it to the same region and Virtual Network as DC-1 After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address From the Azure Portal, restart Client-1 Login to Client-1 Attempt to ping DC-1’s private IP address Ensure the ping succeeded From Client-1, open PowerShell and run ipconfig /allThe output for the DNS settings should show DC-1’s private IP Address


</p>
<br />

<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#2 Install Active Directory  Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />



<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#3 Create a Domain Admin user within the domain In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS” Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! Add jane_admin to the “Domain Admins” Security Group Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin” User jane_admin as your admin account from now on

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#4 Join Client-1 to your domain (mydomain.com)Login to Client-1 as the original local  admin (labuser) and join it to the domain (computer will restart) Login to the Domain Controller and verify Client-1 shows up in ADUC Create a new OU named “_CLIENTS” and drag Client-1 into there

</p>
<br />


<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#5 Setup Remote Desktop for non-administrative users on Client-1 Log into Client-1 as mydomain.com\jane_admin Open system properties Click “Remote Desktop” Allow “domain users” access to remote desktopYou can now log into Client-1 as a normal, non-administrative user nowNormally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)


</p>
<br />


<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
#6 Create a bunch of additional users and attempt to log into client-1 with one of the users Login to DC-1 as jane_admin Open PowerShell_ise as an administrator Create a new File and paste the contents of the script into it Run the script and observe the accounts being created When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES) attempt to log into Client-1 with one of the accounts (take note of the password in the script)

</p>
<br />
