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
<img width="1592" height="917" alt="Image" src="https://github.com/user-attachments/assets/6c55e4ce-b705-457b-880a-08554f430eb0" />
<br />

<p>
Authenticate using the administrative account for continued domain administration tasks.
<p>
<img width="1919" height="868" alt="Image" src="https://github.com/user-attachments/assets/0f9ac43f-d040-4fd9-81e5-742c04ee6aef" />

<br />

## Group Policy Administration
Navigate to the ``Client-1`` system settings and open “Rename this PC (advanced)” to begin the Active Directory domain join process.
<p>
<img width="1596" height="912" alt="Image" src="https://github.com/user-attachments/assets/7726dcfd-e610-4f6a-a4ee-7595c0d7b7cb" />
<br />
  
Configure ``Client-1`` to join the Active Directory domain using the domain name configured during the Active Directory deployment.

Authenticate using Domain Administrator credentials to authorize the domain join operation.

Restart ``Client-1`` to apply the domain membership configuration changes.

<img width="1596" height="917" alt="Image" src="https://github.com/user-attachments/assets/48763e06-f9f4-464d-ba55-768c50273f86" />

<br />
<p>

## Account Lockout Policy Configuration  
Launched gpmc.msc on the ``DC-1`` Domain Controller to configure Account Lockout Policy settings, including lockout duration and failed logon thresholds.
<p>
<img width="1593" height="932" alt="Image" src="https://github.com/user-attachments/assets/cffc9b6f-11e3-4889-bff9-818bcaaf18dd" />

<br />
<p>
  
Logged into ``Client-1`` using Domain Administrator credentials and executed gpupdate /force to apply the newly configured Group Policy settings across the workstation.
<p>
<img width="1592" height="917" alt="Image" src="https://github.com/user-attachments/assets/9c015cb0-4e2b-4688-8f27-cdbcc591a708" />

<br />
<p>

Executed ``gpresult /r`` from an elevated Command Prompt on ``Client-1`` to validate successful application of domain Group Policy configurations.
<p>
<img width="1595" height="916" alt="Image" src="https://github.com/user-attachments/assets/f1cfcc85-8255-4a2a-adb5-3b1bb62d34f0" />
  
<br />
<p>

Verified the configured Account Lockout Policy by attempting multiple failed logon attempts with a domain user account, confirming that the account was locked after exceeding the defined authentication threshold.
<p>
<img width="1919" height="871" alt="Image" src="https://github.com/user-attachments/assets/c6234925-e773-4e58-9921-832d63cdf998" />

<br />
<p>

Located the locked domain user account in Active Directory Users and Computers on ``DC-1`` and unlocked the account to restore authentication access.
</p>
<img width="1595" height="937" alt="Image" src="https://github.com/user-attachments/assets/26eb5cde-9239-4fe5-8f46-8df5ba57f9c4" />

<br />
<p>

Verified restored authentication access after unlocking the domain account.
</p>
<img width="1737" height="892" alt="Image" src="https://github.com/user-attachments/assets/6437bfeb-3a96-4135-9dc5-0ccb862f00cc" />

<br />
</p>

## PowerShell Automation
Launch PowerShell ISE with administrative privileges on the `DC-1` Domain Controller.
<p>
<img width="2056" alt="Screenshot 2024-10-17 at 2 38 51 PM" src="https://github.com/user-attachments/assets/73d96625-1fd1-48df-a14b-3955517c2832">

<br />
<p>

Execute a PowerShell automation script to provision 1,000 domain user accounts within the `_EMPLOYEES` Organizational Unit. 
<p>
<img width="1205" height="811" alt="Image" src="https://github.com/user-attachments/assets/59511a8d-65a4-4bff-b149-cc73287f449e" />

<br />
<p>

Validate successful account provisioning by authenticating to `Client-1` using a randomly selected domain user account generated by the automation script.
<p>
<img width="1796" height="913" alt="Image" src="https://github.com/user-attachments/assets/03757317-7bcf-49e4-9fea-2c5786e6bd63" />

<br />
<p>

## Secure File Share Configuration
Log into ``DC-1`` as ``jane_admin`` and create the following folders on the C: drive:

- `read-access`
- `write-access`
- `no-access`
- `accounting`
<img width="1572" height="912" alt="Image" src="https://github.com/user-attachments/assets/23f4bd00-4076-4e6f-9ef3-9b395bce3eee" />

<br />

<p>
  
Set Permissions on Folders:
- `For read-access`—  assign Domain Users the Read permission.

- `For write-access` — assign Domain Users the Read/Write permission.

- `For no-access` — assign Domain Admins the Read/Write permission.
<img width="1572" height="913" alt="Image" src="https://github.com/user-attachments/assets/2f7a8fb9-7115-4c1a-962d-8d3bb6074d06" />

<br />
<p>

On `Client-1`, log in as a standard domain user (e.g., `MYDOMAIN\john_employee`), open the Run dialog, and enter `\\DC-1` to access the shared folders configured on the domain controller.
<p>
<img width="1568" height="916" alt="Image" src="https://github.com/user-attachments/assets/9c6c54e1-4d6f-4977-87a2-a91fec94533a" />

<br />
<p>
  
Test access permissions for each shared folder:

- `read-access` — Verify the user can open files but cannot edit or delete them.
- `write-access` — Verify the user can create, modify, and delete files.
- `no-access` — Verify the user receives an access denied message.
<img width="1573" height="913" alt="Image" src="https://github.com/user-attachments/assets/5365b4cb-0616-4444-a8f7-2144886a0349" />

<br />
<p>

On `DC-1`, open **Active Directory Users and Computers (ADUC)** and create a security group named `ACCOUNTANTS` for managing access permissions to the accounting shared folder.
<p>
<img width="1568" height="916" alt="Image" src="https://github.com/user-attachments/assets/7f62a9c4-744b-44bb-b596-5912464c4fa3" />

<br />
<p>

Assign `Read/Write` NTFS and share permissions to the `ACCOUNTANTS` security group for the `accounting` shared folder to provide controlled access for authorized accounting users.
<p>
<img width="1570" height="913" alt="Image" src="https://github.com/user-attachments/assets/3ac57ecb-5519-4c3b-aac8-5525c2c04126" />

<br />
<p>

On `Client-1`, attempt to access the `accounting` shared folder using the `aturner` account and verify that access is denied due to insufficient permissions.
<p>
<img width="1573" height="913" alt="Image" src="https://github.com/user-attachments/assets/140d6775-61fe-48a4-ae7a-aab9e3e0c2f9" />

<br />
<p>

Add `aturner` to the `ACCOUNTANTS` security group.
<p>
<img width="1567" height="912" alt="Image" src="https://github.com/user-attachments/assets/d826ebc1-c4e1-48d3-ae7f-877765010c92" />

<br />
<p>

Re-log into `Client-1` and confirm access to the accounting folder now works. 
<p>
<img width="1576" height="917" alt="Image" src="https://github.com/user-attachments/assets/f29a1c16-49e5-4497-ba9e-d1669004ed43" />

<br />
<p>
  
## Security Hardening and Administrative Controls
- Implemented access control through Active Directory security groups, NTFS permissions, and Group Policy enforcement to support least-privilege administrative practices within the domain environment.

- Validated authentication security controls through Account Lockout Policy configuration and failed logon monitoring.

- Monitored authentication and administrative activity using Windows Event Viewer security auditing logs.

## System Monitoring and Log Analysis
Launch Event Viewer on `Client-1` by opening `eventvwr.msc` with administrative privileges to monitor authentication activity and security auditing logs.
<p>
<img width="1572" height="916" alt="Image" src="https://github.com/user-attachments/assets/73dddb57-83d5-488d-9de3-0929b117f1d8" />

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
<img width="1571" height="857" alt="Image" src="https://github.com/user-attachments/assets/ac604f49-e8d4-4782-89fa-bd8e3bbbb9d4" />

<br />
<p>

## Key Takeaways

This lab provided hands-on experience deploying and administering an enterprise Active Directory environment within Microsoft Azure. Core administrative tasks included Active Directory deployment, Group Policy management, PowerShell automation, secure file sharing, and security log monitoring. The project demonstrates foundational cloud administration, identity management, and enterprise security operations skills applicable to real-world IT and cybersecurity environments.

