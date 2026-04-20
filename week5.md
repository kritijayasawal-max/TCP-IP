
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

5. Initial Connectivity Testing

I tested connectivity between all hosts using the ping command.

At this stage:

All hosts were able to communicate with each other successfully
This confirmed that the network was working correctly before VLAN configuration
6. VLAN Configuration

Next, I configured VLANs on the Open vSwitch.

Based on my student ID, I selected VLAN IDs (for example):

VLAN 678
VLAN 679

I assigned:

Host1 and Host2 to VLAN 678
Host3 and Host4 to VLAN 679

I applied VLAN tags to the corresponding switch ports.

7. Connectivity Testing After VLAN Configuration

After configuring the VLANs, I tested connectivity again.

The results were:

Host1 could communicate with Host2
Host3 could communicate with Host4
Hosts in different VLANs could not communicate with each other

This showed that VLANs were successfully isolating network traffic.

8. ARP Table Verification

I checked the ARP tables on each host.

I observed that:

Each host only had ARP entries for devices within the same VLAN
There were no entries for hosts in the other VLAN

This confirmed that the VLAN configuration was working correctly at Layer 2.

9. Outputs Generated
1. Exported Project

I exported the project as:
Vlan-Basics-<studentid>.gns3project

2. Network Screenshot

I captured a screenshot of the network topology and saved it as:
Vlan-Basics-<studentid>-network.png

3. Switch Configuration Screenshot

I captured the switch configuration showing the ports and VLAN tags and saved it as:
Vlan-Basics-<studentid>-ports.png

Conclusion
