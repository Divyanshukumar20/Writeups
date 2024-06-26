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

We've taken a screenshot of the message sent to us by the attacker, you can view it in your browser [here](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/taunt.png).

##### Instructions:

Although many users share their username across different platforms, it isn't uncommon for users to also have alternative accounts that they keep entirely separate, such as for investigations, trolling, or just as a way to separate their personal and public lives. These alternative accounts might contain information not seen in their other accounts, and should also be investigated thoroughly. In order to answer the following questions, you will need to view the screenshot of the message sent by the attacker to the OSINT Dojo on Twitter and use it to locate additional information on the attacker's Twitter account. You will then need to follow the leads from the Twitter account to the Dark Web and other platforms in order to discover additional information.

##### Questions:

1. What is the attacker's current Twitter handle?
2. What is the URL for the location where the attacker saved their WiFi  SSIDs and passwords?
3. What is the BSSID for the attacker's Home WiFi?

##### Writeup:
 After downloading the image we can se the message is from the same twitter account which we found during task 2.

 ![Twitter account](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Twitter%20Handle.png)

Hence the attacker's current handle is *SakuraLoverAiko*.

Now if we research this account little bit we found there is a screenshot related to wifi. His one tweet shows that he must hide something on dark web.If we clearly see this tweet some text are uppercase which gives us some hint.

![That tweet](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Tweet.png)

Let's search this uppercase letter DEEP PASTE on google and research on it.

![Google search Deep paste](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Deep%20Paste%20google%20search.png)

After little bit research I found that Deep paste is a onion website of dark web in which we store data privately.It means this image must be of Deeppaste website.

![Deeppaste tweet image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/FfFDMydaUAAVJPf.png)

We cant't open onion website with normal browser. To open this website I need tor browser. So first I download the tor browser and then search on it. I observe that simply search Deeppaste won't get the the actual website.

So after lot of research on finding the links to open Deeppaste website I finally get the link on reddit.

Next I visit the link and now we are on a Deeppaste website.

![Deeppaste website](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Deep%20Paste%20website.png)

Click on search pastes.On the twitter post of the screenshot that is related to Wi-fi have some information regarding how to search it shows a large text which maybe a MD5 hash as their is option to search using MD5 in website.
![MD5 written](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/MD5%20written.png)

If we search using this MD5 value we get the Wi-Fi details written.So the answer for the second question is the link which we are currently in.

![Wi-FI details](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Wi-Fi%20Details.png)

Next,We have to find the BSSID of the his home's Wi-Fi as we already have SSID of the same.So we just google BSSID finder.

![Google BSSID Finder](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/BASSID%20finder%20google%20search.png)

Let's click on the first result.Now we are into wigle website.First I make account on this website.

![Wigle website](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Wigle%20website.png)

After that finding the BSSID using advance search option and write the SSID here and enter. After little bit scrolling we see there is one result we get and the BSSID is written here as *84:af:ec:34:fc:f8*.

![Searching using SSID](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/SSID%20search.png)

![BSSID](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/BSSID%20found.png)

* Answer 1: SakuraLoverAiko

* Answer 2: http://deepv2w7p33xa4pwxzwi2ps4j62gfxpyp44ezjbmpttxz3owlsp4ljid.onion

* Answer 3: 84:af:ec:34:fc:f8

---

### Task 6: HOMEBOUND

##### Background:

Based on their tweets, it appears our cybercriminal is indeed heading home as they claimed. Their Twitter account seems to have plenty of photos which should allow us to piece together their route back home. If we follow the trail of breadcrumbs they left behind, we should be able to track their movements from one location to the next back all the way to their final destination. Once we can identify their final stops, we can identify which law enforcement organization we should forward our findings to.

##### Instruction:

In OSINT, there is oftentimes no "smoking gun" that points to a clear and definitive answer. Instead, an OSINT analyst must learn to synthesize multiple pieces of intelligence in order to make a conclusion of what is likely, unlikely, or possible. By leveraging all available data, an analyst can make more informed decisions and perhaps even minimize the size of data gaps. In order to answer the following questions, use the information collected from the attacker's Twitter account, as well as information obtained from previous parts of the investigation to track the attacker back to the place they call home.

##### Questions:

1. What airport is closest to the location the attacker shared a photo from prior to getting on their flight?
2. What airport did the attacker have their last layover in?
3. What lake can be seen in the map shared by the attacker as they were on their final flight home?
4. What city does the attacker likely consider "home"?

##### Writeup:

In first question the photo mentioned is this.

![cherry](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/cherry.png)

To find which place in this photo we use google lens first scan this image from every possible aspect.

![GL_cherry1](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/GL%20cherry1.png)
![GL_cherry2](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Gl%20cherry2.png)
![GL_cherry3](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Gl%20cherry3.png)
![GL_cherry4](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/GL%20cherry4.png)

Here we can see the this washington monument is most matched result to this image.We can say that this is a place near washington monument.So we just search nearest airport to washington monument in google.

![nearest airport search](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/nearest%20%20airport%20search.png)

This is the airport we use symbol of airport for answer i.e *DCA*

Going to second question this is the required photo 

![Lounge photo](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Lounge.png)

As done previously we scan this image using google lens and we get the similar image.

![GL_Lounge](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/GL%20lounge.png)

If we click on the link we redirected to the website we found that it is a pic of haneda airport that have code *HND*.So this is our answer for the second question.

![HND link](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/HND%20link.png)

Now headed to the third question the image here is a map or picture clicked by sky.

![japan map](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/japan_map.png)

Again try to use google lens.And here we found some similar image.

![GL_japan map](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/GL_map.png)

If we click on the link we redirected to the website which shows us a map

![Website map1](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Websitemap1.png)

![Website map2](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Website%20map2.png)

Zoom this map to get the name of the lake.

![Lake name](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Lake%20name.png)

The lake name is *Lake Inawashiro*

Going to the fourth question we already know the name of city where his home is, as in task 4 while collecting information about wifi in deeppaste link we can see the name of his city.

![wifi details](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Wi-Fi%20Details.png)

The City name is *Hirosaki*.

---

### Grahlix Exercises

---

### Exercise 6

##### Task review:

On January 19, 2023, a journalist with almost 140k followers on Twitter shared an image of a destroyed vehicle amidst a large cloud of smoke and fire. The tweet said: “BREAKING: TTP carried out a suicide attack on a police post in Khyber city of Pakistan that killed three Pakistani police officers.“

The photo is not of the event described by the journalist.
a) Verify the statement above.

Click [here](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/osintexercise006.webp) to open the photo.

##### Writeup:

If we carefully observed the image then it doesn't seems like a suicide attack here the car blasted so badly.

Anyway if we search this image using reverse image search, on Tineye we got 378 results and after sorting with date the first image shows a date of 2009. But the Task image shows it is of 2023 image.
![Result car blast](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Result%20car%20balst.png)

If we click on the first link of tineye we got to know that this is not a image of pakistan suicide attack but it is a image of Explosions occurred on Sunday near the office of the governor of Baghdad and the Ministry of Justice.It says that explosive devices were planted in cars parked near the buildings which seems true.

![Website baghdad](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Website_baghdad.png)
![scroll down](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Scroll%20down%20baghdad.png)

Hence this is a not a image of pakistan this statement is proved.

---

### Exercise 4

##### Task Review:

This is a photo of a resort located on an island.
a) What is the name of the resort?
b) What are the coordinates of the island?
c) In which cardinal direction was the camera facing when the photo was taken?

Click [here](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/osint-exercise-004-big-picture.jpg) to open the photo on a new tab.

##### Writeup:

As done previously again we are going to search this image on google lens.
And if we see the exact matches we can clearly see the facebook page which display the resort name i.e Oan Resort.

![GL_resort](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Gl_island.png)

Check its coordinates on google and we got it.

![Coordinates](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Coordinates.png)

Now if we try in google map satellite to adjust the island map as same as image we get this.

![Google map island](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/3d%20view_oan%20isalnd.png)

It is almost same as image the compass shows we are facing northwest direction.So camera also must be facing in northwest direction.

*Answer 1:Oan Resort

*Answer 2:7.3626° N, 151.7563° E

*Answer 3:Northwest

---

### Exercise 3

##### Task Review:

In April 2017 Mohamed Abdullahi Farmaajo, the then president of Somalia, visited Turkey. A news agency published a photo where he was seen shaking hands with Recep Tayyip Erdoğan, the country’s president. The article did not disclose where the photo was taken. Your task is to find out the name and coordinates of the location seen below.

Click [here](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/osint-exercise-003-picture.jpg) to see the photo on its own.

##### Writeup:

As usual first we google search this image

![pic 1](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%201.png)

After vising websites, We found that this is a image of president complex of Turkey.

![Pic 2](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%202.png)

Now searching this on google map we got this.This is the satellite view of that presidential complex.

![pic 3](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%203.png)

Given picture is must be the entrance of the building but here two ways and we don't know which one is for what purpose. We have to find exact coordinates where handshake takes place.

![pic 4](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic4.png)

TO get more idea about the structure of required place we pick the more informative pic of same place using google lens.

![pic  5](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%205.png)

Now we get the idea of the strucutre of entrance point but in google map we cant's see the street view of this place so we just search on youtube the presidential complex turkey and we got a video by euronews in which we can clearly see the building from every angle.


Now If we see on this timestamps there is lot more space infront of entrance.

![pic 6](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%206.png)

![pic 7](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%207.png)

And here is the some diamond shape design which is also on map.

![pic 8](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%208.png)

After observing all this we conclude that the pic is of here.

![pic 9](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/pic%209.png)

So the answer is President Complex Turkey,Coordinates (39.931175, 32.799750)

---
### Exercise 14

##### Task Review:

The video below was recorded during an earthquake.
Please find the answer to the following questions:

a) What was the magnitude of this earthquake?
b) What are the coordinates of where the camera was likely located in order to record this scene?

Click [here](https://www.youtube.com/watch?v=myTG1LpMN7g&t=120s) to open the video.

##### Writeup:

So this is a screenshot image from video. We can clearly see the date and time at which the earthquake occur.

![Photo 1](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%201.png)

Let's search earthquake news on this date on goolgle.
We found that at this exact time there was an earhtquake in Vrancea,Romania.This was the same earthquake that is in the video.

![Photo 2](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%202.png)

After getting details of this earthquake by google search finally we get to know that this was a earthquake of magnitude **5.7**

![Photo 3](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%203.png)

Now let's find the location of this footage.First I try to search this image in google lens from every aspect but  it wouldn't give much info.

![Photo 4](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%204.png)

Then I try to search for unique things in building like this green one and the red one.

![Photo 5](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%205.png)

But on google lens I found nothing much.Then I try google searching of unique building in Maldova,chisinau,where earthquake came.

After a lot of scrolling finally I found a building which looks like red one above.

![Photo 6](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%206.png)

![Photo 7](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%207.png)

Getting more details of building.The building name is Atrium.

![Photo 8](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%208.png)

Again after searching about building I finally successfull to locate this on google map.

![Photo 9](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%209.png)

Now analyzing the video again and again I found the direction from the building at which the camera placed.

![Photo 10](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%2010.png)

Following the black path we get our location If we see clearly the black path is same road as in video.
And the red building is same as marked here.
We can relate this map to the picture by connecting every site like car parking that blue building.
Using all this connection we conclude that The yellow one is the building where the camera was fitted.

![Photo 5](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%205.png)

Luckily, the street view of this yellow building is available. If we see the street view it clearly shows that the footage is from here only.

![Photo 11](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%2011.png)

![Photo 12](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/Photo%2012.png)

So the coordinates of this place is **47.017594,28.852750**

---
### Exercise 26

##### Task Review:

The image below shows the contents of a [zip](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/challenge_files/osintexercise026.zip) file. Inside you will find a 31-second video recorded during a train ride, and four photos of undisclosed locations. They were all taken by the same individual in February 2024. Despite having no useful metadata, they still contain enough information to track down this person’s movements.
Your task is to determine:

a) At which train stations did the person board and alight?

Bonus challenges:

Identify the mode of transportation in each image.
Determine the type of train they rode.
Estimate the speed at which the train was travelling when the footage was recorded.
Calculate approximately how far did the person travel overall.

##### Writeup:

This Task is quite lengthy.There are four pictures it seems that we have to identify the locations of this pictures and connect it to get the path travelled by the person.
Here the pictures and video ordered by their name maybe this is ordered as person travels.

So let's start with the first picture.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/1.jpg)

As always we first search this image using google lens.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/2.png)

The result shows the location of this image.

Here we succesfully locate this image and If we connect things in map and in image we get to know the exact place from where the picture is clicked.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/3%20ne.png)

So,This is Chorsu Bazaar located in Tashkent city of Uzbekistan.

Now headed to the next picture.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/4.jpg)

As previously done we again search this image in google lens.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/5.png)

This shows us the result that this picture is of Navruz Park located in the same city Tashkent.

Using this information we successfully locate this image in map.Also we can connect things like Halva Art cafe shop and giant wheel to get the exact location.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/6.png)

Now comes to the third image.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/7%20ne.jpg)

After trying multiple times on google lens we can't find anything great.Now what we have to do is observing small things in the picture like the kapital bank,Samsung galaxy holding,navien building.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/8.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/9.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/10.png)

Searched things on google like samsung galaxy holding in Tashkent city or more but get nothing much.
So start observing more in this image and found that there is Rent a car holding here.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/10%20ne.png)

On searching this on google map get lot of renting car buildings.Start searching each one in Tashkent on map.
Finally found a Orient rent a car as correct.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/11.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/12.png)

If we see the street view near this we are absolutely right as this picture is exactly taken from here.
Hence this image is Oybek,Tashkent.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/7.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/13.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/14.png)

Now going to 4th image.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/15.png)

After a lot of searching on google lens,reversing image by yandex,Tineye,it seems that the pattern on the flyover here is mostly found in samarkand city in uzbekistan.Now let's try to find this image in samarkand.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/16.png)

Observing the little things in image like the pattern,flyover,their is mountain like terrain in background maybe we found this place using this terrain.

Search samarkand on google map and after zooming little bit we can see there is large and small terrains.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/17.png)

If we check the small terrain we see a intersecton of road.And we got it if we check the street view we can see the exact location from where the image is captured.
Our assumption is right this image is from samarkand.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/18.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/19.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/20.png)

Till now our findings conclude many things.
The person visit three place in same city(Tashkent) and pick a train from the station nearby to Oybex and travel to another city samarkand.

If we find the station nearby Oybek, we found that Stantsiya Tashkent Pass Tsentr is the nearest station.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/21.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/22.png)

The person must board the train from here and alight to Samarkand railway station.
This is the answer to our first question.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/23.png)

On searching train between two cities.I found that there is a bullet train who travel between two.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/24.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/25.png)

Watched video atleast 10 times.I found that there is one place which we found easily in the train route between this two station is the road crossing.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/26.png)

The only thing which i can do is to search whole route on map to find this location.

After searching for almost 30 min finally I found this location on map.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/27.png)

And this is the starting position of the video if we clearly see the house and its surrounding in map and video.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/28.png)

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/29.png)

Now calculating distance between two points and the video is of 8 seconds till this location.
We can calculate speed of the train.As the map shows it is 304 m approx and covered in 8 second.
So the speed of the train is 304/8 = 38m/s.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/30.png)

The mode of transport in 1st image must be by foot as seen in the map no vehicle there.
In 2nd pic also the person is by foot as there is no vehicle.
In 3rd pic the person must be using the car because he/she is in the middle of the road and trravelling far distances.
In 4th pic we can clearly see in reflection that the person is in the car.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/15.png)

For calculating the distance travelled by person we first calculate the distance travelled by him/her in tashkent i.e 10.2 km approx as seen in google map.
Now the train travel approx 300 km as seen in map.

![](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeups_files_OSINT/31.png)

So total distance travel is aprrox 311 km.

And here we answer all the questions.

---












