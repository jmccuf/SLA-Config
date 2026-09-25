<div align="center">

# osTicket Post-Installation Configuration

### Roles & Permissions · SLA Plans · Help Topic Routing

Configuring a help desk so requests reach the right team, agents have appropriate permissions, and tickets have defined service targets.

[My Cybersecurity Portfolio](https://github.com/jmccuf) · [Project Repository](https://github.com/jmccuf/SLA-Config) · [Ticket Lifecycle Lab](https://github.com/jmccuf/ticket-lifecycle)

</div>
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>

> **Scope and evidence:** This project documents an osTicket lab. The original screenshots show role names, SLA grace periods, and help-topic configuration. They do not show individual permission selections, effective agent access, or successful routing and overdue tests. The validation steps below are recommended follow-up exercises, not completed results.

## Project Overview

A ticketing system needs more than an installation. It needs a configuration that makes ownership, access, and service expectations clear. This lab focuses on three areas:

- **Roles:** Define what agents are permitted to do within their access scope.
- **SLA plans:** Establish grace periods and applicable schedules for ticket handling.
- **Help topics:** Categorize requests and provide defaults such as department, priority, and SLA where configured.

**Cybersecurity connection:** Role design supports least privilege, request routing supports accountable handoffs, and service targets help teams track overdue work. These are security-relevant foundations, not proof of a hardened production deployment or a completed incident-response program.

## Environment & Software

| Component | Purpose in the documented lab |
| :--- | :--- |
| osTicket | Help desk application used to configure roles, SLAs, and help topics |
| Microsoft Azure Virtual Machines | Hosting environment identified in the original README |
| Internet Information Services (IIS) | Windows web-service environment identified in the original README |
| Remote Desktop | Remote administration of the Windows lab |
| Windows 10 21H2 | Historical lab operating system |

For a new lab, use supported and patched software and confirm current osTicket prerequisites. osTicket is not cloud-only: it can run in compatible on-premises or hosted environments. This project covers ticket management, not a demonstrated asset-inventory or asset-management system.

## Prerequisites

1. Start with a functioning osTicket installation and authorized access to the **Admin Panel**.
2. Use test requester and agent accounts rather than real customer data.
3. Define the departments and teams needed for the lab, such as Support and Maintenance.
4. Record the time zone, business hours, holidays, and service expectations before configuring schedules.
5. Preserve a record of existing settings before changing a shared environment.
6. Keep administration access restricted and use non-administrator accounts for routine ticket handling.

Navigation and available options can vary by osTicket version. Verify labels in your installed release instead of assuming every screen is identical to the historical screenshots.

## 1. Configure Agent Roles

![image](https://github.com/justinmccuff/post-install-config/assets/143865133/71635828-8c61-48ed-93e4-c2d2355d3c58)

*Original lab evidence: All Access, Expanded Access, Limited Access, Supreme Admin, and View only are listed as active roles. The screenshot does not expose their individual permissions or assignments.*

### Walkthrough

1. Open **Admin Panel → Agents → Roles**.
2. Create or edit a role with a clear description of its intended use.
3. Review the available ticket, task, and knowledge-base permissions as applicable to your version.
4. Enable only the actions needed for the role. Pay particular attention to deletion, transfer, assignment, and other high-impact actions.
5. Save the role.
6. Open the relevant agent's access settings and apply the appropriate role for their department access. Review extended access and team membership as applicable.
7. Test using a separate agent account—not the administrator session used to configure the role.

### Suggested permission-design exercise

The table below is a design proposal, not a record of the permissions in the screenshot.

| Role purpose | Intended capability | Validation to perform |
| :--- | :--- | :--- |
| Read-only review | View permitted tickets without changing them | Attempt a permitted read and a prohibited update |
| Front-line support | Reply, add notes, and handle assigned work within the approved scope | Confirm normal work succeeds and restricted actions fail |
| Support lead | Coordinate assignment and escalation within the approved scope | Test transfer/assignment and confirm unrelated sensitive tickets remain restricted |
| Configuration administrator | Manage approved system settings | Confirm administrative access is separately controlled and not granted unnecessarily |

**Important distinctions**

- A role name such as **Supreme Admin** is just a label; it does not by itself prove administrative privileges.
- Roles govern actions, while effective access also depends on department access, assignments, teams, and configuration.
- Do not assume that a role called **View only** is read-only until its permissions and behavior have been tested.

## 2. Configure Service-Level Agreement Plans

![image](https://github.com/justinmccuff/post-install-config/assets/143865133/41b49b39-f554-44d1-b99a-9290eee9e5b1)


*Original lab evidence: the SLA list shows active plans and grace periods. It does not show schedule details, notification behavior, or which tickets received each plan.*

### Values visible in the screenshot

| Plan | Grace period shown | Status |
| :--- | :--- | :--- |
| Default SLA | 18 hours | Active; marked as default |
| Sev-A | 1 hour | Active |
| Sev-B | 4 hours | Active |
| Sev-C | 8 hours | Active |

These are **lab configuration values**, not universal severity standards. A plan named Sev-A does not automatically make it applicable to emergency-priority tickets.

### Walkthrough

1. Open **Admin Panel → Manage → SLA**.
2. Create or edit the desired plan.
3. Enter a meaningful name, grace period, and applicable schedule according to the agreed service policy.
4. Review any relevant options, including transient-plan or overdue-alert settings if available in your version.
5. Save and activate the plan.
6. Review the system default and any department, help-topic, or filter-based SLA settings that may affect assignment.
7. Create test tickets and inspect the plan and due date actually applied.

### Interpret the timing correctly

- A grace period is not proof of a guaranteed first-response or repair time.
- Schedules, working hours, holidays, explicit due dates, and configuration can affect calculated deadlines and overdue behavior.
- A customer-facing SLA agreement may include obligations that the ticketing application alone does not enforce.
- Overdue marking and alert delivery are different behaviors; validate both if notifications are required.

**Recommended test:** In an isolated lab, use a short test grace period and known schedule, record ticket creation time, applied SLA, calculated due date, and observed overdue behavior. Test notifications only with a configured mail system and test recipients. Do not change a production SLA just to accelerate a test.

## 3. Configure Help Topics & Routing

![image](https://github.com/justinmccuff/post-install-config/assets/143865133/5cde95c8-5a4b-47ca-8404-98d5c3242580)


*Original lab evidence: active public help topics with priority and department values. This screen does not prove that submitted tickets followed these routes.*

### Values visible in the screenshot

| Help topic | Priority | Department |
| :--- | :--- | :--- |
| Business Critical Outage | Emergency | Support |
| Equipment Request | Normal | Maintenance |
| Feedback | Low | Support |
| General Inquiry | Normal | Support |
| Password Reset | Normal | Support |
| Personal Computer Issues | Normal | Support |
| Report a Problem | Normal | Maintenance |
| Report a Problem / Access Issue | High | Support |

### Walkthrough

1. Open **Admin Panel → Manage → Help Topics**.
2. Add a topic with a clear requester-facing name.
3. Choose its status and public/private visibility according to the intake policy.
4. Configure a parent topic if a hierarchy will make categories easier to navigate.
5. Review the available department, priority, SLA, assignment, and form options in your installation.
6. Save the topic.
7. Submit a test request through the actual requester portal.
8. Verify the resulting ticket's department, priority, SLA, owner if configured, and visibility to the intended agents.

Help topics provide routing and defaults; they do not guarantee fast responses. Filters and other configuration can affect the final ticket settings. Public topic visibility also does **not** mean the resulting tickets are publicly visible.

### Suggested password-reset handling

The original screenshot includes a **Password Reset** topic. For a security-aware workflow:

- Follow an approved identity-verification process before making account changes.
- Never ask for a password, MFA code, or recovery code in a ticket.
- Route the request to an authorized team and record the action taken without storing secrets.
- Escalate suspicious circumstances under the incident-handling policy.

These are recommended controls; they are not demonstrated in the original screenshot.

## 4. Validate the Configuration End to End

Do not stop at seeing a setting in the Admin Panel. Test the resulting ticket behavior using non-administrator accounts.

| Test | Action | Expected result to verify | Status |
| :--- | :--- | :--- | :--- |
| Role restrictions | Sign in with a limited test agent and attempt allowed/disallowed actions | Approved actions succeed; restricted actions are unavailable or denied | Not yet documented |
| Department access | Compare an authorized and unrelated restricted account | Ticket visibility matches the intended scope | Not yet documented |
| Outage routing | Submit a Business Critical Outage request | Emergency priority and Support department, unless a documented override applies | Not yet documented |
| Equipment routing | Submit an Equipment Request | Normal priority and Maintenance department, unless a documented override applies | Not yet documented |
| SLA assignment | Submit tickets through each configured route | Applied SLA and due date match the intended configuration | Not yet documented |
| Overdue behavior | Use a short, isolated test plan | Observed overdue behavior matches schedule and configuration | Not yet documented |
| Notifications | Trigger a configured test alert | Only intended test recipients receive it | Not yet documented |

### Record each result

```text
Test name:
Test account / role:
Configuration under test:
Expected result:
Actual result:
Timestamp and time zone:
Sanitized evidence reference:
Pass / fail and follow-up:
```

Add screenshots of the resulting ticket metadata, permission checks, and SLA behavior after performing the tests. Do not mark a test as passed based only on a configuration screenshot.

## Troubleshooting

| Symptom | What to check |
| :--- | :--- |
| Agent can do more than expected | Role permissions, department/extended access, team membership, and administrator privileges |
| Ticket goes to the wrong department | Help-topic settings, filters, intake path, and later manual transfers |
| Unexpected SLA or due date | Default, department, topic, and filter settings; schedule, time zone, and explicit due date |
| Topic is not visible to a requester | Active status, public/private setting, hierarchy, and portal context |
| No overdue alert arrives | Alert settings, recipients, mail configuration, and scheduled task/cron processing as applicable |
| Screenshot does not load on GitHub | Confirm the image is beside README.md and filename capitalization matches the link |

## Skills & Evidence Summary

**Documented in the original lab:** creating active role entries, configuring SLA grace periods, and defining help topics with priorities and departments.

**To validate next:** effective least-privilege access, actual ticket routing, applied SLA timing, notification delivery, and security-aware request handling.

No production hardening, compliance certification, or measured response-time improvement is claimed by this project.

## Image Notes

The three PNGs are the original repository screenshots, downloaded and visually reviewed for accurate placement. The original README had the Help Topics and SLA screenshots under the opposite headings; this revision places them with the matching sections. The SVG is a newly created illustration, not a screenshot or verified execution result.

All four image files must be uploaded **beside `README.md` in the repository root**. No `images/` folder is needed for this version.

---

**Next project:** [Follow a ticket from intake through resolution →](https://github.com/jmccuf/ticket-lifecycle)
