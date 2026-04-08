
# <div align="center">VLAN Network Segmentation Lab (VirtualBox Edition)
</div>

<div align="center">Overview</div>

I chose VirtualBox to simulate a network using virtual machines so I can test out various programs and CLI tasks.


# <div align="center">Lab Environment</div>

I used VirtualBox to create and setup 4 different VM's.
- A Windows Server 2022 VM (Domain Controller / Router)
- An Ubuntu 22.04 VM
- A Kali Linux VM for penetration testing and validation
- A second Ubuntu 22.04 VM to represent a segmented service zone 


Windows Server 2022- 8GB RAM 4CPU

<img width="879" height="263" alt="Windows Server CPU " src="https://github.com/user-attachments/assets/74dac302-a4a9-4dea-93f7-5ea33216fa07" />

Ubuntu- 4GB RAM 2CPU

<img width="558" height="335" alt="Ubuntu CPU" src="https://github.com/user-attachments/assets/46cd7f9e-9102-4661-83f1-044dbac019c5" />

Kali Linux- 4GB RAM 2CPU

<img width="704" height="249" alt="Kali CPU" src="https://github.com/user-attachments/assets/36659c88-e17c-435f-aed0-a9da21b9300e" />


Ubuntu- 4GB RAM 2CPU

<img width="581" height="320" alt="Ubuntu#2 CPU" src="https://github.com/user-attachments/assets/8b836f1e-c4ba-4a4d-9de9-078701ce58cd" />




# <div align="center">Lab Network Design</div>

I set static IPs to each VM\
-Windows Server 2022- 192.168.56.10\
-Ubuntu- 192.168.56.20\
-Kali- 192.168.56.30\
-Ubuntu- 192.168.56.40

| VLAN | Subnet | Purpose | Example Host | Description |
|------|---------|----------|---------------|--------------|
| VLAN 1 | 192.168.56.10 | Server | Windows Server 2022 | Domain Controller / Router |
| VLAN 2 | 192.168.56.20 | Workstation | Ubuntu | Victim |
| VLAN 3 | 192.168.56.30 | Security | Kali-Pentest | Penetration Testing & Validation |
| VLAN 4 | 192.168.56.40 | Workstation | Ubuntu | Client 2 |


All virtual machines are connected to a shared host-only network (192.168.56.0/24), forming a flat network topology. In this configuration, all hosts can freely communicate with one another, simulating a non-segmented environment where lateral movement is possible.

<div align='center'>Network: 192.168.56.0/24 (Host-Only Adapter - vboxnet0)</div>

                [ Host Machine ]
                 192.168.56.1
                       │
        ───────────────┼────────────────
                       │
     ┌──────────┬──────────┬──────────┬──────────┐
     │          │          │          │
[ Windows ]  [ Ubuntu ]  [ Kali ]  [ Ubuntu ]
 Server 2022   Victim      Attacker   Client 2
192.168.56.10 192.168.56.20 192.168.56.30 192.168.56.40


At this point I sent pings through ICMP to verify connection and communication between devices, I recieved a return of 100% packet loss and quickly seen my mistake. In my rush I didnt finish inputting the IP Address correctly, once I noticed that I added the last number needed to complete the correct input needed. Once the IP Address was input correctly there was 0% packet loss.

<img width="788" height="503" alt="Human Error Ping (Victim - Client 2)" src="https://github.com/user-attachments/assets/b10ccd66-1757-4cf2-b931-f9f63c73cfed" />

<img width="979" height="569" alt="Human Error Ping (Client 2 - Victim)" src="https://github.com/user-attachments/assets/ad5875f3-314a-472c-9916-82c1eec0c505" />







**Goal:** Simulate a segmented enterprise network using Oracle VirtualBox  
**Focus Areas:** Network Isolation · Subnet Mapping · Firewall Policy  
** Screenshots:** [`/screenshots`](./screenshots)  
**🧠 Skills:** Windows Server · Ubuntu · Kali Linux · VirtualBox · Network Troubleshooting  

---

## 1. Purpose  
This lab demonstrates how to design and deploy a segmented enterprise network using **Oracle VirtualBox**.  
It focuses on **VLAN creation**, **inter-VLAN routing**, and **firewall isolation** to emulate a secure multi-tier enterprise environment.

---

## 2. Scope  
This SOP applies to local lab environments using **VirtualBox** on Windows or Linux hosts.  
It includes configuration of:
- A **Windows Server 2022** VM (Domain Controller / Router)
- An **Ubuntu 22.04** Client VM
- A **Kali Linux** VM for penetration testing and validation
- An optional **Web Server** VM to represent a segmented service zone

---

## 3. Tools & Resources  
| Category | Tools / Systems |
|-----------|----------------|
| Virtualization | Oracle VirtualBox |
| Operating Systems | Windows Server 2022, Ubuntu 22.04 LTS, Kali Linux 2024.x |
| Network Utilities | PowerShell, `ipconfig`, `ping`, `tracert`, `nmap`, `ufw`, `iptables` |
| Documentation | Screenshots stored under `/screenshots` |
| Concepts | VLAN segmentation, subnet mapping, access control, routing, testing isolation |

---

## 4. Lab Network Design  

| VLAN | Subnet | Purpose | Example Host | Description |
|------|---------|----------|---------------|--------------|
| VLAN 1 | 192.168.56.10 | Servers | Windows Server 2022 | Domain Controller / Router |
| VLAN 2 | 192.168.20.0/24 | Workstations | Ubuntu-Client | User Network |
| VLAN 3 | 192.168.30.0/24 | Servers | Web-Server | Service Network |
| VLAN 4 | 192.168.56.30 | Security | Kali-Pentest | Penetration Testing & Validation |

📸 *Screenshot Placeholder:* `screenshots/network_map.png`

---

## 5. Procedure  

### **Step 1 – Environment Setup**
1. Install **Oracle VirtualBox** and **VirtualBox Extension Pack**  
2. Create **four VMs:**
   - `Windows Server 2022` – Windows Server 2022, 4 vCPU, 8 GB RAM  
   - `Ubuntu-Client` – Ubuntu 22.04, 2 vCPU, 2 GB RAM  
   - `Kali-Pentest` – Kali Linux, 2 vCPU, 2 GB RAM  
   -  `Web-Server` – Ubuntu Server
3. Create **four Host-Only Networks** via **File → Host Network Manager**, assigning IP ranges for each VLAN.  
4. Attach each VM NIC to its respective Host-Only Adapter.

📸 *Screenshot Placeholder:* `screenshots/virtualbox_networks.png`

---

### **Step 2 – VLAN & Subnet Configuration**
1. Assign static IPs to each VM:  
   ```bash
   Windows Server 2022:       192.168.56.10
   Ubuntu-Client:192.168.20.10 /24
   Web-Server:   192.168.30.10 /24
   Kali-Pentest: 192.168.56.30

2. On Win-DC, enable Routing and Remote Access (RRAS) or use Static Routes for inter-VLAN connectivity.


3. Confirm routing tables with:

route print



📸 Screenshot Placeholder: screenshots/ipconfig_settings.png


---

Step 3 – Firewall Policy & Isolation

Windows Server (Win-DC):

# Block VLAN 20 ↔ VLAN 30
New-NetFirewallRule -DisplayName "Block_VLAN20_30" -Direction Inbound -Action Block -RemoteAddress 192.168.30.0/24
New-NetFirewallRule -DisplayName "Block_VLAN30_20" -Direction Inbound -Action Block -RemoteAddress 192.168.20.0/24

# Allow management VLAN full access
New-NetFirewallRule -DisplayName "Allow_VLAN10_All" -Direction Inbound -Action Allow -RemoteAddress 192.168.10.0/24

Ubuntu / Web-Server:

sudo ufw enable
sudo ufw default deny incoming
sudo ufw allow out 53,80,443/tcp
sudo ufw allow icmp

Kali Linux (testing isolation):

# Verify segmentation with ping and nmap
ping -c 3 192.168.20.10
sudo nmap -sn 192.168.30.0/24

📸 Screenshot Placeholder: screenshots/firewall_rules.png


---

Step 4 – Validation & Troubleshooting

Run verification tests:

# On Ubuntu client
ping 192.168.10.10   # ✅ Management VLAN reachable
ping 192.168.30.10   # ❌ Server VLAN blocked

# On Kali (security VLAN)
sudo nmap -sS 192.168.20.0/24
sudo traceroute 192.168.30.10

📸 Screenshot Placeholders:

screenshots/ping_tests.png

screenshots/nmap_results.png



---

Step 5 – Documentation

1. Export network configuration:

ip addr show > network_config.txt


2. Save all screenshots under /screenshots/


3. Summarize test results and observations for each VLAN pairing.



📸 Screenshot Placeholder: screenshots/results_summary.png


---

6. Results & Takeaways

✅ Built a multi-VLAN virtual lab using Oracle VirtualBox
✅ Configured firewall rules and routing to enforce segmentation
✅ Used Kali Linux to validate network isolation and test firewall posture
✅ Demonstrated Windows Server, Ubuntu, and security testing skills in a virtualized environment


---

7. References

Oracle VirtualBox Networking Guide

Microsoft Learn – Configure Routing and Remote Access

Ubuntu UFW Documentation

Kali Linux Tools Docs

NIST SP 800-41 Rev. 1 – Guidelines on Firewalls and Firewall Policy



---

Repo Structure Example

vLAN-Network-Segmentation-VirtualBox/
│
├── README.md
├── screenshots/
│   ├── network_map.png
│   ├── virtualbox_networks.png
│   ├── ipconfig_settings.png
│   ├── firewall_rules.png
│   ├── ping_tests.png
│   ├── nmap_results.png
│   └── results_summary.png
└── network_config.txt

