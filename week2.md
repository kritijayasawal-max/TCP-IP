I created project named Setting-IP-12301900.gns3project.
<img width="936" height="598" alt="image" src="https://github.com/user-attachments/assets/54477144-04ff-43fc-b9bf-9c862c8fe0f6" />
Added four (4) nodes of type Linux Host and one Ethernet switch, and connected together into a LAN
<img width="940" height="466" alt="image" src="https://github.com/user-attachments/assets/1b4b95d8-10d6-498d-85db-999a34174b78" />
For two of the hosts( host1 and host2) , I used the GNS3 Configure menu item to set static IP addresses on the nodes.
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/c3d92d8c-d51d-451e-80f5-5de20422e6b0" />
<img width="940" height="636" alt="image" src="https://github.com/user-attachments/assets/8adb4aa9-3109-452e-aa37-a0853e513346" />
And started all nodes.
On the 3rd host (a host that I did not configure with an IP address), opened a console and edit the /etc/network/interfaces file to set a static IP address. 
I used these commands to change the ip address of host3
/ # ip addr add 10.10.1.2/24 dev eth0
/ # ip link set eth0 up
/ # ip rout add default via 10.10.1.1
/ # ip addr 
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/0cf6cb03-9589-4e75-929b-24ac5d6d973e" />
And the I pinged it with host 1 to check and the connection worked properly.
<img width="940" height="568" alt="image" src="https://github.com/user-attachments/assets/a3bef947-6149-46de-8f49-53eb0678407a" />
 On the 4th host,I opened a console and use ip address add command to set a static IP address.
<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/7cb16f91-295b-4ac0-93aa-8002443da3c3" />
Task 2
Four Linux hosts (A, B, C, D) were set up with IP addresses and connected via a switch.
Communication between Host A and Host B was successful, showing the network works properly.
<img width="940" height="395" alt="image" src="https://github.com/user-attachments/assets/a07e3986-c1ba-43f9-a618-a6a3860b7cda" />
Multiple replies were received, and the average RTT showed low delay, meaning fast communication.
When sending to a non-existent IP, no replies were received and packet loss was 100%.
<img width="763" height="95" alt="image" src="https://github.com/user-attachments/assets/d3293d7c-0f84-4355-9dc3-0a6a784dd76c" />
Packet loss indicates that the destination is unreachable.
Pinged -c 5 -i 2 -s 100 10.10.10.2 command (and output) when limiting the count, setting the data size and interval to non-default values.
<img width="940" height="309" alt="image" src="https://github.com/user-attachments/assets/fd41c4c9-2d23-433a-9dbd-fe5a3f657642" />
The experiment helps understand network performance, delay, and reliability.

