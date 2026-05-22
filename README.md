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
- NTFS & SMB Permissions
- Windows 11 (Client)
- PowerShell ISE for automation
- Group Policy Management
- Windows Event Viewer for log monitoring
- Active Directory Domain Services (AD DS)

## Infrastructure Deployment

<p>
  
  Deploy a Windows Server 2022 virtual machine for the ``Domain Controller (DC-1).``
  
  Deploy a Windows 11 virtual machine for the client workstation ``(Client-1).``

  Ensure both virtual machines are configured within the same Resource Group and Virtual Network (VNet).
  <p>
<img width="1918" height="868" alt="Image" src="https://github.com/user-attachments/assets/3c993ab8-2b47-4454-af5e-d3b2714f6bc6" />
</p>
<p>

  Configure a static private IP address for ``DC-1``.
<p>
<img width="1858" height="868" alt="Image" src="https://github.com/user-attachments/assets/bce73495-d872-481c-8153-f7177b96ecef" />
</p>
<p>
Configure Client-1 to use DC-1 as its DNS server and restart the virtual machine to apply the changes.

<p>
<img width="1919" height="864" alt="Image" src="https://github.com/user-attachments/assets/2c6bc8a4-d58b-411b-a180-4da5aa20809c" />
</p>
<p>
Validate network connectivity between Client-1 and DC-1 using PowerShell commands such as:

- ping
- ipconfig /all
<p>
<img width="1592" height="915" alt="Image" src="https://github.com/user-attachments/assets/5bbc60aa-4154-405e-8fcb-083c8c118dd2" />
</p>

Run `ipconfig /all` and verify that ``DC-1’s`` private IP address is configured as the active DNS server.
<p>
<img width="1595" height="917" alt="Image" src="https://github.com/user-attachments/assets/08b67a07-e166-4ead-8304-e45d7073b199" />

<br />

## Active Directory Configuration
Open Server Manager and install the Active Directory Domain Services (AD DS) role through the “Add Roles and Features” wizard.

After installation, promote ``DC-1`` to a Domain Controller by configuring a new Active Directory forest and specifying a custom domain name (e.g., mydomain.com).

Restart the server to complete the Active Directory deployment and domain controller configuration.
<p>
<img width="1500" height="827" alt="Image" src="https://github.com/user-attachments/assets/75b90be4-a8f1-4a10-8db3-e90cb2d8ad19" />
</p>

Authenticate to the domain using the newly configured administrative account.
<p>
<img width="1919" height="868" alt="Image" src="https://github.com/user-attachments/assets/b4f743ef-b2d6-45ea-919d-79603c01231a" />

<br />

## Organizational Unit and User Management
Create Organizational Units (OUs) for administrative organization, including:

- _EMPLOYEES
- _ADMINS
- _CLIENTS
<p>
<img width="1593" height="892" alt="Image" src="https://github.com/user-attachments/assets/188fb48f-6fbd-44c1-9453-0d011d19986f" />
</p>

Create a dedicated administrative user account named `jane_admin`.
<p>
<img width="1596" height="912" alt="Image" src="https://github.com/user-attachments/assets/2e8b2732-b3ce-46aa-b26a-a0ce87e0a988" />

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
Navigate to the ``Client-1`` system settings and open “Rename this PC (advanced)” to begin the Active Directory domain join process.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 01 16 PM" src="https://github.com/user-attachments/assets/f7ad8cc4-cbb7-4c41-b2d5-7bcd7b2e3c37">
<br />
  
Configure ``Client-1`` to join the Active Directory domain using the domain name configured during the Active Directory deployment.

Authenticate using Domain Administrator credentials to authorize the domain join operation.

Restart ``Client-1`` to apply the domain membership configuration changes.

<img width="2056" alt="Screenshot 2024-10-17 at 2 02 43 PM" src="https://github.com/user-attachments/assets/8b14a2eb-120f-4086-b401-68671b5f46f0">

<br />
<p>

## Account Lockout Policy Configuration  
Launched gpmc.msc on the ``DC-1`` Domain Controller to configure Account Lockout Policy settings, including lockout duration and failed logon thresholds.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 07 21 PM" src="https://github.com/user-attachments/assets/0d730396-b70d-47a5-92af-479c447065ab">

<br />
<p>
  
Logged into ``Client-1`` using Domain Administrator credentials and executed gpupdate /force to apply the newly configured Group Policy settings across the workstation.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 12 40 PM" src="https://github.com/user-attachments/assets/0484278e-1fa7-4461-b5d6-4444d5c0b331">

<br />
<p>

Executed gpresult from an elevated Command Prompt on ``Client-1`` to validate successful application of domain Group Policy configurations.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 14 05 PM" src="https://github.com/user-attachments/assets/30a7f0bc-40a3-48de-9c52-c00a05511f3b">
  
<br />
<p>

Verified the configured Account Lockout Policy by attempting multiple failed logon attempts with a domain user account, confirming that the account was locked after exceeding the defined authentication threshold.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 17 15 PM" src="https://github.com/user-attachments/assets/c436404c-4ff6-4c99-80e1-d6b432f303f3">

<br />
<p>

Located the locked domain user account in Active Directory Users and Computers on ``DC-1`` and unlocked the account to restore authentication access.
</p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 18 47 PM" src="https://github.com/user-attachments/assets/b5cbfaa5-9fed-4be9-9815-ac046f7b6c91">

<br />
<p>

Verified restored authentication access after unlocking the domain account.
</p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 19 20 PM" src="https://github.com/user-attachments/assets/2532a9f6-40ac-47f5-ac1c-1dd92f5b75ed">

<br />
</p>

## PowerShell Automation
Launch PowerShell ISE with administrative privileges on the `DC-1` Domain Controller.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 38 51 PM" src="https://github.com/user-attachments/assets/73d96625-1fd1-48df-a14b-3955517c2832">

<br />
<p>

Execute a PowerShell automation script to provision 10,000 domain user accounts within the `_EMPLOYEES` Organizational Unit. 
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 41 33 PM" src="https://github.com/user-attachments/assets/a6142041-808d-4290-bf8a-794e3e10bfc9">

<br />
<p>

Validate successful account provisioning by authenticating to `Client-1` using a randomly selected domain user account generated by the automation script.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 48 45 PM" src="https://github.com/user-attachments/assets/3f738f13-e369-4eb0-9a1e-ca374b4344d5">

<br />
<p>

## Secure File Share Configuration
Log into ``DC-1`` as ``jane_admin`` and create the following folders on the C: drive:

- `read-access`
- `write-access`
- `no-access`
- `accounting`
<img width="2056" alt="Screenshot 2024-10-17 at 3 41 53 PM" src="https://github.com/user-attachments/assets/f4b59ba1-f617-4fc8-95ab-6e867c7e8f9f">

<br />

<p>
  
Set Permissions on Folders:
- `For read-access`—  assign Domain Users the Read permission.

- `For write-access` — assign Domain Users the Read/Write permission.

- `For no-access` — assign Domain Admins the Read/Write permission.
<img width="2056" alt="Screenshot 2024-10-17 at 3 43 10 PM" src="https://github.com/user-attachments/assets/4aa5e84c-badb-4860-aef6-796802ba3f89">

<br />
<p>

On `Client-1`, log in as a standard domain user (e.g., `MYDOMAIN\john_employee`), open the Run dialog, and enter `\\DC-1` to access the shared folders configured on the domain controller.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 45 59 PM" src="https://github.com/user-attachments/assets/bf446b8b-c257-462f-845b-006ba4ac11fa">

<br />
<p>
  
Test access permissions for each shared folder:

- `read-access` — Verify the user can open files but cannot edit or delete them.
- `write-access` — Verify the user can create, modify, and delete files.
- `no-access` — Verify the user receives an access denied message.
<img width="2056" alt="Screenshot 2024-10-17 at 3 47 53 PM" src="https://github.com/user-attachments/assets/cc69b911-e144-4ec3-854d-8430c913c06d">

<br />
<p>

On `DC-1`, open **Active Directory Users and Computers (ADUC)** and create a security group named `ACCOUNTANTS` for managing access permissions to the accounting shared folder.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 50 15 PM" src="https://github.com/user-attachments/assets/4eb4eb5f-f206-4048-a330-d85c3f0ce178">

<br />
<p>

Assign `Read/Write` NTFS and share permissions to the `ACCOUNTANTS` security group for the `accounting` shared folder to provide controlled access for authorized accounting users.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 51 42 PM" src="https://github.com/user-attachments/assets/f2993713-dd08-4c26-804d-ffebfe3ca24d">

<br />
<p>

On `Client-1`, attempt to access the `accounting` shared folder using the `john_employee` account and verify that access is denied due to insufficient permissions.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 52 33 PM" src="https://github.com/user-attachments/assets/c51f377a-8ae6-4613-88b0-8753c8cbc57d">

<br />
<p>

Add `john_employee` to the `ACCOUNTANTS` security group.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 53 05 PM" src="https://github.com/user-attachments/assets/385504ce-02b4-47f0-8dc9-8d5253fecdcb">

<br />
<p>

Re-log into `Client-1` and confirm access to the accounting folder now works. 
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 54 47 PM" src="https://github.com/user-attachments/assets/c7f94971-d415-485b-9612-c3ff0d24133b">

<br />
<p>
  
## Security Hardening and Administrative Controls
- Implemented access control through Active Directory security groups, NTFS permissions, and Group Policy enforcement to support least-privilege administrative practices within the domain environment.

- Validated authentication security controls through Account Lockout Policy configuration and failed logon monitoring.

- Monitored authentication and administrative activity using Windows Event Viewer security auditing logs.

## System Monitoring and Log Analysis
Launch Event Viewer on `Client-1` by opening `eventvwr.msc` with administrative privileges to monitor authentication activity and security auditing logs.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 27 14 PM" src="https://github.com/user-attachments/assets/b90bfe3e-9f1a-49f6-8515-5ca484a7e074">

<br />
<p>

Navigate to `Windows Logs > Security` within Event Viewer to review domain authentication and security auditing events.

Analyze security logs for:

- Successful and failed authentication attempts
- Group Policy processing events
- Network logon activity

Use Event IDs such as:

- `4624` — Successful Logon
- `4625` — Failed Logon

Filter and analyze specific authentication events to support troubleshooting and security monitoring activities.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 3 28 31 PM" src="https://github.com/user-attachments/assets/0c144f09-1ade-49fe-b1d2-ed3c715fa07e">

<br />
<p>

## Key Takeaways

This lab provided hands-on experience deploying and administering an enterprise Active Directory environment within Microsoft Azure. Core administrative tasks included Active Directory deployment, Group Policy management, PowerShell automation, secure file sharing, and security log monitoring. The project demonstrates foundational cloud administration, identity management, and enterprise security operations skills applicable to real-world IT and cybersecurity environments.

