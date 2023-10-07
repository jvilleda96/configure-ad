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
- Windows 10 (22H2)

<h2>Deployment and Configuration Steps</h2>

1) Make sure your domain controller's IP address is switched from dynamic to static. I'm using an Azure virtual machine so I changed it in the Network Interface Card (NIC) settings.

   ![image](https://github.com/jvilleda96/configure-ad/assets/147073936/75071055-625b-4489-bff3-5d51e6061317)

2) Login into your Domain Controller's computer as an administrator and open up the server manager application. Here you can select the "add roles and features" option to begin the process.

   ![Add Roles   Features](https://github.com/jvilleda96/configure-ad/assets/147073936/233f6ed6-2519-48bb-ad82-a6bdd14da198)

   Choose the necessary options to reach the server roles option and select "Active Directory Domain Services".

   ![Active Directory OPTION](https://github.com/jvilleda96/configure-ad/assets/147073936/ad069b4b-a241-4109-871e-6c46df9ec588)

   Once installed go to the top right and select the "Promote this server to a domain controller" option.

   ![Assign Forest](https://github.com/jvilleda96/configure-ad/assets/147073936/0477d19d-a507-4e39-88d8-7c15c615eef3)

   Select the "add a new forest" option and type in the local domain name you want to use, in this example I used mydomain.com.

   ![Local Forest Domain](https://github.com/jvilleda96/configure-ad/assets/147073936/b87c2e12-11eb-4264-b6b0-6b0103cc3ecb)

