<p align="center">
<img src="https://i.imgur.com/Pr4iPzt.png" alt="Microsoft Active Directory"/>
</p>

<h1>Active Directory: Users, Group Policy and Account Management</h1>
In this tutorial, we will Install Active Directory, Create a Domain, Add a Client PC to the Domain, Create Domain Users, Add Group Policies and Manage Accounts. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Group Policy Management

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Windows Server 2022

<h2>High-Level Steps</h2>

- Install Active Directory Domain Services
- Create a Domain Admin User within the Domain
- Join Client VM to the Domain
- Setup Remote Desktop for non-administrative users on the Client VM
- Create additional users and attempt to log into the Client Vm with one of these users
- Configure Group Policy for Acccount Lockout
- Enable and Disable Accounts

<h2>Setting Up Active Directory and the Domain Controller</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 1: Install Active Directory Domain Services and Promote to Domain Controller
  
- Log into the **Domain Controller** virtual machine.
- Open **Server Manager**.
  - Click **Manage > Add Roles and Features**.
  - Select **Active Directory Domain Services**.
  - Complete the wizard and click **Install**.
-  After installation, click **Promote this server to a domain controller**.
-  Choose:
   - **Add a new forest**
   - Root domain name: `capsulecorporation.com`
- Create a Directory Services Restore Mode (DSRM) password (can match the admin password).
- Complete the configuration and **restart** the VM.
- After restart, log in as:  
    `capsulecorporation.com\CapsuleCorpAdmin`

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 2: Create a Domain Admin User in Active Directory
  
- Open **Active Directory Users and Computers (ADUC)**.
- In the domain tree, right-click and create a new **Organizational Unit (OU)** named `_EMPLOYEES`.
- Create another OU named `_ADMINS`.
- Right-click the `_ADMINS` OU > **New > User**:
   - First name: `Prince`
   - Last name: `Vegeta`
   - Username: `vegeta_admin`
   - Password: `PlanetVegeta` (use the same password for lab simplicity)
- Right-click the new user `vegeta_admin` > **Add to a group...**
   - Type `Domain Admins` and click **OK**.
- Log out of the Domain Controller.
- Log back in as:  
   `capsulecorporation.com\vegeta_admin`
- Use this account (`vegeta_admin`) as your **primary admin account** from this point forward.
   
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 3: Join the Client VM to the Domain
  
- RDP into the **Client** virtual machine using the local admin account (e.g., `Bulma`).
- Right-click **Start > System** and click **Rename this PC (advanced)**.
  - Click **Change** to join a domain.
  - Enter the domain: `capsulecorporation.com`
  - Enter domain credentials when prompted:  
     - Username: `capsulecorporation.com\vegeta_admin`  
     - Password: `PlanetVegeta`
- Restart the computer when prompted.
- RDP into the **DC-1** VM.
- Open **Active Directory Users and Computers (ADUC)** and verify `Client-1` appears under **Computers**.
- Create a new OU named `_CLIENTS`.
- Drag `Client-1` from **Computers** into the `_CLIENTS` OU.

</p>
<br />


<h2>Setting Up Remote Desktop and User Creation</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 1: Setup Remote Desktop for Non-Admin Users on the Client VM
  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 2: User Creation and Login Attempt to the Client VM
  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

</p>
<br />


<h2>Group Policy & Account Management</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 1: Configuring a Group Policy for Account Lockout
  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

  ## Step 2: Disabling and Re-Enabling Active Directory Accounts
  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

</p>
<br />
