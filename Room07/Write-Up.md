# Networking Concepts
+ Description: Learn about the ISO OSI model and the TCP/IP protcol suite.
+ Link: https://tryhackme.com/room/networkingconcepts
+ Type: Walkthrough
+ Completed: 2025-03-07
<br/><br/>
+ AttackBox IP Address: 10.10.83.137
+ Target IP Address: 10.10.19.87

## Miscellaneous Abbreviations
+ ICMP = Internet Control Message Protocol
+ IP = Internet Protocol
+ ISO = Intenational Organization for Standardization
+ NAT = Network Address Translation
+ NFS = Network File System
+ RPC = Remote Procedure Call

## Vocabulary
+ **Encapsulation** The process of every layer adding a header (and sometimes a trailer) to the received unit of data and sending the “encapsulated” unit to the layer below.
+ **Network Segment** A group of networked devices using a shared medium or channel for information transfer.
+ **TELNET (Teletype Network)** A protocol is a network protocol for remote terminal connection.

## Task 01 | Introduction
N/A

## Task 02 | OSI Model
+ Conceptual model developed by the ISO.
+ Consists of 7 Layers (1 to 7): Physical, Data Link, Network, Transport, Session, Presentation, & Application.
+ Layer 1 - Physical
  + Physical data transmission media.
  + Deals with the physical connection between devices and the binary digits.
    + Ex.: Electrical, optical, & wireless signals.
  + Data transmission can be via electrical, optical, or wireless signal.
+ Layer 2 - Data Link
  + Reliable data  transfer between adjacent nodes.
  + Represents the protocol that enables data transfer between nodes on the same network segment.
  + Ex.: Ethernet (802.3), WiFi (802.11).
+ Layer 3: Network Layer
  + Logical addressing and routing between networks.
  + Concerned with sending data between different networks.
  + Will route the network packets through the path it deems better.
  + Ex.: IP, ICMP, IPSec.
+ Layer 4: Transport Layer
  + End-to-end communication and data segmentation.
  + Enables end-to-end communication between running applications on different hosts.
  + Ex.: TCP, UDP.
+ Layer 5: Session Layer
  + Establishing, maintaining, and synchronizing sessions.
  + Responsible for establishing, maintaining, and synchronising communication between applications running on different hosts.
  + Establishing a session means initiating communication between applications and negotiating the necessary parameters for the session.
  + Data synchronisation ensures that data is transmitted in the correct order and provides mechanisms for recovery in case of transmission failures.
  + Ex.: NFS, RPC.
+ Layer 6: Presentation Layer
  + Data encoding, encryption, and compression.
  + Ensures the data is delivered in a form the application layer can understand.
  + Handles data encoding, compression, and encryption.
  + Various standards are used at the presentation layer.
  + Ex.: JPEG, PNG, Unicode, MIME, MPEG.
+ Layer 7: Application Layer
  + Providing services and interfaces to applications.
  + Provides network services directly to end-user applications.
  + Top-most layer.
  + Ex.: HTTP, FTP, DNS, POP3, SMTP, IMAP.

## Task 03 | TCP/IP Model
+ Developed in the 1970s by the US Department of Defense.
+ Allows a network to continue to function as parts of it are out of service. This is possible in part due to the design of the routing protocols to adapt as the network topology changes.
+ Layers (Top-to-Bottom)
  + Application
    + Groups the OSI Model's 5, 6, and 7th Layers.
    + Protocols: HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH.
  + Transport
    + OSI Model's Layer 4.
    + Protocols: TCP, UDP.
  + Internet
    + OSI Model's Layer 3.
    + Protocols: IP, ICMP, 
  + Link
    + OSI Model's Layer 2.
    + Protocols: Ethernet 802,3, WiFi 802.11.
  + Physical
    + OSI Model's Layer 1.

## Task 04 | IP Addresses & Subnets
+ For devices on a network, IP addresses serve as a unique identifier for other hosts to communicate with them.
+ When using the TCP/IP protocol suite, an IP address needs to be assigned for each device connected to the network.
+ An IP address comprises four 8-bit octets and each octet can be a decimal number between 0 and 255.
+ The 0 and 255 are reserved for the network and broadcast addresses, respectively.
+ Using the command `ipconfig` (Windows) and `ifconfig` (Liniux) will show your device's IP address.
+ A subnet mask of 255.255.255.0 can also be written as /24. It means that the leftmost 24 bits within the IP address do not change across the network.
+ 2 Types of IP Addresses: Public & Private.
+ RFC 1918 defines 3 ranges of private IP addresses:
  + 10.0.0.0 - 10.255.255.255 (10/8)
  + 172.16.0.0 - 172.31.255.255 (172.16/12)
  + 192.168.0.0 - 192.168.255.255 (192.168/16)
+ For a private IP address to access the Internet, the router must have a public IP address and must support NAT.
+ Routing
  + A router forwards data packets to the proper network; inspecting the IP address and forwarding the packet to the best network (router) so the packet gets closer to its destination.
  + Usually, a data packet passes through multiple routers before it reaches its final destination.
  + Routers function at layer 3.

## Task 05 | UDP & TCP
+ UDP and TCP are 2 transport protocols that enable processes on networked hosts to communicate with each other.
+ User Datagram Protocol (UDP)
  + Allows us to reach a specific process on this target host.
  + Simple connectionless protocol; doesn’t need to establish a connection.
  + Operates on Layer 4.
  + Doesn’t provide a mechanism to verify that a packet has been delivered.
  + Port numbers serve as the mechanism for determining the sending and receiving process.
+ Transmission Control Protocol (TCP)
  + Connection-oriented transport protocol; requires the establishment of a TCP connection before any data can be sent.
  + Uses various mechanisms to ensure reliable data delivery sent by the different processes on the networked hosts.
  + Operates on Layer 4.
  + Each data octet has a sequence number; this makes it easy for the receiver to identify lost or duplicated packets. The receiver, on the other hand, acknowledges the reception of data with an acknowledgement number specifying the last received octet.
  + Uses the 3-way Handshake.
  + Identifies the process of initiating or waiting (listening) for a connection using port numbers.
+ TCP 3-Way Handshake
  1. SYN Packet: The client initiates the connection by sending a SYN packet to the server. This packet contains the client’s randomly chosen initial sequence number.
  2. SYN-ACK Packet: The server responds to the SYN packet with a SYN-ACK packet, which adds the initial sequence number randomly chosen by the server.
  3. ACK Packet: The three-way handshake is completed as the client sends an ACK packet to acknowledge the reception of the SYN-ACK packet.

## Task 06 | Encapsulation
+ Allows each layer to focus on its intended function.
+ Encapsulation Process
  1. Application Data: It starts when the user inputs the data they want to send into the application. The application formats this data and starts sending it according to the application protocol used, using the layer below it, the transport layer.
  2. Transport Protocol Segment or Datagram: The transport layer adds the proper header information and creates the TCP segment or UDP datagram. This segment is sent to the network layer.
  3. Network Packet: The network layer adds an IP header to the received TCP segment or UDP datagram. Then, this IP packet is sent to the data link layer.
  4. Data Link Frame: The Ethernet or WiFi receives the IP packet and adds the proper header and trailer, creating a frame.
+ The process has to be reversed on the receiving end until the application data is extracted.

## Task 07A | Telnet | Notes
+ Allows you to connect to and communicate with a remote system and issue text commands.
+ Although initially used for remote administration, it can connect to any server listening on a TCP port number.
+ The echo and daytime servers are considered security risks and should not be run.
+ The telnet command in this section is used to remotely communicate with the server.

## Task 07B | Telnet | Practical
1. Executed `telnet 10.10.19.87 7` to access the target's Echo server.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room07/Screenshots/1.png)

2. Executed `telnet 10.10.19.87 13` to access the target's Daytime server.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room07/Screenshots/2.png)

3. Executed `telnet 10.10.19.87 7` to access the target's web (HTTP) server.
4. Entered `GET / HTTP/1.1` and `Host: telnet.thm` to get the web server's information.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room07/Screenshots/3.png)

## Task 08 | Conclusion
N/A