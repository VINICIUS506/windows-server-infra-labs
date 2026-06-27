# Lab 02: Active Directory Identity Management & Workstation Sharing

## 🎯 Objective
Provide a secure, shared workstation environment for two Network Operations Center (NOC) Analysts working alternating shifts.

## 👥 Scenario
* **Analyst 1:** Vinicius F (Day Shift)
* **Analyst 2:** Doug M (Night Shift, 12x36)
  
  Workstations must be shared sequentially. To avoid cross-contamination of access levels, human error, and privilege escalation, standard operations will run on unprivileged accounts, while system adjustments require explicit administrative tier credentials.
---
## 🛠️ Step-by-Step Implementation

### Step 1: Access Active Directory Users and Computers (ADUC)
Navigate through the management tools on the Domain Controller to open the core identity repository tree.

<img width="1053" height="788" alt="Captura de tela 2026-06-25 110846" src="https://github.com/user-attachments/assets/b87ea3a3-d444-4b52-8191-a4a1164cac98" />

### Step 2: Create the NOC Organizational Unit (OU)
Provision a dedicated logical container to isolate and group the network operations team assets.

<img width="1054" height="784" alt="Captura de tela 2026-06-25 111352" src="https://github.com/user-attachments/assets/f1627b52-9be5-4972-9385-10c0d2c57c5c" />

### Step 3: Verify the New OU Container
Confirm that the `NOC` container has been successfully added to the active root directory.

<img width="1038" height="784" alt="Captura de tela 2026-06-25 111403" src="https://github.com/user-attachments/assets/7ccb39a7-e987-4fb9-ac35-926bea709831" />

### Step 4: Locate the Target Standard User Account
Find the existing unprivileged identity object within the default containers before migration. In this case, this would be Vinicius F's account, which was already set up.

<img width="1039" height="780" alt="Captura de tela 2026-06-25 111021" src="https://github.com/user-attachments/assets/6d8ebfab-0339-4583-a75f-103142ab22b0" />

### Step 5: Move the Account to the NOC OU
Relocate the standard user identity into the newly established department container.

<img width="1035" height="779" alt="Captura de tela 2026-06-25 111431" src="https://github.com/user-attachments/assets/9fb8b527-64a5-481b-93b2-c98dadf7fb08" />

### Step 6: Confirm Account Relocation
Verify that the account has been securely moved to the logical container.

<img width="1038" height="783" alt="Captura de tela 2026-06-25 111455" src="https://github.com/user-attachments/assets/a86f0710-fa88-47e7-8ee1-5a616e8f2220" />

### Step 7: Provision the Second Analyst Account
Begin creating a fresh identity object for the incoming shift analyst within the same container.

<img width="1050" height="784" alt="Captura de tela 2026-06-25 111521" src="https://github.com/user-attachments/assets/894ab4ca-279f-4a7a-92f0-327a0dbf1b9c" />

### Step 8: Enforce Corporate Security and Privacy Policies
Apply password parameters to the new Identity. Notice how Doug M must change his password upon the first login.

<img width="1041" height="778" alt="Captura de tela 2026-06-25 111848" src="https://github.com/user-attachments/assets/7728063f-5e46-47fe-ac50-f296d41a4ee5" />

### Step 9: Provision Dedicated Administrative Accounts
To enforce the Principle of Least Privilege (PoLP), separate administrative accounts (`vini-admin` and `doug-admin`) are created within the NOC OU. These accounts will be used exclusively for system adjustments, keeping day-to-day operations unprivileged.

<img width="1042" height="779" alt="Captura de tela 2026-06-25 112258" src="https://github.com/user-attachments/assets/c8df904d-cce4-4213-9ed6-f1df63beb7b4" />

### Step 10: Delegate Elevation Rights via Domain Admins Group
To ensure the administrative variants possess the necessary authority over the local network infrastructure, both accounts are explicitly appended to the **Domain Admins** group. 

<img width="1042" height="781" alt="Captura de tela 2026-06-25 115010" src="https://github.com/user-attachments/assets/1bc0af58-5d44-4651-bfc3-39459891ef5a" />

### Step 11: Authenticate Client Workstation with Doug's Account
Switching over to the Windows client workstation, the night shift analyst (`Doug M`) logs into his profile to start his shift. 

<img width="1046" height="783" alt="Captura de tela 2026-06-25 112345" src="https://github.com/user-attachments/assets/ab22ea0d-5af6-4339-b273-4f1cf5e92f55" />

### Step 12: Confirm Workspace Directory Under Shared User Profile
Opening a standard Command Prompt shell maps the active session baseline environment. The terminal path automatically defaults to `C:\Users\doug`, confirming that the client environment correctly isolated the standard user space natively.

<img width="1040" height="785" alt="Captura de tela 2026-06-25 113553" src="https://github.com/user-attachments/assets/124e39e7-27a4-4b77-aacc-11a79da57ac6" />

### Step 13: Establish Private Logging Directory for Incident Tracking
To test individual profile integrity, a custom tracking directory named `MyLogs` is created inside Doug's local personal Documents library.

<img width="1044" height="781" alt="Captura de tela 2026-06-25 113838" src="https://github.com/user-attachments/assets/a0057bff-1fd5-4d25-b74e-530922ceff5b" />

### Step 14: Verify Session Separation and Privacy Compliance
Switching user contexts on the shared workstation to `Vinicius F`'s session reveals a completely clean, isolated area. Vinicius cannot view or tamper with any local incident files created by Doug during alternating shifts, as the default Windows permissions restrict cross-profile directory access.

<img width="1036" height="782" alt="Captura de tela 2026-06-25 114230" src="https://github.com/user-attachments/assets/3da76d4c-2654-420a-a970-2594e2588835" />

### Step 15: Restrict Administrator-Exclusive Commands on Standard Accounts
Running privileged security commands such as `net session` under Vinicius's standard unprivileged account safely forces acess to be denied, validating the restrictive boundaries of the endpoint configuration.

<img width="1039" height="779" alt="Captura de tela 2026-06-25 114534" src="https://github.com/user-attachments/assets/e9999e6f-74f8-4ade-ab24-25bdc5ab6789" />

### Step 16: Request Intentional Privilege Elevation
When an infrastructure operation requires administrative changes, the user must explicitly trigger context elevation through the Windows shell interface rather than operating out of a dangerous persistent high-privilege shell.

<img width="1036" height="779" alt="Captura de tela 2026-06-25 114614" src="https://github.com/user-attachments/assets/f23463c5-0b6d-4696-8500-91c8d045cd26" />

### Step 17: Intercept Elevation via User Account Control (UAC) Prompt
The operating system intercepts the command execution request, throwing a secure UAC prompt that demands explicit structural administrative credentials (`VINI\vini-admin` or `VINI\doug-admin`) before launching the shell.

<img width="1040" height="780" alt="Captura de tela 2026-06-25 120116" src="https://github.com/user-attachments/assets/8528b18b-80ae-45b1-b424-605a078f2094" />

### Step 18: Execute Privileged Infrastructure Operations Securely
Once the administrative credentials are approved, the elevated command line window loads successfully. Running `net session` now executes cleanly without access blocks, demonstrating controlled management capabilities.

<img width="1039" height="785" alt="Captura de tela 2026-06-25 120152" src="https://github.com/user-attachments/assets/db8a5d69-1672-4889-9dac-b18150989bbe" />

---

### Step 19: Audit Active Directory Identity Objects via PowerShell
To quickly query, verify, and track the state of newly provisioned or modified identity objects without navigating the graphical interface, administration terminals can drop directly into Active Directory modules. Running specific cmdlets returns critical operational baselines like SIDs, object names, and precise Distinguished Name (DN) organizational paths.
```powershell
Get-ADUser -Identity "doug"
Get-ADUser -Identity "vini"
