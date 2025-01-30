<p align="center">
    <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
<p>This tutorial walks you through deploying on-premises Active Directory on Azure Virtual Machines, configuring a Domain Controller, and setting up remote desktop access for domain users.</p>


<h2>Environments and Technologies Used</h2>
<ul>
    <li>Microsoft Azure (Virtual Machines/Compute)</li>
    <li>Remote Desktop</li>
    <li>Active Directory Domain Services (AD DS)</li>
    <li>PowerShell</li>
</ul>

<h2>Operating Systems Used</h2>
<ul>
    <li>Windows Server 2022</li>
    <li>Windows 10 (21H2)</li>
</ul>

<h2>High-Level Deployment and Configuration Steps</h2>
<ul>
    <li>Set up the Domain Controller in Azure</li>
    <li>Set up Client-1 in Azure</li>
    <li>Test Connectivity Between DC-1 and Client-1</li>
    <li>Install Active Directory Domain Services (AD DS)</li>
    <li>Configure Remote Desktop Access for Domain Users</li>
</ul>

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Set Up the Domain Controller in Azure</h3>
<p>
    <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="DC Setup"/>
</p>
<ul>
    <li><b>Virtual Machine:</b> Windows Server 2022 for the Domain Controller (DC-1)</li>
    <li><b>Name:</b> DC-1</li>
    <li><b>Username:</b> labuser</li>
    <li><b>Password:</b> Cyberlab123!</li>
    <li><b>Static Private IP:</b> Configured</li>
    <li><b>Firewall:</b> Disabled (for testing connectivity)</li>
</ul>

<h3>Step 2: Set Up Client-1 in Azure</h3>
<p>
    <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client Setup"/>
</p>
<ul>
    <li><b>Virtual Machine:</b> Windows 10 (Client-1)</li>
    <li><b>Name:</b> Client-1</li>
    <li><b>Username:</b> labuser</li>
    <li><b>Password:</b> Cyberlab123!</li>
    <li><b>DNS Settings:</b> Set to DC-1's private IP address</li>
    <li><b>Network:</b> Attached to the same region and Virtual Network as DC-1</li>
</ul>

<h3>Step 3: Test Connectivity Between DC-1 and Client-1</h3>
<p>
    <img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Connectivity Test"/>
</p>
<ul>
    <li>Restart Client-1 from the Azure portal and log in.</li>
    <li>Use PowerShell on Client-1 to run <code>ipconfig /all</code> and check if the DNS points to DC-1's private IP address.</li>
    <li>Ping DC-1's private IP address to verify connectivity.</li>
</ul>

<h2>Azure Active Directory Lab Setup</h2>

<h3>Install Active Directory on DC-1</h3>
<ul>
    <li>Power on DC-1 and Client-1 VMs in Azure.</li>
    <li>Install Active Directory Domain Services (AD DS) on DC-1.</li>
    <li>Promote DC-1 as a Domain Controller and set up a new forest (e.g., mydomain.com).</li>
</ul>

<h3>Create a Domain Admin User</h3>
<ul>
    <li>Open Active Directory Users and Computers (ADUC).</li>
    <li>Create Organizational Units (OUs): _EMPLOYEES and _ADMINS.</li>
    <li>Create a new domain admin user: <b>Jane Doe</b> (username:
