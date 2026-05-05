Task 1 In this task, I shifted from the command line to a graphical environment to simulate how a standard user interacts with the web.

I created a network consisting of three subnets (A, B, and C). 

I connected Subnet A and B via Router 1, and Subnets B and C via Router 2. 

In Subnet A, I included a switch and a Firefox Host, which served as my HTTP client. 

Subnet B acted as the transit network connecting the two routers. 

In Subnet C, I placed a switch and a Linux Server to act as my HTTP server.  

I assigned static IP addresses and default gateways to all nodes, to ensure routing was consistent across the network. 
<img width="583" height="408" alt="image" src="https://github.com/user-attachments/assets/44523d18-890f-494a-aefe-16fd8674d18b" />
<img width="597" height="492" alt="image" src="https://github.com/user-attachments/assets/87419c22-393b-484d-b546-ffe79106eefc" />
<img width="587" height="345" alt="image" src="https://github.com/user-attachments/assets/0b61d622-963d-484e-b61c-0c3c372d9f62" />
<img width="532" height="392" alt="image" src="https://github.com/user-attachments/assets/6ff860e5-0863-45e9-a0d8-6a1c994bdef3" />
<img width="642" height="446" alt="image" src="https://github.com/user-attachments/assets/f177466e-3eeb-481f-8ae7-c0d2b4d1768a" />


I started all nodes and verified reachability by performing a ping test from the Firefox Host to the Linux Server.

I initiated a packet capture on the link in Subnet B (between the two routers) to record the HTTP traffic for later analysis.  

Executing the GUI AccessI launched the noVNC web-based client within GNS3 to access the Host 1 (Firefox) desktop.  Using the Firefox browser on Host 1, I navigated to the IP address of the Linux Server (Host 2).  Before visiting, I ensured I was using a private tab or cleared the browser cache to ensure a fresh HTTP request-response cycle was captured without interference from cached data.  Once the page loaded successfully, I stopped the packet capture in Subnet B.  


Task 2

I ensured that each device had a static IP and a proper gateway so that traffic could route through the entire network. 
<img width="674" height="479" alt="image" src="https://github.com/user-attachments/assets/17c4be7b-b8cf-4e73-a244-25c295738da1" />

Router 1 (Subnet A to B)eth0 (Subnet A): Address 10.10.1.1, Netmask 255.255.255.0.  

eth1 (Subnet B): Address 10.10.2.1, Netmask 255.255.255.0, and Gateway 10.10.2.2.  Forwarding: I enabled up sysctl net.ipv4.ip_forward=1 to allow traffic to pass between subnets.  Router 2 (Subnet B to C)  

eth0 (Subnet B): Address 10.10.2.2, Netmask 255.255.255.0, and Gateway 10.10.2.1.  

eth1 (Subnet C): Address 10.10.3.1, Netmask 255.255.255.0. 

Forwarding: I also enabled up sysctl net.ipv4.ip_forward=1 here. 

Host 1 & Server 1

Host 1 (Client): Address 10.10.1.101, Netmask 255.255.255.0, and Gateway 10.10.1.1.

Server 1 (Web Server): Address 10.10.3.102, Netmask 255.255.255.0, and Gateway 10.10.3.1.  

Once my interfaces were configured, I used the Linux Host console to verify the path to the server.  

I ran ping 10.10.3.102. I confirmed that packets were successfully transmitted and received across the routers with 0% loss. 
<img width="741" height="472" alt="image" src="https://github.com/user-attachments/assets/f7ef40fb-9525-4b18-8f67-6910fc2a98c5" />

Using wget: I executed wget [http://10.10.3.102/](http://10.10.3.102/). This successfully requested the index page from the server and saved it as index.html on my host. 

Using curl: I then ran curl -o index.html [http://10.10.3.102](http://10.10.3.102).
<img width="940" height="362" alt="image" src="https://github.com/user-attachments/assets/da62823a-dbe2-4e08-911a-276b331dcae8" />

This confirmed I could achieve the same result using a different command-line tool.  

During these tests, I kept a packet capture running on the link in Subnet B to satisfy the lab requirements for analyzing the HTTP traffic between the two routers.  
