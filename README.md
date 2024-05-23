<!DOCTYPE html>
<html>
<head>
 
</head>
<body>

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
<p>This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.</p>

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Remote Desktop</li>
  <li>Active Directory Domain Services</li>
  <li>PowerShell</li>
</ul>

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 (21H2)</li>
</ul>

<h2>High-Level Deployment and Configuration Steps</h2>
<ol>
  <li>Setup Resources in Azure</li>
  <li>Ensure Connectivity between the client and Domain Controller</li>
  <li>Install Active Directory</li>
  <li>Create an Admin and Normal User Account in AD</li>
  <li>Join Client-1 to your domain (mydomain.com)</li>
  <li>Setup Remote Desktop for non-administrative users on Client-1</li>
</ol>

<h2>Setup Resources in Azure</h2>

<p>Set up the Domain Controller VM (Windows Server 2022) with the designation "DC-1." Remember to record the Resource Group and Virtual Network (Vnet) generated during this process.</p>

![Screenshot 2024-05-23 152528](https://github.com/RalphgoldIT/configure-ad/assets/170049429/5e915577-53c2-48b1-bb6d-d24e783a666f)

![Screenshot 2024-05-23 152911](https://github.com/RalphgoldIT/configure-ad/assets/170049429/f2682a8d-34ca-44a6-ab45-27e0828341e8)

<p>Configure the Domain Controller's NIC Private IP address to be static.</p>

![Screenshot 2024-05-23 154018](https://github.com/RalphgoldIT/configure-ad/assets/170049429/36701b99-8039-4380-b218-44ca1ec40772)

![Screenshot 2024-05-23 154211](https://github.com/RalphgoldIT/configure-ad/assets/170049429/505c27f6-bc56-4c33-a053-234e79b17208)

<p>Establish the Client VM (Windows 10) named "Client-1," utilizing the identical Resource Group and Vnet generated earlier.</p>

![Screenshot 2024-05-23 153209](https://github.com/RalphgoldIT/configure-ad/assets/170049429/93e642fa-d96a-41a4-9884-ff776c7db9f1)

<p>Confirm that both VMs are within the same Vnet.</p>

![Screenshot 2024-05-23 153600](https://github.com/RalphgoldIT/configure-ad/assets/170049429/0ff16f7a-fe81-415e-8829-1e78b9999ad3)


<h2>Ensure Connectivity between the client and Domain Controller</h2>
<p>Log in to Client-1 via Remote Desktop and initiate a continuous ping to DC-1's private IP address using the command: ping -t &lt;ip address&gt;.</p>

<p>Copy client-1 Public IP Address to Login on remote desktop.</p>

![client 1 public adddress](https://github.com/RalphgoldIT/configure-ad/assets/170049429/7fe50396-5d03-4767-a214-ea6ff201c8eb)

![RDP for  Client 1](https://github.com/RalphgoldIT/configure-ad/assets/170049429/17f71261-67bb-4e2f-83a9-79081b765531)

![RDP login to client 1](https://github.com/RalphgoldIT/configure-ad/assets/170049429/18f5935f-c3ff-43c9-b1e1-b353eda3e617)

<p>Ping DC-1 private IP Address<p>
 
![DC-1 private ip address](https://github.com/RalphgoldIT/configure-ad/assets/170049429/21fdf64e-3647-4563-8dcf-59536e18ff70)

![Faileed perpetual ping for client 1](https://github.com/RalphgoldIT/configure-ad/assets/170049429/aed9f643-753b-4f41-9cc2-d03e5bf1cbe7)

<p>Log in to the Domain Controller and enable ICMPv4 in the local Windows Firewall settings.</p>

![Screenshot 2024-05-23 162633](https://github.com/RalphgoldIT/configure-ad/assets/170049429/8834380d-ba42-4110-8e5b-2c863d19ff1e)

![Screenshot 2024-05-23 162750](https://github.com/RalphgoldIT/configure-ad/assets/170049429/8cdfb3e8-9d5f-451c-9ddc-1009a5a1aea5)

![Screenshot 2024-05-23 162816](https://github.com/RalphgoldIT/configure-ad/assets/170049429/3c50751d-0c50-4438-8a26-f56c83e9712d)

<p>Enable ICMP Echo request (ICMPv4-IN) 1 & 2<p>

![windows firewall](https://github.com/RalphgoldIT/configure-ad/assets/170049429/2c0478b5-0f0a-44e7-88f6-d23bc960b270)

<p>Check back at Client-1 to see the ping succeed.</p>

![logged back in to Clinent 1 and ping DC-1 for connectivity](https://github.com/RalphgoldIT/configure-ad/assets/170049429/32f1d282-4c2e-4962-8616-50c369ccef29)


<h2>Install Active Directory</h2>
<p>Log in to DC-1 and proceed to install Active Directory Domain Services.</p>
<p>Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is).</p>
<p>Restart and then log back into DC-1 as user: mydomain.com\labuser.</p>

<h2>Create an Admin and Normal User Account in AD</h2>
<p>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”.</p>
<p>Create a new OU named “_ADMINS”.</p>
<p>Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”.</p>
<p>Add jane_admin to the “Domain Admins” Security Group.</p>
<p>Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”.</p>
<p>Use jane_admin as your admin account from now on.</p>

<h2>Join Client-1 to your domain (mydomain.com)</h2>
<p>Configure Client-1's DNS settings to use the private IP address of the DC from the Azure Portal.</p>
<p>Restart Client-1 from the Azure Portal.</p>
<p>Log in to Client-1 via Remote Desktop using the original local admin credentials (labuser) and proceed to join it to the domain. The computer will automatically restart as part of this process.</p>
<p>Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.</p>
<p>Create a new Organizational Unit (OU) named "_CLIENTS" and relocate Client-1 into it.</p>

<h2>Setup Remote Desktop for non-administrative users on Client-1</h2>
<p>Log in to Client-1 using the credentials "mydomain.com\jane_admin" and proceed to open the system properties.</p>
<p>Click “Remote Desktop”.</p>
<p>Allow “domain users” access to remote desktop.</p>
<p>You are now able to log in to Client-1 as a regular, non-administrative user.</p>

<p>Leveraging Group Policy is an efficient method for implementing changes across multiple systems simultaneously, ensuring consistency and streamlining administrative tasks.</p>

</body>
</html>
