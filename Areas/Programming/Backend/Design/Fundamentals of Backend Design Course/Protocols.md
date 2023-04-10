## What is a protocol?
- A system that allows two parties to communicate.
- A protocol is designed with a set of properties depending on the purpose of the protocol.
	- What are we trying to solve? TCP bad for Data Centers for example: [It’s Time to Replace TCP in the Datacenter](https://arxiv.org/pdf/2210.00714.pdf)
- Some Protocols: 
	- TCP, UDP, HTTP, gRPC, FT

#### Protocol Properties
- Data format
	- Text based (plain text, JSON, XML)
	- Binary (protobuf, RESP, h2, h3)
- Transfer mode 
	- Message based (UDP, HTTP) 
	- Stream (TCP, WebRTC)
- Addressing system 
	- DNS name, IP, MAC
- Directionality
	- Bidirectional (TCP)
	- Unidirectional (HTTP) 
	- Full/Half duplex
- State
	- Stateful (TCP, gRPC, apache thrift)
	- Stateless (UDP, HTTP) 
- Routing 
	- Proxies
	- Gateways
- Flow & Congestion control
	- TCP (Flow & Congestion) 
	- UDP (No control)
- Error management
	- Error code
	- Retries and timeout

---
## OSI Model - Open Systems Interconnection Model
>[OSI model - Wikipedia](https://en.wikipedia.org/wiki/OSI_model)

#### Layer 7 - Application
The Application Layer in the OSI model is the layer that is the “closest to the end user”. It receives information directly from users and displays incoming data to the user. Oddly enough, applications themselves do not reside at the application layer. Instead the layer facilitates communication through lower layers in order to establish connections with applications at the other end. Web browsers (Google Chrome, Firefox, Safari, etc.) TelNet, and FTP, are examples of communications that rely on Layer 7.

#### Layer 6 - Presentation
The Presentation Layer represents the area that is independent of data representation at the application layer. In general, it represents the preparation or translation of application format to network format, or from network formatting to application format. In other words, the layer “presents” data for the application or the network. A good example of this is encryption and decryption of data for secure transmission; this happens at Layer 6.

#### Layer 5 - Session
When two computers or other networked devices need to speak with one another, a session needs to be created, and this is done at the Session Layer. Functions at this layer involve setup, coordination (how long should a system wait for a response, for example) and termination between the applications at each end of the session.

#### Layer 4 – Transport
The Transport Layer deals with the coordination of the data transfer between end systems and hosts. How much data to send, at what rate, where it goes, etc. The best known example of the Transport Layer is the Transmission Control Protocol (TCP), which is built on top of the Internet Protocol (IP), commonly known as TCP/IP. TCP and UDP port numbers work at Layer 4, while IP addresses work at Layer 3, the Network Layer. This layer deals in **Segments.**

#### Layer 3 - Network
Here at the Network Layer is where you’ll find most of the router functionality that most networking professionals care about and love. In its most basic sense, this layer is responsible for packet forwarding, including routing through different routers. You might know that your Boston computer wants to connect to a server in California, but there are millions of different paths to take. Routers at this layer help do this efficiently. This layer deals in **Packets.**

#### Layer 2 – Data Link
The Data Link Layer provides node-to-node data transfer (between two directly connected nodes), and also handles error correction from the physical layer. Two sublayers exist here as well--the Media Access Control (MAC) layer and the Logical Link Control (LLC) layer. In the networking world, most switches operate at Layer 2. But it’s not that simple. Some switches also operate at Layer 3 in order to support virtual LANs that may span more than one switch subnet, which requires routing capabilities. This layer deals in **Frames.**

#### Layer 1 - Physical
At the bottom of our OSI model we have the Physical Layer, which represents the electrical and physical representation of the system. This can include everything from the cable type, radio frequency link (as in a Wi-Fi network), as well as the layout of pins, voltages, and other physical requirements. When a networking problem occurs, many networking pros go right to the physical layer to check that all of the cables are properly connected and that the power plug hasn’t been pulled from the router, switch or computer, for example.

![[osi-model4.jpeg]]

![[Pasted image 20230327002308.png]]
- One mental modely you can take is that each layer wraps the previous layers data.

![[osi-model3.png]]

---
## IP Protocol
>https://en.wikipedia.org/wiki/Internet_Protocol

The **Internet Protocol** (**IP**) is the [network layer](https://en.wikipedia.org/wiki/Network_layer "Network layer") [communications protocol](https://en.wikipedia.org/wiki/Communications_protocol "Communications protocol") in the [Internet protocol suite](https://en.wikipedia.org/wiki/Internet_protocol_suite "Internet protocol suite") for relaying [datagrams](https://en.wikipedia.org/wiki/Datagram "Datagram") across network boundaries. Its [routing](https://en.wikipedia.org/wiki/Routing "Routing") function enables [internetworking](https://en.wikipedia.org/wiki/Internetworking "Internetworking"), and essentially establishes the [Internet](https://en.wikipedia.org/wiki/Internet "Internet").

### The IP Building Blocks
- TCP/UDP sit inside of an IP Packet.

#### IP Address
- Layer 3
- Can be set automatically or statically
- Network and Host Portion
- 4 bytes in IPv4 - 32bits (IPv6 is 64)

#### Network vs Host
 **Example: 192.168.254.0/24**
- a.b.c.d/x (a.b.c.d are integers) x is the network bits and remaining bits are the host.
- Network is a group of Hosts.
- Hosts are devices such as PC.

#### Subnet Masks
[[Subnet Masks]]

- Tells you what your Network ID is.
- Used to find Network and Host addresses from an IP address.

 **Example: 192.168.254.0/24**
- First 24 bits (3 bytes) are the network - the rest are for the host.
- This means we can have 2^24 (16777216) networks and each network has 2^8 (255) hosts.
- Also called a subnet.
- Subnet mask is used to:
	- Determine where the network part of the address ends and the host part begins. 
	- Whether an IP is in the same subnet.

#### Default Gateway
- Most networks consist of hosts and a Default Gateway (usually your router)
- Host A can talk to B directly using the MAC address if they are on the same subnet.
- If not A sends it to someone who might know (The Gateway)
- The Gateway has an IP address and each Host should know it's Gateway.

![[Pasted image 20230327171656.png]]
![[Pasted image 20230327171814.png]]

### Anatomy of the IP Packet

#### IPv4 Packet
 >https://en.wikipedia.org/wiki/Internet_Protocol_version_4#Packet_structure
 
- The IP Packet has headers and data sections.
- IP Packet header is 20 bytes (can go up to 60 bytes if options are enabled)
- Data section can go up to 65536 (64kilobytes).
- 4 bytes for 5 rows for header:

![[ip1.png]]

#### MTU
>https://en.wikipedia.org/wiki/Maximum_transmission_unit

- Maximize size for a frame in this network
- Packets will become fragmented if they cannot fit in the MTU - this should pretty much never happen but can.

---
## ICMP - Internet Control Message Protocol

- Designed for informational messages.
	- Host unreachable
	- Port unreachable
	- Fragmentation needed
	- Packet expired (infinite loop in routers)
- Uses IP directly (not TCP or UDP)
- PING and Traceroute use it.
	- Traceroute uses ICMP to ping with a TTL that we increment each time. So we can an ICMP message on each router hit giving where the router TTL expired each time. That's how we can trace the route.
- Doesn't require listeners or ports to be opened.

#### ICMP DataGram
>https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol#Datagram_structure

![[icmp1.png]]

#### ICMP Caveats
- Some firewalls block ICMP for security reasons.
	- People found ways to use it to attack things.
	- Ping wouldn't work in these situations.
- Disabled ICMP can cause issues with connection establishment.
	- If a router has it disabled but an issue such as fragmentation needed occurs but the source is never notified of this a [TCP black hole](https://en.wikipedia.org/wiki/Black_hole_(networking)#:~:text=The%20most%20common%20form%20of,addresses%20is%20often%20just%20dropped) can occur.

---
## UDP - User Datagram Protocol

#### UDP
-   Message Based Layer 4 protocol.
-   Ability to address processes in a host using ports.
-   Simple protocol to send and receive messages.
-   Prior communication not required (double edge sword).
-   Stateless no knowledge is stored on the host.
-   8 byte header Datagram.

#### Use Cases
-   Video streaming
-   VPN
-   DNS
-   WebRTC

#### UDP - Multiplexing/Demultiplexing
-   IP target hosts only
-   Hosts run many apps each with different requirements
-   Ports now identify the “app” or “process”
-   Sender multiplexes all its apps into UDP
-   Receiver demultiplex UDP datagrams to each app

![[Pasted image 20230327231744.png]]
#### Pros
-   Simple protocol
-   Header size is small so datagrams are small
-   Uses less bandwidth
-   Stateless
-   Consumes less memory (no state stored in the server/client)  
-   Low latency - no handshake , order, retransmission or guaranteed delivery

#### Cons
-   No acknowledgement.
-   No guarantee delivery.
-   Connection-less - anyone can send data without prior knowledge.
-   No flow control.
-   No congestion control.
-   No ordered packets.
-   Security - can be easily spoofed.

#### Anatomy of a Datagram
>https://en.wikipedia.org/wiki/User_Datagram_Protocol#UDP_datagram_structure
![[Screen Shot 2023-03-28 at 12.02.17 AM.png]]

---
## TCP - Transmission Control Protocol

#### TCP
-   Stream based Layer 4 protocol.
-   Ability to address processes in a host using ports.
-   “Controls” the transmission unlike UDP which is a firehose.
-   There is the concept of a "Connection".
-   Requires handshake.
-   20 bytes headers Segment (can go to 60).
-   Stateful.
- Not a Request/Response protocol - more akin to websockets.

#### Use Cases
-   Reliable communication
-   Remote shell 
-   Database connections
-   Web communications 
-   Any bidirectional communication

#### TCP Connection
- Connection is a Layer 5 (session) - Sometimes called socket or file descriptor.
- Connection is an agreement between client and server.
	- Handshake (Ack and Nak)
- Must create a connection to send data
- Connection is identified by 4 properties
	- SourceIP-SourcePort
	- DestinationIP-DestinationPort
	- These four values are hashed and then are looked up in a lookup table which is connected to a file descriptor in the OS to know if there is an existing connection.
- Can’t send data outside of a connection.
- Requires a 3-way TCP handshake.
- Segments are sequenced and ordered.
	- They can arrive in a different order.
- Segments are acknowledged.
- Lost segments are retransmitted.

#### Multiplexing and Demultiplexing
-   IP target hosts only
-   Hosts run many apps each with different requirements
-   Ports now identify the “app” or “process”
-   Sender multiplexes all its apps into TCP connections
-   Receiver demultiplex TCP segments to each app based on connection pairs

#### Connection Establishment
-   App1 on 10.0.0.1 want to send data to AppX on 10.0.0.2 
-   App1 sends SYN to AppX to synchronizee sequence numbers
-   AppX sends SYN/ACK to synchronous its sequence number
-   App1 ACKs AppX SYN.
-   Three way handshake.

![[Pasted image 20230329010216.png]]

#### Sending Data
- App1 sends data to AppX,
- App1 encapsulate the data in a segment and sends it.
- AppX acknowledges the segment.
- App1 send new segment before ack of old segment arrives?
	- Yes but there's a limit - it's a buffer on the server for handling it and if the routers in betwen can handle it.
![[Pasted image 20230329204345.png]]
![[Pasted image 20230329204758.png]]
![[Pasted image 20230329204835.png]]

#### Close Connection
- App1 wants to close the connection
- App1 sends FIN, AppX ACK
- AppX sends FIN, App1 ACK
- Four way handshake
- Client side file descriptor stays open just in case there are still some segments coming through mid-transit from the closed server.
![[Pasted image 20230329205334.png]]

#### Pros
-   Guarantee delivery.
-   No one can send data without prior knowledge.
-   Flow Control and Congestion Control.
-   Ordered Packets no corruption or app level work.
-   Secure and can’t be easily spoofed.

#### Cons
-   Large header overhead compared to UDP.
-   More bandwidth.
-   Stateful - consumes memory on server and client.
-   Considered high latency for certain workloads (Slow start/ congestion/ acks).
-   Does too much at a low level (hence QUIC).
-   Single connection to send multiple streams of data (HTTP requests).
-   Stream 1 has nothing to do with Stream 2.
-   Both Stream 1 and Stream 2 packets must arrive.
-   TCP Meltdown.
-   Not a good candidate for VPN.

### The TCP handshake
TCP uses a three-way handshake to establish a reliable connection. The connection is full duplex  and both sides synchronize (SYN) and acknowledge (ACK) each other. The exchange of these four flags is performed in three steps—SYN, SYN-ACK, and ACK:

![](https://ars.els-cdn.com/content/image/3-s2.0-B9781597499613000030-f03-08-9781597499613.jpg)

The client chooses an initial sequence number, set in the first SYN packet. The server also chooses its own initial sequence number, set in the SYN/ACK packet shown in Figure 3.8. Each side acknowledges each other's sequence number by incrementing it; this is the acknowledgement number. The use of sequence and acknowledgment numbers allows both sides to detect missing or out-of-order segments.

Once a connection is established, ACKs typically follow for each segment. The connection will eventually end with a RST (reset or tear down the connection) or FIN (gracefully end the connection).

#### Anatomy of a Segment
>https://en.wikipedia.org/wiki/Transmission_Control_Protocol#TCP_segment_structure
![[tcp1.png]]

---
## Maximum Segment Size
- Depends on the MTU of the network.
- Usually 512 bytes but can go up to 1460.
- Default MTU of the internet is 1500 (results is MSS 1460).
- Jumbo frame means MTU goes to 9000 or more.
- MSS can be larger in jumbo frame cases.

---
## TLS - Transport Layer Security

-   Vanilla HTTP not secure.
-   HTTPS - HTTP with TLS.
-   TLS 1.2 Handshake.
-   Diffie Hellman.
-   TLS 1.3 Improvements.

![[Pasted image 20230329221651.png]]

![[Pasted image 20230329221703.png]]

#### Why TLS
-   We encrypt with symmetric key algorithms. 
-   We need to exchange the symmetric key.
-   Key exchange uses asymmetric key (PKI).
-   Authenticate the server.
![[Pasted image 20230329221747.png]]

![[Pasted image 20230329221809.png]]
![[Pasted image 20230329221817.png]]
![[Pasted image 20230329221836.png]]

#### Extensions
-   SNI - Server Name Indication
-   ALPN - Application Layer Protocol Negotiation 
-   Pre-shared key - 0RTT
-   ECH - Encrypted Client Hello

---
## HTTP/1.1

---
## HTTP/2

---
## HTTP/3

---
## WebSockets

---
## gRPC

---
## WebRTC
