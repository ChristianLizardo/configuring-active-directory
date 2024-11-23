<h1 align="center"> Configuring Active Directory  </h1>

 ![download-1](https://github.com/user-attachments/assets/57fe2ce0-8175-4d95-9308-b5ed0806ee69) 

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

![IMG_1419 (2)](https://github.com/user-attachments/assets/44a6f7d3-cece-43db-b685-5dec68143b6c)

<h1 align="center">Check Connectivity between the client and DC</h1>

## Step 1 : Sign into Client- 1 with remote desktop and Ping DC-1's Private IP address with ping -t ( Perpetual ping ):

![IMG_1420 (1)](https://github.com/user-attachments/assets/debb9836-d811-4a80-b3d6-863f7a1fbb15)

## Step 2 : Login to the DC and enable ICMPV4 into the local firewall 

![IMG_1421 (1)](https://github.com/user-attachments/assets/258dfe10-18d7-4395-ad24-f1b51eacf9da)

## Step 3 : Validate client-1 ping , if it was successful

![IMG_1422 (1)](https://github.com/user-attachments/assets/0bcfb678-44c6-449b-9814-bac5a05e2eb5)


<h1 align="center"> Install Active Directory </h1>

## Step 1 : login to "DC-1" and install Active Directory Services 

![IMG_1423 (1)](https://github.com/user-attachments/assets/376b9882-9394-45a0-a176-01fdaa780abd) 

## Step 2 : Promote as a Domain Controller

![IMG_1424 (1)](https://github.com/user-attachments/assets/3e9c872a-0732-4889-95f7-1a81daf998b2)

## Step 3 : Setup a new forest as myactivedirectory.com 

![IMG_1425 (1)](https://github.com/user-attachments/assets/5978bd44-e4a1-4716-a89d-b06af3051df2)

## step 4 : Reboot and then sign back on into DC-1  as a user this time . User: myadproject.com\labuser

![IMG_1426 (1)](https://github.com/user-attachments/assets/0da5c8a7-32e0-403b-b495-446197a0f83b)

<h1 align="center"> Create an AD Admin and normal user  </h1>

## Step 1 : In AD , go TO ADUC -> create an organizational unit -> called "__EMPLOYEES" and another one called "__ADMINS"

![IMG_1427 (1)](https://github.com/user-attachments/assets/1248965e-3936-44ce-aaa1-c60ddcfe7121)

![IMG_1428 (1)](https://github.com/user-attachments/assets/21385df3-8937-414c-9d76-187458900001)

## Step 2 : Create a new employee named "Jane Doe " with the user name of "jane_admin"

![IMG_1429 (1)](https://github.com/user-attachments/assets/75b79764-0637-4c21-9bc8-c3e30d477b39)

## Step 3 : Add "jane_admin" to the "Domain Admins" Security Group 

![IMG_1430 (1)](https://github.com/user-attachments/assets/55d2cb8b-9488-4cdb-bcba-6c3bc0913d42)

## Step 4 : Log out and exit the RDP connection to DC - 1 and sign back on as "myadproject.com\jane_admin". Use this account for admin purposes going forward

![IMG_1431 (1)](https://github.com/user-attachments/assets/4bc92b77-204e-4a1a-8fd1-3eef045f2662)

<h1 align="center"> Join client-1 to your domain (myadproject.com) </h1>

## Step 1 : At the Azure Portal , change Client-1 's DNS Settings to the DC 's Private IP Adress

![IMG_1432 (1)](https://github.com/user-attachments/assets/29c3e045-ae44-4be2-8d4d-1bcadb33ba9c)

## Step 2 : From the Azure Portal , Login to client - 1 ( RDP ) to the original local admin (labuser ) and collaborate it to the domain ( Computer will reboot ) 

![IMG_1433 (1)](https://github.com/user-attachments/assets/83a7fcd2-f27a-4273-80d2-b910aae7d2a7)

## Step 3 : Access the Domain Controller via Remote Desktop and confirm that Client-1 appears in the Active Directory Users and Computers (ADUC) under the default "Computers" container at the root of the domain.

Create a new Organizational Unit (OU) named "_CLIENTS" and move Client-1 into this OU.

![IMG_1434 (1)](https://github.com/user-attachments/assets/aed8db9b-161a-4ea6-95f9-ab86cdc731e5)

<h1 align="center"> Setup RDP for Non - Admin users on Client-1  </h1>

## Log into Client-1 as mydomain.com\jane_admin and navigate to System Properties.

1. Select the Remote Desktop tab.
2. Enable access for domain users to use Remote Desktop.
3. At this point, non-administrative users can log into Client-1 using Remote Desktop.

Note: In a real-world scenario, this configuration is typically managed using Group Policy to apply changes across multiple systems simultaneously (a potential topic for a future lab)

![IMG_1435 (1)](https://github.com/user-attachments/assets/c170e11f-8521-4aa6-b24f-15db7169354b)

<h1 align="center"> Create a list of users and attempt to login to client-1 with one of the users </h1>

## Login to DC-1 as jane_admin

1. Open PowerShell_ise as admin
2. Create a new file and paste the contents of the script (https://github.com/christianlizardo/configuring-active-directory/blob/script/createUsers.ps1) into it:

![IMG_1436 (1)](https://github.com/user-attachments/assets/2f9e6970-fa11-4731-964a-6edcc3f6915b)

3. Run the Script and observe the accounts being created

![IMG_1437 (1)](https://github.com/user-attachments/assets/0b1dad98-8b1c-4a20-9cf6-747d5505c053)

4. Once completed, open Active Directory Users and Computers (ADUC) to verify the accounts are located in the appropriate Organizational Unit (OU). Then, use one of the accounts to attempt logging into Client-1 (ensure you have the password from the script for this step).

![IMG_1438 (1)](https://github.com/user-attachments/assets/2941ff46-5735-4544-be61-509c0cd6a27d)


![IMG_1439 (1)](https://github.com/user-attachments/assets/15479ee9-b2f5-4bb4-bc92-bf79d6a9feef)

![IMG_1440 (1)](https://github.com/user-attachments/assets/e40f26c8-5248-44b1-b663-e42749ed1d81)

## I hope this tutorial provided valuable insights into network security protocols and allowed you to observe traffic between virtual machines. While I ran this on a MacBook Air, it can just as easily be done on a PC without needing to download a remote desktop app, as Windows includes that functionality by default.

Now that the tutorial is complete, remember to clean up your Azure environment to avoid incurring unnecessary charges.

1. Close your Remote Desktop connection.
2. Delete the Resource Group(s) created at the start of this tutorial.
3. Verify that the Resource Group(s) have been successfully deleted.





