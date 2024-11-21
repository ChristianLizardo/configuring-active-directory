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
