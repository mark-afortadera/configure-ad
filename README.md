<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

![Screenshot 2025-02-18 153447](https://github.com/user-attachments/assets/f30d5175-d68d-4336-a34b-1efa32cc46b5)

<p>Create a Resource Group</p>
<br />

![Screenshot 2025-02-18 153433](https://github.com/user-attachments/assets/9c908c42-1ded-4f33-b2a0-1c182a19936f)

<p>Create a Virtual Network and Subnet</p>
<br />

![Screenshot 2025-02-18 153508](https://github.com/user-attachments/assets/12f298d1-30e9-4a8c-9454-0df0fffdee07)

<p>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</p>
<br />

![Screenshot 2025-02-18 153836](https://github.com/user-attachments/assets/91199271-677a-4748-82ee-01f66039d948)
![Screenshot 2025-02-18 153943](https://github.com/user-attachments/assets/36e344ae-3d96-4a4c-8e35-f68ebf04afd5)

<p>After VM is created, set Domain Controller’s NIC Private IP address to be static</p>
<br />

![Screenshot 2025-02-18 154259](https://github.com/user-attachments/assets/414cd021-aaec-4658-a0f0-3644188def68)
![Screenshot 2025-02-18 154328](https://github.com/user-attachments/assets/9e373287-3f16-4408-a01a-cd094709ff54)
![Screenshot 2025-02-18 154355](https://github.com/user-attachments/assets/c143dbac-ed9c-4cd0-84f4-7e3d0a3bfe96)
![Screenshot 2025-02-18 154409](https://github.com/user-attachments/assets/e1654f0a-9f40-4bd6-be70-372c48f5c6a5)
![Screenshot 2025-02-18 154421](https://github.com/user-attachments/assets/52d661af-828c-4116-821c-5e88628b8575)

<p>Log into the VM and disable the Windows Firewall (for testing connectivity)</p>
<br />

![Screenshot 2025-02-18 153516](https://github.com/user-attachments/assets/2e459b53-0d41-44c5-96bb-a32b6d8f991c)

<p>Create the Client VM (Windows 10) named “Client-1”</p>
<br />

![Screenshot 2025-02-18 154720](https://github.com/user-attachments/assets/193f4b44-a6b2-4a0e-b8f1-be8c80814709)

<p>Attach it to the same region and Virtual Network as DC-1</p>
<br />

![Screenshot 2025-02-18 154736](https://github.com/user-attachments/assets/7b7996cb-fca5-4824-8c21-d162d54ba3d3)
![Screenshot 2025-02-18 154907](https://github.com/user-attachments/assets/3be28015-ca0c-4e48-ac89-a355ca2e678f)

<p>After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address</p>
<br />

![Screenshot 2025-02-18 155023](https://github.com/user-attachments/assets/455750c0-154a-40e5-ad05-14469122a5c8)

<p>From the Azure Portal, restart Client-1</p>
<br />

![Screenshot 2025-02-18 155257](https://github.com/user-attachments/assets/8beb59bf-c964-4ee7-b4a7-3f660ec3e2dd)

<p>Login to Client-1</p>
<br />

![Screenshot 2025-02-18 155358](https://github.com/user-attachments/assets/290432c5-b5ad-4514-93c9-f03cc317b4f2)

<p>Attempt to ping DC-1’s private IP address</p>
<p>- Ensure the ping succeeded</p>
<br />

![Screenshot 2025-02-18 155527](https://github.com/user-attachments/assets/a1399e04-7b5e-48f4-afa5-048e60f809fc)

<p>From Client-1, open PowerShell and run ipconfig /all</p>
<p>- The output for the DNS settings should show DC-1’s private IP Address</p>
<br />
