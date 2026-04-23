

# <div align="center">VLAN Network Segmentation Lab (VirtualBox Edition)
</div>

<div align="center">Overview</div>

This lab demonstrates how to design and deploy a segmented enterprise network using Oracle VirtualBox.
It focuses on VLAN creation, inter-VLAN routing, and firewall isolation to emulate a secure multi-tier enterprise environment.


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

![Network Mapping](https://github.com/user-attachments/assets/ceddae41-13d6-439e-ae67-ff559f3c1927)

  



At this point I sent pings through ICMP to verify connection and communication between devices, I recieved a return of 100% packet loss and quickly seen my mistake. In my rush I didnt finish inputting the IP Address correctly, once the IP Address was input correctly there was 0% packet loss.

<img width="788" height="503" alt="Human Error Ping (Victim - Client 2)" src="https://github.com/user-attachments/assets/b10ccd66-1757-4cf2-b931-f9f63c73cfed" />

<img width="979" height="569" alt="Human Error Ping (Client 2 - Victim)" src="https://github.com/user-attachments/assets/ad5875f3-314a-472c-9916-82c1eec0c505" />





\
\
\
\


Once the network setup was verified with successful pings I proceeded to the next step of adding a firewall in the form of a pfsense installed vm. To start this, I created 3 new network adapters which would be used. 

<img width="740" height="96" alt="network adapters configured" src="https://github.com/user-attachments/assets/bb283b3a-e955-42b7-8cf8-773f72ca9fed" />

Once the network adapters were configured properly I downloaded a PFsense ISO file and mounted that on a new virtual machine labeled PFsense. 

<img width="1332" height="186" alt="PFsense Download" src="https://github.com/user-attachments/assets/836b7386-9199-45d2-af8b-a53b92eb0454" />











---
