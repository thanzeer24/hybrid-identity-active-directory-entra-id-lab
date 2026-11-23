# Ticket 1: New Employee Account

| Field | Details |
|-------|---------|
| Ticket ID | SUP-7 |
| Type | New user request |
| Raised by | HR department user |
| Assigned to | IT technician |
| Department of new user | Marketing |
| Status | Resolved |

## Summary

HR raised a ticket asking for an account for a new employee joining the Marketing department. The IT technician created the account in Active Directory, added the user to the Marketing security group, confirmed that the account synchronized to Microsoft Entra ID, and tested the sign-in on both the cloud portal and a domain-joined Windows 11 PC. The ticket was then closed with an internal note.

## 1. The request

The HR user opened a new request in Jira Service Management, described the new joiner and the department they belong to, and submitted it to the IT team.

<img width="1280" height="720" alt="11" src="https://github.com/user-attachments/assets/184a0a47-0169-46e3-b9fc-a91d8d0cb18d" />


On the IT side, the technician opened the queue overview, where the new ticket appeared among the open requests.

<img width="1280" height="720" alt="12" src="https://github.com/user-attachments/assets/e13df639-4e47-413d-9541-54b00ecd85a5" />


The technician clicked **Assign to me** so the rest of the team could see that someone was already working on it. This prevents two technicians from handling the same ticket.

<img width="1280" height="720" alt="13" src="https://github.com/user-attachments/assets/c3a5759d-ceef-4540-8ddc-54d5e1057e00" />


## 2. Creating the account in Active Directory

On the domain controller, the technician opened Active Directory Users and Computers and started creating a new user inside the Marketing OU, since that is the department the employee belongs to.

<img width="1024" height="768" alt="14" src="https://github.com/user-attachments/assets/8798d8f5-1205-47d6-90de-55a66325c404" />


The first and last name and the logon name were entered. The logon name uses the cloud suffix that was prepared earlier in the lab, so the account can sign in to Microsoft Entra ID after synchronization.

<img width="1024" height="768" alt="15" src="https://github.com/user-attachments/assets/c729aa35-711d-411d-baef-c16774c4ee67" />


A temporary password was set, and **User must change password at next logon** was ticked. This way the technician never knows the user's real password, because the employee chooses their own on the first sign-in.

<img width="1024" height="768" alt="16" src="https://github.com/user-attachments/assets/dee0c06b-e8f9-4f88-9606-0052d81fd9a4" />


The wizard finished and the account was created.

<img width="1024" height="768" alt="17" src="https://github.com/user-attachments/assets/203f4399-0ad3-433d-b7a2-62e7fd3bb0bc" />


The new employee now appears inside the Marketing OU.

<img width="1024" height="768" alt="18" src="https://github.com/user-attachments/assets/ed8e0449-574e-46be-ae45-b0e386c2dcae" />


Finally, the user was added to the Marketing security group. Folder access in this lab is given to groups and not to individuals, so this single step gives the new employee access to the Marketing shared folder.

<img width="1024" height="768" alt="18a1" src="https://github.com/user-attachments/assets/482d5359-9537-4a2c-8890-4c70112dc480" />



## 3. Testing on a domain-joined PC

This test is the one the employee will actually use every day. On a Windows 11 PC joined to the domain, the new user signed in with the temporary password.

<img width="1600" height="900" alt="19" src="https://github.com/user-attachments/assets/94b3d2ae-99e9-4a6d-8d59-460ac765d0d6" />


Windows told the user that the password must be changed before signing in, which is the behavior set during account creation.

<img width="1600" height="900" alt="20" src="https://github.com/user-attachments/assets/6f21f584-fb05-43cc-b8c4-f16df529d429" />


The user typed a new personal password and reached the desktop.

<img width="1600" height="900" alt="21" src="https://github.com/user-attachments/assets/8a56cafe-256e-46d4-8aaa-156a399a3e4a" />


In Windows Settings, the account page shows the new user's name and sign-in address, which confirms the computer is using the correct domain identity.

<img width="1280" height="720" alt="22" src="https://github.com/user-attachments/assets/4eacae91-c1bd-4c86-a2b5-740a98e09cd2" />


## 4. Checking the cloud

After the synchronization ran, the new user appeared in the Microsoft Entra ID users list. Nothing was created manually in the cloud. The account came from Active Directory.

<img width="1920" height="1080" alt="18a" src="https://github.com/user-attachments/assets/156b452a-38c5-4568-9c9f-959db10c9839" />


To confirm that the cloud account works, the user signed in to the Entra ID portal with the temporary password.

<img width="1920" height="1080" alt="18b" src="https://github.com/user-attachments/assets/83ee9944-c79c-46af-954b-3262c214eb43" />


The user's overview page in Entra ID shows the account details and confirms it was synchronized from the on-premises directory.

<img width="1920" height="1080" alt="18c" src="https://github.com/user-attachments/assets/8e639e4d-7488-4fbb-b4d0-f9764495dfa2" />


## 5. Closing the ticket

The technician added an internal note to the ticket describing what was done, and updated it. Internal notes are visible to the IT team only, so they serve as a record if a similar request comes up later.

<img width="1280" height="720" alt="23" src="https://github.com/user-attachments/assets/64c704e7-df66-41a1-815a-c75e2c2e73f1" />


The ticket was marked as resolved.

<img width="1280" height="720" alt="24" src="https://github.com/user-attachments/assets/8a8d3040-48f4-4e14-8cb9-42eb3cdc37b9" />


## Resolution

A new Marketing employee account was created in Active Directory with a forced password change at first logon, added to the Marketing security group, synchronized to Microsoft Entra ID, and verified through both a cloud sign-in and a domain sign-in on a Windows 11 PC.

## Root cause and prevention

This was a standard onboarding request and not a fault. To keep these requests quick and consistent:

- Always create the user in the correct department OU
- Always use the correct UPN suffix, because a wrong one stops the account from syncing properly
- Always give access through security groups, never directly to the user
- Always force a password change at first logon, and never put a password in the ticket
