
# Enterprise Active Directory Administration Lab

Building and managing a complete Windows domain from scratch, end to end.

## Objective

Design, deploy, and manage a fully functional Windows Server Active Directory domain in a self-hosted virtual lab — covering everything from initial Domain Controller promotion through the day-to-day administrative tasks a help-desk or junior sysadmin role requires: user and group management, client onboarding, password/account support, delegated administration, and full user lifecycle management.

## Environment

- Windows Server 2022 (Evaluation) VM, promoted to Domain Controller `DC01` for a new forest, `lab.local`
- Windows 11 VM as a domain-joined client
- Both hosted in VirtualBox on a Bridged network adapter so they share a real LAN segment

## Process

### Phase 1: Domain Controller Deployment
Installed Windows Server 2022 (Desktop Experience) on a fresh VM, configured a static IP and hostname, then installed the Active Directory Domain Services role. Promoted the server to a Domain Controller for a new forest, which simultaneously configured integrated DNS. Verified the promotion by confirming the domain resolved correctly and Active Directory Users and Computers displayed the new domain with its default containers.
![image Alt] ()

### Phase 2: Organizational Structure — OUs, Users, Groups
Designed an Organizational Unit structure mirroring a small company (Sales, IT, Finance), each representing a distinct department. Created test user accounts within each OU with enforced password-change-at-next-logon, and built department-aligned security groups (e.g., `Sales-Team`), populating membership through both the GUI and by editing user "Member Of" properties — demonstrating both common paths to the same result.
![image Alt] ()

### Phase 3: Client Domain Join
Configured a Windows 11 client to use the Domain Controller as its DNS server — the critical dependency for a successful domain join — verified name resolution, then joined the client to the domain. Confirmed the join succeeded both from the client (successful domain login) and from the server side (new computer object appearing in Active Directory Users and Computers).
![image Alt] ()

### Phase 4: Password Resets & Account Lockouts
Practiced the most common help-desk request — password resets — through Active Directory Users and Computers. Deliberately triggered an account lockout by repeated bad password attempts to validate the domain's lockout policy, then resolved it via the Unlock Account option. Used Event Viewer's Security log (Event ID 4740) on the Domain Controller to identify the originating computer of the lockout — a real-world technique for diagnosing lockouts caused by cached credentials rather than user error.

### Phase 5: Delegated Administration
Created a limited-privilege "help-desk" account and used the Delegation of Control wizard to grant it password-reset rights scoped to a single OU only — rather than granting full Domain Admin rights, following the principle of least privilege. Verified the account could reset passwords for users inside its assigned OU, but was correctly denied when attempting the same action against users in a different OU, confirming the delegation boundary held.
![image Alt] ()

### Phase 6: User Lifecycle Management
Documented and executed the full lifecycle of an employee account: onboarding with correct group assignment, a mid-lifecycle department transfer (OU move plus group membership update), temporary disablement for leave (without deleting the account), and a full offboarding sequence — disable, random password reset, removal from all groups, and relocation to a dedicated "Disabled Users" OU ahead of a scheduled deletion after a retention window.
![image Alt] ()

## Skills Demonstrated

| Skill | Where It Shows Up |
|---|---|
| Domain Controller setup | AD DS role installation, forest promotion, integrated DNS configuration |
| Organizational design | OU structure mirroring real department boundaries |
| User & group administration | Account creation, group membership via GUI and object properties |
| Client management | DNS-dependent domain join and domain-account authentication |
| Help-desk operations | Password resets, account unlocks, lockout-source diagnosis via Event Viewer |
| Least-privilege administration | Delegated Control scoped to a single OU, with boundary verified |
| Lifecycle governance | Full onboarding-to-offboarding process including retention-based deletion |

## Outcome

This lab produced a fully working, self-managed Active Directory domain and validated proficiency across the tasks that make up the bulk of entry-level IT support and junior sysadmin work — not just configuring settings, but confirming each one actually behaves as intended (lockouts trigger and resolve, delegation boundaries hold, domain joins succeed) rather than assuming success from the wizard completing.

Screenshots documenting each phase — DC promotion, OU/user creation, client join confirmation, lockout and unlock evidence and lifecycle steps — are included in the `/screenshots` folder of this repo.

