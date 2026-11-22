# hybrid-identity-active-directory-entra-id-lab
Hybrid identity home lab: on-premises Windows Server Active Directory synced to Microsoft Entra ID, with password reset and user lifecycle scenarios documented as support tickets.


This project is a hands-on home lab that connects an on-premises Windows Server Active Directory environment to Microsoft Entra ID (formerly Azure AD). Users created and managed in the local domain are synchronized to the cloud, so the same employee can sign in to Microsoft cloud services with the username and password they already use inside the company network. This setup is called hybrid identity, and it is how many organizations run today.

This lab builds on my earlier project, [Domain-Controller-IT-Support-Project](https://github.com/thanzeer24/Domain-Controller-IT-Support-Project)
, which covers the domain controller, departments, Group Policy, file shares and ticketing.

## Overview

The lab has two sides:

- **On-premises:** a Windows Server domain controller running Active Directory for the domain `apex.com`, with four departments (Finance, HR, IT and Marketing) and eight employee accounts, plus two Windows 11 Pro client PCs joined to the domain.
- **Cloud:** a Microsoft Entra ID tenant that receives a copy of the on-premises users through directory synchronization.

Active Directory stays the source of truth. Accounts are created, changed and disabled on-premises, and those changes are reflected in the cloud.

## Environment

| Component | Details |
|-----------|---------|
| Domain controller | Windows Server (add your version), domain `apex.com` |
| Client PCs | 2 x Windows 11 Pro, joined to the domain |
| Cloud identity | Microsoft Entra ID (Free tier) |
| Synchronization | Microsoft Entra Provisioning Agent |
| Users | 7 employees across 4 departments |

## Step 1: Preparing Active Directory for Synchronization

### 1.1 Adding an alternative UPN suffix

Every user has a User Principal Name (UPN), which is the sign-in name in the form `user@domain`. My local domain is `apex.com`, which is not a domain I own, so it cannot be verified in the cloud. Microsoft Entra ID requires users to have a sign-in suffix that exists in the tenant. To solve this, I added the tenant's default `onmicrosoft.com` domain as an alternative UPN suffix in **Active Directory Domains and Trusts** (right-click the top node, then Properties, then UPN Suffixes). This is done once, and every user can then use it.

<img width="1024" height="715" alt="1" src="https://github.com/user-attachments/assets/fffa3324-4b0d-47a0-b37e-7bf2be59c248" />


### 1.2 Assigning the suffix to a user

Next, I changed the user's logon name suffix in **Active Directory Users and Computers**. In the user's Account tab, the dropdown next to "User logon name" now offers the new suffix. The screenshot shows Pooja Saxena from the HR department with her sign-in name ending in the cloud domain. The part before the @ stays the same, and only the domain part changes.

<img width="1024" height="709" alt="2" src="https://github.com/user-attachments/assets/dc3f3951-77c0-4389-bc43-9ba11d1aa5d2" />


### 1.3 Updating all users and verifying

I applied the same suffix to all eight employee accounts and then listed them in PowerShell to confirm every UPN was correct. A single user left on the old suffix would not sign in to the cloud properly, so this check matters.

```powershell
Get-ADUser -Filter * -SearchBase "OU=Departments,DC=apex,DC=com" | Select Name, UserPrincipalName
```

<img width="1024" height="704" alt="3" src="https://github.com/user-attachments/assets/54aa774a-75cd-4b9f-b877-b0e26a1ae705" />


## Step 2: Cloud Tenant

I signed in to the Microsoft Entra admin center and used the tenant created for this lab. The Overview page shows the tenant details and the primary domain, which is the `onmicrosoft.com` domain used as the UPN suffix in Step 1. I used the Free tier of Entra ID, which has no cost and is enough for directory synchronization.

<img width="1920" height="855" alt="4" src="https://github.com/user-attachments/assets/73fb950e-205f-482a-af08-84a918b5482d" />


## Step 3: Synchronizing Users to Microsoft Entra ID

I installed the Microsoft Entra Provisioning Agent on the Windows Server and registered it with the tenant using a cloud administrator account. The agent reads users from the on-premises domain and creates matching accounts in Entra ID. Password hash synchronization is used, so users sign in to the cloud with the same password they use on-premises.

After synchronization, the Entra **Users** list shows the eight employees. All of them have the user type **Member**, which is the normal type for internal users. Entra ID has no Organizational Units. The department OUs exist only on-premises, and in the cloud users appear in a single flat list, grouped by security groups when needed.

<img width="1920" height="863" alt="5" src="https://github.com/user-attachments/assets/07100f9b-4c1f-47e2-b0a0-6eec480b8381" />


### 3.1 Checking an individual user

Opening a synchronized user shows the profile overview, including the sign-in name that ends in the `onmicrosoft.com` domain.

<img width="1920" height="856" alt="6" src="https://github.com/user-attachments/assets/b5e3ec91-65d1-4a4b-ad06-ce6d8cbb7213" />


The properties page of the same user shows the on-premises information, such as the on-premises domain and the sync status. This confirms the account came from Active Directory and was not created manually in the cloud.

<img width="1920" height="869" alt="6a" src="https://github.com/user-attachments/assets/544a9176-0174-4698-8e09-ccfb9d4e593b" />


## Step 4: Verifying Cloud Sign-In

To prove the setup works end to end, I opened a private browser window on one of the Windows 11 PCs and went to `myaccount.microsoft.com`. I entered the synchronized user's cloud sign-in name.

<img width="1920" height="1007" alt="7" src="https://github.com/user-attachments/assets/aab80d80-f29e-48ba-9c5b-b25a5332eece" />


I then entered the same password the user has in Active Directory. The sign-in succeeded and the My Account page loaded, which confirms that both the user account and the password hash were synchronized correctly.

<img width="1920" height="1019" alt="8" src="https://github.com/user-attachments/assets/e42e3097-0707-4402-adb5-1702ee8e6adb" />


## Support Tickets

Support scenarios for this lab, each with the problem, the steps taken and the resolution, are documented as separate files in the [`tickets`](tickets/) folder.

## Skills Demonstrated

- Active Directory user and UPN suffix management
- Hybrid identity with Microsoft Entra ID
- Directory synchronization and password hash synchronization
- Microsoft Entra admin center administration
- PowerShell for Active Directory reporting
- Troubleshooting sign-in and synchronization issues
