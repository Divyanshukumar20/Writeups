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

### Task 3:RECONNAISSANCE

##### BACKGROUND:

It appears that our attacker made a fatal mistake in their operational security. They seem to have reused their username across other social media platforms as well. This should make it far easier for us to gather additional information on them by locating their other social media accounts. 

##### INTRODUCTION:

Most digital platforms have some sort of username field. Many people become attached to their usernames, and may therefore use it across a number of platforms, making it easy to find other accounts owned by the same person when the username is unique enough. This can be especially helpful on platforms such as on job hunting sites where a user is more likely to provide real information about themselves, such as their full name or location information.

 
A quick search on a reputable search engine can help find matching usernames on other platforms, and there are also a large number of specialty tools that exist for that very same purpose. Keep in mind, that sometimes a platform will not show up in either the search engine results or in the specialized username searches due to false negatives. In some cases you need to manually check the site yourself to be 100% positive if the account exists or not. In order to answer the following questions, use the attacker's username found in Task 2 to expand the OSINT investigation onto other platforms in order to gather additional identifying information on the attacker. Be wary of any false positives!

##### WRITEUP:

As question suggest we have to find the different social media handles of username *SakuraSnowAngelAiko*.

We can use tools like `sherlock` to do the same but let's try easy one by searching the username just on google search.

![Google search image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Google%20_search.png)

It shows two social media handles in top.First we open github account of this username.

![github image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/github_progile_pic.png)

Here we find many repository.On exploring this we get nothing in directories except that PGP one.

There is public key present in this.We open it and here we got the pgp public key.

![Public key](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/public_key.png)

Maybe this public key contains some information.So we try to decode this key.

Search on google the pgp public key decoder and we click on the first option.

![Decoder website](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Decoder%20website.png)

Now we paste our key here and click decode.

![Userid image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Userid.png)

After little bit scrolling we get the userid as *SakuraSnowAngel83@protonmail.com*.

Now we check the twitter handle of this username,the 2nd search result of google.

After exploring his/her twitter handle we get this 

![Twitter handle](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/twitter_handle.png)

It suggests that the full name must be *Aiko Abe*.

Hence our final answer is **`SakuraSnowAngel83@protonmail.com`** and **`Aiko Abe`**

---



