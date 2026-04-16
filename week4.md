•	Network Creation: I started by creating the Vlan-Basics project and connected four Linux hosts to an OpenvSwitch using ports eth1 through eth4, ensuring eth0 remained available for future use.

•	Initial Connectivity: I assigned all hosts to the same IP subnet (e.g., 10.10.1.0/24) and verified that every host could successfully ping every other host, as they were initially in the same default broadcast domain.

•	VLAN Configuration: I accessed the switch console and used the ovs-vsctl command to assign specific VLAN IDs to the ports. Following the naming convention based on my student ID (e.g., 20 and 21), I grouped Host 1 and Host 2 into VLAN 20, and Host 3 and Host 4 into VLAN 21.

•	Isolation Testing: After the VLANs were active, I performed a connectivity test. I found that I could still ping between hosts in the same VLAN (1 to 2), but pings between different VLANs (1 to 3) now failed, proving that the switch was successfully isolating the traffic.

•	ARP Table Observation: I checked the ARP tables on each host using ip neigh show. I observed that Host 1 only had a MAC address entry for Host 2; because the broadcast traffic was blocked by the VLAN boundaries, Host 1 could no longer "see" or resolve the MAC addresses for Hosts 3 or 4.

•	Conclusion: My output confirms that by using VLAN tagging on the OpenvSwitch, I successfully partitioned a single physical switch into two separate logical networks, effectively controlling the flow of traffic and improving network security.

 
<img width="625" height="412" alt="image" src="https://github.com/user-attachments/assets/1bc3a5b0-9b0d-44a8-aeaf-d151ba90fb37" />

