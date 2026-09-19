[README_1.md](https://github.com/user-attachments/files/32151866/README_1.md)
# Entra ID & Active Directory Lab Series

A hands-on identity administration lab built to practice the day-to-day work of an IT help desk / support role: user lifecycle, authentication, access control, and hybrid identity.

Everything here was built and broken in a personal lab tenant. Each lab includes the steps I ran, the problems I hit, and how I resolved them.

**Fictional company:** Cove Harbor Logistics — a small logistics company with staff across Operations, Warehouse, Finance, HR, Sales, and IT.

---

## Environment

| Component | Details |
| --- | --- |
| Cloud identity | Microsoft Entra ID |
| Tenant | Microsoft 365 Business Premium trial |
| On-premises | Windows Server (Active Directory Domain Services) |in progress 
| Hypervisor | [Hyper-V} in progress 
| Ticketing | osTicket | in progress

---

## Labs

| # | Lab | Status |
| --- | --- | --- |
| 01 | [Environment Setup](./lab-01-environment-setup/) — tenant provisioning, admin hardening, break-glass account | ✅ Complete |
| 02 | Users & Groups — manual and bulk provisioning, security vs M365 vs dynamic groups | ✅
| 03 | Password Resets & MFA — SSPR, admin-initiated resets, MFA registration | ⬜ Not started |
| 04 | Group Policy — on-premises GPO configuration | ⬜ Not started |
| 05 | Licensing — license assignment, group-based licensing | ⬜ Not started |
| 06 | Conditional Access — policy design, testing, break-glass exclusions | ⬜ Not started |
| 07 | Entra Connect Sync — hybrid identity, on-prem to cloud synchronization | ⬜ Not started |
| 08 | Guest & External Access — B2B collaboration, guest lifecycle | ⬜ Not started |
| 09 | Device Join & Intune — device registration, basic endpoint management | ⬜ Not started |
| 10 | Auditing & Reporting — sign-in logs, audit logs, troubleshooting workflows | ⬜ Not started |

---

## Skills demonstrated

- Microsoft Entra ID administration (users, groups, roles, authentication methods)
- Active Directory Domain Services
- Identity lifecycle management: onboarding, transfers, offboarding
- Multi-factor authentication and self-service password reset
- Conditional Access policy design
- Hybrid identity synchronization
- Help desk ticket workflows and documentation

---

## Note on security

No credentials, tenant secrets, or real personal information appear in this repository. Tenant identifiers are redacted in screenshots.
