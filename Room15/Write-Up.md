# OWASP Juice Shop
+ Description: This room uses the Juice Shop vulnerable web application to learn how to identify and exploit common web application vulnerabilities.
+ Link: https://tryhackme.com/room/owaspjuiceshop
+ Type: Walkthrough
+ Completed: 2025-04-10
<br></br>
+ Target IP Address: 10.10.150.153

## Tools
+ Burp Suite

## Miscellaneous Abbreviations
+ XSS = Cross-Site Scripting

## Vocabulary
+ **Command Injection** When web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.
+ **Document Object Model-Based (DOM) XSS** One of the 3 major XSS attacks that uses the HTML environment to execute malicious javascript. Commonly uses the <script></script> HTML tag. AKA XFS (Cross-Frame Scripting).
+ **Email Injection** A security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.
+ **Horizontal Privilege Escalation** Occurs when a user can perform an action or access data of another user with the same level of permissions.
+ **Open Web Application Security Project (OWASP)** A non-profit foundation focused on understanding web technologies and explooitations and provides resources and tools designed to improve the security of software applications.
+ **Persistent XSS** Javascript that's run when the server loads the page containing it. Can occur when the server does not sanitise the user data when it's uploaded to a page. One of the 3 major XSS attacks.
+ **Reflected XSS** Javascript that's run on the client-side end of the web application. Commonly found when the server doesn't sanitise search data. One of the 3 major XSS attacks.
+ **SQL Injection** Qhen an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.
+ **Vertical Privilege Escalation** Occurs when a user can perform an action or access data of another user with a higher level of permissions.

## External Resources
+ OWASP Top 10 ([Link](https://owasp.org/www-project-top-ten/))

## Task 01 | Open for business!
N/A

## Task 02 | Let's go on an adventure!
N/A

## Task 03 | Inject the juice
+ Injection vulnerabilities are dangerous to a company because they can cause downtime and/or loss of data.
+ Identifying injection points within a web application is usually simple, as most of them will return an error.

#### Question #1: Log into the administrator account!
1. I went to the login page of http://<Target IP Address> using Burp Suite's proxy browser.
2. I made sure Intercept mode was on, filled out the login page, and intercepted the login request.
3. In Burp Suite, I navigated to the part of the request where the login details were sent and edited the email field to have `' or 1=1--`.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room15/Screenshots/1.png)

4. Forwarded the request to the server and got the flag.

+ This works because of what certain characters (e.g. `'` and `OR`) do in an SQL query.
+ Can be done if the email or username isn't known.

#### Question #2: Log into the Bender account!
1. In the same login page as the previous question, I used bender@juice-sh.op:a to login.
2. I intercepted the login request using Burp Suite.
3. I navigated to the part of the request where the login details are and appended the email field to have `'--`.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room15/Screenshots/2.png)

4. Forwarded the request to the server and got the flag.
<br></br>
+ This works because of what certain characters (e.g. `'` and `OR`) do in an SQL query.
+ The technique in Q#1 can be done if the email or username isn't known.

## Task 04 | Who broke my lock?!
+ When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation.

#### Question #1: Bruteforce the Administrator account's password!
1. I intercepted a login request using admin@juice-sh.op:a and sent it to Burp Intruder.
2. I selected the proper field and added the file "/usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt" to the payload.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room15/Screenshots/3.png)

3. I started and waited for the  attack to finish.
4. I looked for the request with a status code of 200, and made a note of the password.
5. I went back to the login page, logged in using admin@juice-sh.op:admin123, and got the flag.

#### Question #2: Reset Jim's password!
I followed the walkthrough and tried out the password reset mechanism and got the flag.

## Task 05 | AH! Don't look!
+ A web application should store and transmit sensitive data safely and securely. Sometimes, the developer may not correctly protect their sensitive data, making it vulnerable.
+ Most of the time, data protection isn’t applied consistently across the web application, so certain pages are accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack.

#### Question #1: Access the Confidential Document!
The vulnerability here was an exposed directory with confidential files. I saved the one named “acquisitions.md” and got the flag.

#### Question #2: Log into MC SafeSearch's account!
This is an exercise in not revealing your login information, because that can be used to hack into your account.

#### Question #3: Download the Backup file!
I tried downloading “package.json.bak” from http://10.10.150.153/ftp, but the action was forbidden. However, the file is downloadable via the URL, so we can use a poison null byte. By adding “%2500.md” to the end of the URL, it'll start the download. This works because a poison null byte is a NULL terminator; by placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string.

## Task 06 | Who's flying this thing?
+ Modern-day systems will allow for multiple users to have access to different pages.
+ Broken Access Control Exploit/Bug Categories
  + Horizontal Privilege Escalation
  + Vertical Privilege Escalation

#### Question #1: Access the administration page!
1. I used Firefox’s Debugger (Open application menu → More tools → Web Developer Tools → Debugger) to look through the “main-es2015.js” file.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room15/Screenshots/4.png)

2. Found the path http://10.10.150.153/#/administration.
3. Logged into the Administrator’s account and went to the path to get the flag.

#### Question #2: View another user's shopping basket!
1. Logged in as the Administrator.
2. Intercepted the request to see the “Your Basket” page.
3. Edited the “GET /rest/basket/1 HTTP/1.1” line to “GET /rest/basket/2 HTTP/1.1” and forwarded the request.
4. Refreshed the page, saw that it's the basket of a different user, and got the flag.

#### Question #3: Remove all 5-star reviews!
1. As the administrator, I went to http://10.10.150.153/#/administration.
2. I deleted a 5-star review and got the flag.

## Task 07 | Where did that come from?
+ XSS is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way.
+ 3 Major Types of XSS Attacks
  + DOM (Special)
  + Persistence (Server-Side)
  + Reflected (Client-Side)

#### Question #1: Perform a DOM XSS!
1. I entered `<iframe src="javascript:alert(`xss`)">` into the search bar.
2. Got the flag.

iframe is a common HTML element found in many web applications. This works because it's common practice that the search bar will send a request to the server and then send back related information. However, without correct input sanitation, attackers are able to perform an XSS attack against the search bar.

#### Question #2: Perform a persistent XSS!
1. As the administrator, I went to the “Last Login IP” page.
2. I used Burp's Interceptor to capture the logout request.
3. I added a new header called “True-Client-IP” with the value `<iframe src="javascript:alert(`xss`)">`.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room15/Screenshots/5.png)

5. I forwarded the modified request, re-logged in as admin, went to the “Last Login IP” page, and got the flag.

#### Question #3: Perform a reflected XSS!
1. As the Administrator, I went to the “Order History” page.
2. I clicked on the truck icon.
3. I replaced the ID with `<iframe src="javascript:alert(`xss`)">` in the URL.
4. I entered the URL and got the flag.

This works because servers have a lookup table or database for things like a "tracking ID". Since the "id" parameter isn't sanitised before it's sent to the server, an attacker is able to perform an XSS attack.

## Task 08 | Exploration
N/A

## Reflection
<p>In this room I learned about some of the OWASP's top 10 vulnerabilities in web applications. These were injection, broken authentication, sensitive data exposure, broken access controls, and XSS. By following along with the walkthrough, I was able to exploit and understand each vulnerbility through reading and practice.</p>