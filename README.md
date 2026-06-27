# Windows Server Infrastructure Labs

Welcome to my infrastructure and systems homelabs portfolio. This specific repository's purpose is to be a public log of all the technical implementations, changes, improvements and networking configurations performed within a virtualized environment, created by myself using **Oracle VirtualBox**.

## 🖥️ Environment Overview
The baseline environment currently consists of an enterprise domain structure originally deployed during an introductory active directory training course. It includes:

* **Domain Controller:** Windows Server 2022 (Standard Edition)
* **Domain Name:** `VINI.local`
* **Client Machines:** Windows 10 Pro (Joined to the `VINI.local` domain)   
* **Network Topology:** Isolated Internal Network (`LAB`) using static IPv4 addressing.

### 📸 Infrastructure Deployment Status
Below are screenshots of the operating systems currently active and integrated within this virtualized enterprise network:

#### 1. Domain Controller (Windows Server 2022)
Primary directory services host, managing DNS resolution and domain policies for `VINI.local`.
> <img width="1055" height="795" alt="Captura de tela 2026-06-23 174831" src="https://github.com/user-attachments/assets/8cf8bd52-55ab-4ec7-832c-1a0792cb5c8c" />


#### 2. Enterprise Client (Windows 10 Pro)
Standard workstation node successfully integrated into the corporate domain infrastructure.
> <img width="1040" height="779" alt="image" src="https://github.com/user-attachments/assets/2b0cdef2-98f2-42b1-9783-ebefb6daee3a" />


---

## 📂 Laboratory Index
1. **[Lab 01: WAN Routing & DNS Forwarding](./labs/lab01-routing-dns.md)** - Implemented dual-NIC routing and DNS forwarders to securely connect to an isolated AD domain to the internet.
2. **[Lab 02: Active Directory Identity Management & Workstation Sharing](./labs/lab02-identity-management-workstation-sharing.md)** - Implemented unprivileged account isolation, tiered administrative segregation, and enforced the Principle of Least Privilege (PoLP) on a shared NOC workstation.


