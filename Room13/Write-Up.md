# Web Application Security
+ Description: Learn about web applications and explore some of their common security issues.
+ Link: https://tryhackme.com/room/introwebapplicationsecurity
+ Type: Walkthrough
+ Completed: 2025-03-27

## Vocabulary
+ Access Control A mechanism that ensures each user can only access files related to their role or work.
+ Authentication The ability to prove that the user is whom they claim to be.
+ Bug Bounty A program offered by a company that offers rewards to anyone who discovers a security vulnerability (weakness) in the company’s systems.
+ Cryptography Focuses on the processes of encryption and decryption of data.
+ Encryption Scrambling cleartext into ciphertext.
+ Insecure Direct Object Reference (IDOR) A type of access control vulnerability that arises when an application uses user-supplied input to access objects directly.
+ Identification The ability to identify a user uniquely.
+ Injection Attack An attack where the user can insert malicious code as part of their input.
+ Server A computer system running continuously to “serve” the clients.
+ Web Application
  + A program that can be used without installation via a modern standard web browser.
  + A program running on a remote server.

## Task 01 | Introduction
+ In the case of web servers, In this case, the server will run a specific type of program that can be accessed by web browsers.
+ A database server is responsible for many functions, including reading, searching, and writing to the database.
+ All the technical infrastructure in a web application is hidden to the user.

## Task 02 | Web Application Security Risks
+ Common Web App Vulnerabilities
  + Identification and Authentication Failure
    + Allowing brute force attacks.
    + Allowing users to choose a weak password.
    + Storing passwords in plaintext.
  + Broken Access Controls
    + Failing to apply the principle of the least privilege and giving users more access permissions than they need.
    + Being able to view or modify someone else’s account by using its unique identifier.
    + Being able to browse pages that require authentication (logging in) as an unauthenticated user.
  + Injection
    + Lack of proper validation and sanitization of the user’s input.
  + Cryptographic Failures
    + Sending sensitive data in clear text.
    + Relying on a weak cryptographic algorithm.
    + Using default or weak keys for cryptographic functions.

## Task 03 | Practical Example of Web Application Security
+ If a web server receives user-supplied input to retrieve objects and are numbered sequentially, it could allow users to access files they’re not meant for by changing the input.
+ Just providing the correct URL for a user or a product doesn’t necessarily mean the user should be able to access that URL.