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

- Setting up Domain Controller in Azure
- Setting up Client-1 in Azure
- Installing Active Directory on DC-1
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)
- Setting up Remote Desktop for Non-Administrative users on Client-1
- Create Multiple Users via PowerShell on DC-1
- Configuring an Account lockout policy in Active Directory using Group Policy
- Unlocking and Password Rest Accounts
- Disabling and Re-enabling Accounts
- Observe Logs on Client Machine

<h2>Deployment and Configuration Steps:</h2>
<br />

<h2>Set Up Domain Controller (DC-1) in Azure</h2>
<br />

<h3>1. Create a Resource Group:</h3>
<br />

![Screenshot 2025-02-18 153447](https://github.com/user-attachments/assets/f30d5175-d68d-4336-a34b-1efa32cc46b5)

<p>- In the Azure Portal, click on Create a resource then select Resource Group.</p>
<p>- Enter a Resource Group name (e.g., Active Directory-Lab).</p>
<p>- Choose the Region where you want to deploy your resources</p>
<p>- Click Review + Create and then Create.</p>
<br />

<h3>2. Create a Virtual Network and Subnet:</h3>
<br />

![Screenshot 2025-02-18 153433](https://github.com/user-attachments/assets/9c908c42-1ded-4f33-b2a0-1c182a19936f)

<p>- In the Azure Portal, click Create a resource > Networking > Virtual Network.</p>
<p>- Set the Name (e.g., Active-Directory-VNet), Region, and Address Space (e.g., 10.0.0.0/16).</p>
<p>- Click Review + Create and then Create.</p>
<br />

<h3>3. Create the Domain Controller VM (Windows Server 2022):</h3>
<br />

![Screenshot 2025-02-18 153508](https://github.com/user-attachments/assets/12f298d1-30e9-4a8c-9454-0df0fffdee07)

<p>- In the Azure Portal, click Create a resource > Compute > Windows Server 2022.</p>
<p>- Create the Domain Controller VM (Windows Server 2022) named “DC-1”</p>
<p>- Choose the Size then set Region to match the Virtual Network created earlier.</p>
<p>- Set NIC to use Dynamic IP initially (will change to Static later).</p>
<p>- Click Review + Create and then Create.</p>
<br />

<h3>4. Set Domain Controller’s NIC Private IP to Static:</h3>
<br />

![Screenshot 2025-02-18 153836](https://github.com/user-attachments/assets/91199271-677a-4748-82ee-01f66039d948)
![Screenshot 2025-02-18 153943](https://github.com/user-attachments/assets/36e344ae-3d96-4a4c-8e35-f68ebf04afd5)

<p>- After VM creation, go to Network Interface > IP Configuration.</p>
<p>- Click on the Private IP address and set it to Static then Save the changes.</p>
<br />

<h3>5. Log into DC-1 and Disable Windows Firewall:</h3>
<br />

![Screenshot 2025-02-18 154328](https://github.com/user-attachments/assets/9e373287-3f16-4408-a01a-cd094709ff54)

<p>- Open Windows Defender Firewall and select Turn Windows Defender Firewall on or off.</p>
<br />

![Screenshot 2025-02-18 154355](https://github.com/user-attachments/assets/c143dbac-ed9c-4cd0-84f4-7e3d0a3bfe96)
![Screenshot 2025-02-18 154409](https://github.com/user-attachments/assets/e1654f0a-9f40-4bd6-be70-372c48f5c6a5)
![Screenshot 2025-02-18 154421](https://github.com/user-attachments/assets/52d661af-828c-4116-821c-5e88628b8575)

<p>- Select Turn off Windows Defender Firewall for both Private and Public networks (for testing purposes) then Click OK to save.</p>
<br />

<h2>Set Up Client-1 in Azure</h2>
<br />

<h3>1. Create the Client VM (Windows 10):</h3>
<br />

![Screenshot 2025-02-18 153516](https://github.com/user-attachments/assets/2e459b53-0d41-44c5-96bb-a32b6d8f991c)

<p>- In the Azure Portal, click Create a resource > Compute > Windows 10.</p>
<p>- Set the Name as Client-1</p>
<p>- Choose the Size and set Region to match the Virtual Network created earlier.</p>
<p>- Under Networking, select the same Virtual Network (Active-Directory-VNet) as DC-1.</p>
<p>- Click Review + Create and then Create.</p>
<br />

<h3>2. Set Client-1's DNS Settings to DC-1's Private IP:</h3>
<br />

![Screenshot 2025-02-18 154736](https://github.com/user-attachments/assets/7b7996cb-fca5-4824-8c21-d162d54ba3d3)
![Screenshot 2025-02-18 154907](https://github.com/user-attachments/assets/3be28015-ca0c-4e48-ac89-a355ca2e678f)

<p>- After VM creation, go to Networking > Network Interface then click on DNS servers and select Custom.</p>
<p>- Set Client-1’s DNS settings to DC-1’s Private IP address then save the changes.</p>
<br />

<h3>3. Restart Client-1 from the Azure Portal:</h3>
<br />

![Screenshot 2025-02-18 155023](https://github.com/user-attachments/assets/455750c0-154a-40e5-ad05-14469122a5c8)

<p>- Go to the Client-1 VM in the Azure Portal.</p>
<p>- Click Restart to reboot the VM.</p>
<br />

<h2>Test Connectivity Between Client-1 and DC-1</h2>
<br />

<h3>1. Log into Client-1:</h3>
<br />

![Screenshot 2025-02-18 155257](https://github.com/user-attachments/assets/8beb59bf-c964-4ee7-b4a7-3f660ec3e2dd)

<p>- Use Remote Desktop to log into Client-1</p>
<br />

![Screenshot 2025-02-18 155358](https://github.com/user-attachments/assets/290432c5-b5ad-4514-93c9-f03cc317b4f2)

<p>Attempt to ping DC-1’s private IP address</p>
<p>- Ensure the ping succeeded</p>
<br />

<h3>2. Ping DC-1’s Private IP Address:</h3>
<br />

![Screenshot 2025-02-18 155527](https://github.com/user-attachments/assets/a1399e04-7b5e-48f4-afa5-048e60f809fc)

<p>- From Client-1, open PowerShell and run ipconfig /all</p>
<p>- Ensure that the ping is successful and returns replies.</p>
<br />


<h2>Install Active Directory on DC-1</h2>
<br />

<h3>1. Login to DC-1 and install Active Directory Domain Services</h3>
<br />

![Screenshot 2025-02-18 162949](https://github.com/user-attachments/assets/2a787513-bbc0-4258-95c8-6b531a286657)

<p>- Use DC-1 credentials to log into the Domain Controller Server.</p>
<p>- Open Server Manager.</p>
<br />

<h3>2. Install Active Directory Domain Services (AD DS):</h3>
<br />

![Screenshot 2025-02-18 162957](https://github.com/user-attachments/assets/b2d7ce81-6c30-463e-8ea2-c79ed3489adb)
<p>Click on Add roles and features.</p>
<br />

![Screenshot 2025-02-18 163103](https://github.com/user-attachments/assets/f012d2ba-d9bd-4a0f-a51a-f0e162ebfeda)
![Screenshot 2025-02-18 163120](https://github.com/user-attachments/assets/6c1b3dad-bfe8-4c30-974d-08722fc23fb1)

<p>- In the Role-based or feature-based installation window, select Active Directory Domain Services.</p>
<br />

![Screenshot 2025-02-18 163146](https://github.com/user-attachments/assets/5077cd9e-5547-4f78-a21a-07c284d3fbc9)

<p>- Follow the prompts to install it.</p>
<br />

<h3>3. Promote DC-1 as a Domain Controller (DC):</h3>
<br />

![Screenshot 2025-02-18 163508](https://github.com/user-attachments/assets/ef031d32-088e-4080-b12f-b12c44c752dd)
<p>- After installation, in Server Manager, click the notification flag and select Promote this server to a domain controller.</p>
<br />

![Screenshot 2025-02-18 163655](https://github.com/user-attachments/assets/3da5ba08-b8d1-4f57-be5e-67281a5e6c66)
<p>- Choose Add a new forest and enter mydomain.com as the root domain (remember this domain name for later steps).</p>
<br />

![Screenshot 2025-02-18 163849](https://github.com/user-attachments/assets/c66d35f8-4c42-4eff-b784-6f31e94a2a45)
<p>- Complete the AD Domain Service Configuration Wizard</p>
<br />

![Screenshot 2025-02-18 164051](https://github.com/user-attachments/assets/16c015ec-c3f7-43bf-a977-282c0803835e)

<p>- Allow the server to restart and then log back into DC-1 as user: mydomain.com\labuser</p>
<p>- Then log back in to DC-1 using the credentials: mydomain.com\labuser.</p>
<br />

<h2>Create Domain Admin User</h2>
<br />

<h3>1. Open Active Directory Users and Computers (ADUC):</h3>
<br />

![Screenshot 2025-02-18 164835](https://github.com/user-attachments/assets/e520ec0a-14bd-40eb-b704-f3e18430e180)
<p>- Press Windows + R, type dsa.msc, and press Enter to open ADUC or click start and search "Active Directory Users and Computers"</p>
<br />

<h3>2. Create an Organizational Unit (OU) for Employees:</h3>
<br />

![Screenshot 2025-02-18 165019](https://github.com/user-attachments/assets/81f2f444-5adc-4b76-b64c-d9a594e7dc5b)
![Screenshot 2025-02-18 165057](https://github.com/user-attachments/assets/0edea339-a3a6-4a6a-985f-fd7fca061e4e)
<p>- In ADUC, right-click on the domain (e.g., mydomain.com), and choose New > Organizational Unit.</p>
<p>- In Active Directory Users and Computers (ADUC), create an Organizational Unit
 (OU) called “_EMPLOYEES”</p>
<br />

<h3>3. Create an Organizational Unit (OU) for Admins:</h3>
<br />

![Screenshot 2025-02-18 165121](https://github.com/user-attachments/assets/b80956a7-d5bd-4d08-b253-5787331f4d0f)
![Screenshot 2025-02-18 165223](https://github.com/user-attachments/assets/c61ff929-3eed-4181-ad0e-5571bc693014)

<p>- Right-click on the domain again, and choose New > Organizational Unit.</p>
<p>- Create a new OU named “_ADMINS”</p>
<br />

<h3>4. Create a new employee named “Jane Doe” with the username of “jane_admin”</h3>
<br />

![Screenshot 2025-02-18 165306](https://github.com/user-attachments/assets/d8986bbb-33b7-4909-8a6c-ef078cb59af5)
![Screenshot 2025-02-18 165523](https://github.com/user-attachments/assets/8495e83e-cda9-42c4-9b4f-71ea7b0f95af)
![Screenshot 2025-02-18 165644](https://github.com/user-attachments/assets/07a587bf-9088-4772-888d-cbd233c6b5f3)
<br />

<h3>5. Add jane_admin to the “Domain Admins” Security Group</h3>
<br />

![Screenshot 2025-02-18 165749](https://github.com/user-attachments/assets/d4acfe81-18a5-4ddb-ac6c-218c80ad7adf)
<p>- Right-click on Domain Admins in ADUC, and choose Properties then click on Add.</p>
<p>- In the Enter the object names to select field, type jane_admin and click Check Names then click OK to add the user.</p>
<br />

<h3>6. Log out and close the connection to DC-1 and log back in as “mydomain.com\jane_admin”</h3>
<br />

![Screenshot 2025-02-18 165926](https://github.com/user-attachments/assets/9efce4ba-24ed-46c2-9d7c-457da4e41b49)

<p>- Log out of DC-1.</p>
<p>- Login back into DC-1 using mydomain.com\jane_admin and the password</p>
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

<h3>1. Log into Client-1 as "mydomain.com\jane_admin"<h3>

![Screenshot 2025-02-18 180520](https://github.com/user-attachments/assets/a6734aea-0ffa-4b7b-af5e-9b37e34a413a)

<h3>2. Open system properties</h3>
<br />

![Screenshot 2025-02-18 180659](https://github.com/user-attachments/assets/9253762b-a2d7-4d51-96c8-1a54050a37b8)

<p>- Go to "settings" then click to "Remote Desktop"</p>
<br />

<h3>3. Allow Domain Users Access to Remote Desktop:</h3>
<br />

![Screenshot 2025-02-18 180840](https://github.com/user-attachments/assets/b7e06eab-353f-4d5d-84a3-19ef13bb9fe0)

<p>- In the Remote Desktop Users window, click Add.</p>
<p>- Type Domain Users and click OK to allow all domain users to access Remote Desktop.</p>
<br />

<h2>Create Multiple Users via PowerShell on DC-1</h2>
<h3>1. Login to DC-1 as jane_admin</h3>
<p>- Confirm that the user can successfully log in via Remote Desktop.</p>
<br />

<h3>2. Open PowerShell_ise as an administrator</h3>
<br />

![Screenshot 2025-02-18 181940](https://github.com/user-attachments/assets/b04dbeaa-ad29-4ce7-a6c8-93489f0a838e)

<p>- Open PowerShell ISE.</p>
<br />

<h3>3. Create a new File</h3>
<br />

![Screenshot 2025-02-18 182708](https://github.com/user-attachments/assets/7882f3f7-b519-4f4e-9d88-a0b4ef30ac10)
![Screenshot 2025-02-18 185259](https://github.com/user-attachments/assets/b2dfaff3-15e5-4851-bbd8-96a20a8a1c4e)

<p>- Click File > New to create a new file.</p>
<p>- Paste the contents of the <a href=https://github.com/mark-afortadera/AD_PS/blob/master/Generate-Names-Create-Users.ps1>script</a> into it</p></p>
<br />

<h3>4. Run the Script:</h3>

![Screenshot 2025-02-18 185337](https://github.com/user-attachments/assets/52e31039-cb44-4602-8ceb-82bdfa8185c7)

<p>- Click the "Run" button to run the script and observe the accounts being created</p></p>
<p>- When finished, open ADUC and observe the accounts in the appropriate OU (_EMPLOYEES)</p>
<br />

<h3>5. Log in as Non-Administrator User</h3>
<br />

![Screenshot 2025-02-18 185706](https://github.com/user-attachments/assets/63a2fe9b-2540-452c-b02f-36238f34ed23)

<p>- Go to Active Directory Users and Computers (ADUC).</p>
<p>- Navigate to the _EMPLOYEES Organizational Unit (OU).</p>
<p>- Attempt to log into Client-1 with one of the accounts or as "menu.suq" user.)</p>
<br />

<h3>6. Attempt to Log Into Client-1 as the Created User:</h3>
<br />

![Screenshot 2025-02-18 185804](https://github.com/user-attachments/assets/b60b7f13-7932-4f4f-a7f8-e1fca11df189)

<p>- Log out of Client-1 and try logging in as one of the newly created users.</p>
<p>- Use the password specified in the PowerShell script.</p>
<br />

<h3>7. Verify Successful Login:</h3>
<br />

![Screenshot 2025-02-18 190022](https://github.com/user-attachments/assets/253c6455-a0c9-412d-b340-e693634121ed)

<p>- Confirm that the new user can successfully log in to Client-1 through Remote Desktop.</p>
<p>- Client-1 is now logged in as "menu.suq" user.</p>
<br />

<h2>Dealing with Account Lockouts</h2>
<br />

<h3>1. Open the Group Policy Management Console (GPMC)</h3>
<br />

![Screenshot 2025-02-19 155921](https://github.com/user-attachments/assets/95faaf00-270b-40c3-987a-a27d8bac98e8)

<p>- Log in to a machine with Group Policy Management Console installed (typically, a Domain Controller)</p>
<p>- Click Start, and type gpmc.msc in the search box, then press Enter. This opens the Group Policy Management Console.</p>
<br />

<h3>2. Create or Edit a Group Policy Object (GPO)</h3>
<br />

![Screenshot 2025-02-19 160341](https://github.com/user-attachments/assets/fb6086e5-f1b0-4576-bc41-1901ddf86736)

<p>- In the GPMC, navigate to the Group Policy Objects section.</p>
<p>- Right-click Group Policy Objects and select New to create a new GPO, or right-click an existing GPO and select Edit to modify it.</p>
<p>- Give the new GPO a descriptive name if you're creating a new one, like "Account Lockout Policy"</p>
<br />

<h3>3. Navigate to the Account Lockout Policy Settings</h3>
<br />

![Screenshot 2025-02-19 160537](https://github.com/user-attachments/assets/36d13b28-79fb-4e2c-bc3a-e026efbacfe0)

<p>- In the Group Policy Management Editor, expand the following:</p>
<p>- Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.</p>
<br />

<h3>4. Configure Account Lockout Policy Settings</h3>
<br />

![Screenshot 2025-02-19 161003](https://github.com/user-attachments/assets/0a3300e4-1b02-4755-b607-e29917816f38)
![Screenshot 2025-02-19 161238](https://github.com/user-attachments/assets/fa47eb71-ad4b-47a5-bb14-68d4282351d2)

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

<h3>5. Update Group Policy</h3>
<br />

![Screenshot 2025-02-19 162110](https://github.com/user-attachments/assets/10ee204d-e6b3-4f49-8a4e-617f222bafda)
![Screenshot 2025-02-19 162429](https://github.com/user-attachments/assets/dd25b82c-d0e6-42f6-a789-ab06e1341679)

<p>You can wait for the Group Policy to propagate automatically, or you can force an update immediately.</p>
<p>- On a client machine or server, open Command Prompt and type gpupdate /force, then press Enter.</p>
<br />

<h3>6. Verify the Policy</h3>
<br />

![Screenshot 2025-02-19 163448](https://github.com/user-attachments/assets/f40ce7a2-52ea-48f9-b292-f6a98d1b107f)

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

<h2>Unlocking and Password Reset Accounts</h2>
<br />

<h3>1. Log into Client-1 as "mydomain.com\menu.suq" or any of the created users and Trigger Account Lockout</h3>
<br />

![Screenshot 2025-02-19 163756](https://github.com/user-attachments/assets/291aa6d2-58af-459d-8b4f-bc3da0cd05cd)

<p>- Open Remote Desktop or Windows login and try to log in with the selected account.</p>
<p>- Attempt to log in with it 6 times with a bad password</p>
<p>- Observe that the account has been locked out within Active Directory</p>
<br />

<h3>2. Unlock the Account</h3>
<br />

![Screenshot 2025-02-19 164050](https://github.com/user-attachments/assets/3051a90c-d875-4f22-9874-9fe65a0e5f11)

<p>- In ADUC, right-click on the locked user account.</p>
<p>- Select Properties.</p>
<p>- Under the Account tab, uncheck Account is locked out and click OK.</p>
<br />

<h3>3. Reset the password</h3>
<br />

![Screenshot 2025-02-19 164215](https://github.com/user-attachments/assets/ea5edfd5-b916-4ffd-8b06-506b99e4f31f)

<p>- Still in ADUC, right-click on the user account and select Reset Password.</p>
<p>- Set a new password (or reset it to the original password).</p>
<p>- Click OK.</p>
<br />

<h3>4. Attempt to Log In Again:</h3>
<br />

![Screenshot 2025-02-19 164400](https://github.com/user-attachments/assets/623350d1-9a38-4c16-8ef1-5b5086818ce1)
![Screenshot 2025-02-19 164627](https://github.com/user-attachments/assets/3fb1a28f-a11e-43ca-8dd7-ffae6f68ef8e)

<p>- Try logging in with the created account</p>
<p>- Ensure that the login is successful.</p>
<br />

<h2> Enabling and Disabling Accounts</h2>
<br />

<h3>1. Disable the User Account in ADUC:</h3>
<br />

![Screenshot 2025-02-19 165202](https://github.com/user-attachments/assets/2e169116-ce99-45dc-9793-942abe04b587)

<p>- Right-click on the user account in ADUC and select Disable the same account in Active Directory</p>
<br />

<h3>2. Attempt to Log in with the Disabled Account:</h3>
<br />

![Screenshot 2025-02-19 165330](https://github.com/user-attachments/assets/71d7a2d7-a185-41a2-81d4-4bd4df8a576b)

<p>- Attempt to login with the disabled user account.</p>
<p>- Observe the error message indicating that the account is disabled.</p>
<br />

<h3>3. Re-enable the Account:</h3>
<br />

![Screenshot 2025-02-19 165351](https://github.com/user-attachments/assets/136dae3f-dc26-4cdb-8be7-82e1af54339a)

<p>- Right-click on the user account in ADUC and select Enable Account.</p>
<p>- Attempt to logging in with the same user account.</p>
<p>- Confirm that the login is successful and now the account has been re-enabled.</p>
<br />

<h3>4. Observe Logs on Domain Controller</h3>
<br />

![Screenshot 2025-02-19 165738](https://github.com/user-attachments/assets/d2c79441-a783-4e70-9498-7fbb64de1cce)

<p>- Open Event Viewer on DC-1 by pressing Windows + R, typing eventvwr.msc, and pressing Enter.</p>
<br />

![Screenshot 2025-02-19 170324](https://github.com/user-attachments/assets/887f48ae-eda0-415e-8639-3cd1d8affe47)

<p>- In the Event Viewer, go to Windows Logs > Security.</p>
<p>- Look for log entries related to Account Lockout and Failed Logins (Event ID 4625 for Audit Failure).</p>
<br />

