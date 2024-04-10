# Configuring an Active Directory within an Azure VM ⚙
<h3>What is an Active Directory?</h3>
An Active Directory is a Microsoft directory service for managing and organizing network resources such as computers and users in Windows domain networks. It focuses on authentications, authorization, and configuration management to make network administration efficient and secure. Large institutions such as universities or businesses use Active Directory in some shape or form. When you log into a work computer or a school, your login information is authenticated and recognized as part of the system. 

<h2>Expectaions 🤔</h2>
During this tutorial, you will:

- Create a Virtual Network and Subnet

- Set-Up a domain controller inside of an Azure VM
  
- Change the NIC's Private IP address to Static
  
- Connect a Client VM to the Server
  

> [!Important]
> You need to create **two** Azure Virtual Machines to continue with this tutorial. For a step-by-step guide to creating an Azure VM, please refer to [Azure Crash Course](https://github.com/EMoniSmall/azurecrashcourse). Specifically, [Virtual Machines](https://github.com/EMoniSmall/azurecrashcourse/edit/main/README.md#virtual-machines--). When creating the domain controller, under the "Image" drop-down menu, select **Windows Server 2022 Datacenter: Azure Edition - x62 Gen 2**. The client VM will still use Windows 10 pro. *Keep in mind that an Azure free subscription will only allow the use of up to 4 vcpus when selecting Size.* **Both Virtual Machines must be created in the same Resource Group.**
> 
> ![Selecting Server](https://github.com/EMoniSmall/ad-configure/assets/166156618/196f799f-4488-4231-951e-bc8b8b3b9a24)


