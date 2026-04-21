Task 1: View Routing Tables

In this task, I created a GNS3 network with two subnets, configured static IP addresses, enabled IP forwarding on the router, and verified connectivity with ping.
1.1  Network Topology
My network consists of Host1 (Subnet 1: 10.10.1.0/24) connected directly to Router1, and Host2 and Host3 (Subnet 2: 10.10.2.0/24) connected via Switch1 to Router1. The two subnets are:
• Subnet 1: 10.10.1.0/24 — Host1 (10.10.1.101) and Router1 eth0 (10.10.1.1)
• Subnet 2: 10.10.2.0/24 — Host2 (10.10.2.102), Host3 (10.10.2.103), Router1 eth2 (10.10.2.1)
 
1.2  GNS3 Projects List
I created the project View-Routes-12301900 as required.
 
1.3  IP Address Configurations
I configured static IP addresses in /etc/network/interfaces for each node. Forwarding was disabled (ip_forward=0) on all hosts and enabled (ip_forward=1) on Router1.

Host1 Configuration
Address: 10.10.1.101/24  |  Gateway: 10.10.1.1  |  Forwarding: disabled (0)
 
Host2 Configuration
Address: 10.10.2.102/24  |  Gateway: 10.10.2.1  |  Forwarding: disabled (0)
 
Host3 Configuration
Address: 10.10.2.103/24  |  Gateway: 10.10.2.1  |  Forwarding: disabled (0)
 
Router1 Configuration – eth0 (Subnet 1)
Address: 10.10.1.1/24  |  Forwarding: enabled (ip_forward=1)
 
Router1 Configuration – eth1 Interface
 
Router1 Configuration – eth2 (Subnet 2)
Address: 10.10.2.1/24  |  Forwarding: enabled (ip_forward=1)
 
 
 
1.5  Ping Test – Cross-Subnet Connectivity
I performed a ping from Host1 (10.10.1.101) to Host2 (10.10.2.102) to verify that Router1 correctly forwards packets between the two subnets. All 6 packets were received with 0% loss, confirming successful inter-subnet routing.
 
 
Task 2: Dynamic Routing with OSPF
In this task, I imported the OSPF-Basics template, started all FRR router nodes, and observed dynamic routing via OSPF. I used the traceroute tool to observe path changes when a link was disabled.
2.1  OSPF Network Topology
The OSPF network (OSPF-Basics-12301900) contains 4 FRR routers (FRR-1 to FRR-4), 2 NETem nodes, Host1 and Host2. Subnet labels visible in the topology:
A: 10.10.1.0/24  |  B: 10.10.2.0/24  |  C: 10.10.3.0/24  |  D: 10.10.4.0/24  |  E: 10.10.5.0/24  |  F: 10.10.6.0/24
2.2  FRR Routers Boot
I started all FRR routers and allowed them to boot. After several minutes, each displayed the FRRouting version prompt (frr#), confirming successful boot and OSPF initialisation.
 
2.3  OSPF Neighbour Routers of FRR-1
I ran the command show ip ospf neighbor on FRR-1. The output shows two neighbour routers:
• Neighbour 10.10.4.2 (FRR-2) — State: Full/DR, Up Time: 2m00s, Address: 10.10.2.2, Interface: eth1
• Neighbour 10.10.5.3 (FRR-3) — State: Full/DR, Up Time: 1m56s, Address: 10.10.3.3, Interface: eth2
 
2.4  OSPF Routing Tables
I ran the commands show ip ospf route and show ip route on FRR-1. The OSPF network routing table shows all six subnets:
 
 

2.6  Traceroute – Before and After Link Disconnection
I performed traceroute from Host1 to Host2 (10.10.6.102) before and after stopping NETem1, which disconnects the top path between FRR-2 and FRR-4.
Before – Path via FRR-1 → FRR-2 (top path)
From Host1: 10.10.1.1 (FRR-1) → 10.10.2.2 (FRR-2) → 10.10.4.4 (FRR-4) → 10.10.6.102 (Host2)
 
After – Rerouted via FRR-3 (bottom path)
After stopping NETem1 (disconnecting FRR-2 to FRR-4 link), OSPF reconverged and my traceroute from FRR-1 shows the new path via FRR-3: FRR-1 → FRR-3 (10.10.3.3) → NETem2/FRR-4 (10.10.5.4) → Host2 (10.10.6.102).
 

Conclusion

In Task 1, I demonstrated static routing by manually configuring IP addresses and a default gateway on each host, with IP forwarding enabled on the router. My ping test confirmed cross-subnet reachability.
In Task 2, I demonstrated dynamic routing with OSPF. The FRR routers automatically discovered neighbours, shared topology information, and calculated optimal paths. When I stopped NETem1 (simulating a link failure), OSPF reconverged and traffic was automatically rerouted via the alternate path through FRR-3, without any manual intervention.
