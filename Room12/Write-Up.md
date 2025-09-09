# Brooklyn Nine Nine
+ Description: This room is aimed for beginner level hackers but anyone can try to hack this box. There are two main intended ways to root the box.
+ Link: https://tryhackme.com/room/brooklynninenine
+ Type: Challenge
+ Completed: 2025-03-26

## Tools
+ Nmap
+ GTFOBins
+ StegCracker
+ Steghide

## External Resources
+ https://harshkahate.medium.com/using-steganography-tools-in-kali-linux-59a4d5a3314b
+ https://cyb3rmind.medium.com/6-tryhackme-series-writeups-brooklyn-nine-nine-a0f7f074cbab
+ https://gtfobins.github.io/

## Task 01 | Deploy and get hacking
1. Recon: `nmap -sS <Target IP Address>` revealed the 3 ports open with the services: FTP, SSH, and HTTP.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room12/Screenshots/1.png)

2. Went to http://<Target IP Address> and inspected the source code. Saw a hint mentioning steganpgraphy.
3. Tried using Steghide and StegCracker, but failed to extract anything.
4. Executed `dirb http//<Target IP Address> /usr/share/wordlists/dirb/common.txt` and `gobuster dir -u http//<Target IP Address> -w /usr/share/wordlists/dirb/common.txt` to try to find anything within the web server.
5. Connected to the FTP server with `ftp <Target IP Address>` and the name "anonymous", `ls` to find a file labeled "note_to_jake.txt", and `get note_to_jake.txt` to download it to the AttackBox.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room12/Screenshots/2.png)

6. Executed `cat note_to_jake.txt` to find a hint about possible users (Jake, Amy, and Holt) and passwords.
7. Executed `hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://<Target IP Address>` and sucessfully cracked Jake's password.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room12/Screenshots/3.png)

8. Connected to the SSH server using `ssh jake@<Target IP Address>` and his password, looked through the directory, found a file named "user.txt", executed `cat user.txt`, and found the first flag.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room12/Screenshots/4.png)

9. Executed `sudo -l` to see what commands Jake could run as sudo.
10. Executed `sudo less /etx/profile`, `!/bin/sh`, and `whoami` to break out of the environment and check that I'm now the root user.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room12/Screenshots/5.png)

11. Looked through the root directory and found the second flag within "root.txt" using `cat root.txt`.

## Summary
<p>In my reconnaissance, I used Nmap to find out what was running on the target. The scan resulted in ports open for FTP, HTTP, and SSH. On the website itself was a hint in the source code mentioning steganography. I tried out Steghide, a tool capable of embedding and extracting data from images, but it required a password to extract anything, so I came up with nothing. Next, I used Dirb and Gobuster to comb through the web server, but also got no significant leads.

Next, I logged into the FTP server. Here is where I found a note from Amy, downloaded it to the AttackBox, and then read it; it Amy telling Jake to to change his password because it’s too weak. As such, I ran Hydra on SSH with the username “jake”, and successfully cracked his password. I used that to log in to his SSH portal, looked around in the directory, and found the first flag within the “user.txt” file.

After failing to access other files and folders, I checked their permissions and found that I didn’t have access. Admittedly, I was stumped. So I decided to look-up an external write-up and found a hint: using `sudo -l`. By executing that I can find out what commands the user, in this case Jake, can run as super user; and, `less` was available. I referenced GTFOBins and found the sequence of commands to “... break out from restricted environments by spawning an interactive system shell.” With that I became root and scoured the directory for the second flag. Muwahahahaha! It was cowering within the file named “root.txt”.</p>