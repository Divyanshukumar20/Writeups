# CSOC Week 1 OSINT

### Challenge 2

---
### Sakura Room

---

### Task 1:INTRODUCTION

##### Instruction:

Ready to get started? Type in "Let's Go!" in the answer box below to continue.

##### Writeup:

Simple type **Let's Go!** in answer box.

---

### Task 2:TIP-OFF

##### Background:

The OSINT Dojo recently found themselves the victim of a cyber attack. It seems that there is no major damage, and there does not appear to be any other significant indicators of compromise on any of our systems. However during forensic analysis our admins found an image left behind by the cybercriminals. Perhaps it contains some clues that could allow us to determine who the attackers were? 

We've copied the image left by the attacker, you can view it in your browser [here](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/sakurapwnedletter.svg).

##### Instruction:

Images can contain a treasure trove of information, both on the surface as well as embedded within the file itself. You might find information such as when a photo was created, what software was used, author and copyright information, as well as other metadata significant to an investigation. In order to answer the following question, you will need to thoroughly analyze the image found by the OSINT Dojo administrators in order to obtain basic information on the attacker.

##### Writeup:

After downloading the image, we see its metadata using `exiftool` in shell.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ exiftool sakurapwnedletter.svg                                  
ExifTool Version Number         : 12.76
File Name                       : sakurapwnedletter.svg
Directory                       : .
File Size                       : 850 kB
File Modification Date/Time     : 2024:06:07 21:34:55+05:30
File Access Date/Time           : 2024:06:07 21:35:23+05:30
File Inode Change Date/Time     : 2024:06:07 21:35:16+05:30
File Permissions                : -rw-rw-r--
File Type                       : SVG
File Type Extension             : svg
MIME Type                       : image/svg+xml
Xmlns                           : http://www.w3.org/2000/svg
Image Width                     : 116.29175mm
Image Height                    : 174.61578mm
View Box                        : 0 0 116.29175 174.61578
SVG Version                     : 1.1
ID                              : svg8
Version                         : 0.92.5 (2060ec1f9f, 2020-04-08)
Docname                         : pwnedletter.svg
Export-filename                 : /home/SakuraSnowAngelAiko/Desktop/pwnedletter.png
Export-xdpi                     : 96
Export-ydpi                     : 96
Metadata ID                     : metadata5
Work Format                     : image/svg+xml
Work Type                       : http://purl.org/dc/dcmitype/StillImage
Work Title                      : 

```
After analyzing the metadata, we found that in Export-filename the path is given for the file 

`/home/SakuraSnowAngelAiko/Desktop/pwnedletter.png`

And in the file the username must be after home directory.

So here the username must be *SakuraSnowAngelAiko*

Hence the answer is **`SakuraSnowAngelAiko`**

---




