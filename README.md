# Lab 04 — Windows Server 2025 File Server

## 1. Overview

This lab documents the creation and configuration of a Windows Server 2025 file server for the CausTech Technologies Active Directory environment.

The objective was to deploy a dedicated file server, join it to the existing Active Directory domain, configure the File Server role, create a shared company folder, and configure sharing and NTFS permissions for domain users and administrators.

The lab also covers the creation of an Active Directory security group and testing access to the shared folder from the Windows 11 domain client.

## 2. Virtual Machine Configuration

A new virtual machine was created in Oracle VirtualBox for the Windows Server 2025 file server.

The virtual machine was named `FS01`.

**Key tasks:**

* Created a new Windows Server 2025 virtual machine.
* Assigned 8 GB of RAM.
* Allocated 2 virtual CPUs.
* Selected the Windows Server 2025 installation ISO.
* Configured the virtual machine for the CausTech internal network.

**Virtual machine:**

* **Hostname:** `FS01`
* **Operating System:** Windows Server 2025
* **RAM:** 8 GB
* **CPU:** 2 cores

![File Server Virtual Machine Configuration](screenshots/01.png)
![File Server Virtual Machine Resources](screenshots/02.png)

## 3. Windows Server 2025 Installation

The Windows Server 2025 installation process was started after configuring the virtual machine.

During installation, the language and regional preferences were selected, followed by the appropriate Windows Server edition.

**Key tasks:**

* Selected the installation language.
* Selected Windows Server 2025 Standard Evaluation (Desktop Experience).
* Continued with the Windows Server installation.
* Completed the initial installation process.

![Windows Server 2025 Language Selection](screenshots/05.png)
![Windows Server 2025 Edition Selection](screenshots/08.png)

## 4. Virtual Network Configuration

After installation, the virtual machine's network adapter was configured in Oracle VirtualBox.

The network adapter was connected to the existing internal network named `CausTech-LAN`, allowing the file server to communicate with the domain controller and other lab machines.

**Key tasks:**

* Opened the FS01 virtual machine settings.
* Navigated to the Network section.
* Selected Internal Network.
* Set the network name to `CausTech-LAN`.

![FS01 Internal Network Configuration](screenshots/14.png)

## 5. Initial Windows Server Configuration

After starting the virtual machine, the Windows Server 2025 desktop was accessed for the initial configuration.

The computer name was changed to `FS01`, and the server was joined to the existing Active Directory domain.

**Key tasks:**

* Accessed the Windows Server configuration settings.
* Changed the computer name to `FS01`.
* Joined the server to the `caustech.local` domain.
* Completed the domain joining process.

**Domain controller:** `DC01`  
**Domain:** `caustech.local`  
**File server:** `FS01`

![Windows Server 2025 Desktop](screenshots/15.png)
![Changing the Computer Name to FS01](screenshots/18.png)

## 6. Network Configuration and Verification

The file server was configured to obtain its IPv4 address automatically through DHCP, using the DHCP server configured in Lab 03.

The DNS server was manually configured to use the domain controller's IP address to ensure proper domain name resolution.

**Network configuration:**

* **Hostname:** `FS01`
* **IP configuration:** DHCP
* **Assigned IPv4 address:** `192.168.10.101`
* **DNS server:** `192.168.10.10`
* **DNS server hostname:** `DC01`

After applying the network settings, Command Prompt was used to verify the assigned IP address and DNS configuration.

**Command used:**

```cmd
ipconfig /all
```

**Key tasks:**

* Configured IPv4 to obtain an address automatically.
* Configured DNS to use `192.168.10.10`.
* Verified the DHCP-assigned IP address.
* Reviewed the network configuration using `ipconfig`.

![FS01 DHCP and DNS Configuration](screenshots/24.png)
![FS01 Network Configuration Verification](screenshots/25.png)

## 7. Installing the File Server Role

After completing the initial server configuration, Server Manager was used to install the File Server role.

The required role service was selected through the Add Roles and Features Wizard.

**Key tasks:**

* Opened Server Manager.
* Started the Add Roles and Features Wizard.
* Navigated to File and Storage Services.
* Expanded File and iSCSI Services.
* Selected File Server.
* Completed the role installation.

**Installed role:**

`File and Storage Services → File and iSCSI Services → File Server`

![Add Roles and Features Wizard](screenshots/27.png)
![Selecting File Server Role](screenshots/32.png)

## 8. Creating the Company Shared Folder

After installing the File Server role, a folder structure was created for the company's shared files.

A main folder named `Shares` was created on the file server. Inside it, another folder named `Company` was created.

**Folder structure:**

```text
C:\
└── Shares\
    └── Company\
```

The `Company` folder was configured as a network share so that domain users could access it from other computers on the network.

**Key tasks:**

* Created the `C:\Shares` directory.
* Created the `Company` folder inside `Shares`.
* Opened the folder's Properties.
* Navigated to the Sharing tab.
* Opened Advanced Sharing.
* Enabled Share this folder.

![Creating the Shares Folder](screenshots/37.png)
![Creating the Company Folder](screenshots/38.png)
![Enabling Folder Sharing](screenshots/41.png)

## 9. Configuring Share Permissions

After enabling sharing, the share permissions were configured to control network access to the Company folder.

The `Everyone` entry was retained, and `Domain Admins` (`CAUSTECH\Domain Admins`) was added to the permissions list.

Full Control was granted to the Domain Admins group.

**Key tasks:**

* Opened the Advanced Sharing permissions.
* Added `CAUSTECH\Domain Admins`.
* Configured Full Control for Domain Admins.

![Adding Domain Admins to Share Permissions and Granting Full Control to Domain Admins](screenshots/43.png)

## 10. Configuring NTFS Security Permissions

In addition to share permissions, NTFS permissions were configured through the Security tab of the Company folder's Properties.

Domain Admins were granted Full Control at the folder level.

This provides administrators with the required permissions to manage the shared folder and its contents.

**Key tasks:**

* Opened the Security tab.
* Clicked Edit to modify permissions.
* Added or selected Domain Admins.
* Granted Full Control.
* Applied the permission changes.

![Company Folder Security Settings](screenshots/45.png)
![Granting Full Control to Domain Admins](screenshots/46.png)

## 11. Creating an Active Directory Security Group

After configuring the file server, the Active Directory Users and Computers console was opened on the domain controller (`DC01`).

A new security group named `Company-File-Users` was created inside the existing Users container under the CausTech Computers organizational unit.

The group was intended to organize users who require access to the company's shared files.

**Key tasks:**

* Opened Active Directory Users and Computers on DC01.
* Opened the Users container.
* Created a new security group.
* Named the group `Company-File-Users`.
* Opened the group's Properties.
* Navigated to the Members tab.
* Added `IT-Admins` as a member.

**Group name:** `Company-File-Users`

![Navigating to Active Directory Users and Computers](screenshots/48.png)
![Creating the Company-File-Users Group](screenshots/49.png)
![Company-File-Users Group](screenshots/50.png)
![Company-File-Users Members Tab](screenshots/52.png)
![Adding IT-Admins to Company-File-Users](screenshots/53.png)

## 12. Configuring IT-Admins Share Permissions

After creating the Active Directory group, the file server was accessed again to configure additional permissions.

The `IT-Admins` security group was added to the Company folder's sharing permissions.

The Change permission was allowed for IT-Admins, providing the group with the ability to modify shared files through the network.

**Key tasks:**

* Returned to FS01.
* Opened the Company folder's Advanced Sharing settings.
* Opened the share permissions.
* Added `IT-Admins` from the `caustech.local` domain.
* Selected the appropriate IT-Admins entry from the name resolution results.
* Allowed the Change permission.

![Adding IT-Admins to Share Permissions](screenshots/54.png)
![Resolving IT-Admins and IT-Users Groups](screenshots/55.png)
![Configuring IT-Admins Share Permissions](screenshots/56.png)

## 13. Configuring IT-Admins NTFS Permissions

The Security tab was opened again to configure the NTFS permissions for the `IT-Admins` group.

The Modify permission was granted, allowing members of the group to create, edit, and delete files and folders within the Company directory, subject to the configured permissions.

**Key tasks:**

* Opened the Company folder's Security tab.
* Added or selected `IT-Admins`.
* Granted the Modify permission.
* Applied the permission changes.

**Configured permissions:**

* **Domain Admins:** Full Control.
* **IT-Admins:** Modify.

![Configuring IT-Admins NTFS Permissions](screenshots/60.png)

## 14. Verifying the File Server Share

After configuring the share and security permissions, Server Manager was used to review the File and iSCSI Services section.

The Shares view displayed the configured shared folder and its local path.

**Key tasks:**

* Opened Server Manager on FS01.
* Navigated to File and Storage Services.
* Opened the Shares section.
* Verified the Company shared folder.
* Confirmed its local folder path.

**Shared folder path:** `C:\Shares\Company`

![File and iSCSI Services](screenshots/61.png)
![Verifying the Company Network Share](screenshots/62.png)

## 15. Testing Access from the Windows 11 Client

The final step was to test the shared folder from the Windows 11 domain client.

The client workstation was accessed using the domain user account belonging to John Carter.

Through File Explorer's Network section, the file server `FS01` was opened, followed by the `Company` shared folder.

The folder was accessible from the client, confirming that the network share was available.

**Key tasks:**

* Opened the Windows 11 client (`CLIENT01`).
* Accessed File Explorer.
* Navigated to the Network section.
* Located the `FS01` file server.
* Opened the `Company` shared folder.
* Verified that the shared folder was accessible.

The Company folder was empty because no files had been added yet.

![John Carter Domain User Account](screenshots/63.png)
![Accessing FS01 Through Network](screenshots/64.png)
![Company Shared Folder on CLIENT01](screenshots/65.png)

## 16. Results and Project Notes

This lab documents the deployment and configuration of a Windows Server 2025 file server integrated into the CausTech Technologies Active Directory environment.

**Work covered:**

* Created a Windows Server 2025 virtual machine named `FS01`.
* Allocated 8 GB of RAM and 2 virtual CPUs.
* Connected the server to the `CausTech-LAN` internal network.
* Joined the server to the `caustech.local` domain.
* Configured DHCP and DNS settings.
* Installed the File Server role.
* Created the `C:\Shares\Company` folder structure.
* Enabled network sharing for the Company folder.
* Configured share and NTFS permissions.
* Created the `Company-File-Users` Active Directory group.
* Added `IT-Admins` to the new group.
* Granted appropriate permissions to Domain Admins and IT-Admins.
* Verified access to the shared folder from CLIENT01.

This lab demonstrates the deployment of a centralized file-sharing service and the use of Active Directory security groups to manage access to shared resources.

This is a personal home lab created for hands-on learning and IT portfolio development. CausTech Technologies is a fictional company.

**Related projects:**

[Lab 01 — Windows Server & Active Directory](https://github.com/NicatGnc/CausTech-Lab-01-Active-Directory)

[Lab 02 — Windows 11 Domain Client](https://github.com/NicatGnc/CausTech-Lab-02-Windows-Client)

[Lab 03 — DHCP Server](https://github.com/NicatGnc/CausTech-Lab-03-DHCP-Server)

**Lab 04 — File Server**
