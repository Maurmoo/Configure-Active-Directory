# On-premises Active Directory Deployed in the Cloud (Azure)

This tutorial walks you through deploying on-premises Active Directory on Azure Virtual Machines, configuring a Domain Controller, and setting up remote desktop access for domain users.

## Environments and Technologies Used
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services (AD DS)
- PowerShell

## Operating Systems Used
- Windows Server 2022
- Windows 10 (21H2)

## High-Level Deployment and Configuration Steps
1. Set up the Domain Controller in Azure
2. Set up Client-1 in Azure
3. Test Connectivity Between DC-1 and Client-1
4. Install Active Directory Domain Services (AD DS)
5. Configure Remote Desktop Access for Domain Users

---

## Deployment and Configuration Steps

### Step 1: Set Up the Domain Controller in Azure
- **Virtual Machine:** Windows Server 2022 for the Domain Controller (DC-1)
  - **Name:** DC-1
  - **Username:** labuser
  - **Password:** Cyberlab123!
  - **Static Private IP:** Configured
  - **Firewall:** Disabled (for testing connectivity)

### Step 2: Set Up Client-1 in Azure
- **Virtual Machine:** Windows 10 (Client-1)
  - **Name:** Client-1
  - **Username:** labuser
  - **Password:** Cyberlab123!
  - **DNS Settings:** Set to DC-1's private IP address
  - **Network:** Attached to the same region and Virtual Network as DC-1

### Step 3: Test Connectivity Between DC-1 and Client-1
- **Restart Client-1** from the Azure portal and log in.
- **Verify Connectivity:** Use PowerShell on Client-1 to run the `ipconfig /all` command and check if the DNS points to DC-1's private IP address.
- **Ping DC-1's IP:** Ensure that Client-1 can reach DC-1 by pinging its private IP address.

---

## Azure Active Directory Lab Setup
This lab covers setting up Active Directory on the Domain Controller, creating organizational units, joining a client machine to the domain, and configuring remote desktop access for non-admin users.

### Part 1: Install Active Directory on DC-1
#### Step 1: Power on DC-1 and Client-1 VMs
- **Action:** Ensure both DC-1 and Client-1 VMs are powered on in the Azure portal.

#### Step 2: Install Active Directory Domain Services (AD DS) on DC-1
- **Action:** Log into DC-1 and install Active Directory Domain Services (AD DS).
- **Promotion to Domain Controller:** Promote DC-1 as a Domain Controller by setting up a new forest with a domain name (e.g., `mydomain.com`).
- **Action:** After promotion, restart DC-1 and log back in using the credentials: `mydomain.com\labuser`.

#### Step 3: Create a Domain Admin User
- **Action:** Open **Active Directory Users and Computers (ADUC)**.
- **Create Organizational Units (OUs):**
  - `_EMPLOYEES`
  - `_ADMINS`
- **Action:** Create a new domain admin user:
  - **User:** Jane Doe (username: `jane_admin`, password: `Cyberlab123!`)
  - **Add** `jane_admin` to the **Domain Admins Security Group**.
- **Action:** Log out and log back in as `mydomain.com\jane_admin`.

#### Step 4: Join Client-1 to Your Domain
- **Action:** Set Client-1's DNS settings to DC-1's private IP address (already done).
- **Action:** Restart Client-1 and log in as the local admin (`labuser`).
- **Action:** Join Client-1 to the domain `mydomain.com`.
  - The machine will restart during this process.
- **Action:** After restart, verify that Client-1 appears in **ADUC** under `_CLIENTS`.

#### Step 5: Finalize Lab 1
- **Action:** Do **not** delete the VMs in Azure—they will be used in future labs.
- **Optional:** To save costs, stop or turn off the VMs in the Azure portal.

---

### Part 2: Set Up Remote Desktop Access for Non-Admin Users
#### Step 1: Power on DC-1 and Client-1 VMs
- **Action:** Ensure both DC-1 and Client-1 VMs are powered on in the Azure portal.

#### Step 2: Configure Remote Desktop on Client-1 for Domain Users
- **Action:** Log into Client-1 as `mydomain.com\jane_admin`.
- **Action:** Open **System Properties** and enable **Remote Desktop** for domain users.

#### Step 3: Create Additional Domain Users
- **Action:** Log into DC-1 as `jane_admin`.
- **Action:** Use **PowerShell_ise** to create a script for adding multiple domain users.
- **Paste and run the script** to create new users in **ADUC** under the `_EMPLOYEES` OU.
- **Action:** Log into Client-1 using one of the newly created domain accounts.

#### Step 4: Verify User Creation in ADUC
- **Action:** After running the script, verify that the new users have been successfully created in **ADUC** under the `_EMPLOYEES` OU.

---

## Conclusion
By completing this lab, you have:
✅ Installed **Active Directory Domain Services** and promoted **DC-1** to a **Domain Controller**.
✅ Created domain users and organized them into `_EMPLOYEES` and `_ADMINS` OUs.
✅ Configured **Client-1** as a domain-joined machine.
✅ Set up **Remote Desktop** for non-administrative domain users.
✅ Created additional domain users and verified their creation in **ADUC**.

This lab has provided a solid foundation for managing users and machines in an **Active Directory environment**. Future labs will build on this infrastructure to enhance **network management capabilities**.

---

![Azure](https://img.shields.io/badge/Azure-Cloud-blue)
