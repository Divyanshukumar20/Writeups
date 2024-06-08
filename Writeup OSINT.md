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

##### QUESTION:

What username does the attacker go by?

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

##### QUESTIONS:

1. What is the full email address used by the attacker?
2. What is the attacker's full real name?

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

### Task 4:UNVEIL

##### Background:

It seems the cybercriminal is aware that we are on to them. As we were investigating into their Github account we observed indicators that the account owner had already begun editing and deleting information in order to throw us off their trail. It is likely that they were removing this information because it contained some sort of data that would add to our investigation. Perhaps there is a way to retrieve the original information that they provided? 

##### INSTRUCTION:

On some platforms, the edited or removed content may be unrecoverable unless the page was cached or archived on another platform. However, other platforms may possess built-in functionality to view the history of edits, deletions, or insertions. When available this audit history allows investigators to locate information that was once included, possibly by mistake or oversight, and then removed by the user. Such content is often quite valuable in the course of an investigation. In order to answer the below questions, you will need to perform a deeper dive into the attacker's Github account for any additional information that may have been altered or removed. You will then utilize this information to trace some of the attacker's cryptocurrency transactions.

##### QUESTIONS:

1. What cryptocurrency does the attacker own a cryptocurrency wallet for?
2. What is the attacker's cryptocurrency wallet address?
3. What mining pool did the attacker receive payments from on January 23, 2021 UTC?
4. What other cryptocurrency did the attacker exchange with using their cryptocurrency wallet?

##### WRITEUP:

As suggested in the instruction we have to find any information regarding cryptocurrency in github account of this username.

![github account](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Github%20handle.png)

If we see the repository of this acoount we found that there is ETH named repository also.Let's check that

![ETH repository](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/ETH%20repository.png)

In the file miningscript we found some relevant keywords like walletid,miningpool.Let's check the history of this file.

![History of eth](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/History%20of%20eth.png)

It shows two files We must see update miningscript commit to see the change.

![Wallet address](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Wallet%20address.png)

And here we got the wallet address as *0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef*

Now using this address we track the transaction of cryptocurrency.

On searching google cryptocurrency transaction tracker we found some good results.

![Transaction tracker](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Transaction%20tracker%20google%20search.png)

Let's click the first one and paste the wallet address in search box of this website.

![Search address](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/search%20wallet%20address.png)

We can see one result which shows that the cryptocurrency used here is *Ethereum*

![Etherum](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Etherum%20bitcoin.png)

Now if we scroll little bit to get the details of the transaction.We see the On 23 January 2021 UTC (Screenshot shown time in IST) there was transaction made from *Ethermine* mining pool.

![Ethermine](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Ethermine.png)

If we explore more to find other cryptocurrency we see that there is token section which gives the details of the other cryptocurrrency exchange which is *Tether*

![Other cryptocurrency](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Other%20cryptocurrency.png)

* Answer 1 : **Ethereum**
  
* Answer 2 : **0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef**

* Answer 3 : **Ethermine**

* Answer 4 : **Tether**

---

### Task 5:TAUNT

##### Background:

Just as we thought, the cybercriminal is fully aware that we are gathering information about them after their attack. They were even so brazen as to message the OSINT Dojo on Twitter and taunt us for our efforts. The Twitter account which they used appears to use a different username than what we were previously tracking, maybe there is some additional information we can locate to get an idea of where they are heading to next?

We've taken a screenshot of the message sent to us by the attacker, you can view it in your browser [here]().

##### Instructions:

Although many users share their username across different platforms, it isn't uncommon for users to also have alternative accounts that they keep entirely separate, such as for investigations, trolling, or just as a way to separate their personal and public lives. These alternative accounts might contain information not seen in their other accounts, and should also be investigated thoroughly. In order to answer the following questions, you will need to view the screenshot of the message sent by the attacker to the OSINT Dojo on Twitter and use it to locate additional information on the attacker's Twitter account. You will then need to follow the leads from the Twitter account to the Dark Web and other platforms in order to discover additional information.

##### Questions:

1. What is the attacker's current Twitter handle?
2. What is the URL for the location where the attacker saved their WiFi  SSIDs and passwords?
3. What is the BSSID for the attacker's Home WiFi?

##### Writeup:
 After downloading the image we can se the message is from the same twitter account which we found during task 2.

 ![Twitter account]()

Hence the attacker's current handle is *SakuraLoverAiko*.

Now if we research this account little bit we found there is a screenshot related to wifi. His one tweet shows that he must hide something on dark web.If we clearly see this tweet some text are uppercase which gives us some hint.

![That tweet]()

Let's search this uppercase letter DEEP PASTE on google and research on it.

![Google search Deep paste]()

After little bit research I found that Deep paste is a onion website of dark web in which we store data privately.It means this image must be of Deeppaste website.

![Deeppaste tweet image]()

We cant't open onion website with normal browser. To open this website I need tor browser. So first I download the tor browser and then search on it. I observe that simply search Deeppaste won't get the the actual website.

So after lot of research on finding the links to open Deeppaste website I finally get the link on reddit.

Next I visit the link and now we are on a Deeppaste website.

![Deeppaste website]()

Click on search pastes.On the twitter post of the screenshot that is related to Wi-fi have some information regarding how to search it shows a large text which maybe a MD5 hash as their is option to search using MD5 in website.
![MD5 written]()

If we search using this MD5 value we get the Wi-Fi details written.So the answer for the second question is the link which we are currently in.

![Wi-FI details]()

Next,We have to find the BSSID of the his home's Wi-Fi as we already have SSID of the same.So we just google BSSID finder.

![Google BSSID Finder]()

Let's click on the first result.Now we are into wigle website.First I make account on this website.

![Wigle weebsite]()

After that finding the BSSID using advance search option and write the SSID here and enter. After little bit scrolling we see there is one result we get and the BSSID is written here as *84:af:ec:34:fc:f8*.

![Searching using SSID]()

![BSSID]()

* Answer 1: SakuraLoverAiko

* Answer 2: http://deepv2w7p33xa4pwxzwi2ps4j62gfxpyp44ezjbmpttxz3owlsp4ljid.onion

* Answer 3: 84:af:ec:34:fc:f8

  







