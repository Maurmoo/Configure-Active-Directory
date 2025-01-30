<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

**Step 1:** Setup Domain Controller in Azure
**Step 2:** Setup Client-1 in Azure 
**Step 3:** Test Connectivity Between DC-1 and Client-1
Step 4
  
<h2>Deployment and Configuration Steps</h2>  

## Step 1: Setup Domain Controller in Azure  
![image](https://github.com/user-attachments/assets/placeholder-image)

- Created a Windows Server 2022 Virtual Machine in Azure for the Domain Controller, with the following specifications:
  - Name: **DC-1**
  - Username: **labuser**
  - Password: **Cyberlab123!**
  - Static Private IP: **Configured**
  - Disabled Windows Firewall (for testing connectivity)

---

## Step 2: Setup Client-1 in Azure  
![image](https://github.com/user-attachments/assets/placeholder-image)

- Created a Windows 10 Virtual Machine in Azure for Client-1, with the following specifications:
  - Name: **Client-1**
  - Username: **labuser**
  - Password: **Cyberlab123!**
  - Attached Client-1 to the same region and Virtual Network as **DC-1**
  - Set Client-1’s DNS settings to DC-1’s Private IP address

---

## Step 3: Test Connectivity Between DC-1 and Client-1  
![image](https://github.com/user-attachments/assets/placeholder-image)

- Restarted **Client-1** from the Azure portal.
- Logged into **Client-1**.
- Pinned **DC-1**’s private IP address from **Client-1** to ensure connectivity.
- Verified DNS settings in **Client-1** using PowerShell with <code>ipconfig /all</code>, confirming that DNS points to **DC-1**'s Private IP address.
