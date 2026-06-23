# Lab 01: WAN Routing & DNS Forwarding in Active Directory

## 🎯 Objective
Establish secure internet connectivity for both the Domain Controller (Windows Server) and the Domain Client (Windows 10) without breaking Active Directory Domain Services (AD DS) or exposing the internal network directly to the WAN.

## Initial Problem
Initially, both VMs were configured on one strict **Internal Network** (`LAB`) inside VirtualBox. While they could communicate with each other, they lacked a default gateway to route traffic outside their local subnet (`192.168.0.0/24`), resulting in an "Unidentified Network" status and total isolation from the internet

By running standard network troubleshooting commands (`ipconfig` and `ping`), it was clear that the client could not resolve external hosts because the Default Gateway was entirely missing from its configuration:
> ![CMD Troubleshooting Missing Gateway](./img/Captura_de_tela_2026-06-23_155236.png)

---

## 🛠️ Step-by-Step Implementation

### Step 1: VirtualBox Dual-NIC Configuration
To bridge the isolated environment with the internet, a second network interface card (NIC) was attached to the Windows Server:
1. **Adapter 1:** Maintained as `Internal Network` (Named: `LAB`) for local domain traffic.
2. **Adapter 2:** Configured as `NAT` to receive internet access from the host machine.

> ![VirtualBox Adapter 1 Configuration](./img/image_371f9b.png)

### Step 2: OS-Level Network Mapping on Server
Inside the Windows Server, the adapters were configured to maintain operational organization. The internal interface (`Internal Network`) was strictly set to handle local traffic, pointing to itself for DNS resolution:
* **IP Address:** `192.168.0.1`
* **Default Gateway:** Blank (Internet is routed through the NAT interface)
* **Preferred DNS:** `192.168.0.1`

> ![Windows Server IPv4 Properties](./img/Captura_de_tela_2026-06-23_160127.jpg)

To confirm the Server was successfully receiving WAN access through the NAT interface, a ping request to `8.8.8.8` was performed from the Server's terminal:

> ![Server Ping Success](./img/Captura_de_tela_2026-06-23_153958.png)

### Step 3: Configuring DNS Forwarders
To allow the Domain Controller (`WIN-FQQIP5E1T9Q`) to resolve external web domains (like google.com) on behalf of the clients:
1. Opened the **DNS Manager** console.
2. Navigated to Server Properties -> **Forwarders** tab.
3. Added Google's public DNS servers (`8.8.8.8` and `8.8.4.4`).

> ![DNS Manager Console](./img/Captura_de_tela_2026-06-23_154221.png)

### Step 4: Client-Side Routing (Fixing the Default Gateway)
The Windows 10 client could talk to the server but didn't know where to send internet packets. 
1. Opened Network Connections (`ncpa.cpl`) on the client machine.
2. Configured the IPv4 properties manually to point the traffic exit node to the Server's internal interface:
    * **IP Address:** `192.168.0.20`
    * **Default Gateway:** `192.168.0.1` *(Routing fix)*
    * **Preferred DNS:** `192.168.0.1` *(Domain resolution fix)*

> ![Client IPv4 Fix](./img/Captura_de_tela_2026-06-23_155333.png)

*(Note: During configuration, the Preferred DNS must strictly point to the Server IP `192.168.0.1` to ensure AD DS communication and Forwarder functionality remain intact).*

---

## 🚀 Verification & Results

To validate the configuration, the standard network troubleshooting methodology was applied again on the Windows 10 Client terminal. The client successfully reached Google's servers, confirming the DNS Forwarder and Routing layers were active:

> ![Client Ping Google Success](./img/Captura_de_tela_2026-06-23_155832.png)

Even though Windows might show a temporary visual glitch in the network taskbar icon due to NCSI delay, external HTTP/HTTPS traffic is fully functional, as verified by external browser navigation on the client VM:

> ![Client Browsing Internet](./img/Captura_de_tela_2026-06-23_160148.png)
