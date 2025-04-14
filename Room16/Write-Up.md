# Basic Pentesting
+ Description: This is a machine that allows you to practise web app hacking and privilege escalation.
+ Link: https://tryhackme.com/room/basicpentestingjt
+ Type: Challenge
+ Completed: 2025-04-13
<br></br>
+ Target IP Address: 10.10.219.188

## Tools
+ enum4linux
+ Gobuster
+ John the Ripper
+ LinPEAS
+ NMAP

## External Resources
+ https://youtu.be/xl2Xx5YOKcI?si=QGpVfBUtBSUoH9nc
+ https://ss64.com/bash/ls.html
+ https://trevorxcohen.medium.com/linux-privilege-escalation-with-linenum-75d20a3b59f6

## Task 1
1. I executed `nmap -sV 10.10.219.188` to find any exposed services.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/1.png)
  
2. I used `gobuster dir -u http://10.10.219.188 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt` to brute-force the target's web directories and found something called "developments".
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/2.png)
  
3. I looked through http://10.10.219.188/development and found that they've set up SMB, using Apache version 2.5.12, and another directory called "/etc/shadow".
4. I tried `smbclient -L \10.10.219.188` and found a share called "Anonymous", but it's looked behind a password.
5. I used `enum4linux 10.10.219.188` to see if Gobuster missed anything, and found the previously mentioned shares and two users "jan" and "kay".
6. There was a note saying that Jan had a weak password, so I executed `hydra -l jan -P /usr/share/wordlists/rockyou.txt 10.10.219.188 ssh` to find it.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/3.png)
  
7. With that result, I executed `ssh jan@10.10.219.188` with password "armando" and sucessfully logged in as Jan.
8. I looked around the directory (`ls`, `ls -la`, and `pwd`), but anything noteworthy seemed to be locked from this user.
9. I executed `sudo -l` to see if Jan can run any commands as sudo, but they can't.
10. I tried out LinPEAS by executing `scp /usr/share/peass/linpeas/linpeas.sh jan@10.10.219.188:/dev/shm` to send the script over to Jan, and `./dev/shm/linpeas.sh` to run it. It found the private SSH key of Kay, which I copy-pasted to a file on my Kali machine using nano.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/4.png)
   
11. I tried to log into Kay's SSH with `ssh -i kay_rsa kay@10.10.219.188`, but it requires a password for the key.
12. I ran the `locate ssh2john.py` to find the ssh2john tool, `/usr/share/john/ssh2john.py kay_rsa > johnOutput.txt` to output its hash, and `john johnOutput.txt --wordlist=/usr/share/wordlists/rockyou.txt` to crack the password.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/5.png)

13. I executed `ssh -i kay_rsa kay@10.10.219.188` with the password “beeswax”, but it didn’t work. After some trial and error I remembered that, in the video, he changed the permissions, so I did that using `chmod 600 kay_rsa` and tried it again. Success!
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room16/Screenshots/6.png)

14. With access to Kay’s files, I looked through the directory with `ls`, found a “pass.bak” file, and executed `cat pass.bak` to find the final flag.

## Reflection
<p>This room's completion has been a long time coming. I remember starting it in mid-to-late 2024 and getting stuck because TryHackMe AttackBox didn't have LinPEAS. I soon forgot about it due to life and attempting other rooms, but have come back with a Kali VM to finish the job.

Back then I was aware of John the Ripper, although I never used it. However, this room introduced me to enum4linux and LinPEAS. The latter of which turned out to be quite powerful, because it found the RSA key of Kay. I did use NMAP and Gobuster before, so there's not much to add on that front.

I believe I got stuck at step 5 because the attached video tutorial used enum4linux, which I've never heard of before. The video is also where I learned about LinPEAS and just how thorough it is at searching for privilege escalation pathways on Linux.

All in all, this room has been quite informative. I even remember using enum4linux in some other THM rooms I did after this one. I've learned new things, practiced some techniques, and reinfocred what I already know. With these in hand I believe I've come one step closer to becoming a more skilled cyber security professional.</p>