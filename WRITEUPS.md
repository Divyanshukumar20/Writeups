# CSOC Week 1 Forensics

### Challenge 1

---

### Information
##### Challenge Description:

Files can always be changed in a secret way. Can you find the flag?

[cat.jpg](cat.jpg)

##### Writeup:
First we use `file` command to see the type of file.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ file cat.jpg
cat.jpg: JPEG image data, JFIF standard 1.02, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 2560x1598, components 3
```
As we can see this is JPEG format and nothing is interesting here.

Now,we see the metadata of this file using `exiftool`.This tool helps us to read,write and edit metadata.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ exiftool cat.jpg                                             
ExifTool Version Number         : 12.76
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2024:06:05 22:47:17+05:30
File Access Date/Time           : 2024:06:05 22:47:17+05:30
File Inode Change Date/Time     : 2024:06:05 22:47:17+05:30
File Permissions                : -rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1

```
Here everything is right but the thing here is License which is little bit weird.The license string doesn't look like license.This is the mixture of uppercase lowercase letters,number.

May be this is a base64 encoded data.

We can decode this string using `base64` command with `-d` parameter to decode.

First we print the string using `echo` command and then use `base64 -d`

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ echo cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 | base64 -d
picoCTF{the_m3tadata_1s_modified}  
```
And here we got our flag as **`picoCTF{the_m3tadata_1s_modified}`**

---

### Matryoshka doll
##### Challenge Desrciption:
Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? 

Image: [this](dolls.jpg)


##### Writeup:

As challenge suggest that there is nested files here.We know that to idnetifies and extract the embedded files we use `binwalk` tool.

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ binwalk dolls.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378950, uncompressed size: 383938, name: base_images/2_c.jpg
651608        0x9F158         End of Zip archive, footer length: 22

```
As we can see there is a embedded file.

We use `-e` parameter to extract with `binwalk` tool to extract the embedded file.

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ binwalk -e dolls.jpg       

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378950, uncompressed size: 383938, name: base_images/2_c.jpg
651608        0x9F158         End of Zip archive, footer length: 22

                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads]
└─$ ls
_dolls.jpg.extracted  cat.jpeg  cat.jpg  dolls.jpg  download.jpeg  google-chrome-stable_current_amd64.deb  t.txt  x.bin
```
As we can see after extracting we got a new directory `_dolls.jpg.extracted`.

Lets explore this directory.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ cd _dolls.jpg.extracted  
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted]
└─$ ls
4286C.zip  base_images
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted]
└─$ cd base_images         
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images]
└─$ ls
2_c.jpg

```
Here there is another image file which we extract from the initial one.

Now we do the same again, use `binwalk -e` command again and again till we get the flag.

```shell
──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images]
└─$ binwalk -e 2_c.jpg  

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 526 x 1106, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
187707        0x2DD3B         Zip archive data, at least v2.0 to extract, compressed size: 196043, uncompressed size: 201445, name: base_images/3_c.jpg
383805        0x5DB3D         End of Zip archive, footer length: 22
383916        0x5DBAC         End of Zip archive, footer length: 22

                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images]
└─$ ls                
2_c.jpg  _2_c.jpg.extracted
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images]
└─$ cd _2_c.jpg.extracted  
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted]
└─$ ls
2DD3B.zip  base_images
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/Downloads/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted]
└─$ cd base_images       
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images]
└─$ ls
3_c.jpg
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images]
└─$ binwalk -e 3_c.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 428 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
123606        0x1E2D6         Zip archive data, at least v2.0 to extract, compressed size: 77651, uncompressed size: 79809, name: base_images/4_c.jpg
201423        0x312CF         End of Zip archive, footer length: 22

                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images]
└─$ ls
3_c.jpg  _3_c.jpg.extracted
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_dolls.jpg.extracted/base_images/_2_c.jpg.extracted/base_images]
└─$ cd _3_c.jpg.extracted 
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/base_images/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted]
└─$ ls
1E2D6.zip  base_images
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/base_images/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted]
└─$ cd base_images       
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted/base_images]
└─$ ls
4_c.jpg
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted/base_images]
└─$ binwalk -e 4_c.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 320 x 768, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
79578         0x136DA         Zip archive data, at least v2.0 to extract, compressed size: 65, uncompressed size: 81, name: flag.txt
79787         0x137AB         End of Zip archive, footer length: 22

                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted/base_images]
└─$ ls
4_c.jpg  _4_c.jpg.extracted
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/_2_c.jpg.extracted/base_images/_3_c.jpg.extracted/base_images]
└─$ cd _4_c.jpg.extracted 
                                                                                                                                                                                             
┌──(rinshu㉿kali)-[~/…/base_images/_3_c.jpg.extracted/base_images/_4_c.jpg.extracted]
└─$ ls
136DA.zip  flag.txt
```
After extracting the file from multiple images, finally we get our flag file.

Catenate this to get the flag.
```shell
┌──(rinshu㉿kali)-[~/…/base_images/_3_c.jpg.extracted/base_images/_4_c.jpg.extracted]
└─$ cat flag.txt         
picoCTF{4f11048e83ffc7d342a15bd2309b47de}
```
Here we got our flag as **`picoCTF{4f11048e83ffc7d342a15bd2309b47de}`**.

---

### tunn3l v1s10n

##### Challenge Description
We found this [file](tunn3l_v1s10n (1)). Recover the flag.

##### Writeup:

After Downloading the file we open it using image viewer.

Here the image can't be open as image viewer shows that BMP image has unsupported header size.

![Image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Screenshot_2024-06-06_19_33_53.png)

It means that there is something error in hex value of Bitmap header format.

We have to check the hexvalue of file and edit it with `hexedit` command.

First we see the format of hex values i.e. signature,size of file,offset,etc. of BMP file on google.

![Image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Screenshot_2024-06-06_20-31-20.png)
![Image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Screenshot_2024-06-06_20-32-16.png)

As this searching suggests that how much bytes particular function take.From this information we can easily identify which byte is dedicated to which function.

Now we check the hexvalue of the file and edit it where needed using `hexedit` command.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ hexedit tunn3l_v1s10n1

```
![Old hex value](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Old%20hex%20value.png)

Here the signature is correct `42 4D`. The offset mainly have 54 bytes value(36 00 00 00),but here it is not correct so we change `BA D0` to `36 00`.

There are more errors in this.The header size must be of 40 bytes (28 00 00 00).

After modifying the hex value we save this by pressing **F2** key.

![New hex value](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/New%20hex%20value.png)

After this we check the image again in image viewer.
Now the image is showing but we don't get the flag as this flag is fake, but the image seems like it is more.

![Fake flag image](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/tunn3l_v1s10np)

We have to increase the height of image using `hexedit` again.

We can easily identify where the height value is, using above reference.
we change `32 01` to a increased size,let `32 04` and press F2.

![Increased height hex value](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Increased%20height%20hex%20value.png)

If we open our Image now it gives us the flag although the height of image is not perfect but doesn't matter.

![Final image of flag](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/tunn3l_v1s10n)

Here we got our flag as **`picoCTF{qu1t3_a_v13w_2020}`**

---

### MacroHard WeakEdge

##### Challenge Description:
I've hidden a flag in this file. Can you find it? 

[Forensics is fun.pptm](https://github.com/Divyanshukumar20/Writeups/blob/main/Writeup_files/Forensics%20is%20fun.pptm)

##### Writeup:
As we can see this file have extension .pptm so first we check the type of file using `file` command.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ file 'Forensics is fun.pptm'
Forensics is fun.pptm: Microsoft PowerPoint 2007+
```
It is a powerpoint file so we open it using Microsoft Powerpoint to find the flag.

But we finds nothing interesting in powerpoint.
So we try to find the hiddden files inside the file.

We try to unzip the file using `unzip` command to get the files hidden in it.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ unzip 'Forensics is fun.pptm' 
Archive:  Forensics is fun.pptm
  inflating: [Content_Types].xml     
  inflating: _rels/.rels             
  inflating: ppt/presentation.xml    
  inflating: ppt/slides/_rels/slide46.xml.rels  
  inflating: ppt/slides/slide1.xml   
  ....
  inflating: ppt/vbaProject.bin      
  inflating: ppt/presProps.xml       
  inflating: ppt/viewProps.xml       
  inflating: ppt/tableStyles.xml     
  inflating: docProps/core.xml       
  inflating: docProps/app.xml        
  inflating: ppt/slideMasters/hidden 
```
After extracting we get so many files but all .xml and .rels files are not of our use because this are powerpoint files.

Also if we see the last file which is unzip have name hidden.
So there may be a chance of hidden flag inside it.

If we try to catenate this file

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ cat ppt/slideMasters/hidden
Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q     
```
We get a text which maybe a encode flag text.

First we remove the spaces between the alphabet and then we try to decode this text using `base64`.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ echo ZmxhZzogcGljb0NURntEMWRfdV9rbjB3X3BwdHNfcl96MXA1fQ | base64 -d
flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}base64: invalid input

```
Thus, We get our flag as **`picoCTF{D1d_u_kn0w_ppts_r_z1p5}`**

---

### Enhance!
##### Challenge Description:

Download this image file and find the flag.

[Download image file](drawing.flag.svg)

##### Writeup:

After downlaoding this svg file, first we open this file in a image viewer but we see nothing here.

Then we check the file type using `file` command.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ file drawing.flag.svg 
drawing.flag.svg: SVG Scalable Vector Graphics image

```
It shows simple svg file.No hint at all.

Now we see the metadata of this file using `exiftool` to get some hints.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ exiftool drawing.flag.svg 
ExifTool Version Number         : 12.76
File Name                       : drawing.flag.svg
Directory                       : .
File Size                       : 4.1 kB
File Modification Date/Time     : 2024:06:07 00:12:11+05:30
File Access Date/Time           : 2024:06:07 00:12:12+05:30
File Inode Change Date/Time     : 2024:06:07 00:12:12+05:30
File Permissions                : -rw-rw-r--
File Type                       : SVG
File Type Extension             : svg
MIME Type                       : image/svg+xml
Xmlns                           : http://www.w3.org/2000/svg
Image Width                     : 210mm
Image Height                    : 297mm
View Box                        : 0 0 210 297
SVG Version                     : 1.1
ID                              : svg8
Version                         : 0.92.5 (2060ec1f9f, 2020-04-08)
Docname                         : drawing.svg
Metadata ID                     : metadata5
Work Format                     : image/svg+xml
Work Type                       : http://purl.org/dc/dcmitype/StillImage
Work Title                      : 

```
Everything is fine but here the Work Format shows svg+xml means this is a xml file.

It means we can see the code of xml language using `cat` command.

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ cat drawing.flag.svg                 
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!-- Created with Inkscape (http://www.inkscape.org/) -->

<svg
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   width="210mm"
   height="297mm"
   viewBox="0 0 210 297"
   version="1.1"
   id="svg8"
   inkscape:version="0.92.5 (2060ec1f9f, 2020-04-08)"
   sodipodi:docname="drawing.svg">
  <defs
     id="defs2" />
  <sodipodi:namedview
     id="base"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageopacity="0.0"
     inkscape:pageshadow="2"
     inkscape:zoom="0.69833333"
     inkscape:cx="400"
     inkscape:cy="538.41159"
     inkscape:document-units="mm"
     inkscape:current-layer="layer1"
     showgrid="false"
     inkscape:window-width="1872"
     inkscape:window-height="1016"
     inkscape:window-x="48"
     inkscape:window-y="27"
     inkscape:window-maximized="1" />
  <metadata
     id="metadata5">
    <rdf:RDF>
      <cc:Work
         rdf:about="">
        <dc:format>image/svg+xml</dc:format>
        <dc:type
           rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
        <dc:title></dc:title>
      </cc:Work>
    </rdf:RDF>
  </metadata>
  <g
     inkscape:label="Layer 1"
     inkscape:groupmode="layer"
     id="layer1">
    <ellipse
       id="path3713"
       cx="106.2122"
       cy="134.47203"
       rx="102.05357"
       ry="99.029755"
       style="stroke-width:0.26458332" />
    <circle
       style="fill:#ffffff;stroke-width:0.26458332"
       id="path3717"
       cx="107.59055"
       cy="132.30211"
       r="3.3341289" />
    <ellipse
       style="fill:#000000;stroke-width:0.26458332"
       id="path3719"
       cx="107.45217"
       cy="132.10078"
       rx="0.027842503"
       ry="0.031820003" />
    <text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:0.00352781px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#ffffff;fill-opacity:1;stroke:none;stroke-width:0.26458332;"
       x="107.43014"
       y="132.08501"
       id="text3723"><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.08501"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3748">p </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.08942"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3754">i </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.09383"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3756">c </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.09824"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3758">o </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.10265"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3760">C </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.10706"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3762">T </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.11147"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3764">F { 3 n h 4 n </tspan><tspan
         sodipodi:role="line"
         x="107.43014"
         y="132.11588"
         style="font-size:0.00352781px;line-height:1.25;fill:#ffffff;stroke-width:0.26458332;"
         id="tspan3752">c 3 d _ d 0 a 7 5 7 b f }</tspan></text>
  </g>
</svg>

```
Yes,we are correct this file store a xml code with svg code inside it.Now if analyze the code written in `<text>` tag then we can see our flag.

In every `<tspan>` tag there is a alphabet, combining this all we get the flag.


Thus our flag is **`picoCTF{3nh4nc3d_d0a757bf}`**

---

### advance-potion-making

##### Challenge Description:
Ron just found his own copy of advanced potion making, but its been corrupted by some kind of spell. Help him recover it!
[advance-potion-making]()

##### Writeup:
After downlaoding we check the file type using `file` command.
```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ file advanced-potion-making
advanced-potion-making: data

```
We get nothing interesting here.

Let's try `exiftool` to get the metadata of the file.

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ exiftool advanced-potion-making
ExifTool Version Number         : 12.76
File Name                       : advanced-potion-making
Directory                       : .
File Size                       : 30 kB
File Modification Date/Time     : 2024:06:07 01:36:23+05:30
File Access Date/Time           : 2024:06:07 01:36:24+05:30
File Inode Change Date/Time     : 2024:06:07 01:36:24+05:30
File Permissions                : -rw-rw-r--
Error                           : Unknown file type
                    
```
Here also we see nothing much as this is unknown file type.

Let's check the hexdata of the file using `xxd` command to identify the file type.

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ xxd advanced-potion-making  
00000000: 8950 4211 0d0a 1a0a 0012 1314 4948 4452  .PB.........IHDR
00000010: 0000 0990 0000 04d8 0802 0000 0004 2de7  ..............-.
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 0000 1625 0000 1625 0149  ..pHYs...%...%.I

```
The signature is not known to any file but it is similar to png signature (89 50 4E 47).Also there is IHDR,sRGB,gAMA written in strings which gives us hint that it can be a png image file.







