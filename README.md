<p align="center">
    <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# On-premises Active Directory Deployed in the Cloud (Azure)

This tutorial guides you through deploying an on-premises Active Directory on Azure Virtual Machines, configuring a Domain Controller (DC), and setting up remote desktop access for domain users.

## Environments and Technologies Used
- **Microsoft Azure** (Virtual Machines/Compute)
- **Remote Desktop Protocol** (RDP)
- **Active Directory Domain Services** (AD DS)
- **PowerShell**

## Operating Systems Used
- **Windows Server 2022** (for the Domain Controller)
- **Windows 10** (21H2) (for Client-1)

## High-Level Deployment and Configuration Steps
1. Set up the Domain Controller (DC-1) in Azure
2. Set up Client-1 in Azure
3. Test connectivity between DC-1 and Client-1
4. Install Active Directory Domain Services (AD DS) on DC-1
5. Configure Remote Desktop access for domain users
6. Create and manage domain users

## Detailed Deployment and Configuration Steps

## Step 1: Set Up the Domain Controller (DC-1) in Azure

## Create the DC-1 VM:
   
   ![image](https://github.com/user-attachments/assets/808dc7e2-b446-49b7-8d05-d4379947b0f3)
   - Launch a new Virtual Machine (VM) with **Windows Server 2022**.
   - **VM Name**: `DC-1`
   - **Username**: `labuser`
   - **Password**: `Cyberlab123!`
   - **Network**: Attach to the same Virtual Network as Client-1, and configure the private IP as **static**.
     ![image](https://github.com/user-attachments/assets/0db6d2a8-cb4a-4f3b-b47e-6dc61f981ba6)

       - Go to the Azure Portal: https://portal.azure.com.
       - In the left sidebar, click on Virtual Machines.
       - Select the DC-1 VM.
       - Under Settings, click on Networking.
       - Find the Network Interface name (it usually starts with "dc-1-nic").
       - Click on the Network Interface name to open its settings.
       - Under Settings, click on IP Configurations.
       - In the same IP Configurations section:
       - Click on the existing IPv4 address (it should say something like "Dynamic").
       - In the new window:
       - Change the Assignment from Dynamic to Static.
       - Click Save to apply the changes.
   - **Firewall**: Temporarily disable for testing connectivity (you can enable it later).
   
  ![image](https://github.com/user-attachments/assets/a5cec0af-9f3d-4f23-945a-bcee9faef380)

   - Log into DC-1 (Remote Desktop or Azure VM Console).
   - Open PowerShell as Administrator.
   - Run the following command to disable the firewall for all network profiles
       - `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`
   - To verify that the firewall is disabled, run
       - `Get-NetFirewallProfile`
   - The Enabled status should now be False.



## Step 2: Set Up Client-1 in Azure

## Create the Client-1 VM:

  ![image](https://github.com/user-attachments/assets/a6cd88e8-3400-4268-8c37-d01c3daf31bc)
  
   - Launch a new VM with **Windows 10 (21H2)**.
   - **VM Name**: `Client-1`
   - **Username**: `labuser`
   - **Password**: `Cyberlab123!`
   - **Network Configuration**: Ensure Client-1 is in the same **Region** and **Virtual Network** as DC-1.

## DNS Configuration:
   
![image](https://github.com/user-attachments/assets/1fc0d551-c85f-466e-ad40-35e543831f84)


   - After the VM is created, go to the **Networking** section for Client-1.
   - Set the **DNS Servers** to DC-1's **Private IP address**.
       - Go to the Azure Portal: https://portal.azure.com.
       - In the left sidebar, click Virtual Machines.
       - Select Client-1 from the list of VMs.
       - In the Settings section, click Networking.
       - Click the Network Interface name (e.g., client-1-nic).
       - Under Settings, select DNS Servers.
       - Choose Custom.
       - Enter DC-1’s Private IP Address (e.g., 10.0.0.4).
       - Click Save.
       - Restart Client-1 from the Azure portal 
         


## Step 3: Test Connectivity Between DC-1 and Client-1

## Ping Test:
   
   ![image](https://github.com/user-attachments/assets/bc169284-9419-49d7-bb56-4399b8e63d4a)
   - After logging into **Client-1**, open **Command Prompt** or **PowerShell** and run:
   -  ping <DC-1 Private IP>
   - Ensure the ping is successful, confirming connectivity.
     
## Check DNS Settings:
   
  ![image](https://github.com/user-attachments/assets/fcb7069b-45f9-4488-aaf6-3ed4aa812bb3)


   - On **Client-1**, open **PowerShell** and run:
   - ipconfig /all
   - Verify that the DNS server points to **DC-1’s private IP address**.

## Step 4: Install Active Directory Domain Services (AD DS) on DC-1

## Install AD DS
   
![image](https://github.com/user-attachments/assets/5eb2b38d-a9c2-4cdb-a0fb-d9190cd8a41a)

   - On **DC-1**, open **Server Manager** / **Add Roles and Features**.
   - Click Next on the **Before You Begin**
   - Select **Role-based or feature-based installation** and click Next.
   - Select **DC-1 (local server) as the destination server**, then click Next.
   - Server Roles list, scroll down and check **Active Directory Domain Services** (AD DS).
   - A new window will pop up click **Add Features**, then click Next.
   - Keep the default selections and click Next.
   - AD DS Overview page, click Next.
   - Install
   

## Promote DC-1 to Domain Controller

![image](https://github.com/user-attachments/assets/5b5d8c05-c139-4a7b-b4f9-1bf93f2066ce)

   - After AD DS installation, click the notification to promote DC-1 as a Domain Controller.
   - Add a new forest, for example: `mydomain.com`.
   - Set the **Domain Functional Level** to **Windows Server 2016**.
   - **Set Directory Services Restore Mode (DSRM) password** (e.g., `Cyberlab123!`).
   - Click Next through the DNS options and NetBIOS name settings.
   - Review the Paths for AD DS Database, Log Files, and SYSVOL (default locations are fine).
   - Click Next to review the settings.
   - Click Install to begin the promotion process.
   - Log into DC-1 as `mydomain.com\labuser`.

## Step 5: Create a Domain Admin User
## Open Active Directory Users and Computers (ADUC)

![image](https://github.com/user-attachments/assets/85f5ddb2-2ad0-415a-8a59-7b9fe7db0329)


   - Press `Win + R`, type `dsa.msc`, and press Enter.
     
## Create Organizational Units (OUs):

![image](https://github.com/user-attachments/assets/ff1b5472-9d1e-4a75-8269-30efc3b8c081)


   - Right-click the domain (`mydomain.com`) > New > Organizational Unit.
   - Name it `_EMPLOYEES.`
   - Repeat the steps to create another OU called `_ADMINS`.
     
## Create a new user (Jane Doe):

![image](https://github.com/user-attachments/assets/e6cba3c4-46ac-4c3d-8493-9d50e75c1955)


   - In _ADMINS, right-click > New > User.
   - First Name: `Jane`, Last Name: `Doe`, Username: `jane_admin`.
   - Set a password: `Cyberlab123!`.
   - Check Password never expires.
     
## Add `jane_admin` to the Domain Admins Group:

![image](https://github.com/user-attachments/assets/8566f0ea-8ac6-4a82-abeb-63e12d91b077)

   - Open Jane Doe's properties > Member Of tab.
   - Click Add > Type `Domain Admins` > Click OK.
   - Log out of DC-1 and log back in as **mydomain.com\jane_admin**.
   - Use **jane_admin** for all future administrative tasks.

## Step 6: Join Client-1 to the Domain
## Open System Properties:

![image](https://github.com/user-attachments/assets/c895e7dc-bc45-46ff-9d1d-7ca8fa49160e)


- Log into Client-1 using the local admin account labuser.
- Press Win + R, type sysdm.cpl, and press Enter.
- Go to the Computer Name tab > Click Change.
- Select Domain, enter mydomain.com, and click OK.

## When prompted, enter domain credentials:

![image](https://github.com/user-attachments/assets/71d19f97-4bb8-4b1f-9a3d-adea6bae36c7)


- Username: `mydomain\jane_admin`
- Password: `Cyberlab123!`
- Restart Client-1.

## Verify Client-1 in ADUC:

![image](https://github.com/user-attachments/assets/7cee7b4c-b534-46a1-80dd-940948233b59)


- Log into DC-1.
- Open Active Directory Users and Computers (ADUC).
- Client-1 should appear under Computers.

## Move Client-1 to the _CLIENTS OU:

![image](https://github.com/user-attachments/assets/e70de060-8069-4950-a2d6-5dc59ccec351)

- Right click mydomain.com / new / Organizational unit / _Client
- In ADUC, right-click Computers > Move > Select _CLIENTS.

## Step 7: Configure Remote Desktop Access for Domain Users
## Open System Properties:

![image](https://github.com/user-attachments/assets/e7dc4bc4-254e-4893-87ae-0b497deada9e)


- Log into Client-1 as mydomain.com\jane_admin.
- Press Win + R, type sysdm.cpl, and press Enter.
- Click the Remote tab.
- Under Remote Desktop, enable Allow remote connections to this computer.
- Click Select Users... > Add mydomain\domain users.
- Apply changes and restart Client-1.

## Step 8: Create Multiple Users with PowerShell



- Open PowerShell ISE on DC-1
- Log into DC-1 as mydomain.com\jane_admin.
- Press Win + R, type PowerShell_ISE, and press Enter.
- Right-click PowerShell ISE and select Run as Administrator.

## Load the Active Directory Module



- Run the following command in PowerShell: `Import-Module ActiveDirectory`

## Define the Users You Want to Create



- In PowerShell run:
- `  $users = @(
    @{FirstName="John"; LastName="Doe"; UserName="john.doe"},
    @{FirstName="Alice"; LastName="Smith"; UserName="alice.smith"},
    @{FirstName="Bob"; LastName="Jones"; UserName="bob.jones"}   `

## Create Each User in Active Directory



- In PowerShell run:
`  Foreach ($user in $users) {
    $Password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
    New-ADUser -GivenName $user.FirstName -Surname $user.LastName -Name "$($user.FirstName) $($user.LastName)" 
    -SamAccountName $user.UserName -UserPrincipalName "$($user.UserName)@mydomain.com" -Path "OU=_EMPLOYEES,DC=mydomain,DC=com" 
    -AccountPassword $Password -Enabled $true -ChangePasswordAtLogon $false   `

## Verify the Users in ADUC



- Press Win + R, type `dsa.msc`, and hit Enter.
- Expand mydomain.com / Open the _EMPLOYEES OU.
- Verify that John Doe, Alice Smith, and Bob Jones were created.

## Test Logging in on Client-1



- Go to Client-1 and log out.
- Try logging in as:
- Username: `mydomain.com\john.doe`
- Password: `Password123!`
