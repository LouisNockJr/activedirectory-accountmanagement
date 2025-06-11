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
<img src="https://i.imgur.com/eA0hj2M.png" height="80%" width="80%" alt="AD DS Promotion Wizard"/>
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
<img src="https://i.imgur.com/dd9g5g0.png" height="80%" width="80%" alt="Add to Group"/>
</p>
<p>

  ## Step 2: Create a Domain Admin User in Active Directory
  
- Open **Active Directory Users and Computers (ADUC)**.
- In the domain tree, right-click **capsulecorporation.com** and create a **New > Organizational Unit (OU)** named `_EMPLOYEES`.
- Create another OU named `_ADMINS`.
- Right-click the `_ADMINS` OU > **New > User**:
   - First name: `Prince`
   - Last name: `Vegeta`
   - Username: `vegeta_admin`
   - Password: `Sayonara89!` 
- Right-click the new user `vegeta_admin` > **Add to a group...**
   - Type `Domain Admins` and click **OK**.
- Log out of the Domain Controller.
- Log back in as:  
   `capsulecorporation.com\vegeta_admin`
- Use this account (`vegeta_admin`) as your **primary admin account** from this point forward.
   
</p>
<br />

<p>
<img src="https://i.imgur.com/1v9gYWQ.png" height="80%" width="80%" alt="Windows Domain Join Prompt"/>
</p>
<p>

  ## Step 3: Join the Client VM to the Domain
  
- RDP into the **Client** virtual machine using the local admin account (e.g., `Bulma`).
- Right-click **Start > System** and click **Rename this PC (advanced)**.
  - Click **Change** to join a domain.
  - Enter the domain: `capsulecorporation.com`
  - Enter domain credentials when prompted:  
     - Username: `capsulecorporation.com\vegeta_admin`  
     - Password: `Sayonara89!`
- Restart the computer when prompted.
- RDP into the **Domain Controller** VM.
- Open **Active Directory Users and Computers (ADUC)** and verify the **Client** VM appears under **Computers**.
- Create a new Organizational Unit (OU) named `_CLIENTS`.
- Drag the **Client** VM from **Computers** into the `_CLIENTS` OU.

</p>
<br />


<h2>Setting Up Remote Desktop and User Creation</h2>

<p>
<img src="https://i.imgur.com/ARcqAlZ.png" height="80%" width="80%" alt="Remote Desktop Settings"/>
</p>
<p>

  ## Step 1: Enable Remote Desktop Access for Domain Users on the Client VM
  
- Log into the **Client** using the Domain Admin account:  
   `capsulecorporation.com\vegeta_admin`
- Open the **System Properties**:
   - Right-click on **Start**, select **System**.
   - Click on **Remote Desktop** in the left menu.
   - Click **Select users that can remotely access this PC**.
- In the Remote Desktop Users window:
   - Click **Add...**
   - Enter `Domain Users` and click **Check Names**.
   - Click **OK** to confirm.

You can now log in to the **Client** VM using standard (non-admin) domain user accounts via Remote Desktop.

> 💡 **Note:** In production, this would normally be handled using **Group Policy** to apply the setting across multiple systems at once.

</p>
<br />

<p>
<img src="https://i.imgur.com/WAkG8V0.png" height="80%" width="80%" alt="ADUC"/>
</p>
<p>

  ## Step 2: Create Additional Test Users in Active Directory
  
- RDP into the **Domain Controller** and log in as:  
   `capsulecorporation.com\vegeta_admin`
- Open **Active Directory Users and Computers (ADUC)**.
  - Navigate to the `_EMPLOYEES` Organizational Unit (OU).
  - Right-click the OU and select **New > User**.
  - Create the following users:

   ### User 1
   - **Full name:** Son Goku  
   - **Username:** `goku`
   - **Password:** `Dragonball84!`

   ### User 2
   - **Full name:** Master Roshi  
   - **Username:** `roshi`  
   - **Password:** `TurtleSage84!`

   ### User 3
   - **Full name:** Son Gohan  
   - **Username:** `gohan`  
   - **Password:** `Kakarrot88!`

- Ensure that **Password never expires** and **User must change password at next logon** are unchecked for simplicity.
- Click **Finish** after each user is created.

</p>
<br />

<p>
<img src="https://i.imgur.com/poBSIag.png" height="80%" width="80%" alt="Successful Login"/>
</p>
<p>

  ## Step 3: Test Login with a Standard Domain User
  
- On a separate system or using a Remote Desktop client, attempt to log in to **Client-1** as one of the newly created users. For example:
   - Username: `capsulecorporation.com\goku`
   - Password: `Dragonball84!`
- Verify that login is successful and access is granted to the desktop environment.

</p>
<br />

<h2>Group Policy & Account Management</h2>

<p>
<img src="https://i.imgur.com/bNydYLB.png" height="80%" width="80%" alt="Screenshots of Various Points of Progress"/>
</p>
<p>

  ## Step 1: Configuring Account Lockout Threshold in Group Policy
  
## Log in to the Domain Controller

- Log in as:  
  `capsulecorporation.com\vegeta_admin`

## Open the Group Policy Management Console (GPMC)

- Click **Start** and search for `gpmc.msc`, then press **Enter**.

## Create or Edit a Group Policy Object (GPO)

- In the **Group Policy Management Console**, expand your domain.
- Navigate to **Group Policy Objects**.
- Right-click and choose:
  - **New** to create a *New GPO*, `Account Lockout Policy`

## Navigate to Account Lockout Policy Settings

- In the Group Policy Editor:
  - Expand **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Account Policies** > **Account Lockout Policy**

## Configure the Account Lockout Policy

- Edit the Properties of the following three settings:

  - **Account Lockout Duration**  
    - Set to: `30 minutes`

  - Confirm **Account Lockout Threshold** is set to `5 invalid logon attempts`

  - **Reset Account Lockout Counter After**  
    - Set to: `15 minutes`

> Ensure "Define this policy setting" is checked in each setting.

## Link the GPO to the Target OU

- In GPMC, right-click the desired **OU**, `_EMPLOYEES` and choose:
  - **Link an Existing GPO**
  - Select the "Account Lockout Policy" GPO

## Force Group Policy Update (Optional)

- In **Powershell**, run the following command: *gpupdate /force* 

</p>
<br />

<p>
<img src="https://i.imgur.com/UWHg36K.png" height="80%" width="80%" alt="Successful Login"/>
</p>
<p>

  ## Step 2: Testing the Account Lockout Policy
  
## Select a Domain User

- Use any existing user, such as:
  - Username: `capsulecorporation.com\roshi`

## Attempt to Log in with Invalid Password

- Try logging into the **Client** as `capsulecorporation.com\roshi` using the wrong password 6 times.
- Observe that the account becomes locked out.

## Unlock the Account from the Domain Controller

- Log into the **Domain Controller** as `capsulecorporation.com\vegeta_admin`:
- Open **Active Directory Users and Computers** (ADUC).
  - Locate `capsulecorporation.com\roshi=` under the **_EMPLOYEES** OU.
  - Right-click > **Properties** > **Account** tab.
  - Click **Unlock** account and reset the password (optional).

## Test Login Again

- On the **Client**, log in as `capsulecorporation.com\roshi` with the correct password.
- Confirm successful login.

</p>
<br />

<p>
<img src="https://i.imgur.com/uUy1ArT.png" height="80%" width="80%" alt="Failed Login in Windows APP"/>
</p>
<p>

  ## Step 3: Enabling and Disabling Accounts
  
## Disable the Account

- On the **Domain Controller**:
  - Open **ADUC**, locate `Master Roshi`, right-click > **Disable Account**.

## Attempt Login with Disabled Account
  
- On the **Client** VM, attempt to log in as `capsulecorporation.com\roshi`.
- Observe the error message indicating the account is disabled.

## Re-enable the Account

- Back in **ADUC** on the **Domain Controller**:
  - Right-click `Master Roshi` > **Enable Account**
  - Attempt to log in again with the correct password.

</p>
<br />


<p>

  ## 🔐 Important Considerations
  
- **Account Lockout Threshold**: Avoid setting it too low (1-2) to prevent accidental lockouts.
- **Account Lockout Duration**: Higher durations improve security but may inconvenience users.
- **Reset Lockout Counter**: Shorter reset times allow repeated attempts and may reduce security.

</p>
<br />
