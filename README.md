# Azure Active Directory

## On-premises Active Directory Deployed in the Cloud (Azure)

This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)

## High-Level Deployment and Configuration Steps

1. Setup Resources in Azure
    - Create the Domain Controller VM (Windows Server 2022) named “DC-1”
    - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
    - Set Domain Controller’s NIC Private IP address to be static
    - Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
    - Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

2. Ensure Connectivity between the client and Domain Controller
    - Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
    - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
    - Check back at Client-1 to see the ping succeed

3. Install Active Directory
    - Login to DC-1 and install Active Directory Domain Services
    - Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
    - Restart and then log back into DC-1 as user: mydomain.com\labuser

4. Create an Admin and Normal User Account in AD
    - In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
    - Create a new OU named “_ADMINS”
    - Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
    - Add jane_admin to the “Domain Admins” Security Group
    - Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
    - Use jane_admin as your admin account from now on

5. Join Client-1 to your domain (mydomain.com)
    - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
    - From the Azure Portal, restart Client-1
    - Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
    - Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
    - Create a new OU named “_CLIENTS” and drag Client-1 into there

6. Setup Remote Desktop for non-administrative users on Client-1
    - Log into Client-1 as mydomain.com\jane_admin and open system properties
    - Click “Remote Desktop”
    - Allow “domain users” access to remote desktop
    - You can now log into Client-1 as a normal, non-administrative user now

7. Create a bunch of additional users and attempt to log into client-1 with one of the users
    - Login to DC-1 as jane_admin
    - Open PowerShell_ise as an administrator
    - Run the script and observe the accounts being created
    - When finished, open ADUC and observe the accounts in the appropriate OU
    - Attempt to log into Client-1 with one of the accounts (take note of the password in the script)

## Finish.
