<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
In this tutorial, I'm going to be outlining the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2> Setting up a Domain Controller in Azure</h2>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setting Domain and Client VM
- Installing Active Directory
- Deployment
- Creating users with PowerShell
- Group policy

<h1 align="center"> Setting up Domain and Client VM </h1>

<h2>Step 1: Setting up the Domain Controller Virtual Machine (VM)</h2>

- Create a Resource Group

- Create a Virtual Network and Subnet

<p>
  <img src="https://i.imgur.com/hnhRKQo.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/NZC4lLW.png" height="80%" width="80%"/>

- Create the Domain Controller VM (Windows Server 2022) named “DC-1”

- For the Administrator Account; User: mr_admin Password: Password1
  
  <img src="https://i.imgur.com/tJExBRG.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/uIxWc03.png" height="80%" width="80%"/>
  
- After the VM is created, set the Domain Controller’s NIC Private IP address to be static
  
  <img src="https://i.imgur.com/AMubBay.png" height="80%" width="80%"/>
  <br><br/>
  
<h2>Step 2: Setup Client-1 in Azure</h2>

- Create the Client VM (Windows 10) named “Client-1”

- Within the same region and Virtual Network as DC-1

- User: mr_admin Password: Password1
  
  <img src="https://i.imgur.com/ukR7m5G.png" height="80%" width="80%"/>  
  <img src="https://i.imgur.com/uQec1nk.png" height="80%" width="80%"/>
  
- After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address

  <img src="https://i.imgur.com/OptjMyC.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/xnhpmxb.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/mRQbjmy.png" height="80%" width="80%"/>
  
- Restart VM
  
  <img src="https://i.imgur.com/NAldMlz.png" height="80%" width="80%"/>
  
</p>
<br><br/> 

<h2>Step 3: Ping DC-1's private IP through Client-1</h2>

- But First, login to DC-1 using "mr_admin" and "Password1" 
  
- Open Windows Firewall settings as an Admin, and disable all firewalls. (This allows us to be able to ping DC-1 from Client-1)
  
  <img src="https://i.imgur.com/9VWmAVM.png" height="80%" width="80%"/>
<p>
  
- Login to Client-1

<img src="https://i.imgur.com/BSEXAEY.png" height="80%" width="80%"/>

- Attempt to ping DC-1’s private IP address
  
  <img src="https://i.imgur.com/qz0hsLi.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/ErteWLE.png" height="80%" width="80%"/>
  
- To ensure the ping succeeded
  
- From Client-1, open PowerShell and run ipconfig /all
  
- The output for the DNS settings should show DC-1’s private IP Address

  <img src="https://i.imgur.com/fEqdFG0.png" height="80%" width="80%"/>
  
  <h3 align="center">This concludes the setup process </h3>
</p>
<br><br/> 

<h1 align="center">Installing an Active Directory<h1/> 

<h2>Step 1: Login to DC-1 and install Active Directory Domain Services </h2>

- Promote as a DC: Set up a new forest as mydomain.com (can be anything, just remember what it is) 

<p>
  <img src="https://i.imgur.com/nNeLHp4.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/uwl7IrW.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/BaHiOMJ.png" height="80%" width="80%"/>  
  <img src="https://i.imgur.com/Jv5PuYj.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/BggkALw.png" height="80%" width="80%"/>

  - Install 
  
  <img src="https://i.imgur.com/KGm80bo.png" height="80%" width="80%"/>
  
  - Restart and then log back into DC-1 as user: mr_admin
    
  <img src="https://i.imgur.com/cXtF2fA.png" height="80%" width="80%"/>
</p>
<br><br/> 

<h1 align="center"> Creating a Domain Admin user within the Domain<h1/> 

<h2>Step 1: Active Directory Users and Computers (ADUC) </h2>

<p>
  <img src="https://i.imgur.com/pPK8M3z.png" height="80%" width="80%"/>

- Create a NEW Organizational Unit (OU) called “_EMPLOYEES”

- Create a NEW OU named “_ADMINS”
  
  <img src="https://i.imgur.com/Akl34dX.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/yiF0XvK.png" height="80%" width="80%"/>

- Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Password123!
  
  <img src="https://i.imgur.com/XN6nbsM.png" height="80%" width="80%"/>  
  <img src="https://i.imgur.com/07WdvZj.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/GrGpSUl.png" height="80%" width="80%"/>

 - Add jane_admin to the “Domain Admins” Security Group
    
  <img src="https://i.imgur.com/wmIIKqs.png" height="80%" width="80%"/> 
  <img src="https://i.imgur.com/B2vEzVu.png" height="80%" width="80%"/>  

- Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”

- User jane_admin as your admin account from now on
  
  <img src="https://i.imgur.com/7oD7Y5n.png" height="80%" width="80%"/>
</p>
<br><br/> 

<h1 align="center"> Join Client-1 to your domain (mydomain.com) <h1/> 
  
<h2>Step 1: </h2>
- s
- s

<p>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>  
</p>

<br><br/> 


<h2>Step 1: </h2>
- s
- s

<p>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>
  <img src="" height="80%" width="80%"/>  
</p>

<br><br/> 



<br />
