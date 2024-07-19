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

- Step 1: Setup Resources in Azure
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
 ![Virtual machines](https://github.com/user-attachments/assets/922993ae-51a6-4c07-aeb5-0febe64889e8)

 </p>
Set Domain Controller’s NIC Private IP address to be static
 </p>
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet of DC-1
 </p>
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
</p>
<br />
- Step 2: Ensure Connectivity between the client and Domain Controller
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
   </p>
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
   </p>
Check back at Client-1 to see the ping succeed
</p>
<br />
<h2>Deployment and Configuration Steps</h2>

- Step 3: Install Active Directory

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services
   </p>
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
 </p>
Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />
- Step 4: Create an Admin and Normal User Account in AD

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
   </p>
Create a new OU named “_ADMINS”
 </p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
 </p>
Add jane_admin to the “Domain Admins” Security Group
 </p>
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
 </p>
User jane_admin as your admin account from now on
</p>
<br />
- Step 5: Join Client-1 to your domain (mydomain.com)
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
   </p>
From the Azure Portal, restart Client-1
 </p>
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
 </p>
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
</p>
Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)
</p>
<br />
- Step 6:Setup Remote Desktop for non-administrative users on Client-1
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into Client-1 as mydomain.com\jane_admin and open system properties
  </p>
Click “Remote Desktop”
</p>
Allow “domain users” access to remote desktop
</p>
You can now log into Client-1 as a normal, non-administrative user now
</p>
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
</p>
<br />
- Step 7: Create a bunch of additional users and attempt to log into client-1 with one of the users
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 as jane_admin
  </p>
Open PowerShell_ise as an administrator
</p>
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
</p>
Run the script and observe the accounts being created
</p>
When finished, open ADUC and observe the accounts in the appropriate OU
</p>
attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>
<br />
