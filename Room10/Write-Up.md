# Pentesting Fundamentals
+ Description: Learn the important ethics and methodologies behind every pentest.
+ Link: https://tryhackme.com/room/pentestingfundamentals
+ Type: Walkthrough
+ Completed: 2025-03-18

## Miscellaneous Abbreviations
+ NCSC = National Cyber Security Centre

## Vocabulary
+ **Penetration Test**
  + An ethically-driven attempt to test and analyse the security defences to protect these assets and pieces of information.
  + An authorised audit of a computer system's security and defences as agreed by the owners of the systems.
  + Involves using the same tools, techniques, and methodologies that someone with malicious intent would use and is similar to an audit.
  + AKA pentest.
+ **Ethics** The moral debate between right and wrong.
+ **White Hat** A hacker that's considered a “good person”. They remain within the law and use their skills to benefit others.
+ **Grey Hat** A person that uses their skills to benefit others often; however, they do not respect/follow the law or ethical standards at all times.
+ **Black Hat** Criminals who often seek to damage organisations or gain some form of financial benefit at the cost of others.
+ **Rules of Engagement (ROE)** A document that’s created at the initial stages of a penetration testing engagement. This document consists of three main sections, which are ultimately responsible for deciding how the engagement is carried out.
+ **Permission** A section of the rules of engagement that gives explicit permission for the engagement to be carried out. Essential to legally protect individuals and organisations for the activities they carry out.
+ **Test Scope** A section of the rules of engagement that'll annotate specific targets to which the engagement should apply.
+ **Rules** A section of the rules of engagement that exactly defines the techniques that are permitted during the engagement.
+ **Methodology** The steps a penetration tester takes during an engagement.
+ **Information Gathering** A stage in the general pentesting methodology that involves collecting as much publically accessible information about a target/organisation as possible. Doesn’t involve scanning any systems.
+ **Enumeration/Scanning** A stage in the general pentesting methodology that involves discovering applications and services running on the systems.
+ **Exploitation** A stage in the general pentesting methodology that involves leveraging vulnerabilities discovered on a system or application. Can involve the use of public exploits or exploiting application logic.
+ **Privilege Escalation** A stage in the general pentesting methodology where once a system/application has been successfully exploited, the pentester attempts to expand their access within it. Can be escalated horizontally and vertically.

## Task 01 | What is Penetration Testing?
N/A

## Task 02 | Penetration Testing Ethics
+ Legality and ethics in cybersecurity testing is always controversial.
+ Before a pentest a formal discussion occurs between the penetration tester and the system owner to agree on the tools, techniques, and systems to be tested. Forms the scope of the pentesting agreement and determines the course it takes.
+ Companies that provide penetration testing services are held against legal frameworks and industry accreditation.
+ Pentesters will often be faced with potentially morally questionable decisions during a pentest.
+ Hacker Types
  + White Hat
  + Grey Hat
  + Black Hat
+ ROE Sections
  + Permission
  + Test Scope
  + Rules

## Task 03 | Penetration Testing Methodologies
+ Pentests can have a wide variety of objectives and targets within scope.
  + No penetration test is the same.
  + No one-case fits all as to how a pentester should approach it.
+ General Pentesting Methodology
  1. Information Gathering
  2. Enumeration/Scanning
  3. Exploitation
  4. Privilege Escalation
  5. Post-Exploitation: Starts when you’ve gained unauthorised access to a system.
+ Open Source Security Testing Methodology Manual (OSSTMM)
  + Provides a detailed framework of testing strategies for systems, software, applications, communications and the human aspect of cybersecurity.
  + Focuses primarily on how systems and applications communicate.
  + Includes methodologies for telecommunications, wired networks, & wireless communications.
+ Open Web Application Security Project (OWASP)
  + A framework that’s used solely to test the security of web applications and services.
  + Community-driven and frequently updated.
+ NIST Cybersecurity Framework 1.1
  + A popular framework used to improve an organisations cybersecurity standards and manage the risk of cyber threats.
  + Provides guidelines on security controls & benchmarks for success for organisations from critical infrastructure all through to commercial.
  + There’s a limited section on a standard guideline for the methodology a pentester should take.
+ NCSC Cyber Assessment Framework (CAF)
  + An extensive framework of fourteen principles used to assess the risk of various cyber threats and an organisation's defences against them.
  + Applies to organisations considered to perform “vitally important services and activities”.

## Task 04 | Black box, White box, Grey box Penetration Testing
+ There are three primary scopes when testing an application or service: black-box, grey-box, & white-box.
+ Black-Box Testing
  + A high-level process where the tester isn’t given any information about the inner workings of the application or service.
  + The tester acts as a regular user testing the functionality and interaction of the application or piece of software.
  + Can involve interacting with the interface and testing to see whether the intended result is returned.
  + No knowledge of programming or understanding of the programme is necessary for this type of testing.
  + Significantly increases the amount of time spent during the information gathering and enumeration phase to understand the attack surface of the target.
+ Grey-Box Testing
  + Most popular for things such as pentesting.
  + Combination of both black-box and white-box testing processes.
  + The tester will have some limited knowledge of the internal components of the application or software. They’ll still be interacting with the application as if it were a black-box scenario and then using their knowledge of the application to try and resolve issues as they find them.
  + The limited knowledge given saves time, and is often chosen for extremely well-hardened attack surfaces.
+ White-Box Testing
  + A low-level process usually done by a software developer who knows programming and application logic.
  + The tester will be testing the internal components of the application/software and full knowledge of the application and its expected behaviour.
  + Much more time consuming than black-box testing.
  + The full knowledge provides a testing approach that guarantees the entire attack surface can be validated.

## Task 05 | Practical: ACME Penetration Test
N/A