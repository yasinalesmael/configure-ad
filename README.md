<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022, 2vCPUs, 8GM RAM
- Windows 10 Pro (22H2), 2vCPUs, 8GB RAM

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin user for the domain
- Step 3: Join client-1 to domain

<h2>Deployment and Configuration Steps</h2>

<p>
Before deploying Active Directory, we'll need to install 2 VMs on Azure, one will be a Windows Server for the domain controller and one will be Windows Pro machine as a client. We can name the server machine DC-1 and the client machine Client-1. We also need to make sure that both of these machines are on the same virtual network.
  
Once that's done, we'll log into DC-1.
</p>
<br />
<p>
<img src="https://i.imgur.com/6c1SsyO.png" height="80%" width="80%" alt="server manager in DC-1"/>
</p>
<p>
The Server manager should automatically load up if not click on Start and you should see the Server Manager icon. Click on Add roles and features and accept the defaults until Select Server Roles. We'll select Active Directory Domain Services.
</p>
<br />
<p>
<img src="https://i.imgur.com/mlcnGPx.png" height="80%" width="80%" alt="installing AD"/>
</p>
<p>
Continue with this and check Restart the destination server automatically if required before clicking Install. After installation is complete, go back to the Server manager and click on the flagged icon in the top right. Then, click on the link that says to Promote this server to a domain controller. 
</p>
<br />
<p>
<img src="https://i.imgur.com/NI66WoN.png" height="80%" width="80%" alt="promote to domain controller"/>
</p>
<p>
In the deployment configuration, click on Add a new forest and input mydomain.com. In reality, you can name this domain anything you like.
</p>
<br />
<p>
<img src="https://i.imgur.com/1mUA3fe.png" height="80%" width="80%" alt="add new forest mydomain"/>
</p>
<p>
Click next and where it says to put in Directory services restore mode password, you can put in anything you like as it is unlikely we will use it. Continue with the configuration and you will be automatically signed out of the machine as Active Directory completes installation.

Now, we'll try to log into our Client-1 machine. If you try to login with your usual credentials, it will fail since no domain is specified as we just set up DC-1 as the domain controller. So to log into the machine, we need specify the domain like so: mydomain.com\\(your username), and then enter the password. 

Once that's confirmed, we jump back log back into the DC-1 machine. The next step is to create a Domain admin user within the domain. The person will be able to administer the entire domain of users and is quite an important task. So we can click on Start -> Windows Administrative Tools -> Active Directory Users and Computers. Now right click on mydomain.com -> New -> Organizational Unit (OU).
</p>
<br />
<p>
<img src="https://i.imgur.com/ughCcXr.png" height="80%" width="80%" alt="create OU for domain"/>
</p>
<p>
We can name the new OU _EMPLOYEES and we can create another one called _ADMINS. Then we'll create an admin user. Right click on _ADMINS -> New -> User. We'll name this admin user Jane Doe, with as username of jane_admin. Set whatever password that you wish. 

Although Jane Doe is in a OU called _ADMINS, she technically is not yet a Domain admin since we have not given those privileges to her. So to do this, we click on _ADMINS and see that Jane Doe is there. Right click on Jane Doe -> Properties -> Member Of then type in Domain Admins in the box. Click on Check names, then OK and apply these settings.
</p>
<br />
<p>
<img src="https://i.imgur.com/pFZDwJT.png" height="80%" width="80%" alt="set Jane as Domain admin"/>
</p>
<br />
Now we'll need to change Client-1's DNS servers such that they point to the private IP address of DC-1

<p>
<img src="https://i.imgur.com/46hi28j.png" height="80%" width="80%" alt="preconfigure vms before deploy AD"/>
</p>
<br />
<p>
Log back into Client-1 as Jane, be sure to do mydomain.com\ before the username. Right click on the Start menu -> Settings -> Rename this PC (advanced). Then under Computer Name click on Change and in Domain, type mydomain.com and OK.
</p>

<p>
<img src="https://i.imgur.com/pDVdUjS.png" height="80%" width="80%" alt="join client-1 to domain"/>
<img src="https://i.imgur.com/aiAhl9L.png" height="80%" width="80%" alt="client joined domain"/>
</p>
<p>
You will have to restart Client-1 for these changes to take effect. So do that and go back to DC-1. We'll verify if Client-1 has joined the domain. While still on Active Directory Users and Computers, click on mydomain.com and the Computers and you should see Client-1 there.
</p>
<br />

<p>
  <img src="https://github.com/user-attachments/assets/1344ee78-88e2-49e9-8496-933bd354572d" height="80%" width="80%" alt="confirm client 1 in domain"/>
</p>
<p>

</p>
