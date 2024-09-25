# Gotta Catch'em All!
+ Link: https://tryhackme.com/r/room/pokemon
+ Type: Challenge
  
+ Target IP Address: 10.10.188.5

## Resources
N/A

## Tools
+ NMAP
+ DIRB

## Task 1
1. `nmap -sV 10.10.188.5`
2. `dirb http://10.10.188.5` *Led to nothing useful.
3. Found credentials for the "pokemon" user by looking through the webpage's source code.
4. `sudo -i`
5. `ssh pokemon@10.10.188.5` with password hack_the_pokemon.
6. `ls -la`
8. `cd Desktop`
9. `ls`
10. `unzip P0kEm0N.zip`
11. `cat P0kEm0N/grass-type.txt`
12. `locate water-type`
13. `cat /var/www/html/water-type.txt`
14. `locate fire-type`
15. `cat /etc/why_ami_i_here?/fire-type.txt`
16. `cd /home`
17. `cat roots-pokemon.txt`
18. `cd /Videos/Gotta/Catch/Them/All!`
19. `cat Could_this_be_what_Im_looking_for\?.cplusplus`
20. `ssh ash@10.10.188.5` with password pikapika.
22. `cd /home`
23. `cat roots-pokemon.txt`

## Learned
+ New Tools: DIRB.
+ `locate` command in linux.