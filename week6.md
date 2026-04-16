In this task, I explored how a host discovers and stores the physical hardware (MAC) addresses of other devices on its local subnet.

I first created a single subnet where four Linux hosts—Host 1, Host 2, Host 3, and Host 4—were connected to a central Ethernet switch.
<img width="940" height="811" alt="image" src="https://github.com/user-attachments/assets/91cfa898-dc13-4c90-9169-d9725b441118" />

And then, I checked the initial state of the neighbor table on Host 1 using the command ip neigh show. Initially, the table was empty because no communication had occurred.

From Host 1, I used the ping command to send messages to the other hosts' IP addresses:

I pinged Host 2 at 10.10.1.102.

I pinged Host 3 at 10.10.1.103.

I pinged Host 4 at 10.10.1.104.
<img width="940" height="630" alt="image" src="https://github.com/user-attachments/assets/2efcc6cf-5501-413b-8142-fb759717d6ce" />
<img width="940" height="317" alt="image" src="https://github.com/user-attachments/assets/4f703338-3f53-41d7-9c2f-25220f1d7ccc" />

After these communications, I ran ip neigh show again. The table was now populated with entries for each IP address alongside its corresponding MAC address (link-layer address or lladdr).
<img width="940" height="204" alt="image" src="https://github.com/user-attachments/assets/63e362ad-6603-4986-9fdb-661c6bc23478" />



Task 2
In this task, I built a more complex network spanning three subnets to demonstrate how routers and default gateways enable cross-network communication.

I connected two separate subnets via two routers (Router 1 and Router 2), which were themselves connected to form a third "backbone" subnet.
or every host, I edited the /etc/network/interfaces file. I specified a static IP address and set the gateway to the IP of the local router's interface. For example:

On Host 1, I set the gateway to 10.10.1.1 (Router 1).

On Host 4, I set the gateway to 10.10.3.1 (Router 2).

On both routers, I enabled IP forwarding to allow them to pass traffic between different subnets. I did this by adding the line up sysctl net.ipv4.ip_forward=1 to their interface configuration.

To ensure the end-hosts did not act as routers, I explicitly disabled forwarding on them using the command up sysctl net.ipv4.ip_forward=0.

I verified the entire setup by pinging from Host 1 in the first subnet to Host 4 (10.10.3.104) in the second subnet.
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/95355f63-047d-4716-b3a7-eae9c7509399" />

To conclude Task 1, I successfully demonstrated the dynamic nature of the ARP protocol by showing how Host 1 populated its neighbor table with IP-to-MAC address mappings after communicating with other devices, as evidenced by the detailed list of physical addresses. For Task 2, I verified the implementation of a multi-subnet routing architecture by configuring default gateways on all hosts and enabling IP forwarding on the routers,where Host 1 successfully pinged a remote host across two router hops while maintaining its own forwarding status as disabled.
