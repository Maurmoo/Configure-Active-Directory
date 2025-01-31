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
![image](https://github.com/user-attachments/assets/808dc7e2-b446-49b7-8d05-d4379947b0f3)


1. **Create the DC-1 VM**:
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
![image](https://github.com/user-attachments/assets/a6cd88e8-3400-4268-8c37-d01c3daf31bc)



1. **Create the Client-1 VM**:
   - Launch a new VM with **Windows 10 (21H2)**.
   - **VM Name**: `Client-1`
   - **Username**: `labuser`
   - **Password**: `Cyberlab123!`
   - **Network Configuration**: Ensure Client-1 is in the same **Region** and **Virtual Network** as DC-1.

2. **DNS Configuration**:
   
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
![image](https://github.com/user-attachments/assets/bc169284-9419-49d7-bb56-4399b8e63d4a)

1. **Ping Test**:
   - After logging into **Client-1**, open **Command Prompt** or **PowerShell** and run:
   -  ping <DC-1 Private IP>
   - Ensure the ping is successful, confirming connectivity.
     
![image](https://github.com/user-attachments/assets/da70956d-7de2-4760-bcf0-bcc0938bd029)
2. **Check DNS Settings**:
   - On **Client-1**, open **PowerShell** and run:
   - ipconfig /all
   - Verify that the DNS server points to **DC-1’s private IP address**.

## Step 4: Install Active Directory Domain Services (AD DS) on DC-1

1. **Install AD DS**:
   - On **DC-1**, open **Server Manager** → **Add Roles and Features**.
   - Select **Active Directory Domain Services** (AD DS) and complete the installation.

2. **Promote DC-1 to Domain Controller**:
   - After AD DS installation, click the notification to promote DC-1 as a Domain Controller.
   - Create a new forest, for example: `mydomain.com`.
   - Set the **Domain Functional Level** to **Windows Server 2022**.
   - **Set Directory Services Restore Mode (DSRM) password** (e.g., `Cyberlab123!`).

## Step 5: Configure Remote Desktop Access for Domain Users

1. **Enable Remote Desktop on Client-1**:
   - Log into **Client-1** as **labuser**.
   - Open **System Properties** → **Remote Settings** → Select **Allow remote connections**.

2. **Remote Desktop for Domain Users**:
   - Log into **Client-1** as **mydomain.com\jane_admin**.
   - Ensure that domain users have permission to connect via RDP by adding them to the **Remote Desktop Users** group.

## Step 6: Create and Manage Domain Users

1. **Create Organizational Units (OUs)**:
   - On **DC-1**, open **Active Directory Users and Computers (ADUC)**.
   - Create two OUs: `_EMPLOYEES` and `_ADMINS`.

2. **Create Domain Admin User**:
   - Inside **ADUC**, create


