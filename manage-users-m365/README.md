# Managing Users in the Microsoft 365 Admin Center

In this lab I created a cloud user in the Microsoft 365 admin center, set their password, location, profile details and admin roles, checked that it was a cloud-only account, and then deleted the account as if the person had left the company.

## About this lab

I did this lab in my own Microsoft 365 tenant while working through the user management module of my SC-300 (Microsoft Identity and Access Administrator) course at QA. The course shows the idea; the build, the screenshots and these notes are mine. The user is a made-up test account. Phone numbers and address details are hidden in the screenshots.

## Goal

Practise the first and last steps of a user's life at a company: set up a new account, and close it when they leave.

## What I built

- A cloud user called **Peter Obi** on my custom domain
- Job title, department and office filled in on the profile
- A password I chose, with "change password at first sign-in" turned on
- The **Exchange Administrator** and **SharePoint Administrator** roles
- A check of the account's sync status
- The account deleted at the end, as a leaver

## Steps

### 1. Open Active users
In the Microsoft 365 admin center I went to **Users**, then **Active users**. This page lists every account in the tenant.

![Active users page](images/01-active-users-page.png)

### 2. Add a user and fill in the basics
I chose **Add a user**, then entered first name, last name and display name, a username (`pobi`), and my custom domain.

![Add a user: basics](images/02-add-user-basics.png)

### 3. Choose the location and licence
I set the location to **United Kingdom**. I created the user **without a product licence**. The screen marks this "not recommended" because the user may have limited or no access to Microsoft 365 until a licence is assigned. My tenant also had no free Microsoft 365 Business Premium licence left.

![Location and licence](images/03-location-and-licence.png)

### 4. Fill in the profile
Under Optional settings I added a job title (IAM Engineer), department (Engineering) and office (Birmingham), plus contact details, which are hidden here.

![Job title, department and office](images/04-profile-job-details.png)

![Address and contact details](images/05-profile-address-details.png)

### 5. Set the password
I chose my own password instead of an auto-generated one. The screen checks the strength and shows the rules (8 to 256 characters, three of four character types). I ticked **Require this user to change their password when they first sign in**.

![Password settings](images/06-password-settings.png)

### 6. Choose the role
The default is **User (no admin center access)**.

![Default role: user with no admin access](images/07-roles-default-user.png)

I chose **Admin center access** instead and ticked **Exchange Administrator** and **SharePoint Administrator**. The screen reminds you to give people only the access they need, using the least-permissive role.

![Exchange and SharePoint administrator roles](images/08-roles-exchange-sharepoint-admin.png)

### 7. Review and finish
The review page lists the username, password type, licences (none), roles and profile information. I checked it and chose **Add**. The next page confirmed that the user was added to active users.

![Review settings](images/09-review-settings.png)

![User added](images/10-user-added.png)

### 8. Check the sync status
Back in Active users, the **Sync status** column shows a cloud icon for Peter Obi. That means this is a cloud-only account. A user synced from an on-premises Active Directory has to be changed in that directory, not here. I saw this in my Hybrid Identity lab, where I could not edit a synced group in Entra ID.

![Sync status shows a cloud user](images/11-sync-status-cloud.png)

### 9. Delete the user (a leaver)
I imagined that Peter Obi had left. I selected the account and chose **Delete user**. The prompt says a deleted user, and their data except calendar items and aliases, can be restored for up to 30 days.

![Delete user prompt](images/12-delete-user-prompt.png)

![User deleted](images/13-user-deleted.png)

## Result

The cloud user was created with the details and roles I chose, showed as a cloud account, and was deleted. The account can be restored from the Deleted users list for 30 days.

## What I learned

- A cloud user is created and deleted in the admin center. A synced user must be created and deleted in the on-premises directory, then synced.
- Choosing the password type, "change at first sign-in" and the role at creation saves rework later.
- I gave this test user two admin roles. In real work I would give a new starter no admin role unless their job needs it.
- A leaver's mailbox and OneDrive files are kept for a limited time after deletion, so I should think about what needs to be saved before deleting the account.

## Not covered

I created the user without a licence, so I did not assign Exchange Online or SharePoint Online licences. I also did not restore the deleted user, and I did not delete a synced user.

## Skills shown

Microsoft 365 admin center, user lifecycle (joiner and leaver), password settings, admin roles, least privilege, cloud and synced accounts.
