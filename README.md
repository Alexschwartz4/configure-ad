<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



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

</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
 </p>
<img src="https://imgur.com/AfsY0Rp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
Set Domain Controller’s NIC Private IP address to be static
 </p>
 <img src="https://imgur.com/dkd3YDU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet of DC-1
 </p>
  <img src="https://imgur.com/QJptNyi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
<br />
- Step 2: Ensure Connectivity between the client and Domain Controller
</p>
<p>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
   </p>
 <img src="https://imgur.com/zlhA8Rw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
 Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
   </p>
  <img src="https://imgur.com/u0f30WX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
Check back at Client-1 to see the ping succeed
</p>
   <img src="https://imgur.com/eKOqPsx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

- Step 3: Install Active Directory
<p>
Login to DC-1 and install Active Directory Domain Services
   </p>
   <img src="https://imgur.com/undefined.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Promote as a DC: Setup a new forest as a .com
 </p>
  <img src="https://imgur.com/undefined.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Restart and then log back into DC-1 as user: mydomain.com\labuser
 </p>
  <img src="https://imgur.com/b2eBzPo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />
- Step 4: Create an Admin and Normal User Account in AD

<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and  “_ADMINS”
 </p>
  <img src="https://imgur.com/5PqiPt6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 </p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
 </p>
  <img src="https://imgur.com/GAriWA4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 </p>
Add jane_admin to the “Domain Admins” Security Group
 </p>
  <img src="https://imgur.com/rBtczpP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 </p>
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
 </p>
</p>
<br />
- Step 5: Join Client-1 to your domain (mydomain.com)
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
  </p>
  <img src="https://imgur.com/LE6z4j9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
   </p>
From the Azure Portal, restart Client-1
 </p>
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
 </p>
  <img src="https://imgur.com/O0i40yI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 </p>
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
 </p>
  <img src="https://imgur.com/Pizr6Mh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />
- Step 6:Setup Remote Desktop for non-administrative users on Client-1

<p>
Log into Client-1 as mydomain.com\jane_admin and open system properties

  </p>
Click “Remote Desktop”
</p>
Allow “domain users” access to remote desktop
</p>
You can now log into Client-1 as a normal, non-administrative user now
<p>
<img src="https://imgur.com/ij6Kzzw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
- Step 7: Create a bunch of additional users and attempt to log into client-1 with one of the users
<p>

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
<img src="https://imgur.com/EeRJqPv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

When finished, open ADUC and observe the accounts in the appropriate OU
</p>
<img src="https://imgur.com/ZbbQU0Y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>
<img src="https://imgur.com/vIPDxJz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
