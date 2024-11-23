# configuring-active-directory

## Description
This guide details the process of implementing an on-premises Active Directory setup using Azure Virtual Machines.

## Enviroments and Technologies used 
- Microsoft Azure ( Virtual Machines / Computer )
-  Remote Desktop
-  Active Directory Domain Services
-  Powershell

## Operating Systems Used
- Windows Server 2022
- Windows 10 ( 21H12 )

## High Level Deployment and Configuration Steps 
1. Create resources
2. Check connectivity between the client and the domain controller
3. Download and install Active Directory
4. Create an Admin and end user account in AD
5. Join Client-1 to the domain (myadproject.com)
6. Setup RDP for non administrative users on client -1
7. Create a manual additional users and attempt to sign in into client-1 with one of the users

## Deployment and Configuration steps 

<h1 align="center"> Setup Resources in Azure </h1>

## Step 1: Create Domain Controller VM ( Windows Server 2022 ) titled , "DC-1"

![IMG_1414 (1)](https://github.com/user-attachments/assets/077421d1-d57c-4f18-a3ed-bdbc02e301de)

![IMG_1415 (1)](https://github.com/user-attachments/assets/5e0afe8e-1f99-4f4f-8cc6-2684b78f8543) 

## Step 2 : Creare the client VM ( Windows 10 ) named , "Client-1" . You must use the same resource group and Vnet that was created in step 1 . 

![IMG_1416 (1)](https://github.com/user-attachments/assets/d7428ead-0998-4ec2-9331-ac72e4e3a539)

## Step 3 : Set the Domain Controller's NIC Private  IP address to be static 

![IMG_1418 (1)](https://github.com/user-attachments/assets/209133b7-dd00-4775-83ca-16355f8dd26d) 

## Step 4 : Check that both virtual machines are in the same Vnet ( Tip :  Check network topology with the Network watcher ) 

Insert Image Here 

<h1 align="center">Check Connectivity between the client and DC</h1>

## Step 1 : Sign into Client- 1 with remote desktop and Ping DC-1's Private IP address with ping -t ( Perpetual ping ):

Insert Image Here 

## Step 2 : Login to the DC and enable ICMPV4 into the local firewall 

Insert Image Here 

## Step 3 : Validate client-1 ping , if it was successful

Insert Image Here 


<h1 align="center"> Install Active Directory </h1>

## Step 1 : login to "DC-1" and install Active Directory Services 

Insert Image Here 

## Step 2 : Promote as a Domain Controller

Inseer Image Here 

## Step 3 : Setup a new forest as myactivedirectory.com 

Insert Here 

## step 4 : Reboot and then sign back on into DC-1  as a user this time . User: myadproject.com\labuser

Insert Here 

<h1 align="center"> Create an AD Admin and normal user  </h1>

## Step 1 : In AD , go TO ADUC -> create an organizational unit -> called "__EMPLOYEES" and another one called "__ADMINS"

Insert Here

Insert Here 

## Step 2 : Create a new employee named "Jane Doe " with the user name of "jane_admin"

Insert Here 

## Step 3 : Add "jane_admin" to the "Domain Admins" Security Group 

Insert Here 

## Step 4 : Log out and exit the RDP connection to DC - 1 and sign back on as "myadproject.com\jane_admin". Use this account for admin purposes going forward

Insert Here

<h1 align="center"> Join client-1 to your domain (myadproject.com) </h1>

## Step 1 : At the Azure Portal , change Client-1 's DNS Settings to the DC 's Private IP Adress

Insert Here 

## Step 2 : From the Azure Portal , Login to client - 1 ( RDP ) to the original local admin (labuser ) and collaborate it to the domain ( Computer will reboot ) 

![IMG_1431](https://github.com/user-attachments/assets/3020e37a-dd8e-4be1-a6b8-96a94dae97c6)

## Step 3 : Access the Domain Controller via Remote Desktop and confirm that Client-1 appears in the Active Directory Users and Computers (ADUC) under the default "Computers" container at the root of the domain.

Create a new Organizational Unit (OU) named "_CLIENTS" and move Client-1 into this OU.

Insert Here

<h1 align="center"> Setup RDP for Non - Admin users on Client-1  </h1>

## Log into Client-1 as mydomain.com\jane_admin and navigate to System Properties.

1. Select the Remote Desktop tab.
2. Enable access for domain users to use Remote Desktop.
3. At this point, non-administrative users can log into Client-1 using Remote Desktop.

Note: In a real-world scenario, this configuration is typically managed using Group Policy to apply changes across multiple systems simultaneously (a potential topic for a future lab)

Insert Here 

<h1 align="center"> Create a list of users and attempt to login to client-1 with one of the users </h1>

## Login to DC-1 as jane_admin

1. Open PowerShell_ise as admin
2. Create a new file and paste the contents of the script (https://github.com/christianlizardo/configuring-active-directory/blob/script/createUsers.ps1) into it:

Insert Here 

3. Run the Script and observe the accounts being created

Insert Here

4. Once completed, open Active Directory Users and Computers (ADUC) to verify the accounts are located in the appropriate Organizational Unit (OU). Then, use one of the accounts to attempt logging into Client-1 (ensure you have the password from the script for this step).

Insert Here

Insert HEre

Insert Here 

## I hope this tutorial provided valuable insights into network security protocols and allowed you to observe traffic between virtual machines. While I ran this on a MacBook Air, it can just as easily be done on a PC without needing to download a remote desktop app, as Windows includes that functionality by default.

Now that the tutorial is complete, remember to clean up your Azure environment to avoid incurring unnecessary charges.

1. Close your Remote Desktop connection.
2. Delete the Resource Group(s) created at the start of this tutorial.
3. Verify that the Resource Group(s) have been successfully deleted.





