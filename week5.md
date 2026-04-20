
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
VLAN Router 

I copied my previous project Vlan-Basics-12301900 and renamed it to Vlan-Router-12301900. 


I added a Linux Router node to the topology.
I connected the router to the Open vSwitch using:

Router → Switch port eth0

This is the only interface used on the router for this task.


I configured the hosts into two different subnets, based on VLAN separation that is 900 and 901.

I started all devices:

4 Linux Hosts
1 Open vSwitch
1 Linux Router

I verified that all nodes were running correctly.


I configured eth0 on the switch (connected to the router) as a trunk port.
ovs-vsctl set port trunks=0(switch)
This port was set to:
Accept traffic from multiple VLANs
Carry VLAN 900 and VLAN 901

This allows communication between the switch and router for both VLANs.


I configured the router to support VLANs using sub-interfaces on its eth0 interface.

Example configuration:
ip link add link eth0 name eth0.900 type vlan id 900
ip link add link eth0 name eth0.901 type vlan id 901

ip addr add 10.10.1.1/24 dev eth0.900
ip addr add 10.10.2.1/24 dev eth0.901

ip link set eth0.900 up
ip link set eth0.901 up
<img width="940" height="169" alt="image" src="https://github.com/user-attachments/assets/49083adc-fe61-4127-b26a-63e923328f18" />


These IP addresses act as the default gateways for each VLAN.


I configured each host to use the router as its gateway.

VLAN 900:
Gateway: 10.10.1.1
VLAN 901:
Gateway: 10.10.2.1

I tested connectivity using the ping command.

Results:
Hosts within the same VLAN could communicate successfully
Hosts in different VLANs could now also communicate through the router

This confirmed that inter-VLAN routing was working correctly.


 I exported the project as:
Vlan-Router-12301900.gns3project
<img width="779" height="468" alt="image" src="https://github.com/user-attachments/assets/bd859c60-2f1b-491a-a5c2-c744a8d55f47" />


I captured the full network topology.
<img width="940" height="568" alt="image" src="https://github.com/user-attachments/assets/0846124b-8683-43ea-b790-aa8f0b4d7b83" />



<img width="940" height="1439" alt="image" src="https://github.com/user-attachments/assets/e84dba6f-462e-4b1b-ba15-fa14b97131dd" />

Conclusion
In this task, I successfully configured VLANs with inter-VLAN routing using a router. Unlike the previous task where VLANs were isolated, I enabled communication between VLANs by configuring a trunk port and router sub-interfaces. This demonstrated how routers can be used to allow controlled communication between different network segments.
