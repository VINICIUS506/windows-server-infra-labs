# Windows Server Infrastructure Labs

Welcome to my infrastructure and systems homelabs portfolio. This specific repository's purpose is to be a public log of all the technical implementations, changes, improvements and networking configurations performed within a virtualized environment, created by myself using **Oracle VirtualBox**.

## 🖥️ Environment Overview
The baseline environment currently consists of an enterprise domain structure originally deployed during an introductory active directory training course. It includes:

* **Domain Controller:** Windows Server 2022 (Standard Edition)
* **Domain Name:** `VINI.local`
* **Client Machines:** Windows 10 Pro (Joined to the `VINI.local` domain)   
* **Network Topology:** Isolated Internal Network (`LAB`) using static IPv4 addressing.

---

## 📂 Laboratory Index
1. **[Lab 01: WAN Routing & DNS Forwarding](./labs/lab01-routing-dns.md)** - Implemented dual-NIC routing and DNS forwarders to securely connect to an isolated AD domain to the internet.
  


