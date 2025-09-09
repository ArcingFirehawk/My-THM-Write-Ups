# Incident Response Process
+ Description: Practice the NIST Incident Response lifecycle steps on a compromised Windows workstation.
+ Link: https://tryhackme.com/room/incidentresponseprocess
+ Type: Walkthrough
+ Completed: 2025-05-05

## Miscellaneous Abbreviations
+ EDR = Endpoint Detection and Response
+ IoC = Indicators of Compromise
+ IDS = Intrusion Detection System
+ IRP = Incident Response Plan
+ IRT = Incident Response Team
+ PID = Process ID
+ SIEM = Security Information and Event Management

## Vocabulary
**Incident Response (IR)** The processes and technologies an organisation employs to detect and react to cyber threats, security breaches, or cyber attacks.
**Macro** A set of instructions or a script that automates repetitive tasks by performing a sequence of actions or commands within software applications.
**Windows Registry** A hierarchical database that stores configuration settings and options for the Windows operating system and installed applications.

## Task 01 | Introduction
N/A

## Task 02 | Incident Response Lifecycle
+ Ideally, an organisation establishes incident response procedures and technologies within a formal IRP, detailing the specific steps for identifying, containing, and resolving various types of cyber attacks. It empowers cyber security teams to mitigate or prevent damage effectively.
+ NIST Incident Response Framework
  + Preparation: Establishing and maintaining an incident response capability.
  + Detection and Analysis: Identifying and understanding the scope and impact of an incident.
  + Containment, Eradication, and Recovery: Limiting the incident's impact, eliminating the threat, and restoring normal operations.
  + Post-Incident Activity: Reviewing and improving the incident response process and documentation.
+ SANS Incident Response 101
  1. Preparation
  2. Identification
  3. Containment
  4. Eradication
  5. Recovery
  6. Lessons Learned
+ Stating that security is a process is a very concise way to convey the idea that security cannot be achieved simply by purchasing and deploying security products or tools.
  + True security requires ongoing, comprehensive efforts that comprise a wide range of activities and practices.
  + Security is a dynamic, multifaceted process that requires a coordinated and continuous effort.
  + It's about creating a resilient system that must adapt to new challenges and threats over time.
+ Incident responders are usually called to action in the middle of NIST Incident Response Framework’s second phase—after detection.
+ Goals of Incident Responders
  + Feed new information to the next cycle.
  + Eradicate the threat and prevent it from impacting our organisation a second time.

## Task 03 | Detection and Analysis
+ Detection
  + Deeply dependent on the preparation step.
     + Organizations need to put in place monitoring and detection systems (e.g., SIEM, IDS, EDR) to help them proactively identify any potential threat within their infrastructure.
  + All these tools and systems must be integrated with policies and procedures to ensure that the proper teams are alerted in the event of a potential incident.
+ Analysis
  + Is when the IRT actually comes into action.
  + Windows Task Manager is a useful system utility that provides information about running applications, processes, and system performance. It allows users to monitor and manage system resources and troubleshoot issues.
  + Identifying the Infection Vector
      + Once the incident has been confirmed, the IRT must understand and document the initial access vector.
      + Some of the most common vectors are exploiting known vulnerabilities in internet-facing systems, phishing and social engineering, credential stuffing and brute force attacks, drive-by downloads, and supply-chain attacks.
      + Understanding the initial access vector is crucial because it helps pinpoint the “hole” in the system, allowing for targeted remediation efforts to patch it.
      + Statistically, a workstation is most commonly compromised by careless actions carried out by end-users.
  + To fully understand a malicious process's actions, you’ll need to carry out more advanced actions, but this goes beyond the scope of this room. However, for the purposes of this room, it’s assumed that the IRT has already analyzed and identified the malicious program.
+ DOCM indicates that the file is a Macro-enabled Word Document, which means that it likely contains macros.
  + This can indicate the file might contain malicious embedded code.
  + This code was most probably automatically executed when the user opened the document the first time.
+ Macros are often used to save time and improve efficiency by streamlining complex or frequently performed operations. However, malicious actors can also leverage them to carry out dangerous actions.
+ `certutil`
  + A command-line utility in Windows used for managing and manipulating certificates and certificate authority databases.
  + Part of the Windows Certificate Services and can perform various functions.
  + Can be used to stealthily download a file because it leverages a legitimate, pre-installed Windows utility, trusted by default and often allowed through security measures. This method avoids the need for additional tools that might be detected by antivirus software. The command generates minimal noise, blending in with normal administrative operations, making it less likely to be flagged.
+ The Run keys in the Windows Registry specify programs to be automatically executed when a user logs in. Often leveraged by malware to ensure persistence in the infected system even after rebooting.

#### Scenario
1. I read the scenario to understand the context of the situation.
2. Upon starting the VM, I noticed that it was slow by clicking and dragging my mouse and opening the File Explorer. So, I opened Task Manager to see what’s running. A process called “32th4ckm3.exe” is hogging 50% of the CPU.
3. I looked at the process’s properties and found that it’s located in a temp folder. This is suspicious because it’s a common trait of malware, but extremely uncommon for legitimate programs.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room18/Screenshots/1.png)

4. Another thing we can check is if the process is making any outbound connections. To do this, I need its process ID; which can be found in Task Manager by right-clicking on it. Its PID is 4768.
5. I executed `netstat -aofn | find "{4768}"` in the Command Prompt to see its connections. The command returned an IP address and port combo of  45.33.32.156:42424.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room18/Screenshots/2.png)


6. As per the IT team’s report, the user said that they were browsing the Internet when their computer started slowing down. Thus, the first thing to do would be to look in the browser.  Upon opening Microsoft Edge’s download history, I found a file with an odd name, a worrying extension, and a suspicious origin called “invoice n. 65748224.docm”.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room18/Screenshots/3.png)

7. I opened the document, looked at its macros, and found one called “AutoOpen”. By looking at its VBA code I see two lines of code stick out because they have the process name found earlier; the first is a variable with the URL of the file and another mentioning an environment variable. I’m not too familiar with VBA, so I used Google and the room to guide me through it. To summarize, the macro does the following when the user opens the word document: checks if a similar program already exists on the machine, preps the download by declaring where it should go, make sure the download is stealthy by using the `certutil` command, downloads the malicious program from the URL, and adds a Windows Registry entry to ensure the program runs every time the user logs in.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room18/Screenshots/4.png)

## Task 04 | Containment, Eradication, and Recovery
+ Incident responders need to compile a report containing all the details of the actions we've taken.
  + Before deleting any artefact from the machine or killing any involved process, remember to keep track of file names, folders, and other details that have been encountered.
  + These are all very important data that need to be included in our report.
+ Containment
  1. Isolate the affected machine(s).
  2. Kill the process(es).
  3. Compile a list of the IoCs collected during the analysis and action on them by sweeping the organisation with all the tools at our disposal for any other occurrences.
+ Searching across the network for these IoCs will help identify and remediate any other compromised host within the organisation. Feeding the IoCs to monitoring tools will prevent later infection from the same threat.
+ To fully eradicate a threat from a machine, you’ll need to delete any artefacts that were dropped on it.

## Task 05 | Closing the Cycle
+ Post-Incident Activity
  + A critical step that focuses on learning from the incident to enhance future response efforts and overall security posture.
  + Involves thoroughly reviewing the incident, documenting lessons learned, and integrating these insights into the IRP developed during the preparation phase.
  + By doing so, organisations can continuously improve their readiness and resilience against future threats.
+ Back to Preparation
  + The foundational and most crucial step both in the NIST Incident Response lifecycle and the SANS Incident Response 101, yet it’s often overlooked despite its importance.
  + This phase involves creating a comprehensive IRP, which is pivotal for ensuring an organisation's readiness to handle cyber security incidents effectively.
  + After each incident is closed, the insights coming from the post-incident activity step should be used to integrate the IRP that was defined during the original preparation phase.

## Task 06 | Conclusion
+ Mastering the incident response process is essential for safeguarding an organisation's digital assets and ensuring business continuity in the face of cyber threats.
+ The NIST Incident Response lifecycle—with its phases of preparation; detection and analysis; containment, eradication and recovery; and post-incident activity—provide a comprehensive framework for effectively managing and mitigating incidents. The critical insights gained from each phase must be reintegrated into the preparation phase to continually enhance the IRP.
+ The cycle is continuous and essential for staying ahead in the ever-evolving landscape of cyber security.