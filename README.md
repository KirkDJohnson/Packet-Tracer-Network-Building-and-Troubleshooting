<h1>Network Architecture and Troubleshooting with Packet Tracer </h1>

<br />
<h2>Description</h2>
I started with a simple network topology consisting of: one router, two switches, and six PCs. From the router, I created two networks, one for each interface, connecting to the two switches, with three PCs attached to each. Initially, I manually assigned IP addresses and default gateways on the endpoints, achieving cross-network communication. Next, I added a dedicated DHCP server with two IP address pools to dynamically lease IP addresses for the respective networks. During troubleshooting, I resolved an issue where the 192.168.10.1 network was receiving IP addresses but not the default gateway. This occurred because the default server pool in Packet Tracer's DHCP server was overwriting the custom pool I had configured. After resolving this, I restored cross-network connectivity. I then expanded the network by adding a printer to the 192.168.20.1 network, assigning it a static IP outside the DHCP pool, and a wireless laptop. For the laptop, I switched its network interface card to a wireless one, added a wireless access point, set up a Wi-Fi network, and successfully configured the laptop to obtain an IP address from the DHCP server over Wi-Fi. This project was a great refresher in Cisco Packet Tracer, allowing me to review networking principles and architecture. In the future, I plan to explore more advanced topics, such as creating a segmented network with VLANs.


<h2>Utilities Used</h2>

- <b>Packet Tracer</b>

<h2>Lab Overview:</h2>

<p align="center">
 <br />
 <br />
I started the project by first making a simple network. I had one core router connected to two switches that each connected to three PCs. Within the router's command line, I went into global configuration mode and assgined each interface a different IP address to have seperate networks and make it so the interfaces would stay online with the command: no shutdown.<br/>
<img src="https://github.com/user-attachments/assets/79e5e5a5-ba74-48c4-8ae2-3be73dfa2558" alt="Packet Tracer"/>
<br />
<br />
I then statically assigned IP addresses to the endpoints. For those on the left side, with the interface g0/0 that was set to 192.168.10.1, and put in the that netowrk and those on the right I put in the 192.168.20.1 network. With the addresses assigned I was successfully able to ping accross the networks. <br/>
<img src="https://github.com/user-attachments/assets/2dcd0710-a549-49b4-8331-ea3ced0d290d" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/fdf1b514-66b5-41de-9404-d87c801277eb" alt="Packet Tracer"/>
<br />
<br />
From here I wanted to get more advanced and implement a dedicated DHCP server to automatically assign IP addresses and move away from static entries. I created and did manually assign the layer 3 addresses on the DHCP server as those should not change. From there, I went to the command line on the router and added the DHCP server to both interfaces via the ip helper-address command followed the by IP address that I statically assigned to it. I set the max a number of IP address to lease to 64. <br/>
<img src="https://github.com/user-attachments/assets/5a330506-895c-4c6e-8de4-697681dadccc" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/1bb1190e-2cd3-4746-8f5f-91e85534b3e8" alt="Packet Tracer"/>
<br />
<br />
I then went into the DHCP server, enabled the service and created two IP address pools, one for the interface 192.168.10.1 and one for 192.168.20.1. I had set the default gateway to those respected addresses and started the .10.1 network at .10.3 because the DHCP server was statically assigned .10.2, so I did not want any double IP addresses.<br/>
<img src="https://github.com/user-attachments/assets/642d0d1c-9889-48c9-962a-7ece201b61b7" alt="Packet Tracer"/>
<br />
<br />
I then went through all the PCs changing them from static to DHCP and noticed that they were receiving the correct IP addresses, however, the .10 network PCs were not being assigned the the default gateway of 192.168.10.1 that I has set in the DHCP pool. I could ping the DHCP server that is on the .10.1 network but not the PCs, likely because they do not have a default gateway. This is where the troubleshooting began.<br/>
<img src="https://github.com/user-attachments/assets/53e3ec7e-5749-4ab5-91fc-d98606d3ac1c" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/7ac957e0-a326-4306-b383-f82fe5ba3145" alt="Packet Tracer"/>
<br />
<br />
I went back and confirmed that I input a default gateway in the DHCP pool, (I did as could be seen the previous picture) yet the DHCP server was only giving the PCs an IP address and not the default gateway that I had assigned it. I saw the serverPool that in the DHCP server by default and could not remove, when examining it further it was set in give the 192.168.10.0 network and the default gateway was set 0.0.0.0. I had assumed that it was taking priority over the switch1 pool that I had created. I changed the severpool to 192.168.30.0, a network that does not exist, so switch1 would be the pool that uses the 192.168.10.0 IP address pool. I was right, it fixed the issue that of the PCs not receiving a default gateway. I was able to confirm a previous theory that because the PCs in the .10.1 network did not have a default gateway, I could ping them from the .20.1 network and now they were assigned default gateways, pinging them again was successful this time.<br/>
<img src="https://github.com/user-attachments/assets/ba63c8b0-6b0d-4b04-ab25-fe6f5a84b976" alt="Packet Tracer"/>
<img src="https://github.com/user-attachments/assets/7feb682a-0a79-4af1-b24f-1ae22ec74d6c" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/d38d98fe-fbb1-4c1e-81b7-9eb98cd0aed8" alt="Packet Tracer"/>
<br />
<br />
From here, I began to pretend this was an active and expanding organization, so I wanted to add a printer to the network that could be connected to. I connected it to the switch handling the .20.1 network, and similarly to the DHCP server, from what I know, printers are usually statically assigned or at least have a DHCP reservation so that the endpoints and administators always know where the printer is. I statically assigned it an IP address in the .20.1 network and was careful to assign it an IP outside the DHCP IP pool that went from 192.168.20.3 to 192.168.20.67. This was ensure there would no duplicate IP address that would cause network issues. I pinged the printer to ensure the reset of the network could access it.<br/>
<img src="https://github.com/user-attachments/assets/c62dec4f-a041-4f4b-9368-ccd07d0ca715" alt="Packet Tracer"/>
<br />
<br />
Next, I simulated someone walking in with a laptop and not being able to connect to the network. I began this by adding a laptop and changing it network interface card to a wireless one. On the laptop in IP configuration, it was set to an APIPA address because it could be leased an IP address from the DHCP server. <br/>
<img src="https://github.com/user-attachments/assets/ab45b732-0426-4e7a-9df9-6dbee7e1f989" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/27b2ad49-21b9-43b8-b1c2-9a569a880d89" alt="Packet Tracer"/>
<br />
<br />
To remedy this, I added a wireless access point and configed it with WPA2-PSK, to ensure only devices that know the pre-shared key could access the network. In a real enterprise environment it would like have WPA2-Enterprise which uses Kerberos and an authentication server that each individual would log into with seperate credentials for a more secure network, but that not present in Packet Tracer.<br/>
<img src="https://github.com/user-attachments/assets/3850c903-9f2d-4125-9883-a31f6799de88" alt="Packet Tracer"/>
<br />
<br />
I then pivotted to the laptop and under wireless settings, found the SSID of the access point, entererd the PSK, and successfully obtained an IP address in the .20.1 network from the DHCP server.<br/>
<img src="https://github.com/user-attachments/assets/d00d1128-d0b3-4396-9a88-b6566e887056" alt="Packet Tracer"/>
 <img src="https://github.com/user-attachments/assets/a566b38d-456d-41ba-bddc-0925f3728cf2" alt="Packet Tracer"/>
<br />
<br />


<h2>Thoughts</h2>
This was a great refresher for Packer Tracer and Cisco CLI. Cisco Skill4all was where I first was introduced to IT, and almost two years since then, I know a lot more and am able to experiment with building a network of my own now rather than following along step by step. This is something I would love to and will likely go more indepth with in the future using VLANs and firewalls to make an even more secure and larger network. I would also like to a project in the physical view of packet tracer, connecting patch cables to patch panels and switches, replacing cables and other activities that an IT support specialist may do. Overall, this was a great project, and I happy that Packet Tracer is a free resource.
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
