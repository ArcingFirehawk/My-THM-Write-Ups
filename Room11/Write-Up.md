# Vulnerabilities 101
+ Description: Understand the flaws of an application and apply your researching skills on some vulnerability databases.
+ Link: https://tryhackme.com/room/vulnerabilities101
+ Type: Walkthrough
+ Completed: 2025-03-19

## Miscellaneous Abbreviations
+ CVE = Common Vulnerabilities and Exposures
+ OS = Operating System

## Vocabulary
+ Exploit Something that utilises a vulnerability on a system or application. 
+ Proof of Concept (PoC) A technique or tool that often demonstrates the exploitation of a vulnerability.
+ Vulnerability A weakness or flaw in the design, implementation, or behaviours of a system or application.
+ Vulnerability Management The process of evaluating, categorising, and ultimately remediating threats (vulnerabilities) faced by an organisation.
+ Vulnerability Scoring Used to determine the potential risk and impact a vulnerability may have on a network or computer system. Serves a vital role in vulnerability management

## Task 01 | Introduction
N/A

## Task 02 | Introduction to Vulnerabilities
+ Vulnerabilities can originate from many factors.
+ 5 Main Categories of Vulnerabilities
  + OS: Vulnerabilities found within OSs that often result in privilege escalation.
  + (Mis)Configuration-based: Vulnerabilities that stem from an incorrectly configured application or service.
    + Ex.: A website exposing customer details.
  + Weak or Default Credentials: 	Applications and services that have an element of authentication will come with default credentials when installed. These are easy to guess by an attacker.
    + Ex.: An administrator dashboard may have the username and password of “admin”.
  + Application Logic: Vulnerabilities that are a result of poorly designed applications.
    + Ex.:  Poorly implemented authentication mechanisms that may result in an attacker being able to impersonate a user.
  + Human-Factor: Vulnerabilities that leverage human behaviour.
    + Ex.: Phishing emails are designed to trick humans into believing they are legitimate.

## Task 03 | Scoring Vulnerabilities (CVSS & VPR)
+ It’s arguably impossible to patch and remedy every single vulnerability in a network or computer system and is sometimes a waste of resources.
+ It’s best to address the most dangerous vulnerabilities and reduce the likelihood of an attack vector being used to exploit a system.
+ There are many frameworks for vulnerability scoring, such as CVSS and VPR.
+ Common Vulnerability Scoring System (CVSS)
  + First introduced in 2005.
  + Popular framework for vulnerability scoring and has three major iterations.
  + The current version (CVSSv3.1) is a score that’s essentially determined by some of the following factors:
    + How easy is it to exploit the vulnerability?
    + Do exploits exist for this?
    + How does this vulnerability interfere with the CIA triad?
    + Free and open-source.
+ Vulnerability Priority Rating (VPR)
  + A more modern framework in vulnerability management.
  + Developed by Tenable.
  + Considered to be risk-driven; meaning that vulnerabilities are given a score with a heavy focus on the risk a vulnerability poses to the organisation itself, rather than factors such as impact.
  + Takes into account the relevancy of a vulnerability.
  + Considerably dynamic in its scoring, where the risk that a vulnerability may pose can change almost daily as it ages.
  + Uses a similar scoring range as CVSS with 2 notable differences:
    + It doesn’t have a "None/Informational" category.
    + Because of its scoring method, the same vulnerability will have a different score using VPR than when using CVSS.

## Task 04 | Vulnerability Databases
+ National Vulnerability Database (NVD)
  + A website that lists all publically categorised vulnerabilities.
  + These CVEs have the formatting of CVE-YEAR-IDNUMBER.
  + NVD allows you to see all the CVEs that have been confirmed, using filters by category and month of submission.
  + Link: https://nvd.nist.gov/vuln
+ Exploit-DB
  + A resource hackers will find much more helpful during an assessment. 
  + Retains exploits for software and applications stored under the name, author, and version of the software or application.
  + Can be used to look for snippets of code (known as PoCs) that are used to exploit a specific vulnerability.
  + Link: http://exploit-db.com/

## Task 05 | An Example of Finding a Vulnerability
+ Throughout an assessment, you will often combine multiple vulnerabilities to get results.

## Task 06 | Showcase: Exploiting Ackme's Application
N/A

## Task 07 | Conclusion
N/A