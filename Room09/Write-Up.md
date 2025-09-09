# Enumeration & Brute Force
+ Description: Enumerate and brute firce authenication mechanisms.
+ Link: https://tryhackme.com/room/enumerationbruteforce
+ Type: Walkthrough
+ Completed: 2025-03-14

## Tools
+ Burp Suite
+ Crunch
+ Hydra

## Miscellaneous Abbreviations
+ OSINT = Open-Source Intelligence

## Vocabulary
+ **Google Dork** A specific search query used in Google.

## Task 01 | Introduction
+ Authentication enumeration is a fundamental aspect of security testing, concentrating specifically on the mechanisms that protect sensitive aspects of web applications.
+ Involves methodically inspecting various authentication components.
+ Each element is meticulously tested because they represent potential vulnerabilities that, if exploited, could lead to significant security breaches.

## Task 02 | Authentication Enumeration
+ Analogous to peeling back the layers of an onion; you remove and identify each layer to understand what they entail and how everything is connected.
+ Common Places for Authentication Enumeration
  + Identifying valid usernames.
  + Password Policies
+ Common Places to Enumerate
  + Registration Pages
  + Password Reset Features
  + Verbose Errors
  + Data Breach Information

## Task 03A | Enumerating Users via Verbose Errors | Notes
+ Detailed error messages can unintentionally expose sensitive data to those who know how to listen.
+ Possible Insights of Verbose Errors
  + Internal Paths
  + Database Details
  + User Information
+ Common practices for inducing verbose errors:
  + Invalid Login Attempts: By intentionally entering incorrect usernames or passwords, attackers can trigger error messages that help distinguish between valid and invalid usernames.
  + SQL Injection: This technique involves slipping malicious SQL commands into entry fields, hoping the system will stumble and reveal information about its database structure.
  + File Inclusion/Path Traversal: By manipulating file paths, attackers can attempt to access restricted files, coaxing the system into errors that reveal internal paths.
  + Form Manipulation: Tweaking form fields or parameters can trick the application into displaying errors that disclose backend logic or sensitive user information.
  + Application Fuzzing: Sending unexpected inputs to various parts of the application to see how it reacts can help identify weak points.
+ When it comes to breaching authentication, enumeration and brute forcing often go hand in hand:
  + User Enumeration: Discovering valid usernames sets the stage, reducing the guesswork in subsequent brute-force attacks.
  + Exploiting Verbose Errors: The insights gained from these errors can illuminate aspects like password policies and account lockout mechanisms, paving the way for more effective brute-force strategies.

## Task 03B | Enumerating Users via Verbose Errors | Practical
1. I navigated to http://enum.thm/labs/verbose_login/ and tried logging in to the page.
2. I used the `touch` command to create files for the provided python script ("script.py") and usernames ("usernames.txt) on the Attackbox.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/1.png)
   
3. I executed `python3 scrypt.py emails.txt` to find the valid email.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/2.png)

## Task 04A | Exploiting Vulnerable Password Reset Logic | Notes
+ Password reset mechanisms require careful security considerations because poorly secured processes can be easily exploited.
+ Types of Resets
  + Email-Based Reset
  + Security Question-Based Reset
  + SMS-Based Reset
+ Possible Vulnerabilities
  + Predictable Tokens: If the reset tokens used in links or SMS messages are predictable or follow a sequential pattern, attackers might guess or brute-force their way to generate valid reset URLs.
  + Token Expiration Issues: Tokens that remain valid for too long or do not expire immediately after use provide a window of opportunity for attackers. It’s crucial that tokens expire swiftly to limit this window.
  + Insufficient Validation: The mechanisms for verifying a user’s identity might be weak and susceptible to exploitation if the questions are too common or the email account is compromised.
  + Information Disclosure: Any error message that specifies whether an email address or username is registered can inadvertently help attackers in their enumeration efforts, confirming the existence of accounts.
  + Insecure Transport: The transmission of reset links or tokens over non-HTTPS connections can expose these critical elements to interception by network eavesdroppers.

## Task 04B | Exploiting Vulnerable Password Reset Logic | Practical
1. I navigated to http://enum.thm/labs/predictable_tokens/ and tried attempted a password reset using admin@admin.com.
2. I used Burp Suite's proxy browser to navigate to http://enum.thm/labs/predictable_tokens//reset_password.php?token=123 and intercepted the request. Then, sent it to Intruder.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/3.png)

3. I executed `crunch 3 3 -o file.txt -t %%% -s 100 -e 200` to create a dictionary to be used in a brute-force attack.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/4.png)

4. In Burp Intruder I configured the payload using the generated file, then started the attack.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/5.png)

5. Once the attacks finished, I located the request with the highest length and inspected its contents to find my new password.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/6.png)

6. I logged into the webpage using admin@admin.com:6JgDcoZp and got the flag.

## Task 05A | Exploiting HTTP Basic Authentication | Notes
+ Basic Authentication
  + Offers a more straightforward method when securing access to devices.
  + It requires only a username and password.
  + Easy to implement and manage on devices with limited processing capabilities.
  + Primary goal is to prevent unauthorized access with minimal setup.
  + Typically used by network devices to control access to their administrative interfaces.
  + Suitable for environments where session management and user tracking are not required or are managed differently.
+ HTTP Basic Authentication provides a simple challenge-response mechanism for requesting user credentials.
  + “username:password” format.
  + Encoded in base64.

## Task 05 | Exploiting HTTP Basic Authentication | Practical
1. I navigated to http://enum.thm/labs/predictable_tokens/ and using Burp's browser.
2. I tried logging in using admin:password, captured the failed request, and sent it to Burp Intruder.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/7.png)

3. In Intruder, I decoded the authorization using base64 and added it to the payload.
4. To configure the payload I: changed it to type “Simple List”, loaded the “500-worst-passwords.txt” file, added rules (which added a prefix of “admin:” and encoded it to base64) to format it, and removed the “=” from payload encoding.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/8.png)

5. Once the attack finished, I located the request with a status code of 200 and decoded its authorization.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/9.png)

6. With valid credentials in hand, I successfully logged into the website and got the flag.
7. I executed `hydra -l admin -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt http-get://enum.thm/labs/basic_auth/` to accomplish the same thing using Hydra.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room09/Screenshots/10.png)

## Task 06 | OSINT
+ Digging into a web application’s past can be as revealing as examining its present.
+ The Internet Archive's Wayback Machine (https://archive.org/web/) allows someone to explore older versions of websites.
  + By doing so one can uncover files and directories that are no longer visible but might still linger on the server.
  + These relics can sometimes provide a backdoor right into the present system.
+ By using Google dorks, you can find information that wasn’t meant to be public.

## Task 07 | Conclusion
N/A