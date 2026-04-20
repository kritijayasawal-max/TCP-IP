
I created a new project in GNS3 and named it Vlan-Basics-<studentid>. This project was used to perform VLAN configuration and testing.

I added the following devices to the workspace:
Four (4) Linux Host nodes
One (1) Open vSwitch

I then connected each Linux host directly to the Open vSwitch using the following ports:
Host1 to eth1
Host2 to eth2
Host3 to eth3
Host4 to eth4
I made sure to leave eth0 on the switch unused, as instructed.

I configured all four hosts with IP addresses in the same subnet so they could communicate initially.

The IP addresses I used were:

Host1: 10.10.1.101 /24
Host2: 10.10.1.102 /24
Host3: 10.10.1.103 /24
Host4: 10.10.1.104 /24
<img width="748" height="425" alt="image" src="https://github.com/user-attachments/assets/47dcfb62-a5f7-4fb6-a231-3dbbc8afe3d8" />

After assigning the IP addresses, I enabled the network interfaces on each host.

I started all five nodes (four Linux hosts and one Open vSwitch) in GNS3 and confirmed that they were running properly.


I tested connectivity between all hosts using the ping command.

At this stage:

All hosts were able to communicate with each other successfully
This confirmed that the network was working correctly before VLAN configuration

Next, I configured VLANs on the Open vSwitch.

Based on my student ID, I selected VLAN IDs (for example):

VLAN 900
VLAN 901

I assigned:

Host1 and Host2 to VLAN 900
Host3 and Host4 to VLAN 901
<img width="911" height="241" alt="image" src="https://github.com/user-attachments/assets/bf8983f2-e8f9-4384-9f6d-19615fc721cd" />


I applied VLAN tags to the corresponding switch ports.


After configuring the VLANs, I tested connectivity again.

The results were:

Host1 could communicate with Host2
Host3 could communicate with Host4
Hosts in different VLANs could not communicate with each other
<img width="941" height="546" alt="image" src="https://github.com/user-attachments/assets/5fe5d096-f078-40a4-8e9f-5563fe3aab28" />


This showed that VLANs were successfully isolating network traffic.



I checked the ARP tables on each host.

I observed that:

Each host only had ARP entries for devices within the same VLAN
There were no entries for hosts in the other VLAN

This confirmed that the VLAN configuration was working correctly at Layer 2.

 Outputs Generated
I exported the project as:
Vlan-Basics-<studentid>.gns3project
<img width="625" height="412" alt="image" src="https://github.com/user-attachments/assets/8a8042a1-539a-492b-8c99-0c5167c1a074" />



I captured a screenshot of the network topology.
<img width="749" height="425" alt="image" src="https://github.com/user-attachments/assets/7defedca-14c9-4c02-9d47-d4fdd6203ad8" />


I captured the switch configuration showing the ports and VLAN tags.
Switch Configuration Screenshot
ovs-vsctl show
<img width="940" height="1439" alt="image" src="https://github.com/user-attachments/assets/9e3d948f-aebe-41b4-b5ab-4bb307910df4" />

Task 2
VLAN Router Lab – Vlan-Router-<studentid>
1. Project Setup

I copied my previous project Vlan-Basics-<studentid> and renamed it to Vlan-Router-12301900. 
2. Adding the Router

I added a Linux Router node to the topology.
I connected the router to the Open vSwitch using:

Router → Switch port eth0

This is the only interface used on the router for this task.

3. IP Address Configuration

I configured the hosts into two different subnets, based on VLAN separation.

Subnet 1 (VLAN 678):
Host1: 192.168.10.1 /24
Host2: 192.168.10.2 /24
Subnet 2 (VLAN 679):
Host3: 192.168.20.1 /24
Host4: 192.168.20.2 /24

This ensures that routing is required for communication between the two groups.

4. Starting All Nodes

I started all devices:

4 Linux Hosts
1 Open vSwitch
1 Linux Router

I verified that all nodes were running correctly.

5. VLAN Configuration on Switch (Access Ports)

I configured VLANs on the switch similar to Task 1.

Host1 and Host2 → VLAN 678
Host3 and Host4 → VLAN 679

Each port connected to a host was configured as an access port with the appropriate VLAN tag.

6. Configure Trunk Port

I configured eth0 on the switch (connected to the router) as a trunk port.

This port was set to:

Accept traffic from multiple VLANs
Carry VLAN 678 and VLAN 679

This allows communication between the switch and router for both VLANs.

7. Router VLAN Configuration (Sub-Interfaces)

I configured the router to support VLANs using sub-interfaces on its eth0 interface.

Example configuration:
ip link add link eth0 name eth0.678 type vlan id 678
ip link add link eth0 name eth0.679 type vlan id 679

ip addr add 192.168.10.254/24 dev eth0.678
ip addr add 192.168.20.254/24 dev eth0.679

ip link set eth0 up
ip link set eth0.678 up
ip link set eth0.679 up

These IP addresses act as the default gateways for each VLAN.

8. Default Gateway Configuration on Hosts

I configured each host to use the router as its gateway.

VLAN 678:
Gateway: 192.168.10.254
VLAN 679:
Gateway: 192.168.20.254
9. Connectivity Testing

I tested connectivity using the ping command.

Results:
Hosts within the same VLAN could communicate successfully
Hosts in different VLANs could now also communicate through the router

This confirmed that inter-VLAN routing was working correctly.

10. Outputs Generated
 I exported the project as:
Vlan-Router-12301900.gns3project
<img width="779" height="468" alt="image" src="https://github.com/user-attachments/assets/bd859c60-2f1b-491a-a5c2-c744a8d55f47" />

2. Network Screenshot

I captured the full network topology and saved it as:
Vlan-Router-<studentid>-network.png

3. Switch Configuration Screenshot

I captured the switch configuration using:

ovs-vsctl show

The screenshot clearly shows:

Access ports with VLAN tags
The trunk port (eth0) connected to the router



