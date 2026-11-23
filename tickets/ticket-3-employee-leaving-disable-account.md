# Ticket 3: Employee Leaving, Disable Account

| Field | Details |
|-------|---------|
| Ticket ID | SUP-8 |
| Type | Offboarding request |
| Raised by | HR department user |
| Assigned to | IT technician |
| Department of leaving employee | Marketing |
| Tools used | Active Directory Users and Computers, Microsoft Entra admin center |
| Status | Resolved |

## Summary

HR informed IT that an employee in the Marketing department was leaving the company that day and asked for the account to be disabled. The IT technician disabled the account in Active Directory and removed the user from the Marketing security group. After synchronization, the account showed as disabled in Microsoft Entra ID as well. The technician then revoked the user's active cloud sessions, tested the sign-in on a domain-joined Windows 11 PC to confirm the account was blocked, and closed the ticket with an internal note.

## 1. The request

The HR user opened a new request in Jira Service Management, explaining that a Marketing employee was leaving that day and the account should be disabled.

<img width="1280" height="720" alt="a" src="https://github.com/user-attachments/assets/267d2640-c677-41d0-a9d9-bbb05515263d" />


On the IT side, the technician saw the ticket in the Jira Service Management queue overview.

<img width="1280" height="720" alt="12" src="https://github.com/user-attachments/assets/794b3823-cb9b-4801-ad87-d430154c3f0f" />


The technician clicked **Assign to me**, so the rest of the team knew the ticket was being handled.

<img width="1280" height="720" alt="a1" src="https://github.com/user-attachments/assets/0e873bfa-3f8e-4506-8308-5a85fec48d09" />


## 2. Disabling the account in Active Directory

On the domain controller, the technician opened Active Directory Users and Computers and found the employee inside the Marketing OU.

<img width="1024" height="768" alt="b" src="https://github.com/user-attachments/assets/063faa7d-721b-429f-9476-fea5b5d92613" />


The technician right-clicked the account and selected **Disable Account**. The account is disabled and not deleted, because a leaving employee's files, mailbox and records may still be needed by the company, and a deleted account is difficult to bring back.

<img width="1024" height="768" alt="c" src="https://github.com/user-attachments/assets/06983787-a3e7-4953-8327-7bfda351e958" />


Windows asked for confirmation, and the technician clicked **OK**.

<img width="1024" height="768" alt="d" src="https://github.com/user-attachments/assets/8c0cd2b0-ce29-4cef-a0bd-144629af62b8" />


Next, the employee was removed from the Marketing security group. Folder access in this lab is given through groups, so removing the membership takes away access to the Marketing shared folder as well.

<img width="1024" height="768" alt="e" src="https://github.com/user-attachments/assets/502dee3b-4912-43f1-a463-1c4d7a141100" />


## 3. Checking the cloud

After the next synchronization, the change reached Microsoft Entra ID without any manual work in the cloud. The employee's account in Entra ID shows as disabled, which confirms that the status change in Active Directory was synchronized.

<img width="1920" height="1080" alt="f" src="https://github.com/user-attachments/assets/e5b2f729-01b7-4367-80d1-bc3dcf569fe3" />


Disabling an account stops new sign-ins, but sessions that were already open can stay valid for a while. To close that gap, the technician opened the user's profile in Entra ID and clicked **Revoke sessions**, which forces every existing session to sign in again, and a disabled account cannot do that.

<img width="1920" height="1080" alt="g" src="https://github.com/user-attachments/assets/1ca73c69-75cc-41ad-9947-ab814a4b120c" />


## 4. Testing the block on a domain-joined PC

To prove the account is really locked out, the technician tried to sign in with the employee's credentials on a Windows 11 PC joined to the domain. Windows refused the sign-in with the message that the account is disabled.

<img width="1599" height="899" alt="g1" src="https://github.com/user-attachments/assets/10209b70-87c1-4991-8805-f7e415d85832" />


## 5. Closing the ticket

The technician marked the ticket as resolved.

<img width="1280" height="720" alt="h" src="https://github.com/user-attachments/assets/2123bedc-b5f9-4bc5-a0c4-6cdbaca95f04" />


An internal note was added, recording what was done: account disabled, group membership removed, sessions revoked, and sign-in tested. Internal notes are visible to the IT team only, so they work as an audit record if anyone asks later what happened to the account.

<img width="1280" height="720" alt="i" src="https://github.com/user-attachments/assets/be8b6f95-40f1-41ff-adb8-06c9ea2c1b0b" />


The ticket overview now shows the request as resolved.

<img width="1280" height="720" alt="j" src="https://github.com/user-attachments/assets/6ad75940-5989-4e76-a563-e77577ba3444" />


## Resolution

The employee's account was disabled in Active Directory, the user was removed from the Marketing security group, the disabled status synchronized to Microsoft Entra ID, active cloud sessions were revoked, and a test sign-in on a Windows 11 PC confirmed that access was blocked.

## Root cause and prevention

This was a standard offboarding request, not a fault. To handle leavers safely and consistently:

- Disable the account first and delete it later, after the company's retention period, so data and records are not lost
- Disable on the employee's last working day and not before, unless HR says otherwise
- Remove group memberships so no access is left behind
- Revoke cloud sessions, because disabling alone does not always end sessions that are already open
- Confirm the request really comes from HR or a manager, since a fake offboarding request could be used to lock out a real employee
- Record each step in the ticket so there is an audit trail
