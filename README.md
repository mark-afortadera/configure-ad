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
- Windows 10 Pro (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one
 of the users

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

![Screenshot 2025-02-18 162949](https://github.com/user-attachments/assets/2a787513-bbc0-4258-95c8-6b531a286657)
![Screenshot 2025-02-18 162957](https://github.com/user-attachments/assets/b2d7ce81-6c30-463e-8ea2-c79ed3489adb)
![Screenshot 2025-02-18 163103](https://github.com/user-attachments/assets/f012d2ba-d9bd-4a0f-a51a-f0e162ebfeda)
![Screenshot 2025-02-18 163120](https://github.com/user-attachments/assets/6c1b3dad-bfe8-4c30-974d-08722fc23fb1)
![Screenshot 2025-02-18 163146](https://github.com/user-attachments/assets/5077cd9e-5547-4f78-a21a-07c284d3fbc9)

<p>Login to DC-1 and install Active Directory Domain Services</p>
<br />

![Screenshot 2025-02-18 163508](https://github.com/user-attachments/assets/ef031d32-088e-4080-b12f-b12c44c752dd)
![Screenshot 2025-02-18 163655](https://github.com/user-attachments/assets/3da5ba08-b8d1-4f57-be5e-67281a5e6c66)
![Screenshot 2025-02-18 163849](https://github.com/user-attachments/assets/c66d35f8-4c42-4eff-b784-6f31e94a2a45)

<p>Promote as a DC: Setup a new forest as mydomain.com</p>
<br />

![Screenshot 2025-02-18 164051](https://github.com/user-attachments/assets/16c015ec-c3f7-43bf-a977-282c0803835e)
![Screenshot 2025-02-18 164335](https://github.com/user-attachments/assets/bdb65d54-7b2b-4e4c-b567-83d4f0db50b9)
![Screenshot 2025-02-18 164454](https://github.com/user-attachments/assets/4e9735bd-7525-4700-b3ab-6ad48390c5a6)

<p>Restart and then log back into DC-1 as user: mydomain.com\labuser</p>
<br />

![Screenshot 2025-02-18 164835](https://github.com/user-attachments/assets/e520ec0a-14bd-40eb-b704-f3e18430e180)
![Screenshot 2025-02-18 165019](https://github.com/user-attachments/assets/81f2f444-5adc-4b76-b64c-d9a594e7dc5b)
![Screenshot 2025-02-18 165057](https://github.com/user-attachments/assets/0edea339-a3a6-4a6a-985f-fd7fca061e4e)

<p>In Active Directory Users and Computers (ADUC), create an Organizational Unit
 (OU) called “_EMPLOYEES”</p>
<br />


![Screenshot 2025-02-18 165121](https://github.com/user-attachments/assets/b80956a7-d5bd-4d08-b253-5787331f4d0f)
![Screenshot 2025-02-18 165223](https://github.com/user-attachments/assets/c61ff929-3eed-4181-ad0e-5571bc693014)

<p>Create a new OU named “_ADMINS”</p>
<br />

![Screenshot 2025-02-18 165306](https://github.com/user-attachments/assets/d8986bbb-33b7-4909-8a6c-ef078cb59af5)
![Screenshot 2025-02-18 165523](https://github.com/user-attachments/assets/8495e83e-cda9-42c4-9b4f-71ea7b0f95af)
![Screenshot 2025-02-18 165644](https://github.com/user-attachments/assets/07a587bf-9088-4772-888d-cbd233c6b5f3)

<p> Create a new employee named “Jane Doe” with the username
 of “jane_admin”</p>
<br />

![Screenshot 2025-02-18 165749](https://github.com/user-attachments/assets/d4acfe81-18a5-4ddb-ac6c-218c80ad7adf)

<p>Add jane_admin to the “Domain Admins” Security Group</p>
<br />

![Screenshot 2025-02-18 165926](https://github.com/user-attachments/assets/9efce4ba-24ed-46c2-9d7c-457da4e41b49)

<p>Log out / close the connection to DC-1 and log back in as
 “mydomain.com\jane_admin”</p>
<p>User jane_admin as your admin account from now on</p>
<br />

![Screenshot 2025-02-18 170529](https://github.com/user-attachments/assets/6d863162-30e4-4c4e-832e-4943bb7c3095)
![Screenshot 2025-02-18 170644](https://github.com/user-attachments/assets/33fd511e-123f-4a5f-9868-3cc472c69074)

<p>Login to Client-1 as the original local admin (labuser) and join it to the domain
 (computer will restart)</p>
<p>Login to the Domain Controller and verify Client-1 shows up in ADUC</p>
<br />

![Screenshot 2025-02-18 171125](https://github.com/user-attachments/assets/639b8615-1a47-4443-8802-b31e6279c0e1)
![Screenshot 2025-02-18 171235](https://github.com/user-attachments/assets/b9fc8da2-f659-43fc-bc6a-045f13ba70da)


<p>Create a new OU named “_CLIENTS” and drag Client-1 into there (same steps as how "_EMPLOYEES" and "_ADMINS" group were created.</p>
<br />


<p></p>
<br />



<p></p>
<br />


<p></p>
<br />



<p></p>
<br />



<p></p>
<br />



<p></p>
<br />



<p></p>
<br />



<p></p>
<br />
