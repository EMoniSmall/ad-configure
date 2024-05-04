# Active Directory Configuration within Azure VMs ⚙
<h3>What is Active Directory?</h3>
Active Directory is a Microsoft directory service for managing and organizing network resources such as computers and users in Windows domain networks. It focuses on authentications, authorization, and configuration management to make network administration efficient and secure. Large institutions such as universities or businesses use Active Directory in some shape or form. When you log into a work computer or a school, your login information is authenticated and recognized as part of the system. 

<h2>Expectaions 🤔</h2>
During this tutorial, you will:

- Create a Virtual Network and Subnet

- Set-Up a domain controller inside of an Azure VM
  
- Change the NIC's Private IP address to Static
  
- Connect a Client VM to the Server

- Create an Admin and Normal User Account in AD

- Join Client-1 to your domain

- Set-up Remote Desktop for Non-Admin Users on Client-1

- Create additional users and attempt to log in with one.
  

> [!Important]
> You need to create **two** Azure Virtual Machines to continue with this tutorial. For a step-by-step guide to creating an Azure VM, please refer to [Azure Crash Course: Virtual Machines](https://github.com/EMoniSmall/azurecrashcourse/edit/main/README.md#virtual-machines--). When creating the domain controller, under the "Image" drop-down menu, select **Windows Server 2022 Datacenter: Azure Edition - x62 Gen 2**. The client VM will still use Windows 10 pro. *Keep in mind that an Azure free subscription will only allow the use of up to 4 vcpus when selecting Size.* **Both Virtual Machines must be created in the same Resource Group.**
> 
> ![Selecting Server](https://github.com/EMoniSmall/ad-configure/assets/166156618/196f799f-4488-4231-951e-bc8b8b3b9a24)

<h2>Setting the Domain Controller's IP to Static 🏗</h2>

Step 1: Return to Virtual Machines in Azure and click into your Domain Controller. *(DC1 in this tutorial)*

Step 2: Scroll down for "Networking," locate the "Network Interface" and open it.

Step 3: Scroll down til you find ipconfig1 and open. 

Step 4: Set the Allocation to Static and save.

![Dynamic to Static](https://github.com/EMoniSmall/ad-configure/assets/166156618/ddb3a248-fb02-4cbe-8323-a6040dbd5e3a)

> [!Note]
> What does setting it to Static do?
> Static IP addresses are often used for servers, network printers, routers, and other devices that need a consistent address for accessibility and management purposes. This makes it easier to track client devices, the server does not need to assign an IP address each time for every login, and it makes the server easier to secure. 

<h2>Testing Connectivity between DC-1 and Client-1</h2>

Step 1: First you'll connect to Client-1 via Remote Desktop.

Step 2: Test the connectivity from within Client-1 to DC-1 by using Powershell to perpetual ping DC-1. 

(ping -t [ip address])

![mstsc_yaxrhkpPsg](https://github.com/EMoniSmall/ad-configure/assets/166156618/23b33e98-20ec-48ca-9526-ad133f8f4c53)

You'll notice that the requests are timing out. This can be due to DC-1's Firewall blocking ICMP traffic

Step 3: Log into the Domain Controller via Remote Desktop.

Step 4: Open Windows Defender Firewall with Advanced Security and go to inbound rules.

Step 5: You can sort the Protocol to ICMP and search for [Core Networking Diagnostics - ICMP Echo Requests.] You can right click and enable both.

![mstsc_I138tEntyk](https://github.com/EMoniSmall/ad-configure/assets/166156618/71f01098-58de-49f9-a88b-b6dca8c3c28e)

Step 6: Return to Client-1 Remote Desktop and check powershell. There should be replies from DC-1

<h2>Installing Active Directory</h2>

Step 1: Inside DC-1, your [Server Manager] should be open, if not, you can go to the start menu and search for it.

Step 2: Select Add Roles and Features. Click next til you reach Server Roles. Check Active Directory Domain Services and continue to install. It may take a moment to install. 

![mstsc_s7Trmt4zrY](https://github.com/EMoniSmall/ad-configure/assets/166156618/fff62480-8ccd-40f4-9a5c-3fda0fb5bf92)

Step 3: After AD is finished installing, you'll notice a flag in the top right corner of the Server Manager. 

![image](https://github.com/EMoniSmall/ad-configure/assets/166156618/a8e8378d-015b-41d1-9c8d-b4a9f36faf36)

Click then click "Promote this server to a domain controller."

![image](https://github.com/EMoniSmall/ad-configure/assets/166156618/768f398c-b6a6-4801-8c01-4494c3955d6b)

Step 4: Check Add a New Forest and Add a Root Domain Name. Enter a Password on the next screen. 

Step 5: Continue to hit Next until you reach the point of Installation and hit Install. 

![mstsc_S1l19JObIb](https://github.com/EMoniSmall/ad-configure/assets/166156618/e625ffad-ff66-4843-872a-bb02442ab55c)

The DC-1 will automatically restart once the installation is finished. 

<h2>Creating Admin and Normal User Accounts in AD</h2>

Step 1: Log back into DC-1 using your domain name and Username. [mydomain.com\AzureUser]

Step 2: In the top right corner of the Server Manager, open Tools and click on Active Directory Users and Computers. From here you'll create an Admins and Employee Organizational Unit.

Step 3: Right-click mydomain.com > New > Organizational Unit and create an Admins OU and an Employees OU. 

Step 4: Right-click inside the new ADMINS OU and Create a new User. In this example, the user will be John Doe. Enter any credentials you wish.

![mstsc_HAA3Pi6qfA](https://github.com/EMoniSmall/ad-configure/assets/166156618/79817c2b-03e1-457b-9e95-8c0e3d5216cb)

Step 5: Once your user account has been created, Right-click and enter Properties > Member Of tab > Add...

Step 6: Type Domain and hit Check Names. Select Domain Admins, hit OK, and apply. This will turn this User account into an Admin account.

![mstsc_9myTVZLPrV](https://github.com/EMoniSmall/ad-configure/assets/166156618/06cd9e17-6888-4a13-ba1b-d4e6a1608d8d)

<h2>Joining Client-1 to the Domain</h2>

Step 1: Through Azure, first retrieve the private IP Address of DC-1. 

Step 2: Enter Client-1's Network Settings > DNS Servers and select Custom. Add DC-1's Private IP Address as the new DNS Server. Restart Client-1 once the new DNS has finished saving. 

![chrome_ZleUZdMlMe](https://github.com/EMoniSmall/ad-configure/assets/166156618/393c053c-1ade-426d-a91f-5449af6475da)

![Screenshot 2024-04-24 123320](https://github.com/EMoniSmall/ad-configure/assets/166156618/d67cbf2a-f4ac-4c75-909b-846d0681fe91)

Step 3: Log into Client-1 via Remote Desktop and Right-click the start menu to open Settings > About > Rename this PC (Advanced).

Step 4: Click Change... and under Member Of, select domain and enter your domain address. Once Completed, Client-1 will automatically restart. 
You can verify that Client-1 has joined the domain but checking inside DC-1's Remote Desktop connection > Active Directory Users and Computers > Computer container on the root of the domain. 

Step 5: Create another OU for Clients and drag Client-1 into the new OU.

![mstsc_va87dP7INV](https://github.com/EMoniSmall/ad-configure/assets/166156618/f8405d8b-d39a-4f05-873d-656cbc03bd83)

<h2>Setting Up Remote Desktop for Normal Users on Client-1</h2>

> [!Note]
> Because Client-1 is now a part of the domain, you will now be able to use Client-1 to log into mydomain.com

Step 1: Log into Client-1 as your Admin account. (John_Admins in this example)

Step 2: Right-Click the start menu and enter Settings > Remote Desktop > Select users that can remotely access this PC > Add

Step 3: Type in Domain Users > Check Names > OK > OK

![mstsc_cBQlbpAVR1](https://github.com/EMoniSmall/ad-configure/assets/166156618/1d98c4db-aba9-46ee-b684-af9796dd471f)

> [!Note]
> You should now be able to see Domain Users under the User OU in Active Directory Users and Computers on DC-1 Remote Desktop. You can now log into Client-1 as a normal user account. Group Policies makes it easier to apply settings such as this to thousands of computers at once.

<h2>Creating Addtional Users for the Domain</h2>

Step 1: Log into DC-1 as your Admin Account.

Step 2: Open Powershell_ise as an Administrator.

Step 3: File > New > and copy and paste [this script](https://github.com/EMoniSmall/PSCreation/blob/main/PSISECode) into the space.

> [!Note]
> The script can be changed according to your needs but it is fine as default. It may take some time to generate all the users but you can attempt to log into one while it is generating. You can open Active Directory Users and Computers, pick a randomly generated User, and use the Username and Password to log into Client-1. 




