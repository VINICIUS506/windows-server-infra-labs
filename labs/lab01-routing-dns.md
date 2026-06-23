# Lab 01: WAN Routing & DNS Forwarding in Active Directory

## 🎯 Objective
Establish secure internet connectivity for both the Domain Controller (Windows Server) and the Domain Client (Windows 10) without breaking Active Directory Domain Services (AD DS) or exposing the internal network directly to the WAN.

## 🔎 Initial Problem
Initially, both VMs were configured on one strict **Internal Network** (`LAB`) inside VirtualBox. While they could communicate with each other, they lacked a default gateway to route traffic outside their local subnet (`192.168.0.0/24`), resulting in an "Unidentified Network" status and total isolation from the internet

By running standard network troubleshooting commands (`ipconfig` and `ping`), it was clear that the client could not resolve external hosts because the Default Gateway was entirely missing from its configuration:
> <img width="1075" height="798" alt="Captura de tela 2026-06-23 155236" src="https://github.com/user-attachments/assets/d7ec8cce-04a6-4d04-a7cb-4a982b0a714b" />

---

## 🛠️ Step-by-Step Implementation

### Step 1: VirtualBox Dual-NIC Configuration
To bridge the isolated environment with the internet, a second network interface card (NIC) was attached to the Windows Server:
1. **Adapter 1:** Maintained as `Internal Network` (Named: `LAB`) for local domain traffic.
2. **Adapter 2:** Configured as `NAT` to receive internet access from the host machine.

> <img width="598" height="161" alt="Captura de tela 2026-06-23 151329" src="https://github.com/user-attachments/assets/7d8c6ba2-1dd9-4523-8479-45ff6bcb8bbb" />



### Step 2: OS-Level Network Mapping on Server
Inside the Windows Server, the adapters were configured to maintain operational organization. The internal interface (`Internal Network`) was strictly set to handle local traffic, pointing to itself for DNS resolution:
* **IP Address:** `192.168.0.1`
* **Default Gateway:** Blank (Internet is routed through the NAT interface)
* **Preferred DNS:** `192.168.0.1`

> <img width="1091" height="820" alt="Captura de tela 2026-06-23 160127" src="https://github.com/user-attachments/assets/c701fdae-1ae4-447c-b261-8f6e0aca6719" />


To confirm the Server was successfully receiving WAN access through the NAT interface, a ping request to `8.8.8.8` was performed from the Server's terminal:

> <img width="1053" height="781" alt="Captura de tela 2026-06-23 153958" src="https://github.com/user-attachments/assets/6dffa1aa-93c5-406a-9b2a-fc087cc7dd9a" />


### Step 3: Configuring DNS Forwarders
To allow the Domain Controller (`WIN-FQQIP5E1T9Q`) to resolve external web domains (like google.com) on behalf of the clients:
1. Opened the **DNS Manager** console.
2. Navigated to Server Properties -> **Forwarders** tab.
3. Added Google's public DNS servers (`8.8.8.8` and `8.8.4.4`).

> <img width="1064" height="795" alt="Captura de tela 2026-06-23 154221" src="https://github.com/user-attachments/assets/9c6e9456-d997-4b95-a203-2dcc05248c60" />


### Step 4: Client-Side Routing (Fixing the Default Gateway)
The Windows 10 client could talk to the server but didn't know where to send internet packets. 
1. Opened Network Connections (`ncpa.cpl`) on the client machine.
2. Configured the IPv4 properties manually to point the traffic exit node to the Server's internal interface:
    * **IP Address:** `192.168.0.20`
    * **Default Gateway:** `192.168.0.1` *(Routing fix)*
    * **Preferred DNS:** `192.168.0.1` *(Domain resolution fix)*

> <img width="1102" height="807" alt="Captura de tela 2026-06-23 155333" src="https://github.com/user-attachments/assets/5b384923-0f67-4537-a7aa-b36ab80819ab" />
*(Note: During configuration, the Preferred DNS must strictly point to the Server IP `192.168.0.1` to ensure AD DS communication and Forwarder functionality remain intact, I fixed it later on).*

---

## 🚀 Verification & Results

To validate the configuration, I again pinged `google.com` on the Windows 10 Client. It successfully reached Google's servers, confirming the DNS Forwarder and Routing layers were active:

> <img width="1192" height="828" alt="Captura de tela 2026-06-23 155832" src="https://github.com/user-attachments/assets/a3673c10-4c6d-4571-bafc-1eb796c8d5c7" />


Even though Windows might show a temporary visual glitch in the network taskbar icon due to NCSI delay, external HTTP/HTTPS traffic is fully functional, as verified by external browser navigation on the client VM:

> <img width="1143" height="830" alt="Captura de tela 2026-06-23 160148" src="https://github.com/user-attachments/assets/ba6c8576-94b7-4855-8b9d-09605001a3bd" />
