# Secure Network Architecture
+ Description: Learn about and implement security best practices for network environments.
+ Link: https://tryhackme.com/room/introtosecurityarchitecture
+ Type: Walkthrough
+ Completed: 2025-03-08

## Miscellaneous Abbreviations
+ ARP = Address Resolution Protocol
+ DHCP = Dynamic Host Configuration Protocol
+ IEEE = Institute of Electrical and Electronics Engineers
+ LAN = Local Area Network
+ MitM = Man-in-the-Middle
+ Remote Access Trojan (RAT)
+ ROAS = Router on a Stick
+ SIEM = Security Information and Event Management
+ UTM = Unified Threat Management
+ VLAN = Virtual LAN

## Vocabulary
+ **Access Control Entry (ACE)** Rules that define a list’s profile based on predefined criteria.
+ **Access Control List (ACL)** A list of permissions that determine who can access a specific resource in a computer network. it's used to grant or deny access to network resources.
+ **ARP Inspection** A security feature that validates ARP packets in a network.
+ **Demilitarized Zone (DMZ)** A perimeter network that protects and adds an extra layer of security to an  organization’s internal LAN from untrusted traffic. The end goal is to allow an organization to access untrusted networks (e.g. Internet) while ensuring its private network remains secure.
+ **DHCP Snooping** A security feature that acts like a firewall between untrusted hosts and trusted DHCP servers.
+ **Security Zone** Defines what or who is in a VLAN and how traffic can travel in and out.
+ **Switch Port** The designated interface of a switch. 
+ **Traffic Filtering** A network security control that provides network security, validation, and segmentation by filtering network traffic based on predefined criteria.
+ **Trunk** The connection between the switch and router.
+ **Zone-Pair** A direction-based and stateful policy that will enforce the traffic in single directions per each VLAN.

## Task 01 | Introduction
+ Networking is one of the most critical components of a corporate environment but can often be overlooked from a security standpoint.
+ A properly designed network permits not only internet usage and device communication but also redundancy, optimization, and security.

## Task 02 | Network Segmentation
+ With subnetting in place, there are no restrictions to where an infected device could connect as long as the proper routes are in place, leaving sensitive information and servers open to the unknown device.
+ VLANs
  + Used to segment portions of a network at layer two and differentiate devices.
  + Are configured on a switch by adding a "tag" to a frame. The 802.1q tag will designate the VLAN that the traffic originated from.
  + The 802.1 tag provides a standard between vendors that will always define the VLAN of a frame.
+ Native VLAN
  + Used for any traffic that's not tagged and passes through a switch.
  + To configure it, determine the interface and tag to assign, then set the interface as the default native VLAN.
+ Routers can be used to route between VLANs.
+ Routing Between VLANS
  + Before modern solutions were introduced, network engineers would physically connect a switch and router separately for each VLAN present. Nowadays, that problem is solved through the ROAS design.
  + VLANs are configured to communicate with a router through the switch port.
  + VLANs are routed through the switch port, requiring only one trunk between the switch and router, hence, "on a stick."
  + Before configuring the router, the trunk must be configured on a pre-existing connection.
  + Each vendor configures their trunks and switch ports differently; refer to their specific documentation.
  + Virtual sub-interfaces are used to keep each tagged frame separate because all tagged traffic comes from a single connection.
+ Virtual sub-interfaces act similarly to physical interfaces and are commonly defined by the VLAN ID.
+ Physically, they’re isolated, but because routes exist between them, there’s no security boundary and aren’t necessarily isolated. As long as a route exists between two VLANs, any device can communicate between the two.

## Task 03 | Common Secure Network Architecture
+ With the introduction of VLANs, there's a shift in network architecture design to include security as a key consideration.
+ Security, optimization, and redundancy should all be considered when designing a network, ideally without compromising one component.
+ Security zones are used to properly implement VLANs as a security boundary.
+ Common Standardized Zones
  + External: All devices and entities outside of our network or asset control.
    + Ex.: Devices connecting to a web server.
  + DMZ: Separates untrusted networks or devices from internal resources.
    + Ex.: BYOD, remote users/guests, public servers.
  + Trusted: Internal networks or devices. A device may be placed in the trusted zone if there's no confidential or sensitive information.
    + Ex.: Workstations, B2B.
  + Restricted: Any high-risk servers or databases.
    + Ex.: Domain controllers, client information.
  + Management: Any devices or services dedicated to network or other device management. This zone is less commonly seen and can be grouped with the audit zone.
    + Ex.: Virtualization management, backup servers
  + Audit: Any devices or services dedicated to security or monitoring. Less commonly seen and can be grouped with management.
    + Ex.: SIEM, telemetry.
+ While security zones mostly factor in what will happen internally, it's equally important to consider how new traffic or devices will enter the network, be assigned, and interact with internal systems.
+ Security zones and access controls will physically direct how and where traffic goes.
+ Traffic rules are often governed by company security policy or compliance as equally as security controls that determine access permissions.

## Task 04 | Network Security Policies & Controls
+ Policies aid in defining how network traffic is controlled.
+ A network traffic policy may determine how and if a router will route traffic before other routing protocols are employed.
+ The IEEE has standardized a handful of access control and traffic policies.
+ ACLs
  + The most common standards of defining traffic filtering.
  + Contain ACEs.

## Task 05 | Zone-Pair Policies & Filtering
+ Traffic correlation is standardized as the state of a packet.
+ Firewalls
+ 2 Categories: Stateless & Stateful.
  + A protocol's category is determined based on its ability to consider the state of a packet.
  + A stateful firewall can better correlate information in a network connection. This allows the firewall to filter based on protocols, ports, processes, or other information from a device.
  + Before configuration, consider how to apply the requirements of zones to firewall rules.
+ Zone-Pairs
  + Each zone in a given topology must have a different zone-pair for each other in the topology and every possible direction.
  + This approach provides the most visibility from a firewall and drastically improves the filtering capabilities.

## Task 06 | Validating Network Traffic
+ SSL/TLS Inspection Process
  1. Uses an SSL proxy to intercept protocols or other SSL/TLS encrypted traffic.
  2. Once intercepted, the proxy will decrypt the traffic and send it to be processed by a UTM platform.
  3. UTM solutions will employ deep SSL inspection, feeding the decrypted traffic from the proxy into other UTM services to process the information.
+ Requires an SSL proxy or MitM.

## Task 07 | Addressing Common Attacks
+ DHCP Snooping
  + Was introduced to combat rogue DHCP servers; it will validate and rate-limit DHCP traffic as necessary. If a host is untrusted, its traffic will be filtered and rate-limited.
  + Operates on the switch at Layer 2.
  + The switch will store untrusted hosts with leased IP addresses in a DHCP Binding Database.
  + The database is used to validate traffic and can be used by other protocols.
  + Conditions the protocol will inspect to determine if a DHCP packet should be dropped:
    + Any DHCP packet is received from outside of the network.
    + The source MAC address and DHCP client hardware address do not match.
    + A DHCPRELEASE or DHCPDECLINE packet is received on an untrusted interface that doesn't match an interface that the source address already has registered.
    + A DHCP packet that includes a relay agent address that's not 0.0.0.0
  + There's no standardization of DHCP snooping.
+ Dynamic ARP Inspection
  + Will validate and rate-limit ARP packets as necessary.
  + If an ARP packet's MAC and IP address do not match, the protocol will intercept, log, and discard the packet.
  + Uses the DHCP binding database as its list of binding IP addresses.
+ The DHCP binding database provides the expected MAC and IP address pair of untrusted hosts; ARP inspection will compare the source IP address and MAC address to the binding pair; if they are mismatched, it will drop the packet.

## Task 08 | Conclusion
N/A