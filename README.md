<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# On-premises Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

---

## Environments and Technologies Used

- **Microsoft Azure** (Virtual Machines/Compute)
- **Remote Desktop**
- **Active Directory Domain Services**
- **PowerShell**

---

## Operating Systems Used

- **Windows Server 2022**
- **Windows 10** (21H2)

---

## High-Level Deployment and Configuration Steps

1. **Setup Domain Controller in Azure**
2. **Setup Client-1 in Azure**
3. **Test Connectivity Between DC-1 and Client-1**
4. **Setup Active Directory (AD) and Domain Admins**
5. **Join Client-1 to the Domain**
6. **Configure Remote Desktop Access**

---

## Deployment and Configuration Steps

### Step 1: Setup Domain Controller in Azure  

![Domain Controller Setup](https://github.com/user-attachments/assets/placeholder-image)

- Created a **Windows Server 2022** Virtual Machine in Azure for the Domain Controller with the following specifications:
  - **VM Name:** **DC-1**
  - **Username:** **labuser**
  - **Password:** **Cyberlab123!**
  - **Static Private IP:** Configured
  - **Firewall:** Disabled (for testing connectivity)

---

### Step 2: Setup Client-1 in Azure  

![Client-1 Setup](https://github.com/user-attachments/assets/placeholder-image)

- Created a **Windows 10** Virtual Machine in Azure for Client-1 with the following specifications:
  - **VM Name:** **Client-1**
  - **Username:** **labuser**
  - **Password:** **Cyberlab123!**
  - **DNS Settings:** Set to **DC-1**'s Private IP address
  - **Region and Virtual Network:** Same as **DC-1**

---

### Step 3: Test Connectivity Between DC-1 and Client-1  

![Connectivity Test](https://github.com/user-attachments/assets/placeholder-image)

- Restarted **Client-1** from the Azure portal.
- Logged into **Client-1** and confirmed connectivity to **DC-1** using **ping** and **DNS settings**.
- Verified DNS resolution on **Client-1** using PowerShell command:  
  ```bash
  ipconfig /all


