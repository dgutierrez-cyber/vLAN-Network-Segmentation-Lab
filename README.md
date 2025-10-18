Project: vLAN Network Segmentation Lab

Objective: Simulate an enterprise-grade segmented network in Hyper-V for security testing, traffic isolation, and policy enforcement validation.


---

1. Purpose

This SOP outlines the steps to design, deploy, and validate a segmented virtual network using Hyper-V, demonstrating the principles of network isolation, firewall enforcement, and inter-VLAN communication control. The goal is to showcase practical skills in virtualized enterprise network architecture.


---

2. Scope

This procedure applies to local lab environments running Windows 11/10 Pro with Hyper-V enabled. It includes configuration of Windows Server (as a Domain Controller/Router), Ubuntu (as a client system), and firewall rules to enforce network segmentation.


---

3. Tools & Resources

Virtualization Platform: Hyper-V

Operating Systems:

Windows Server 2022 (Core / GUI)

Ubuntu 22.04 LTS


Network Tools: PowerShell, ipconfig, ping, tracert, nmap, ufw, Windows Firewall

Documentation: screenshots stored under screenshots/

Skills Demonstrated: Network configuration, VLAN mapping, access control, troubleshooting



---

4. Lab Network Design

VLAN	Subnet	Purpose	Example Host	Description

VLAN 10	192.168.10.0/24	Management	Win-DC	Domain Controller / Router
VLAN 20	192.168.20.0/24	Workstations	Ubuntu-Client	Internal User Network
VLAN 30	192.168.30.0/24	Servers	Web-Server	Isolated Service Network



---

5. Procedure

Step 1 – Environment Setup

1. Enable Hyper-V on Windows via “Turn Windows Features On/Off”.


2. Create a Virtual Switch for internal lab traffic (type: Internal).


3. Create three VMs:

Win-DC – Windows Server (2 cores, 4 GB RAM)

Ubuntu-Client – Ubuntu Desktop (2 cores, 2 GB RAM)

Web-Server – Windows Server / Ubuntu Server (optional)





---

Step 2 – VLAN & Subnet Configuration

1. Assign static IPs per VLAN:

Win-DC: 192.168.10.10

Ubuntu-Client: 192.168.20.10

Web-Server: 192.168.30.10



2. Configure each VM NIC to the corresponding VLAN ID using Hyper-V Virtual Switch Manager → Advanced Features.


3. On Win-DC, enable Routing and Remote Access (RRAS) for inter-VLAN routing.




---

Step 3 – Firewall Policy & Isolation

1. On Win-DC, configure Inbound/Outbound Rules:

Block traffic between VLAN 20 ↔ VLAN 30

Allow Management VLAN 10 access to all



2. On Ubuntu-Client and Web-Server, configure:

sudo ufw enable

sudo ufw default deny incoming

Allow only ICMP and DNS outbound



3. Test connectivity with ping and nmap to confirm isolation.




---

Step 4 – Validation & Troubleshooting

1. Test connectivity:

VLAN 10 ↔ VLAN 20 ✅

VLAN 20 ↔ VLAN 30 ❌ (blocked)



2. Capture firewall logs and export results.


3. Save verification screenshots under screenshots/ folder:

ipconfig / ifconfig output

Hyper-V switch settings

Firewall rule configuration

Successful/failed ping results





---

Step 5 – Documentation

1. Save configuration commands and logs in network_config.txt.


2. Store all screenshots under /screenshots/ for GitHub submission.


3. Summarize findings and lessons learned:

Importance of VLANs in reducing attack surface

Benefits of defense-in-depth with routing and firewall rules





---

6. Results & Takeaways

✅ Successfully implemented a three-tier segmented network in Hyper-V.
✅ Verified network isolation and policy enforcement via firewalls.
✅ Demonstrated applied knowledge of Windows Server, Ubuntu, and network troubleshooting.


---

7. References

Microsoft Learn – Configure Hyper-V Networking

Ubuntu Docs – UFW Firewall

NIST SP 800-41 – Guidelines on Firewalls and Firewall Policy
