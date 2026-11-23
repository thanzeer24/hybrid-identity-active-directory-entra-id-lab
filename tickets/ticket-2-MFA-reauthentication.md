# Ticket 2: MFA Re-registration After a Phone Change

| Field | Details |
|-------|---------|
| Ticket ID | SUP-9 |
| Type | Multifactor authentication (MFA) request |
| Raised by | HR department user |
| Assigned to | IT technician |
| Tool used | Microsoft Entra admin center, Microsoft Authenticator |
| Status | Resolved |

## Summary

An HR employee changed her phone, so her Microsoft Authenticator app was no longer linked to her account and she could not complete the MFA prompt. She raised a ticket asking for her authentication to be set up again. The IT technician required her to re-register multifactor authentication from the Microsoft Entra admin center. She then registered Microsoft Authenticator on her new phone, and the ticket was closed with an internal note.

## 1. The request

The HR user opened a new request in Jira Service Management and explained that she had changed her phone and needed to authenticate again.

<img width="1280" height="669" alt="25" src="https://github.com/user-attachments/assets/08f6d1fa-9013-4927-afc7-983a6950bdbc" />


On the IT side, the technician saw the ticket in the queue overview.

<img width="1280" height="660" alt="12" src="https://github.com/user-attachments/assets/120ba651-1c14-4eae-ba42-bd3d53b5f0f8" />


The technician clicked **Assign to me**, so the rest of the team knew the ticket was being handled.

<img width="1280" height="662" alt="26" src="https://github.com/user-attachments/assets/28ac1a4a-96d5-40ba-bf72-990a8e08cf3c" />


## 2. The user's current state

Before changing anything, the HR user's session in Microsoft Entra ID shows she is signed in and her account is active. The account itself is healthy. Only the authentication method tied to her old phone needs to be replaced.

<img width="1920" height="1017" alt="27" src="https://github.com/user-attachments/assets/d8bd5a24-4f58-410c-9fd5-098ca3da2a20" />


## 3. Requiring MFA re-registration

In the Microsoft Entra admin center, the technician opened the user's **Authentication methods** page and selected **Require re-register multifactor authentication**, then confirmed with **OK**. This clears the user's old MFA registration, so at her next sign-in she must set up MFA again. Her password and account stay unchanged.

<img width="1920" height="1007" alt="28" src="https://github.com/user-attachments/assets/0d71734c-ac48-463b-a0de-6069f85fd3a8" />


## 4. The user registers the new phone

On her next sign-in, the HR user saw a prompt telling her that she needs to set up authentication again.

<img width="1920" height="1011" alt="29" src="https://github.com/user-attachments/assets/29e38d83-d07a-4051-8df5-700c8d27b99f" />


She was then guided to install the **Microsoft Authenticator** app on her new phone and link it to her work account by scanning the code shown on the page.

<img width="1920" height="1014" alt="30" src="https://github.com/user-attachments/assets/c0ca044b-4b7b-4d29-b0c3-c22a666d8537" />


Once the app was linked, the authentication method was added successfully, and her account is protected by MFA again with the new device.

<img width="1920" height="1014" alt="31" src="https://github.com/user-attachments/assets/1528961f-7df8-4b22-a5f0-618f6c52eeaa" />


## 5. Closing the ticket

The technician added an internal note recording what was done and that the issue was solved. The note stays visible to the IT team only and is a record for similar requests in the future.

<img width="1280" height="664" alt="32" src="https://github.com/user-attachments/assets/f4eb9321-d636-4772-8bc1-a583c829b002" />


The ticket was marked as resolved.

<img width="1280" height="660" alt="33" src="https://github.com/user-attachments/assets/d81327e9-b370-4185-93c9-457611cf440e" />


## Resolution

The technician required MFA re-registration for the user in Microsoft Entra ID. The user installed Microsoft Authenticator on her new phone and registered it, so she can sign in with MFA again.

## Root cause and prevention

The old phone was the only registered authentication method, so changing phones locked the user out of MFA. To prevent this and handle similar requests safely:

- Verify the user's identity before resetting MFA (for example, a call back to a known number or a confirmation from their manager), because an attacker could pretend to be an employee who "lost a phone"
- Ask users to register a second method, such as a backup phone number, before replacing a phone
- Tell users to move or re-add Microsoft Authenticator before wiping their old phone
- Record each MFA reset in the ticket so there is an audit trail
