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

   <img src="https://i.imgur.com/TzMQ1vS.png" height="80%" width="80%"/>

- Use the user jane_admin as your admin account from now on
  
  <img src="https://i.imgur.com/7oD7Y5n.png" height="80%" width="80%"/>
  
- Login to Client-1 as the original local admin (mr_admin) and join it to the domain (computer will restart)
 
  <img src="https://i.imgur.com/M3B6GL8.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/VMRF6sU.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/srUU8hD.png" height="80%" width="80%"/>

- Login to the Domain Controller and verify Client-1 shows up in ADUC

  <img src="https://i.imgur.com/0Zsbc3a.png" height="80%" width="80%"/>

- Create a new OU named “_CLIENTS” and drag Client-1 into there (organizational purposes)
  
<h3 align="center"> Great! now moving on to...  <h3/>  
</p>
<br><br/> 

<h1 align="center"> Remote Desktop for non-administrative users & Generating Users <h1/> 
  
<h2>Step 1: Log into Client-1 as mydomain.com\jane_admin </h2>

- Open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop

<img src="https://i.imgur.com/n5EEDGI.png" height="80%" width="80%"/>
<h3 align="center"> You can now log into Client-1 as a normal, non-administrative user now<h3/> 
<p>
 <h2>Step 2: Create a bunch of additional users and attempt to log into client-1 with one of the users </h2>
  
- Login to DC-1 as jane_admin
  
- Open PowerShell_ise as an administrator

- Next: Create a new File and paste the contents of [this script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it 

  <img src="https://i.imgur.com/9MW0dxq.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/DZEgyGE.png" height="80%" width="80%"/>

- Run the script and observe the accounts being created
  
  <img src="https://i.imgur.com/JEjbMgX.png" height="80%" width="80%"/>

- When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)

  <img src="https://i.imgur.com/3M8akM3.png" height="80%" width="80%"/>
  
- Log into Client-1 with one of the accounts (take note of the password in the script)
  
  <img src="https://i.imgur.com/eF1BAeV.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/CXU8TlC.png" height="80%" width="80%"/>  
</p>
    <h3 align="center"> Now we know how to generate Domain Users using a Powershell script. From the Active Directory and Users menu, we are able to perform password resets and other account modifications if need be. On to the next step  </h3>
<br><br/> 

<h1 align="center"> Group Policy <h1/> 

<h2>Step 1: Open the Group Policy Management Console (GPMC) </h2>

- Log in to a machine with Group Policy Management Console installed (typically, a Domain Controller).
  
- Click Start, and type gpmc.msc in the search box, then press Enter. This opens the Group Policy Management Console.

- In the GPMC, navigate to the Group Policy Objects section.

- Right-click Group Policy Objects and select New to create a new GPO, or right-click an existing GPO and select Edit to modify it.
  
- Give the new GPO a descriptive name if you're creating a new one, like "Account Lockout Policy

- In the Group Policy Management Editor, expand the following:
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.

<p>
  <img src="https://i.imgur.com/BlYjbGT.png" height="80%" width="80%"/>
  <img src="https://i.imgur.com/6ILK4UH.png" height="80%" width="80%"/> 
  <p align="center">Link the GPO to an Organizational Unit (OU). Once the GPO is configured, you need to link it to the appropriate Organizational Unit (OU) or domain where you want the policy to apply.
In the GPMC, right-click the OU or domain where you want to apply the GPO and select Link an Existing GPO.
Choose the GPO you created or modified earlier and click OK.</p>

  - Customize policy to your liking, for this tutorial I've set mine to these.
    
  <img src="https://i.imgur.com/e0UW1WC.png" height="80%" width="80%"/>

<h2>Step 2: Update policy </h2>

- You can wait for the Group Policy to propagate automatically, or you can force an update immediately.

  <img src="https://i.imgur.com/26A6Vc2.png" height="80%" width="80%"/>

- Once the policy updates, you can log out and try to login once again (with a wrong password) to see if the GPO is in affect.
  
  <img src="https://i.imgur.com/1ueq0Bi.png" height="80%" width="80%"/>
  
<p align="center">We should have something like this if it works</p> 
<br></br> 
- Important Considerations: 

- Account Lockout Threshold: Setting this too low (e.g., 1 or 2 attempts) can lead to unnecessary lockouts.

- Account Lockout Duration: Setting this too high can be inconvenient for users but increases security.
  
- Reset Account Lockout Counter After: Setting this too short could allow attackers to repeatedly attempt to log in without triggering a lockout.
  
<br></br> 

   <h1 align="center">Dealing with Account Lockouts</h1>
   
<p> 
  
- Observe that the account has been locked out within Active Directory
- Unlock the account
<img src="https://i.imgur.com/4ekBX8M.png" height="80%" width="80%"/>
  
- Reset the password
<img src="https://i.imgur.com/xzX4D3R.png" height="80%" width="80%"/>
then attempt to login with it

  <p align="center">or</p>
    Observe the logs in the Domain Controller via Event View to see additional information about the login attempts. 
  <img src="https://i.imgur.com/EOT0VLZ.png" height="80%" width="80%"/>  
  </p>
</p>

<br><br/> 

<h5 align="center"> This concludes the on-premises Active Directory Installation, Deployment, and Navigation within Microsoft Azures Cloud services.</h5>

<br />
