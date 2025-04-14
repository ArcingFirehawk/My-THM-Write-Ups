# Gotta Catch'em All!
+ Description: This room is based on the original Pokemon series. Can you obtain all the Pokemon in this room?
+ Link: https://tryhackme.com/room/pokemon
+ Type: Challenge
+ Completed: 2024-??-??

## Tools
+ NMAP
+ DIRB

## Task 1
1. `nmap -sV <Target IP Address>`
2. `dirb http://<Target IP Address>` *Led to nothing useful.
3. Found credentials for the "pokemon" user by looking through the webpage's source code.
4. `sudo -i`
5. `ssh pokemon@<Target IP Address>` with password hack_the_pokemon.
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
20. `ssh ash@<Target IP Address>` with password pikapika.
22. `cd /home`
23. `cat roots-pokemon.txt`

## Learned
+ New Tools: DIRB.
+ `locate` command in linux.