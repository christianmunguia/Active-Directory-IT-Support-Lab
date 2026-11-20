# Active Directory Lab Setup

## Overview

This project simulates a small business Active Directory environment using Windows Server 2022 and Windows 11 in Oracle VirtualBox.

The lab was designed to develop hands-on experience with Windows Server administration, Active Directory, DNS, Group Policy, file sharing, user account management, and PowerShell automation.

Five simulated IT help desk scenarios were completed and documented using osTicket, including troubleshooting steps, resolutions, and verification.

## Lab Environment

| Component | Configuration |
|---|---|
| Virtualization Platform | Oracle VirtualBox |
| Domain Controller | DC01 — Windows Server 2022 |
| Client Workstation | WS01 — Windows 11 |
| Active Directory Domain | `corp.local` |
| Server Roles | Active Directory Domain Services (AD DS), DNS Server |
| Virtual Network | NAT Network (`ADNetwork`) |
| Ticketing System | osTicket, hosted on a separate Ubuntu VM |

## Network Configuration

DC01 and WS01 were configured on the same VirtualBox NAT network to support domain authentication, DNS resolution, and communication between the virtual machines.

| Setting | DC01 | WS01 |
|---|---|---|
| IPv4 Address | `10.0.2.15` | `10.0.2.16` |
| Subnet Mask | `255.255.255.0` | `255.255.255.0` |
| Default Gateway | `10.0.2.1` | `10.0.2.1` |
| Preferred DNS Server | `127.0.0.1` | `10.0.2.15` |

DC01 hosts the DNS service for the `corp.local` domain. WS01 uses DC01 as its preferred DNS server for internal domain name resolution.

## Initial Configuration

1. Created Windows Server 2022 and Windows 11 virtual machines in Oracle VirtualBox.
2. Configured both virtual machines to use the same NAT network (`ADNetwork`).
3. Renamed the Windows Server machine to `DC01` and the Windows 11 workstation to `WS01`.
4. Configured DC01 with a static IPv4 address of `10.0.2.15`, subnet mask `255.255.255.0`, default gateway `10.0.2.1`, and preferred DNS server `127.0.0.1`.
5. Installed the Active Directory Domain Services role on DC01.
6. Promoted DC01 to a domain controller and created a new forest named `corp.local`.
7. Configured WS01 to use DC01 (`10.0.2.15`) as its preferred DNS server.
8. Joined WS01 to the `corp.local` domain.
9. Verified that WS01 appeared in Active Directory Users and Computers.
10. Successfully signed in to WS01 using a domain user account.

## Active Directory Organization

### Organizational Units

The following organizational units were created to organize accounts and administrative objects.

| Organizational Unit | Purpose |
|---|---|
| Engineering | Engineering department accounts |
| Management | Management department accounts |
| IT | IT department accounts |
| Groups | Organization of security groups |

### Security Groups

| Security Group | Purpose |
|---|---|
| `EngineeringShare` | Controls access to the Engineering shared folder |
| `GPO-Wallpaper-Pilot` | Used for Group Policy security filtering |
| `Management_Users` | Used for Management department group membership |

### Test Accounts

The lab uses simulated employee accounts to reproduce common IT support requests.

| Account | Lab Purpose |
|---|---|
| John Doe (`jdoe`) | Domain sign-in verification and EngineeringShare access troubleshooting |
| Damian Wayne | Account lockout and password reset |
| Jason Todd | Group Policy wallpaper troubleshooting |
| Jalen Brunson | DNS and domain connectivity troubleshooting |
| Marcus Reed (`mreed`) | New employee onboarding and PowerShell automation |

These accounts are fictional lab identities. Their configurations and group memberships were used to test different administrative and troubleshooting scenarios.

### Group Policy

**GPO:** `SetEngineeringBackground`

Created a Group Policy Object to configure desktop wallpaper settings.

Security filtering was used to control which users received the policy. The GPO was also used to reproduce and troubleshoot a wallpaper-related help desk issue.

### File Sharing

**Shared Resource:** `EngineeringShare`

Configured a shared folder with access managed through the `EngineeringShare` security group.

NTFS permissions and group membership were used to control access rather than assigning permissions directly to individual users.

The shared folder was used to simulate an access-denied issue, investigate the user's group membership, restore access, and verify the resolution.

## osTicket Integration

The help desk ticketing system was hosted on a separate Ubuntu virtual machine from the Windows/Linux troubleshooting lab.

The existing osTicket installation was reused to document the Active Directory support scenarios.

Simulated end users submitted help desk requests, and a technician account was used to investigate, document, and resolve the incidents.

Each ticket records the reported issue, troubleshooting process, corrective action, and verification results.

The ticketing system is separate from the `corp.local` domain environment and is used for incident documentation rather than Active Directory authentication.

## Environment Verification

The environment was validated through configuration checks and hands-on testing.

| Verification Area | Method |
|---|---|
| Network Configuration | Checked IPv4 addressing and network settings using `ipconfig` |
| Domain Membership | Confirmed WS01 was joined to `corp.local` |
| Active Directory | Verified the WS01 computer object in Active Directory Users and Computers |
| Domain Authentication | Successfully signed in to WS01 using the `jdoe` domain account |
| File Share Access | Tested access to `EngineeringShare` using security group membership |
| Group Policy | Tested wallpaper policy application and security filtering |
| DNS Connectivity | Performed DNS and network troubleshooting tests during the connectivity scenario |
| PowerShell Automation | Verified new user creation and group membership in Active Directory |

Supporting screenshots are maintained in the `screenshots` directory.

## Help Desk Scenarios

Five simulated IT help desk incidents were completed using this environment.

| Ticket | Scenario | Skills Demonstrated |
|---|---|---|
| 01 | Account Lockout and Password Reset | Account administration, unlocking accounts, password resets |
| 02 | File Share Access Denied | Security groups, NTFS permissions, access troubleshooting |
| 03 | Group Policy Wallpaper Troubleshooting | Group Policy Management, security filtering, policy troubleshooting |
| 04 | DNS and Domain Connectivity Troubleshooting | DNS configuration, network diagnostics, domain connectivity |
| 05 | New Employee Onboarding | PowerShell automation, AD user creation, group membership verification |

Each scenario is documented separately with the issue description, investigation, resolution, verification, and supporting evidence.

The PowerShell onboarding script is maintained separately in the project's `scripts` directory.

## Scope and Limitations

This project was completed in a controlled home lab rather than a production business environment.

- The environment uses a single domain controller and one domain-joined Windows 11 workstation.
- VirtualBox NAT networking provides the virtual network infrastructure.
- Employee accounts, departments, and help desk requests are simulated.
- Real employee identity verification and management approval processes were not performed.
- Where required, technician-controlled test sign-ins were used to verify account access and troubleshooting results.
- The environment does not implement production-level redundancy, high availability, or centralized monitoring.
- Passwords, temporary credentials, and other authentication secrets are excluded from the public repository.

## Project Purpose

This lab demonstrates practical experience with Windows Server administration, Active Directory, DNS, Group Policy, security groups, file permissions, and PowerShell.

By reproducing common help desk issues, investigating their causes, implementing corrective actions, and documenting the outcomes, the project demonstrates troubleshooting and technical documentation skills relevant to entry-level IT support roles.
