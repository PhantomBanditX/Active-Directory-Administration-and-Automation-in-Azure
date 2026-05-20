# Azure Active Directory Administration and Automation Lab 

<p align="center">
<img width="700" alt="Image" src="https://github.com/user-attachments/assets/75e16b51-9a24-41b6-90a0-f4eba501282d" />
</p>

<p align="center"> <h1>Enterprise Active Directory management in Azure using PowerShell and Group Policy</h1> </p>
</p>

<br />

This project demonstrates the deployment and administration of an Active Directory environment in Azure, including user and group management, Group Policy configuration, PowerShell automation, secure network file sharing, and system monitoring through Event Viewer. <br />

## Project Objectives

- Deploy an enterprise Active Directory environment in Microsoft Azure
- Configure Organizational Units (OUs) and Group Policy Objects (GPOs)
- Automate administrative tasks using PowerShell
- Implement secure SMB file sharing and access controls
- Configure domain-joined client systems
- Monitor authentication and security events using Event Viewer

## Technologies Used
- Microsoft Azure (Virtual Machines)
- Remote Desktop Protocol (RDP)
- Windows Server 2022 (Domain Controller)
- Windows 10 (Client)
- PowerShell ISE for automation
- Event Viewer for log monitoring

## Infrastructure Deployment

<p>
  
  Deploy a Windows Server 2022 virtual machine for the ``Domain Controller (DC-1).``
  
  Deploy a Windows 11 virtual machine for the client workstation ``(Client-1).``

  Ensure both virtual machines are configured within the same Resource Group and Virtual Network (VNet).
  <p>
<img width="2056" alt="Screenshot 2024-10-17 at 12 48 31 PM" src="https://github.com/user-attachments/assets/69be9975-7170-4d72-beac-798495852c97">
</p>
<p>

  Configure a static private IP address for DC-1.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 12 54 33 PM" src="https://github.com/user-attachments/assets/219f8a01-4047-4aa9-819b-bb77b5a2adf2">
</p>
<p>
Configure Client-1 to use DC-1 as its DNS server and restart the virtual machine to apply the changes.

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 14 36 PM" src="https://github.com/user-attachments/assets/338d0cd6-be64-4ed0-9578-de491d6bb6be">
</p>
<p>
Validate network connectivity between Client-1 and DC-1 using PowerShell commands such as:

- ping
- ipconfig /all
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 30 22 PM" src="https://github.com/user-attachments/assets/fdda1c54-a7bb-4042-99ca-5de758d83e0d">
</p>

Run `ipconfig /all` and verify that DC-1’s private IP address is configured as the active DNS server.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 31 28 PM" src="https://github.com/user-attachments/assets/274fc3fc-b96c-45fd-b1d5-494072552416">

<br />

## Active Directory Configuration
Open Server Manager and install the Active Directory Domain Services (AD DS) role through the “Add Roles and Features” wizard.

After installation, promote DC-1 to a Domain Controller by configuring a new Active Directory forest and specifying a custom domain name (e.g., mydomain.com).

Restart the server to complete the Active Directory deployment and domain controller configuration.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 47 37 PM" src="https://github.com/user-attachments/assets/37512fdc-d011-4964-a210-29419f4695c0">
</p>

Authenticate to the domain using the newly configured administrative account.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 45 44 PM" src="https://github.com/user-attachments/assets/a4d48875-7934-438e-b5b3-bcfa29d7c484">

<br />

## Organizational Unit and User Management
Create Organizational Units (OUs) for administrative organization, including:

- _EMPLOYEES
- _ADMINS
- _CLIENTS
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 52 29 PM" src="https://github.com/user-attachments/assets/90c55542-6016-4cd3-9c98-e3495f84f72f">
</p>

Create a dedicated administrative user account named `jane_admin`.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 54 32 PM" src="https://github.com/user-attachments/assets/4e87cb11-9589-40fb-b589-e77add4a41a1">

<br />

<p>
  
Assign the account to the ``Domain Admins`` security group.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 56 41 PM" src="https://github.com/user-attachments/assets/b945aa4d-879b-4b71-9061-88b14566e20f">
<br />

<p>
Authenticate using the administrative account for continued domain administration tasks.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 1 59 09 PM" src="https://github.com/user-attachments/assets/9158095d-ad1e-4086-9121-e2af5cf86103">

<br />

## Group Policy Administration

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 01 16 PM" src="https://github.com/user-attachments/assets/f7ad8cc4-cbb7-4c41-b2d5-7bcd7b2e3c37">
</p>
<p>
  
Configure ``Client-1`` to join the Active Directory domain using the domain name configured during the Active Directory deployment. 
  
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 02 43 PM" src="https://github.com/user-attachments/assets/8b14a2eb-120f-4086-b401-68671b5f46f0">
</p>
<p>
Authenticate using Domain Administrator credentials to authorize the domain join operation.

Restart ``Client-1`` to apply the domain membership configuration changes..
</p>
<br />

<p>
</p><img width="2056" alt="Screenshot 2024-10-17 at 2 08 27 PM" src="https://github.com/user-attachments/assets/fc670165-b0c4-41f1-970f-76adb92f9f9e">
<p>
Verify successful domain enrollment through Active Directory Users and Computers (ADUC).
</p>
<br />

## Secure File Share Configuration

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 07 21 PM" src="https://github.com/user-attachments/assets/0d730396-b70d-47a5-92af-479c447065ab">
</p>
<p>
Type gpmc.msc in Search Bar to open the Group Policy Management Console and navigate to "Account Lockout Policy" so we can adjust some settings such as lockout duration and threshold of invalid login attempts.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 12 40 PM" src="https://github.com/user-attachments/assets/0484278e-1fa7-4461-b5d6-4444d5c0b331">
</p>
<p>
Then log into Client-1 as the Domain Admin run the command "gpupdate /force"
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 14 05 PM" src="https://github.com/user-attachments/assets/30a7f0bc-40a3-48de-9c52-c00a05511f3b">
</p>
<p>
Next we open Command Prompt as an Administrator and run the command "gpresult" to confirm we made our changes.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 17 15 PM" src="https://github.com/user-attachments/assets/c436404c-4ff6-4c99-80e1-d6b432f303f3">
</p>
<p>
Now we verify this new lockout policy has taken place by logging into one of our random users with the wrong password and seeing if we get locked out.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 18 47 PM" src="https://github.com/user-attachments/assets/b5cbfaa5-9fed-4be9-9815-ac046f7b6c91">
</p>
<p>
In DC-1 we will find the user in Active Directory Users and Computers and unlock that account.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 19 20 PM" src="https://github.com/user-attachments/assets/2532a9f6-40ac-47f5-ac1c-1dd92f5b75ed">
</p>
<p>
We will log into that account again but this time with the right password and verify that our account was unlocked.
</p>
<br />

## PowerShell Automation

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 38 51 PM" src="https://github.com/user-attachments/assets/73d96625-1fd1-48df-a14b-3955517c2832">
</p>
<p>
Open PowerShell ISE as an administrator on DC-1.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 41 33 PM" src="https://github.com/user-attachments/assets/a6142041-808d-4290-bf8a-794e3e10bfc9">
</p>
<p>
Execute a PowerShell script to create 10 thousand user accounts in the _EMPLOYEES OU.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 48 45 PM" src="https://github.com/user-attachments/assets/3f738f13-e369-4eb0-9a1e-ca374b4344d5">
</p>
<p>
Test one of the newly created accounts by logging into Client-1 (mydomain.com\[randomly chosen username the script generated])
</p>
<br />

## System Monitoring and Log Analysis

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 41 53 PM" src="https://github.com/user-attachments/assets/f4b59ba1-f617-4fc8-95ab-6e867c7e8f9f">
</p>
<p>
Log into DC-1 as jane_admin and create the following folders on the C: drive:
  
- read-access
- write-access
- no-access
- accounting
</p>
<br />

<p>
Set Permissions on Folders:
<img width="2056" alt="Screenshot 2024-10-17 at 3 43 10 PM" src="https://github.com/user-attachments/assets/4aa5e84c-badb-4860-aef6-796802ba3f89">
</p>  
<p>
For read-access: assign Domain Users the Read permission.

For write-access: assign Domain Users the Read/Write permission.

For no-access: assign Domain Admins the Read/Write permission.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 45 59 PM" src="https://github.com/user-attachments/assets/bf446b8b-c257-462f-845b-006ba4ac11fa">
</p>
<p>
On Client-1, log in as a normal user (e.g., mydomain.com\john_employee), open the Run dialog and type \\DC-1 to access the shared folders.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 47 53 PM" src="https://github.com/user-attachments/assets/cc69b911-e144-4ec3-854d-8430c913c06d">
</p>
<p>
Now we can test access to each folder:

- read-access: User should be able to view files but not modify.
- write-access: User should be able to both view and modify files.
- no-access: User should be denied access.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 50 15 PM" src="https://github.com/user-attachments/assets/4eb4eb5f-f206-4048-a330-d85c3f0ce178">
</p>
<p>
On DC-1, create a Security Group named ACCOUNTANTS in ADUC.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 51 42 PM" src="https://github.com/user-attachments/assets/f2993713-dd08-4c26-804d-ffebfe3ca24d">
</p>
<p>
Assign Read/Write permissions to the ACCOUNTANTS group on the accounting folder.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 52 33 PM" src="https://github.com/user-attachments/assets/c51f377a-8ae6-4613-88b0-8753c8cbc57d">
</p>
<p>
On Client-1, attempt to access the accounting folder as john_employee (should fail).
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 53 05 PM" src="https://github.com/user-attachments/assets/385504ce-02b4-47f0-8dc9-8d5253fecdcb">
</p>
<p>
Add john_employee to the ACCOUNTANTS security group.
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 54 47 PM" src="https://github.com/user-attachments/assets/c7f94971-d415-485b-9612-c3ff0d24133b">
</p>
<p>
Re-log into Client-1 and confirm access to the accounting folder now works.
</p>
<br />

## Security and Administrative Controls

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 27 14 PM" src="https://github.com/user-attachments/assets/b90bfe3e-9f1a-49f6-8515-5ca484a7e074">
</p>
<p>
Search for eventvwr.msc on Client-1 and run as administrator
</p>
<br />

<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 28 31 PM" src="https://github.com/user-attachments/assets/0c144f09-1ade-49fe-b1d2-ed3c715fa07e">
</p>
<p>
Navigate to Windows Logs > Security.

Here, we can view logs for:

- Successful/failed logon attempts.
- Group Policy application events.
- Network logon attempts.

Use Event IDs (e.g., 4624 for logon and 4625 for failed logon) to filter and identify specific events.
</p>
<br />

## Key Takeaways

This lab provided hands-on experience deploying and administering an enterprise Active Directory environment within Microsoft Azure. Core administrative tasks included Active Directory deployment, Group Policy management, PowerShell automation, secure file sharing, and security log monitoring. The project demonstrates foundational cloud administration, identity management, and enterprise security operations skills applicable to real-world IT and cybersecurity environments.

