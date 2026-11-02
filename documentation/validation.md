# Lab 04 — File Server Validation

## Environment

* File Server: `FS01`
* Operating System: Windows Server 2025
* Active Directory Domain: `caustech.local`
* Domain Controller: `DC01`
* File Server IP Address: `192.168.10.101`
* DNS Server: `192.168.10.10`
* Network: `CausTech-LAN`
* Shared Folder: `C:\Shares\Company`
* Network Share: `\\FS01\Company`

## Validation Checklist

| Test | Expected Result | Status |
| -------------------------------- | ---------------------------------------------- | ------ |
| Virtual machine configuration | FS01 configured with 8 GB RAM and 2 CPUs | Passed |
| Network configuration | FS01 connected to `CausTech-LAN` | Passed |
| Domain joining | FS01 successfully joined to `caustech.local` | Passed |
| DHCP configuration | FS01 obtains its IPv4 address automatically | Passed |
| IP configuration | FS01 receives `192.168.10.101` | Passed |
| DNS configuration | DNS server configured as `192.168.10.10` | Passed |
| File Server role installation | File Server role successfully installed | Passed |
| Folder structure | `C:\Shares\Company` created | Passed |
| Share configuration | Company folder configured as a network share | Passed |
| Share permissions | Domain Admins granted Full Control | Passed |
| NTFS permissions | Domain Admins granted Full Control | Passed |
| Active Directory group | `Company-File-Users` security group created | Passed |
| Group membership | `IT-Admins` added to `Company-File-Users` | Passed |
| IT-Admins share permissions | Change permission configured | Passed |
| IT-Admins NTFS permissions | Modify permission configured | Passed |
| Share verification | Company share visible in Server Manager | Passed |
| Client access | CLIENT01 can access `FS01\Company` through Network | Passed |

## Conclusion

The Windows Server 2025 File Server deployment and configuration tasks were completed in the existing CausTech Technologies lab environment.

FS01 was successfully integrated into the `caustech.local` domain, configured to obtain its IP address through DHCP, and deployed with the File Server role.

The `Company` folder was shared over the network, and share and NTFS permissions were configured for Domain Admins and IT-Admins. An Active Directory security group named `Company-File-Users` was also created to organize file access management.

Access to the shared folder was verified from the Windows 11 domain client, CLIENT01.

The screenshots in the repository provide supporting evidence of the configuration and validation steps.
