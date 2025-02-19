<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This walkthrough outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
- Configuring an account lockout policy in Active Directory using Group Policy
- Unlocking and Password Rest Accounts
- Enabling and Disabling Accounts
-  Observing Logs

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


<h2>Install Active Directory on DC-1</h2>
<br />

<h3>1. Login to DC-1 and install Active Directory Domain Services</h3>
<br />

![Screenshot 2025-02-18 162949](https://github.com/user-attachments/assets/2a787513-bbc0-4258-95c8-6b531a286657)
<p>Open Server Manager.</p>

![Screenshot 2025-02-18 162957](https://github.com/user-attachments/assets/b2d7ce81-6c30-463e-8ea2-c79ed3489adb)
<p>Click on Add roles and features.</p>

![Screenshot 2025-02-18 163103](https://github.com/user-attachments/assets/f012d2ba-d9bd-4a0f-a51a-f0e162ebfeda)
![Screenshot 2025-02-18 163120](https://github.com/user-attachments/assets/6c1b3dad-bfe8-4c30-974d-08722fc23fb1)
<p>In the Role-based or feature-based installation window, select Active Directory Domain Services.</p>

![Screenshot 2025-02-18 163146](https://github.com/user-attachments/assets/5077cd9e-5547-4f78-a21a-07c284d3fbc9)

<p>Follow the prompts to install it.</p>
<br />

<h3>2. Promote DC-1 as a Domain Controller (DC)</h3>
<p><b>- Promote to Domain Controller:</b></p>
<br />

![Screenshot 2025-02-18 163508](https://github.com/user-attachments/assets/ef031d32-088e-4080-b12f-b12c44c752dd)
<p>After installation, in Server Manager, click the notification flag and select Promote this server to a domain controller.</p>

![Screenshot 2025-02-18 163655](https://github.com/user-attachments/assets/3da5ba08-b8d1-4f57-be5e-67281a5e6c66)
<p>Choose Add a new forest and enter mydomain.com as the root domain (remember this domain name for later steps).</p>

![Screenshot 2025-02-18 163849](https://github.com/user-attachments/assets/c66d35f8-4c42-4eff-b784-6f31e94a2a45)
<p>Complete the AD Domain Service Configuration Wizard</p>

![Screenshot 2025-02-18 164051](https://github.com/user-attachments/assets/16c015ec-c3f7-43bf-a977-282c0803835e)

<p>Allow the server to restart and then log back into DC-1 as user: mydomain.com\labuser</p>
<p>Then log back in to DC-1 using the credentials: mydomain.com\labuser.</p>
<br />

<h3>3. Create Domain Admin User</h3>
<br />

![Screenshot 2025-02-18 164835](https://github.com/user-attachments/assets/e520ec0a-14bd-40eb-b704-f3e18430e180)
<p><b>Open Active Directory Users and Computers (ADUC):</b></p>
<p>- Press Windows + R, type dsa.msc, and press Enter to open ADUC or click start and search "Active Directory Users and Computers"</p>

![Screenshot 2025-02-18 165019](https://github.com/user-attachments/assets/81f2f444-5adc-4b76-b64c-d9a594e7dc5b)
![Screenshot 2025-02-18 165057](https://github.com/user-attachments/assets/0edea339-a3a6-4a6a-985f-fd7fca061e4e)
<p><b>Create an Organizational Unit (OU) for Employees:</b></p>
<p>- In ADUC, right-click on the domain (e.g., mydomain.com), and choose New > Organizational Unit.</p>
<p>In Active Directory Users and Computers (ADUC), create an Organizational Unit
 (OU) called “_EMPLOYEES”</p>
<br />

![Screenshot 2025-02-18 165121](https://github.com/user-attachments/assets/b80956a7-d5bd-4d08-b253-5787331f4d0f)
![Screenshot 2025-02-18 165223](https://github.com/user-attachments/assets/c61ff929-3eed-4181-ad0e-5571bc693014)

<p><b>Create an Organizational Unit (OU) for Admins:</b></p>
<p>Right-click on the domain again, and choose New > Organizational Unit.</p>
<p>Create a new OU named “_ADMINS”</p>
<br />

![Screenshot 2025-02-18 165306](https://github.com/user-attachments/assets/d8986bbb-33b7-4909-8a6c-ef078cb59af5)
![Screenshot 2025-02-18 165523](https://github.com/user-attachments/assets/8495e83e-cda9-42c4-9b4f-71ea7b0f95af)
![Screenshot 2025-02-18 165644](https://github.com/user-attachments/assets/07a587bf-9088-4772-888d-cbd233c6b5f3)

<p><b>Create a new employee named “Jane Doe” with the username
 of “jane_admin”</b></p>
<br />

![Screenshot 2025-02-18 165749](https://github.com/user-attachments/assets/d4acfe81-18a5-4ddb-ac6c-218c80ad7adf)

<p>Add jane_admin to the “Domain Admins” Security Group</p>
<br />

![Screenshot 2025-02-18 165926](https://github.com/user-attachments/assets/9efce4ba-24ed-46c2-9d7c-457da4e41b49)

<p><b>Log out / close the connection to DC-1 and log back in as
 “mydomain.com\jane_admin”</b></p>
<p>User jane_admin as the admin account from now on</p>
<br />

<h3>4. Join "Client-1" to the Domain Server (mydomain.com)</h3>
<br />

![Screenshot 2025-02-18 170529](https://github.com/user-attachments/assets/6d863162-30e4-4c4e-832e-4943bb7c3095)
![Screenshot 2025-02-18 170644](https://github.com/user-attachments/assets/33fd511e-123f-4a5f-9868-3cc472c69074)

<p>Login to Client-1 as the original local admin (labuser) and join it to the domain
 (computer will restart)</p>
<p>Then verify Client-1 shows up in ADUC</p>
<br />

![Screenshot 2025-02-18 171125](https://github.com/user-attachments/assets/639b8615-1a47-4443-8802-b31e6279c0e1)
![Screenshot 2025-02-18 171235](https://github.com/user-attachments/assets/b9fc8da2-f659-43fc-bc6a-045f13ba70da)

<p>Create a new OU named “_CLIENTS” and drag Client-1 into there (same steps as how "_EMPLOYEES" and "_ADMINS" group were created.</p>
<br />

<h2>Set Up Remote Desktop for Non-Administrative Users on Client-1</h2>
<br />

<h3>1. Log into Client-1 as mydomain.com\jane_admin<h3>

![Screenshot 2025-02-18 180520](https://github.com/user-attachments/assets/a6734aea-0ffa-4b7b-af5e-9b37e34a413a)
![Screenshot 2025-02-18 180659](https://github.com/user-attachments/assets/9253762b-a2d7-4d51-96c8-1a54050a37b8)

<h3>2. Open system properties</h3>
<p>- Go to "settings" then click to "Remote Desktop"</p>
<br />

![Screenshot 2025-02-18 180840](https://github.com/user-attachments/assets/b7e06eab-353f-4d5d-84a3-19ef13bb9fe0)

<h3>3. Allow Domain Users Access to Remote Desktop:</h3>
<p>- In the Remote Desktop Users window, click Add.</p>
<p>- Type Domain Users and click OK to allow all domain users to access Remote Desktop.</p>
<br />

<h2>Create Multiple Users via PowerShell on DC-1</h2>
<h3>1. Login to DC-1 as jane_admin</h3>
<p>- Confirm that the user can successfully log in via Remote Desktop.</p>
<br />

![Screenshot 2025-02-18 181940](https://github.com/user-attachments/assets/b04dbeaa-ad29-4ce7-a6c8-93489f0a838e)

<p>- Open PowerShell_ise as an administrator</p>
<br />

![Screenshot 2025-02-18 182708](https://github.com/user-attachments/assets/7882f3f7-b519-4f4e-9d88-a0b4ef30ac10)

<p>Create a new File and paste the contents of the <a href=https://github.com/mark-afortadera/AD_PS/blob/master/Generate-Names-Create-Users.ps1>script</a> into it</p>
<br />

![Screenshot 2025-02-18 185259](https://github.com/user-attachments/assets/b2dfaff3-15e5-4851-bbd8-96a20a8a1c4e)

<p>Run the script and observe the accounts being created</p>
<br />

![Screenshot 2025-02-18 185337](https://github.com/user-attachments/assets/52e31039-cb44-4602-8ceb-82bdfa8185c7)

<p>When finished, open ADUC and observe the accounts in the appropriate OU　
(_EMPLOYEES)</p>
<br />

![Screenshot 2025-02-18 185524](https://github.com/user-attachments/assets/62741149-4338-4740-969f-57a330a1b1e1)

<p>attempt to log into Client-1 with one of the accounts (take note of the password in
 the script)</p>
<br />

![Screenshot 2025-02-18 185706](https://github.com/user-attachments/assets/63a2fe9b-2540-452c-b02f-36238f34ed23)
![Screenshot 2025-02-18 185804](https://github.com/user-attachments/assets/b60b7f13-7932-4f4f-a7f8-e1fca11df189)
![Screenshot 2025-02-18 190022](https://github.com/user-attachments/assets/253c6455-a0c9-412d-b340-e693634121ed)

<p>Client-1 is now able to log in as "menu.suq" or non-administrative user now</p>
<br />

<h2>Dealing with Account Lockouts</h2>
<br />

![Screenshot 2025-02-19 155921](https://github.com/user-attachments/assets/95faaf00-270b-40c3-987a-a27d8bac98e8)

<h3>1. Open the Group Policy Management Console (GPMC)</h3>
<p>Log in to a machine with Group Policy Management Console installed (typically, a Domain Controller)</p>
<p>Click Start, and type gpmc.msc in the search box, then press Enter. This opens the Group Policy Management Console.</p>
<br />

![Screenshot 2025-02-19 160341](https://github.com/user-attachments/assets/fb6086e5-f1b0-4576-bc41-1901ddf86736)

<h3>2. Create or Edit a Group Policy Object (GPO)</h3>
<p>In the GPMC, navigate to the Group Policy Objects section.</p>
<p>Right-click Group Policy Objects and select New to create a new GPO, or right-click an existing GPO and select Edit to modify it.</p>
<p>- Give the new GPO a descriptive name if you're creating a new one, like "Account Lockout Policy"</p>
<br />

![Screenshot 2025-02-19 160537](https://github.com/user-attachments/assets/36d13b28-79fb-4e2c-bc3a-e026efbacfe0)

<h3>3. Navigate to the Account Lockout Policy Settings</h3>
<p>In the Group Policy Management Editor, expand the following:</p>
<p>- Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.</p>
<br />


![Screenshot 2025-02-19 161003](https://github.com/user-attachments/assets/0a3300e4-1b02-4755-b607-e29917816f38)
![Screenshot 2025-02-19 161238](https://github.com/user-attachments/assets/fa47eb71-ad4b-47a5-bb14-68d4282351d2)


<h3>4. Configure Account Lockout Policy Settings</h3>
<p><b>Account Lockout Duration:</b></p>
<p>- Definition: The time in minutes that an account remains locked before it is automatically unlocked.</p>
<p>- Configuration: Double-click on this setting, select Define this policy setting, and then set the duration (e.g., 30 minutes).</p>
<p><b>Account Lockout Threshold:</b></p>
<p>- Definition: The number of failed logon attempts that will trigger an account lockout.</p>
<p>- Configuration: Double-click on this setting, select Define this policy setting, and then set the threshold (e.g., 3 invalid attempts).</p>
<p><b>Reset Account Lockout Counter After:</b></p>
<p>- Definition: The time in minutes after which the failed logon attempts counter is reset to 0, assuming there are no additional failed logon attempts.</p>
<p>- Configuration: Double-click on this setting, select Define this policy setting, and then set the time (e.g., 15 minutes).</p>
<br />

![Screenshot 2025-02-19 162110](https://github.com/user-attachments/assets/10ee204d-e6b3-4f49-8a4e-617f222bafda)
![Screenshot 2025-02-19 162429](https://github.com/user-attachments/assets/dd25b82c-d0e6-42f6-a789-ab06e1341679)

<h3>5. Update Group Policy</h3>
<p>You can wait for the Group Policy to propagate automatically, or you can force an update immediately.</p>
<p>- On a client machine or server, open Command Prompt and type gpupdate /force, then press Enter.</p>
<br />


![Screenshot 2025-02-19 163448](https://github.com/user-attachments/assets/f40ce7a2-52ea-48f9-b292-f6a98d1b107f)

<h3>6. Verify the Policy</h3>
<p>To verify the policy, you can use the rsop.msc (Resultant Set of Policy) tool on a client machine to see the applied settings.</p>
<p>Alternatively, you can also check the settings using the Group Policy Management Console.</p>
<br />


<h3>Important Considerations</h3>
<p><b>Account Lockout Threshold:</b> Setting this too low (e.g., 1 or 2 attempts) can lead to unnecessary lockouts.</p>
<p><b>Account Lockout Duration:</b>Setting this too high can be inconvenient for users but increases security.</p>
<p><b>Reset Account Lockout Counter After:</b>Setting this too short could allow attackers to repeatedly attempt to log in without triggering a lockout.</p>
<br />
<p>- By following these steps, you can successfully configure an account lockout policy in Active Directory using Group Policy.
</p>
<br />

<h3>Unlocking and Password Reset Accounts</h3>
<br />


![Screenshot 2025-02-19 163756](https://github.com/user-attachments/assets/291aa6d2-58af-459d-8b4f-bc3da0cd05cd)

<p>Attempt to log in with it 6 times with a bad password</p>
<p>Observe that the account has been locked out within Active Directory</p>
<br />


![Screenshot 2025-02-19 164050](https://github.com/user-attachments/assets/3051a90c-d875-4f22-9874-9fe65a0e5f11)

<p>Unlock the account</p>
<br />

![Screenshot 2025-02-19 164215](https://github.com/user-attachments/assets/ea5edfd5-b916-4ffd-8b06-506b99e4f31f)

<p>Reset the password</p>
<br />

![Screenshot 2025-02-19 164400](https://github.com/user-attachments/assets/623350d1-9a38-4c16-8ef1-5b5086818ce1)
![Screenshot 2025-02-19 164627](https://github.com/user-attachments/assets/3fb1a28f-a11e-43ca-8dd7-ffae6f68ef8e)

<p>Attempt to login with it</p>
<br />

<h3> Enabling and Disabling Accounts</h3>
<br />

![Screenshot 2025-02-19 165202](https://github.com/user-attachments/assets/2e169116-ce99-45dc-9793-942abe04b587)

<p>Disable the same account in Active Directory</p>
<br />

![Screenshot 2025-02-19 165330](https://github.com/user-attachments/assets/71d7a2d7-a185-41a2-81d4-4bd4df8a576b)

<p>Attempt to login with it, observe the error message</p>
<br />

![Screenshot 2025-02-19 165351](https://github.com/user-attachments/assets/136dae3f-dc26-4cdb-8be7-82e1af54339a)

<p>Re-enable the account and attempt to login with it.</p>
<br />

<h3>Observing Logs</h3>
<br />

![Screenshot 2025-02-19 165738](https://github.com/user-attachments/assets/d2c79441-a783-4e70-9498-7fbb64de1cce)

<p>Observe the logs in the Domain Controller</p>
<br />

![Screenshot 2025-02-19 170324](https://github.com/user-attachments/assets/887f48ae-eda0-415e-8639-3cd1d8affe47)

<p>In the Event Viewer, expand the following:</p>
<p>Event Viewer > Windows Logs > Security > right click "find" > input the name of the user</p>
<p>Observe the logs on the client Machine</p>
<br />

