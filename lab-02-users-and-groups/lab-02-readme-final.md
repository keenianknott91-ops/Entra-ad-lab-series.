# Lab 02 — Users & Groups

**Completed:** 09/18/2026
**Time spent:** [total hours]

---

## Objective

Build out the Cove Harbor Logistics directory, organize it with groups, and work four realistic identity lifecycle tickets against it — onboarding, name change, termination, and transfer.

---

## Directory built

10 users across Operations, Warehouse, Finance, HR, Sales, and IT, each with job title, department, and office location populated. Attributes were filled deliberately rather than left blank, since dynamic group membership and later Conditional Access targeting both depend on them.

---

## Steps performed

**1. Manual user creation**
- Created three users through the Entra admin center (Identity → Users → All users → New user)
- Set UPN, display name, initial password, and populated Properties: job title, department, office location

![Entra ID All users list showing the Cove Harbor Logistics directory](screenshots/all-users.png)

**2. Bulk creation via CSV**
- Downloaded the bulk create CSV template from Bulk operations → Bulk create
- Populated the remaining seven users, preserving the template's instruction and header rows
- Uploaded and monitored the job under Bulk operation results

**3. Group creation**

| Group | Type | Membership | Purpose |
| --- | --- | --- | --- |
| `SG-Warehouse-Staff` | Security | Assigned | Access control for warehouse staff; owner assigned |
| `M365-Operations-Team` | Microsoft 365 | Assigned | Collaboration — shared mailbox, calendar, SharePoint |
| `SG-All-Baltimore-Staff` | Security | Assigned | Nested group containing `SG-Warehouse-Staff` plus direct members |
| `DG-Finance-Auto` | Security | Dynamic User | Automatic membership by department attribute |

![Group membership blade](screenshots/group-members.png)

**4. Dynamic membership**

Rule used:

```
(user.department -eq "Finance")
```

Membership populated automatically from the department attribute rather than manual assignment. Dynamic evaluation is not instantaneous — membership took several minutes to reflect after the rule was saved.

![Dynamic membership rule syntax](screenshots/dynamic-rule.png)

---

# Tickets worked

---

## Ticket #2041 — New hire setup: Caleb Nowak

**Requested by:** Sofia Reyes (HR)
**Status:** Resolved
**Date:** 09/18/2026
**Time spent:** 15 min

**Request**
Create an account for new dispatcher Caleb Nowak (starts Monday) with the same access as Dana Whitfield. Due EOD Friday.

**Work performed**

1. Checked Dana Whitfield's account (reference user) for job title, department, office location, and group memberships.
2. Created the user account in Entra ID:
   - Display name: Caleb Nowak
   - Username: `cnowak@covehl.onmicrosoft.com`
   - Job title: Dispatcher
   - Department: Operations
   - Office location: Baltimore
3. Added to the same groups as Dana:
   - `M365-Operations-Team`
   - `SG-All-Baltimore-Staff`
4. Set a temporary password with "require password change on next sign-in" enabled.
5. Verified: test sign-in prompted for a password change, and Caleb's group memberships match Dana's.

**Resolution**
Account created and matched to Dana Whitfield's title, department, office, and group access. The temporary password was delivered to Sofia Reyes separately and is not stored in this ticket. Caleb will be required to set a new password at first sign-in.

**Note on method**
Rather than assuming which groups a dispatcher needs, I opened the reference employee's profile and copied actual membership. Assuming access based on job title is how people end up over- or under-provisioned.

![Caleb Nowak account created with matching attributes and group membership](screenshots/ticket-2041-new-hire.png)

---

## Ticket #2042 — Name change: Priya Raman → Priya Raman-Osei

**Requested by:** Priya Raman
**Status:** Escalated to Tier 2
**Date:** 09/18/2026
**Time spent:** 10 min

**Request**
User's last name changed to Raman-Osei after her marriage. Her email and display name still show the old name.

**Work performed**

1. Confirmed the name change with HR before making any changes.
2. Updated the fields within Tier 1 scope:
   - Last name: Raman-Osei
   - Display name: Priya Raman-Osei
3. Did not change the sign-in name (UPN) or primary email address. Changing these changes how the user signs in and can affect saved credentials, the Outlook and Teams profile, OneDrive links, and single sign-on applications. This requires Tier 2 to plan and schedule.

**User communication**
Informed Priya that her display name has been updated. Explained that her email address and sign-in name will be changed by Tier 2, that we will contact her before that happens so she knows when to start using the new sign-in name, and that her password will not change.

**Escalation to Tier 2**
Requested a change of the UPN and primary SMTP address to `praman-osei@covehl.onmicrosoft.com`, retaining `praman@covehl.onmicrosoft.com` as an alias so mail sent to the old address still delivers. Asked Tier 2 to coordinate timing with the user.

**Why this split**
Display name is what colleagues see, so updating it resolves the visible problem immediately at no authentication risk. The UPN is the sign-in name, and changing it is a coordinated change rather than a silent edit. Worth noting that the UPN and the primary email address are separate attributes — changing one does not change the other.

![Updated display name and surname on the user profile](screenshots/ticket-2042-name-change.png)

---

## Ticket #2043 — Termination: Diego Moreno

**Requested by:** Marisol Vega (Operations Manager)
**Priority:** High
**Status:** Resolved (mailbox preservation escalated to Tier 2)
**Date:** 09/18/2026
**Time spent:** 20 min

**Request**
Diego Moreno is no longer with the company as of today. Cut his access immediately. Legal requires his mailbox to be preserved.

**Work performed**

1. Verified the termination request with Marisol Vega by phone at 2:10 PM.
2. Captured screenshots of group memberships and license assignments before making any changes.
3. **Blocked sign-in** — set Account enabled to No at 2:14 PM.
4. **Revoked all active sessions** at 2:15 PM, signing him out of every device and application he was currently signed into.
5. **Reset the password** so the previous credentials no longer work.
6. **Removed group memberships:**
   - `M365-Operations-Team`
   - `SG-All-Baltimore-Staff`
7. Left in place deliberately:
   - The Exchange license. Removing it would delete the mailbox after 30 days, which conflicts with the legal hold.
8. **Did not delete the account**, per legal's instruction to preserve the mailbox.

**Why the order matters**
Blocking sign-in prevents new authentication, but access tokens already issued remain valid until they expire. Without revoking sessions, a user who is already signed in keeps working. Block, then revoke, then remove access.

**Verification**
- User profile shows Account enabled: No
- Sign-in logs show no successful sign-ins after 2:14 PM
- Group memberships match the list in step 7

**Escalation to Tier 2**
Requested mailbox preservation as directed by legal (litigation hold, or conversion to a shared mailbox). Flagged that the license is still assigned and must not be removed until the hold is in place. Also requested follow-up on recovering the company laptop and phone and removing them from device management.

**Communication**
Informed Marisol Vega that access was removed at 2:15 PM and that mailbox preservation has been escalated to Tier 2.

![User profile showing sign-in blocked and sessions revoked](screenshots/ticket-2043-termination.png)

---

## Ticket #2044 — Transfer: Claire Sutton (Warehouse → Operations)

**Requested by:** Sofia Reyes (HR)
**Effective date:** 10/01/2026
**Status:** Resolved
**Date:** 09/18/2026
**Time spent:** 20 min

**Request**
Claire Sutton is moving from Warehouse to Operations effective the 1st. Update her department and job title, remove her from `SG-Warehouse-Staff`, and add her to `M365-Operations-Team`.

**Work performed**

1. In production these changes would be scheduled for the effective date so Claire keeps Warehouse access until she moves. Lab note: completed early for practice.
2. Recorded current group memberships before making changes.
3. Checked the membership type of each group — both `SG-Warehouse-Staff` and `M365-Operations-Team` are Assigned, so both changes had to be made by hand.
4. Updated her profile:
   - Department: Warehouse → Operations
   - Job title: Warehouse Associate → Operations Coordinator
   - Manager: set to Marisol Vega (Operations Manager)
5. Updated group memberships:
   - Removed from `SG-Warehouse-Staff`
   - Added to `M365-Operations-Team`
6. Checked for other Warehouse-only access (groups, shared mailboxes, applications). None found.

**Nested group gotcha**
Claire was a member of `SG-All-Baltimore-Staff` only through the nested `SG-Warehouse-Staff` group, not directly. Removing her from Warehouse silently dropped her Baltimore access as well — nothing in the removal dialog warns you about this. I added her to `SG-All-Baltimore-Staff` directly to restore it.

This is the practical argument for auditing effective membership rather than direct membership when moving someone between teams.

**Assigned vs dynamic**
Both groups in this ticket were Assigned, so neither updated on its own when her department changed. By contrast, `DG-Finance-Auto` is attribute-driven — when I tested a department change against that rule, membership updated automatically within a few minutes with no manual step. Attribute-driven groups self-correct; assigned groups drift the moment someone moves.

**Verification**
- Profile shows Department: Operations and Job title: Operations Coordinator
- No longer a member of `SG-Warehouse-Staff`
- Member of `M365-Operations-Team` and `SG-All-Baltimore-Staff`
- Access to the group's Teams team and SharePoint site may take a few hours to appear

**Communication**
Informed Sofia Reyes and Claire that the transfer is complete, and told Claire the Operations Teams team may take a few hours to show up.

![Updated profile and group membership after transfer](screenshots/ticket-2044-transfer.png)

---

## Issues encountered

[The CSV upload. Write out what the Bulk operation results actually reported, how you diagnosed it, and what the fix was. If both uploads succeeded and the second only errored on duplicates, say that — describing how you verified actual tenant state instead of assuming it is the point worth making.]

---

## Verification

- [ ] 10 users present with attributes populated
- [ ] Bulk operation results reviewed, no unresolved failures
- [ ] Four groups created, correct types and membership
- [ ] Dynamic rule evaluating correctly
- [ ] Terminated account blocked and sessions revoked, not deleted
- [ ] Transferred user's group membership reflects new department

---

## Takeaways

[Two or three sentences in your own words. Strong candidates from this lab: what nested group membership hid from you, why assigned groups drift, or what the bulk upload taught you about verifying state rather than assuming it.]

---

**Next:** Lab 03 — Password Resets & MFA
