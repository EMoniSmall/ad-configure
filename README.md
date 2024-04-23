# Active Directory Configuration within Azure VMs ⚙
<h3>What is an Active Directory?</h3>
An Active Directory is a Microsoft directory service for managing and organizing network resources such as computers and users in Windows domain networks. It focuses on authentications, authorization, and configuration management to make network administration efficient and secure. Large institutions such as universities or businesses use Active Directory in some shape or form. When you log into a work computer or a school, your login information is authenticated and recognized as part of the system. 

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
> Static IP addresses are often used for servers, network printers, routers, and other devices that need a consistent address for accessibility and management purposes. This makes it easier to track client devices, the server does not need to assign an IP address each time for every log in, and it makes the server easier to secure. 

<h2>Testing Connectivity between DC-1 and Client-1</h2>

Step 1: First you'll connect to Client-1 via Remote Desktop.

Step 2: Test the connectivity from within Client-1 to DC-1 by using Powershell to perpetual ping DC-1. (ping -1 <ip address)

Step 3: Log into the Domain Controller and enable ICMPv4 on the local Windows firewall.
