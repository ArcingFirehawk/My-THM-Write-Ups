# Intro to Digital Forensics
+ Description: Learn about digital forensics and related processes and experiment with a practical example.
+ Link: https://tryhackme.com/room/introdigitalforensics
+ Type: Walkthrough
+ Completed: 2025-06-04

## Tools
+ ExifTool
+ pdfinfo

## Vocabulary
+ **Digital Forensics**
  + A branch of forensics focusing on investigating crimes relating to digital systems (e.g., computers, smartphones).
  + The application of computer science to investigate digital evidence for a legal purpose.
+ **Exchangeable Image File Format (EXIF)** A standard for saving metadata to image files.

## Task 01 | Introduction To Digital Forensics
+ Asks the questions:
  + How should the police collect digital evidence?
    + What are the procedures to follow if a digital system is running?
  + How to transfer the digital evidence?
  + How to analyze the collected digital evidence?
+ Digital forensics is used in two types of investigations:
  + Public-Sector: Refer to the investigations carried out by government and law enforcement agencies. They would be part of a crime or civil investigation.
  + Private-Sector: Refer to the investigations carried out by corporate bodies by assigning a private investigator. They are triggered by corporate policy violations.

## Task 02 | Digital Forensics Process
+ Basic Plan for On-Site Digital Forensics
  1. Acquire the evidence.
     + Note that computers require special handling if they are turned on.
  2. Establish a chain of custody.
     + Fill out the related form appropriately.
     + The purpose is to ensure that only the authorized investigators had access to the evidence and no one could have tampered with it.
  3. Place the evidence in a secure container.
     + You want to ensure that the evidence does not get damaged.
     + In the case of smartphones, you want to ensure that they cannot access the network, so they don’t get wiped remotely.
  4. Transport the evidence to your digital forensics lab.
+ Basic Plan for Lab Digital Forensics
  1. Retrieve the digital evidence from the secure container.
  2. Create a forensic copy of the evidence.
     + The forensic copy requires advanced software to avoid modifying the original data.
  3. Return the digital evidence to the secure container.
     + You will be working on the copy. If you damage the copy, you can always create a new one.
     + Start processing the copy on your forensics workstation.
+ Includes
  + Proper search authority: Investigators cannot commence without the proper legal authority.
  + Chain of custody: This is necessary to keep track of who was holding the evidence at any time.
  + Validation with mathematics: Using a special kind of mathematical function, called a hash function, we can confirm that a file has not been modified.
  + Use of validated tools: The tools used in digital forensics should be validated to ensure that they work correctly. For example, if you are creating an image of a disk, you want to ensure that the forensic image is identical to the data on the disk.
  + Repeatability: The findings of digital forensics can be reproduced as long as the proper skills and tools are available.
  + Reporting: The digital forensics investigation is concluded with a report that shows the evidence related to the case that was discovered.

## Task 03 | Practical Example of Digital Forensics
+ Everything we do on our digital devices leaves traces.
+ When you create a text file, some metadata gets saved by the OS.
  + Much of the information is kept within the file’s metadata when you use a more advanced editor.
  + There are various ways to read the file metadata (e.g., open the file within their official viewer/editor or use a forensic tool).
  + Exporting the file to other formats, such as PDF, would maintain most of the metadata of the original document, depending on the PDF writer used.
  + pdfinfo can be installed on a Linux machine using `sudo apt install poppler-utils`.
+ Whenever you take a photo with your smartphone or with your digital camera, plenty of information gets embedded in the image.
  + E.g., camera/smartphone model, date-time of image capture, photo settings.
+ ExifTool can be installed on a Linux machine using `sudo apt install libimage-exiftool-perl`.

#### Scenario
1. I navigated to the directory with the files.
2. I used `pdfinfo ransom-letter.pdf` to display the file’s metadata.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room19/Screenshots/1.png)
3. I ran `exiftool letter-image.jpg` to output the image’s metadata, and more specifically its GPS information.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room19/Screenshots/2.png)
4. After noticing that the output was lengthy, I reran the command with the grep utility, `exiftool letter-image.jpg | grep -i “gps”`.
5. I did the same—`exiftool letter-image.jpg | grep -i “model”`—to find the camera’s model.
   ![](https://github.com/ArcingFirehawk/My-THM-Write-Ups/blob/main/Room19/Screenshots/3.png)