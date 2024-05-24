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

![Server manager click on add roles and features](https://github.com/RalphgoldIT/configure-ad/assets/170049429/afe326d0-21f4-4dd5-9192-43771bbb7bf6)

![Active directory domain services](https://github.com/RalphgoldIT/configure-ad/assets/170049429/4e8e1b0d-977a-4e6b-8118-b26c2632a8cd)

![Active Directory domain servces installed](https://github.com/RalphgoldIT/configure-ad/assets/170049429/3d58ce27-33ac-4798-8f6f-a4babd3f6091)

<p>Promote as a DC: Setup a new forest as mydomain.com</p>

![Promote server to a domain controller](https://github.com/RalphgoldIT/configure-ad/assets/170049429/47a7e51e-a999-4984-aa6e-7b42a8c1a150)

![Domain controler options](https://github.com/RalphgoldIT/configure-ad/assets/170049429/ff03c6fc-723a-47b3-9c9c-2264f5c4ea92)

![Additional Options](https://github.com/RalphgoldIT/configure-ad/assets/170049429/c27eeb72-188b-4f4a-9eb7-2bb5f69aceff)

![Prerequiistite check](https://github.com/RalphgoldIT/configure-ad/assets/170049429/eea991d0-74dc-4622-8140-9ba9a4687fa7)

<p>Restart and then log back into DC-1 as user: mydomain.com\labuser.</p>


<h2>Create an Admin and Normal User Account in AD</h2>

<p>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and "_ADMINS".</p>

![Created new Organization units Employees and admins](https://github.com/RalphgoldIT/configure-ad/assets/170049429/f3b60561-769a-49c8-aa64-dbde236babe5)

<p>Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”.</p>

![Created new user jane doe](https://github.com/RalphgoldIT/configure-ad/assets/170049429/1d3817f7-6879-444f-a5c7-1f96afc020d3)

![Password for user account jane doe](https://github.com/RalphgoldIT/configure-ad/assets/170049429/2cd85aa2-a36c-4b3d-9eef-184e2c03cbeb)


<p>Add jane_admin to the “Domain Admins” Security Group.</p>

![Add jane doe to a group](https://github.com/RalphgoldIT/configure-ad/assets/170049429/c18a57b1-bbc1-44ec-8842-00de603ed3d9)

![add hane to domain admin group](https://github.com/RalphgoldIT/configure-ad/assets/170049429/4783ca1b-7b65-4091-9d08-b52eab9257ce)

![Added jane to domain admin group](https://github.com/RalphgoldIT/configure-ad/assets/170049429/525e4940-e368-4780-95c7-3e913088eca6)

<p>Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”.</p>

![Logout from DC-1 and login as Jane admin](https://github.com/RalphgoldIT/configure-ad/assets/170049429/659c734a-dc6f-40c0-9053-50dc01998cad)

![Logged back into DC as Jane_admin](https://github.com/RalphgoldIT/configure-ad/assets/170049429/1270793a-1b19-4706-b6b3-092fa6ed81f9)


<p>Use jane_admin as your admin account from now on.</p>

<h2>Join Client-1 to your domain (mydomain.com)</h2>

<p>Configure Client-1's DNS settings to use the private IP address of the DC from the Azure Portal.</p>

![DC-1 Private IP Address](https://github.com/RalphgoldIT/configure-ad/assets/170049429/cd2a2c08-edbd-4842-986a-fba515b2a7ef)

![change client 1 NIC](https://github.com/RalphgoldIT/configure-ad/assets/170049429/174fc7aa-e4d3-4803-a9f5-f3bc1e8fa5ad)

![Add DC private ip to Client 1 DNS Server and change the dns server settings from inherit to network](https://github.com/RalphgoldIT/configure-ad/assets/170049429/5f205cfa-70a6-4cfb-8158-6c0987aca84c)

<p>Restart Client-1 from the Azure Portal.</p>

![Restart client 1](https://github.com/RalphgoldIT/configure-ad/assets/170049429/796b1ccd-e6fb-4d1d-9223-7c4db6ad3cbc)

<p>Log in to Client-1 via Remote Desktop using the original local admin credentials (labuser) and proceed to join it to the domain. The computer will automatically restart as part of this process.</p>

![Copy client 1 public address](https://github.com/RalphgoldIT/configure-ad/assets/170049429/53caa5b8-aff4-49a2-802d-fe0a0d04f409)

![Screenshot 2024-05-23 185936](https://github.com/RalphgoldIT/configure-ad/assets/170049429/a26eda98-c0cb-438b-ba41-bd4451ec8437)

![Screenshot 2024-05-23 190031](https://github.com/RalphgoldIT/configure-ad/assets/170049429/79df15ae-4b33-4a92-9c71-a3d6029c8a5a)

![join domain](https://github.com/RalphgoldIT/configure-ad/assets/170049429/3cffaa8c-e6a5-4f44-8217-9aed42849fa8)

![join a domain](https://github.com/RalphgoldIT/configure-ad/assets/170049429/41daad31-b491-4227-9148-b75978e3a8ac)

![Screenshot 2024-05-23 200620](https://github.com/RalphgoldIT/configure-ad/assets/170049429/0b33bdd0-0328-4669-a8ce-06ab728ac22e)

![Screenshot 2024-05-23 200555](https://github.com/RalphgoldIT/configure-ad/assets/170049429/6d13e599-42bd-4261-acf8-ac85a4523a6a)

![Logged back into DC as Jane_admin](https://github.com/RalphgoldIT/configure-ad/assets/170049429/c35ccbf9-3856-4b0a-ba3a-d6e476d5f324)

![go to remote desktop](https://github.com/RalphgoldIT/configure-ad/assets/170049429/da96070c-f74c-4a24-ba78-b2ffd40478cb)

![Add desktop users](https://github.com/RalphgoldIT/configure-ad/assets/170049429/c17a69dd-971b-42a6-b614-f2252c5a2e20)

![Add domain Users and click ok](https://github.com/RalphgoldIT/configure-ad/assets/170049429/52ea61c8-af9f-4918-bdb8-e0e4a4e6dc39)

![checking users](https://github.com/RalphgoldIT/configure-ad/assets/170049429/dca74dff-e4a8-43be-aea4-6b57eee4b9f3)




</body>
</html>
