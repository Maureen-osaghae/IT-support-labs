# Hybrid Identity Home Lab: Windows Server 2025 and Microsoft Entra ID

I built a small "hybrid identity" lab at home. A Windows Server 2025 domain controller holds my users and groups, and Microsoft Entra Connect Sync copies them to Microsoft Entra ID in the cloud. I then used it to practise real help desk tickets.

Many small and mid-sized companies run identity this way: accounts are created in on-premises Active Directory, synced to the cloud, and protected with multi-factor authentication (MFA).

**Full write-up with screenshots:** [Hybrid_Identity_Home_Lab.pdf](Hybrid_Identity_Home_Lab.pdf)

## What I built

- A Windows Server 2025 virtual machine in VirtualBox, running Active Directory Domain Services and DNS
- A free Microsoft Entra ID tenant with a verified custom domain
- Microsoft Entra Connect Sync (Express Settings, password hash sync) joining the two
- An OU, test users and security groups, and MFA with Microsoft Authenticator

## Environment

| Part | Details |
|---|---|
| Host | Windows 10 (64-bit), 4 CPUs, 20 GB RAM |
| Hypervisor | Oracle VirtualBox 7.2.4 |
| Server VM | Windows Server 2025 (build 26100), 4 GB RAM, 1 vCPU, 50 GB disk |
| Network | Bridged adapter, static IP |
| Cloud | Microsoft Entra ID Free |
| Sync tool | Microsoft Entra Connect Sync 2.6.92.0 |

## Help desk tickets I practised

| Ticket | What I did | Result |
|---|---|---|
| New user must register for MFA | Checked Security Defaults, registered Microsoft Authenticator, added a backup method | User signs in with an approval prompt |
| Password reset | Reset the password in AD, ran a delta sync | New password worked in the cloud after about 3 minutes |
| Leaver | Disabled the account in AD, ran a delta sync | Sign-in blocked; a real offboarding should also revoke sessions |
| Group change | Added the user to the group in AD, ran a delta sync | Group updated in Entra; editing it in Entra was blocked because AD owns the group |

## What went wrong, and how I fixed it

**Problem:** Entra Connect setup stopped on the Configure page with "could not configure application-based authentication for this server."

**What I checked:**
1. Read the trace log, which showed the software had installed but the configuration was not finished.
2. Confirmed I was using a cloud-only Global Administrator on a verified custom domain.
3. Checked time and network. The server's clock had not synced for months, and the time zone was set to US Pacific, eight hours behind UK time. The server could reach Microsoft sign-in on port 443.
4. Checked that Windows was up to date.
5. Removed the half-finished install, deleted leftover app registrations in Entra, and ran setup again.

**Fix:** I set the time zone to GMT Standard Time, set reliable time sources, and reinstalled. Setup then completed.

**Honest note:** I also switched to a dedicated cloud-only admin account at the same time, so I cannot prove which change was decisive. Time is my best explanation, because this sign-in method relies on certificates and tokens that depend on the clock.

**What I learned:**
- Read the logs before changing settings.
- When a sign-in or certificate step fails, check time, time zone, DNS, network access and account type first.
- A half-finished install can block a retry, so know how to remove it cleanly.
- "Connector is busy" is not always a failure. It can mean a sync is already running.

## Tests

| Test | Result |
|---|---|
| New AD user appears in Entra after a delta sync | Pass |
| User signs in with their AD password | Pass |
| MFA registration with Microsoft Authenticator | Pass |
| AD password reset reflected in the cloud | Pass |
| Disabled AD user is blocked from cloud sign-in | Pass |
| AD group membership change syncs | Pass |

## Next steps

- Add a Windows 11 VM and join it to the domain
- Create Group Policy objects (password policy, drive mapping)
- Assign Microsoft 365 licences to synced users and practise mailbox, Teams and OneDrive tickets
- Set up self-service password reset
- Install a free ticketing tool such as osTicket and log each practice scenario
- Practise reading Entra sign-in logs and Conditional Access results

## Skills shown

Virtualisation, Windows Server, Active Directory, DNS, Microsoft Entra ID, Entra Connect Sync, password hash sync, MFA, PowerShell, reading logs, documentation.
