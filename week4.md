Task 1

In this task, I created a GNS3 network with two subnets, configured static IP addresses, enabled IP forwarding on the router, and verified connectivity with ping.
My network consists of Host1 (Subnet 1: 10.10.1.0/24) connected directly to Router1, and Host2 and Host3 (Subnet 2: 10.10.2.0/24) connected via Switch1 to Router1. The two subnets are:
• Subnet 1: 10.10.1.0/24 — Host1 (10.10.1.101) and Router1 eth0 (10.10.1.1)
• Subnet 2: 10.10.2.0/24 — Host2 (10.10.2.102), Host3 (10.10.2.103), Router1 eth2 (10.10.2.1)
 <img width="463" height="825" alt="image" src="https://github.com/user-attachments/assets/d3990e3a-533c-4fa4-9a33-8f283d5e40cd" />


I created the project View-Routes-12301900 as required.
 <img width="1050" height="574" alt="image" src="https://github.com/user-attachments/assets/633c390f-b455-431d-829b-3be85bf88cc7" />


I configured static IP addresses in /etc/network/interfaces for each node. Forwarding was disabled (ip_forward=0) on all hosts and enabled (ip_forward=1) on Router1.

Host1 Configuration
Address: 10.10.1.101/24  |  Gateway: 10.10.1.1  |  Forwarding: disabled (0)
 <img width="794" height="570" alt="image" src="https://github.com/user-attachments/assets/68d48247-7c9e-4c8a-82dc-8bb9a61d803b" />

Host2 Configuration
Address: 10.10.2.102/24  |  Gateway: 10.10.2.1  |  Forwarding: disabled (0)
 <img width="780" height="580" alt="image" src="https://github.com/user-attachments/assets/4209c1b3-da72-496e-a90c-707badaa5f52" />

Host3 Configuration
Address: 10.10.2.103/24  |  Gateway: 10.10.2.1  |  Forwarding: disabled (0)
 <img width="811" height="572" alt="image" src="https://github.com/user-attachments/assets/78ad3e05-0ffd-457a-a037-34752d211f44" />

Router1 Configuration – eth0 (Subnet 1)
Address: 10.10.1.1/24  |  Forwarding: enabled (ip_forward=1)
 <img width="784" height="514" alt="image" src="https://github.com/user-attachments/assets/e3b01e77-d24d-4275-87fc-74d5d86d859c" />

Router1 Configuration – eth1 Interface
 <img width="786" height="580" alt="image" src="https://github.com/user-attachments/assets/fefed840-6ca5-4262-9f2f-234af1810989" />

Router1 Configuration – eth2 (Subnet 2)
Address: 10.10.2.1/24  |  Forwarding: enabled (ip_forward=1)
 <img width="794" height="570" alt="image" src="https://github.com/user-attachments/assets/253f035f-bfc9-4f49-a1b0-3f57aa50357c" />

 <img width="800" height="502" alt="image" src="https://github.com/user-attachments/assets/a7aeacc1-b286-40f4-b9fd-b77ddb2efcdf" />

 <img width="519" height="417" alt="image" src="https://github.com/user-attachments/assets/dae7c5aa-7626-4374-b4e7-790600d52394" />


I performed a ping from Host1 (10.10.1.101) to Host2 (10.10.2.102) to verify that Router1 correctly forwards packets between the two subnets. All 6 packets were received with 0% loss, confirming successful inter-subnet routing.
 <img width="666" height="486" alt="image" src="https://github.com/user-attachments/assets/61d4987f-a34b-4c50-8978-8d55332acfa1" />

 
Task 2
In this task, I imported the OSPF-Basics template, started all FRR router nodes, and observed dynamic routing via OSPF. I used the traceroute tool to observe path changes when a link was disabled.

The OSPF network (OSPF-Basics-12301900) contains 4 FRR routers (FRR-1 to FRR-4), 2 NETem nodes, Host1 and Host2. Subnet labels visible in the topology:
A: 10.10.1.0/24  |  B: 10.10.2.0/24  |  C: 10.10.3.0/24  |  D: 10.10.4.0/24  |  E: 10.10.5.0/24  |  F: 10.10.6.0/24

I started all FRR routers and allowed them to boot. After several minutes, each displayed the FRRouting version prompt (frr#), confirming successful boot and OSPF initialisation.
 <img width="403" height="358" alt="image" src="https://github.com/user-attachments/assets/fc1c466a-4ecb-4f1f-b50b-0315da8d2b33" />


I ran the command show ip ospf neighbor on FRR-1. The output shows two neighbour routers:
• Neighbour 10.10.4.2 (FRR-2) — State: Full/DR, Up Time: 2m00s, Address: 10.10.2.2, Interface: eth1
• Neighbour 10.10.5.3 (FRR-3) — State: Full/DR, Up Time: 1m56s, Address: 10.10.3.3, Interface: eth2
 <img width="405" height="320" alt="image" src="https://github.com/user-attachments/assets/42f6f221-f3e6-45f1-af57-8076f3bb3f85" />


I ran the commands show ip ospf route and show ip route on FRR-1. The OSPF network routing table shows all six subnets:
 <img width="408" height="355" alt="image" src="https://github.com/user-attachments/assets/16822dd9-6a07-42ce-b3a9-d09230841cab" />

 <img width="408" height="327" alt="image" src="https://github.com/user-attachments/assets/7f095af7-384e-43db-a9f3-d5ed45923cdb" />



I performed traceroute from Host1 to Host2 (10.10.6.102) before and after stopping NETem1, which disconnects the top path between FRR-2 and FRR-4.
Before – Path via FRR-1 → FRR-2 (top path)
From Host1: 10.10.1.1 (FRR-1) → 10.10.2.2 (FRR-2) → 10.10.4.4 (FRR-4) → 10.10.6.102 (Host2)
 <img width="405" height="264" alt="image" src="https://github.com/user-attachments/assets/acdd7dca-af02-41e4-ba51-fdc0f24277d5" />

After – Rerouted via FRR-3 (bottom path)
After stopping NETem1 (disconnecting FRR-2 to FRR-4 link), OSPF reconverged and my traceroute from FRR-1 shows the new path via FRR-3: FRR-1 → FRR-3 (10.10.3.3) → NETem2/FRR-4 (10.10.5.4) → Host2 (10.10.6.102).
 <img width="405" height="328" alt="image" src="https://github.com/user-attachments/assets/f0c270cd-3385-4337-8737-46e53dec2868" />


