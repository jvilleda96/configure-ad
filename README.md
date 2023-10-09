<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial will showcase what I've learned about on-premises Active Directory using Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>Deployment and Configuration Steps</h2>

1) Make sure your domain controller's IP address is switched from dynamic to static. I'm using an Azure virtual machine so I changed it in the virtual Network Interface Card (NIC) settings.

   ![image](https://github.com/jvilleda96/configure-ad/assets/147073936/75071055-625b-4489-bff3-5d51e6061317)

2) Log into your Domain Controller's computer as an administrator and open up the server manager application. Here you can select the "add roles and features" option to begin the process.

   ![Add Roles   Features](https://github.com/jvilleda96/configure-ad/assets/147073936/233f6ed6-2519-48bb-ad82-a6bdd14da198)

   Choose the necessary options to reach the server roles option and select "Active Directory Domain Services".

   ![Active Directory OPTION](https://github.com/jvilleda96/configure-ad/assets/147073936/ad069b4b-a241-4109-871e-6c46df9ec588)

   Once installed go to the top right and select the "Promote this server to a domain controller" option.

   ![Assign Forest](https://github.com/jvilleda96/configure-ad/assets/147073936/0477d19d-a507-4e39-88d8-7c15c615eef3)

   Select the "add a new forest" option and type in the local domain name you want to use, in this example I used "mydomain.com".

   ![Local Forest Domain](https://github.com/jvilleda96/configure-ad/assets/147073936/b87c2e12-11eb-4264-b6b0-6b0103cc3ecb)

   Once installed the computer will restart, login will now require the domain name in front of the user as shown below.

   ![mydomain login](https://github.com/jvilleda96/configure-ad/assets/147073936/d9877d99-1fe7-4538-8d4d-3a87d091c73c)

4) Open up "Active Domain Users and Computers" from the "Windows Administrative Tools" in the start menu.

   ![ADUC start menu](https://github.com/jvilleda96/configure-ad/assets/147073936/d7a5b468-b4a2-4469-bd00-174f046ef963)

   Go to your newly created domain and right click for more options, from here you can create new organizational units as shown below. Make two new units, one named "_EMPLOYEES" and the other "_ADMINS".
   
   ![New Organi Uni](https://github.com/jvilleda96/configure-ad/assets/147073936/65dcc190-8d1a-4992-a3d8-66ab1990f4d0)

   Now create a new administrator in the "_ADMINS" unit.

   ![NEW ADMIN ADUC](https://github.com/jvilleda96/configure-ad/assets/147073936/b2e1468c-a719-4e35-b934-d28d54bd96c9)

   It will ask you to create a password for the new admin account, make sure you select the "password never expires" option.

   ![ADMINS PW NEVER EXPIRES](https://github.com/jvilleda96/configure-ad/assets/147073936/a800c4be-34a2-4335-968f-d5ff02cbe350)

   Once created add the new admin account to the "Domain Admins" group, you can edit this by double clicking the user and going to the "Member of" tab in properties.

   ![member of admins](https://github.com/jvilleda96/configure-ad/assets/147073936/862f5880-7766-45db-83fa-f815e7053446)

5) Now we can add the client computer ini our network to the new domain but first we have to make sure they connect to the Domain Controller's DNS server. I'm using Azure so I will edit this setting in the virtual NIC settings.

   ![DNS SERVER CLIENT](https://github.com/jvilleda96/configure-ad/assets/147073936/a349a148-1509-4b3e-a080-6f3395d67a38)

   To confirm you're connected to the DC's DNS server go into the command prompt and type in "ipconfig /all" and make sure the DNS server is the same address as your DC's private IP address,  in this case it's "10.0.0.4".

   ![CLIENT DNS CONFIRMATION](https://github.com/jvilleda96/configure-ad/assets/147073936/12fd2c63-f4ea-4fb3-96a6-8e9240d0513f)

6) Open up the system settings and go down to the "About" option then select "Rename this PC (advanced)". Then select the "Change" option, select "Domain" and type in the same domain name you used when you added the new forest. You'll be prompted to         login, use the newly added admin account and continue, if successful the system will restart.

   ![ADD CLIENT TO DOMAIN SETTINGS](https://github.com/jvilleda96/configure-ad/assets/147073936/f56eef9d-ef2b-46b9-9aec-ea02b57a70a0)

   Back at the DC's computer, check your active directory to confirm that the client computer is now registered as such.

   ![confirmed client in AD](https://github.com/jvilleda96/configure-ad/assets/147073936/2baed208-d39e-4381-a82d-16619dd896ac)

8) Re-connect and login with the newly added admin account, using the domain name before the username. Once logged in go back to system settings and select the "Remote Desktop" option and "Select users that can remotely access this PC". Click "Add" then     look up "Domain Users" then select ok.
   
   ![ADDING ALL USERS](https://github.com/jvilleda96/configure-ad/assets/147073936/e5110477-b8e9-43ac-ad1a-37332a030034)


9) To test that we've done everything correctly we'll create some users and login from a random one. I used a script and ran it on Powershell.ISE as an admin.

   ![Create users script](https://github.com/jvilleda96/configure-ad/assets/147073936/93f62fa2-1c55-4049-9888-77b8273fb549)

10) Log out from the client computer and re-login using a random created user, I chose the below user.

    ![created users](https://github.com/jvilleda96/configure-ad/assets/147073936/26402551-d1ee-4436-be12-bb7d95ded009)

    I was able to succesfully login, as shown by the command prompt.

    ![confirmed created user login](https://github.com/jvilleda96/configure-ad/assets/147073936/9f0b941e-0ee4-462d-aa34-23d58c8242f9)

Active Directory is now successfully installed in your local network. As an admin you can do a number of things, such as resetting passwords, unlocking acccounts, change file sharing permissions, etc.
