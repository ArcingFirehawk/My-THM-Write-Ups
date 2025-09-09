# What is Networking?
+ Description: Begin learning the fundamentals of computer networking in this bite-sized and interactive module.
+ Link: https://tryhackme.com/room/whatisnetworking
+ Type: Walkthrough
+ Completed: 2025-02-28

## Misc. Abbreviations
+ ISP = Internet Service Provider
+ ICMP = Internet Control Message Protocol

## Vocabulary
+ **Network** A group of (at least 2) computers that are connected to each other.
+ **Internet** A giant network that consists of many smaller networks.
+ **World Wide Web (WWW)** The Internet as we know it.
+ **Internet Protocol (IP) Address** The set of numbers divided into octets that's used to identify a device.
+ **Media Access Control (MAC) Address** The 12-character hexadecimal number found on a devices physical network interface that's used to identify a device.
+ **Protocol** A set of standards that form that backbone of networking and force many devices to communicate in the same language.
+ **Spoofing** Occurs when a networked device pretends to identfy as another using its MAC address.
+ **Ping** A fundamental network tool that uses iCMP packets to determine the performance of a connection between devices.

## Notes
+ Networks
  + Comes in all shapes and sizes.
  + Are used for a variety of purposes.
  + Can be 2 types: private or public.
+ The Internet
  + It's 1st iteration was within the ARPANET project (late 1060s).
  + The WWW was invented by Tim Berners-Lee.
  + it only started to be used as a repository to store and share information after the creation of the WWW.
+ Devices use a set of labels to idenify themselves.
+ All devices on a network must be identifying and identifiable to communicate and maintain order.
+ Each device has 2 means of identification: its IP Address and MAC Address.
+ IP Addresses
  + Used to identify a host on a network for a period of time.
  + The value of each octet summarizes to be the device's address on the network.
  + Calculated though the technique of IP addressing and subnetting.
  + Can change from device-to-device but can't be actuve simutaneously for mutliple devices within a network.
  + Follows protocols.
  + Can be public or private.
  + A public address is used to identify a device on the Internet.
  + A private address is used to identify a device on amongst other devices.
  + Public IP addresses are given by your ISP.
  + There's a concern that we're running out of public IP addresses.
  + 2 types of addressing schemes: IPv4 and IPv6.
  + IPv6 supports up to 2^128 different addresses and is more efficient due to new methodologies.
+ MAC Addresses
  + Is split into 2 characters and separated by colons.
  + The first 6 characters repressent the company that manufactured the network interface and the last 6 are a unique number.
  + Can be spoofed.
  + When spoofed, it can break poorly implemented security designs by them assuming the device in question is trustworthy.
+ Ping
  + Measures the time taken for ICMP packets to travel between devices.
  + Measurements use ICMP's echo and echo reply packets.
  + Can be performed against devices on a network.