
# VLAN Network Segmentation Lab (VirtualBox Edition)  
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
| VLAN 10 | 192.168.10.0/24 | Management | Win-DC | Domain Controller / Router |
| VLAN 20 | 192.168.20.0/24 | Workstations | Ubuntu-Client | User Network |
| VLAN 30 | 192.168.30.0/24 | Servers | Web-Server | Service Network |
| VLAN 40 | 192.168.40.0/24 | Security | Kali-Pentest | Penetration Testing & Validation |

📸 *Screenshot Placeholder:* `screenshots/network_map.png`

---

## 5. Procedure  

### **Step 1 – Environment Setup**
1. Install **Oracle VirtualBox** and **VirtualBox Extension Pack**  
2. Create **four VMs:**
   - `Win-DC` – Windows Server 2022, 2 vCPU, 4 GB RAM  
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
   Win-DC:       192.168.10.10 /24
   Ubuntu-Client:192.168.20.10 /24
   Web-Server:   192.168.30.10 /24
   Kali-Pentest: 192.168.40.10 /24

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

