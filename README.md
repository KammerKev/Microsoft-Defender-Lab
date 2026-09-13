# Home Microsoft Defender Lab

## Objective

This project establishes a home lab environment to build hands-on skills in **Microsoft Defender for Endpoint** and the broader Microsoft Defender / Microsoft 365 security stack. The lab uses a VM running Windows 10 Pro, licensed under a Microsoft 365 E5 trial, to explore Defender's detection, investigation, and hunting capabilities. The goal is to learn the platform's functionality hands-on — running built-in attack simulations, exploring the Defender portal, and eventually writing Advanced Hunting (KQL) queries that correlate activity across Defender tables, such as tracing an incident by access token.

## Skills Learned

*[Update as you go]*

- Creating a Windows 10 Pro ISO and using it to build a VM in VMware
- Troubleshooting VM configuration issues (e.g., disk allocation/space) during OS install
- Provisioning a Microsoft 365 E5 trial tenant and licensing a device for Defender
- Navigating the Microsoft Defender portal (security.microsoft.com)
- *(Add more as skills are developed: attack simulations, Advanced Hunting/KQL, incident correlation, etc.)*

## Tools Used

- **Lab VM:** Windows 10 Pro, installed in VMware Workstation from a self-built ISO
- **Licensing:** Microsoft 365 E5 free trial (provides Defender for Endpoint Plan 2, Defender for Office 365 Plan 2, and Advanced Hunting)
- **Security Platform:** Microsoft Defender portal (security.microsoft.com)
- **Query/Analysis:** Microsoft Defender Advanced Hunting (Kusto Query Language / KQL) — *planned*

## Steps

### Step 1: Build the Lab VM

- Downloaded Microsoft Windows 10 Pro and converted the installer to an ISO
- Used the ISO to create a new virtual machine in VMware
- Hit an initial configuration issue during install — resolved by reallocating more disk space to the VM
- Successfully installed Windows 10 Pro on the VM

### Step 2: License the Lab for Defender

- Went to the Microsoft website and signed up for a free trial of **Microsoft 365 E5**
- Once provisioned, gained access to the Microsoft Defender portal and associated security tooling

### Step 3: Orient in the Software

- Spending the next several days exploring the Microsoft Defender portal to understand its layout and core functionality
- Learning how Defender tracks and logs activity/access across a network (devices, identities, sign-ins)

### Step 4: Learn Defender for Endpoint Functionality

Plan for hands-on exercises to build familiarity with Defender for Endpoint:

- **Run the built-in attack simulations.** Defender for Endpoint ships with guided, safe simulation scenarios specifically for learning purposes, found under **Endpoints > Evaluation & tutorials > Tutorials & simulations** in the Defender portal. Available scenarios include:
  - *Document drops backdoor* — simulates a socially engineered lure document launching a backdoor
  - *PowerShell script in fileless attack* — simulates a fileless attack, useful for seeing attack surface reduction and behavioral detection in action
  - *Automated incident response* — triggers Defender's automated investigation and remediation
  - Each scenario comes with its own walkthrough document and a downloadable simulation file/script to run on the test device (all benign — won't actually harm the VM)
- **Try Attack Simulation Training** under **Email & Collaboration > Attack simulation training**, included with the M365 E5 trial, for phishing/credential-harvest style simulations against test mailboxes
- **Explore the Incidents & Alerts view** — after running a simulation, see how Defender aggregates related alerts into a single incident and review the incident graph/story
- **Review the Device Timeline** for the test VM to see raw activity logged around the simulated attack

### Step 5: Advanced Hunting (KQL) — Learning to Query and Join Defender Tables

Goal: get comfortable enough with KQL to track an incident end-to-end by correlating identity/session data with device activity.

- Start in **Advanced Hunting** in the Defender portal and get familiar with the schema reference panel (lists all available tables and columns)
- Practice basic queries against a single table first, e.g. `DeviceProcessEvents`, `DeviceLogonEvents`, `DeviceNetworkEvents`
- Learn the tables most relevant to **tracking identity/access**:
  - `DeviceLogonEvents` — logon activity on a device, including `LogonId` / `AccountSid`
  - `IdentityLogonEvents` — authentication activity seen by Defender for Identity
  - `AADSignInEventsBeta` — Entra ID (Azure AD) sign-in events, including token/session details
  - `CloudAppEvents` — cloud app activity, useful for correlating actions taken after a sign-in
- Practice `join`-ing tables on a shared key (e.g., account identifier, session/logon ID, or device ID) to follow a single user session across sign-in → device logon → process execution → network connection
- Use a simulated incident (from Step 4) as the test case: pull its alerts, then use Advanced Hunting to reconstruct the same story manually via joined queries — this is the concrete exercise for "tracking an incident by access token"

### Step 6: Findings & Lessons Learned

*[Summarize what worked, what didn't, and what you'd do differently]*

---

## Open Questions / Notes to Self

- Confirm exactly which token/session identifier is most reliable to join on across tables (e.g., `LogonId` vs a sign-in `CorrelationId`) once in Advanced Hunting — this may vary by table
- Check whether the M365 E5 trial's Defender for Endpoint tier includes full Advanced Hunting retention, or if there's a data retention limit to plan around
- Consider adding a second VM later (e.g., a lightweight client) to generate more realistic cross-device sign-in activity to hunt across
