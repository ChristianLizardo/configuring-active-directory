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

![IMG_1414](https://github.com/user-attachments/assets/d9938aca-12a2-4d03-8feb-531439f8c11d)

## Step 2 : Creare the client VM ( Windows 10 ) named , "Client-1" . You must use the same resource group and Vnet that was created in step 1 . 

![IMG_1415](https://github.com/user-attachments/assets/c3e5ffb9-27b3-4a7b-ab87-cff531c9e984)

## Step 3 : Set the Domain Controller's NIC Private  IP address to be static 

![IMG_1416](https://github.com/user-attachments/assets/814cf5be-cb8f-429b-a4da-d54413099251)

## Step 4 : Check that both virtual machines are in the same Vnet ( Tip :  Check network topology with the Network watcher ) 



<h1 align="center">Check Connectivity between the client and DC</h1>

## Step 1 : Sign into Client- 1 with remote desktop and Ping DC-1's Private IP address with ping -t ( Perpetual ping ):

![IMG_1418](https://github.com/user-attachments/assets/4542a1c0-3d14-4119-ab21-c784fd2cb000)

## Step 2 : Login to the DC and enable ICMPV4 into the local firewall 

![IMG_1417](https://github.com/user-attachments/assets/4fed4f56-84f3-4811-9927-1d9aa3ff0f13)

## Step 3 : Validate client-1 ping , if it was successful

![IMG_1420](https://github.com/user-attachments/assets/3e751c1e-26f5-4786-bad3-648f699b1796)


<h1 align="center"> Install Active Directory </h1>

## Step 1 : login to "DC-1" and install Active Directory Services 

![IMG_1421](https://github.com/user-attachments/assets/4a869f71-382a-4218-8b47-0a03818f40d8)

## Step 2 : Promote as a Domain Controller

![IMG_1422](https://github.com/user-attachments/assets/df95c78e-68f4-4806-b2fb-8074bed313a2)

## Step 3 : Setup a new forest as myactivedirectory.com 

![IMG_1423](https://github.com/user-attachments/assets/6ff72ff5-8781-4e9c-88db-4e67cf6238b4)

## step 4 : Reboot and then sign back on into DC-1  as a user this time . User: myadproject.com\labuser

![IMG_1424](https://github.com/user-attachments/assets/8d3c7c61-0b5b-4964-90b5-0c2fde7652a5)

<h1 align="center"> Create an AD Admin and normal user  </h1>

## Step 1 : In AD , go TO ADUC -> create an organizational unit -> called "__EMPLOYEES" and another one called "__ADMINS"

![IMG_1425](https://github.com/user-attachments/assets/d989596e-ceb2-4ae5-b0aa-44214a649c01)

![IMG_1426](https://github.com/user-attachments/assets/d7cceb56-a60a-4459-adc0-e04e65fca74e)

## Step 2 : Create a new employee named "Jane Doe " with the user name of "jane_admin"

![IMG_1427](https://github.com/user-attachments/assets/f1f80cea-951e-43c5-9bcc-5e13be87d2e6)

## Step 3 : Add "jane_admin" to the "Domain Admins" Security Group 

![IMG_1428](https://github.com/user-attachments/assets/2ec25e13-5aa4-4296-b054-a7b947cb709f)

## Step 4 : Log out and exit the RDP connection to DC - 1 and sign back on as "myadproject.com\jane_admin". Use this account for admin purposes going forward

![IMG_1429](https://github.com/user-attachments/assets/8b98ef72-5f9c-4d0c-af54-419268850284)

<h1 align="center"> Join client-1 to your domain (myadproject.com) </h1>

## Step 1 : At the Azure Portal , change Client-1 's DNS Settings to the DC 's Private IP Adress

![IMG_1430](https://github.com/user-attachments/assets/d0bee36b-2684-4d35-91cb-a9accc2a37f4)

## Step 2 : From the Azure Portal , Login to client - 1 ( RDP ) to the original local admin (labuser ) and collaborate it to the domain ( Computer will reboot ) 

![IMG_1431](https://github.com/user-attachments/assets/3020e37a-dd8e-4be1-a6b8-96a94dae97c6)

## Step 3 : Access the Domain Controller via Remote Desktop and confirm that Client-1 appears in the Active Directory Users and Computers (ADUC) under the default "Computers" container at the root of the domain.

Create a new Organizational Unit (OU) named "_CLIENTS" and move Client-1 into this OU.

![IMG_1432](https://github.com/user-attachments/assets/351d307b-bcc8-4575-aa70-8ace4fff0014)

<h1 align="center"> Setup RDP for Non - Admin users on Client-1  </h1>

## Log into Client-1 as mydomain.com\jane_admin and navigate to System Properties.

1. Select the Remote Desktop tab.
2. Enable access for domain users to use Remote Desktop.
3. At this point, non-administrative users can log into Client-1 using Remote Desktop.

Note: In a real-world scenario, this configuration is typically managed using Group Policy to apply changes across multiple systems simultaneously (a potential topic for a future lab)

![IMG_1433](https://github.com/user-attachments/assets/0e18b2cc-da4b-4d70-ae33-91a8831e34f6)

<h1 align="center"> Create a list of users and attempt to login to client-1 with one of the users </h1>

## Login to DC-1 as jane_admin

1. Open PowerShell_ise as admin
2. Create a new file and paste the contents of the script (https://github.com/christianlizardo/configuring-active-directory/blob/script/createUsers.ps1) into it:

![IMG_1436](https://github.com/user-attachments/assets/33f564d2-767c-49a4-955b-2eb508042a18)

3. Run the Script and observe the accounts being created

![IMG_1437](https://github.com/user-attachments/assets/cd36b622-14d4-498f-9029-020751180f48)

4. Once completed, open Active Directory Users and Computers (ADUC) to verify the accounts are located in the appropriate Organizational Unit (OU). Then, use one of the accounts to attempt logging into Client-1 (ensure you have the password from the script for this step).

![IMG_1438](https://github.com/user-attachments/assets/39c6e0ab-4414-4f0d-91c2-10336464fc3b)

![IMG_1439](https://github.com/user-attachments/assets/add46289-bd57-45a2-8612-261849c1258e)

![IMG_1440](https://github.com/user-attachments/assets/ecbfb6ab-6e9c-4858-a06c-c342ebc9c756)

## I hope this tutorial provided valuable insights into network security protocols and allowed you to observe traffic between virtual machines. While I ran this on a MacBook Air, it can just as easily be done on a PC without needing to download a remote desktop app, as Windows includes that functionality by default.

Now that the tutorial is complete, remember to clean up your Azure environment to avoid incurring unnecessary charges.

1. Close your Remote Desktop connection.
2. Delete the Resource Group(s) created at the start of this tutorial.
3. Verify that the Resource Group(s) have been successfully deleted.





