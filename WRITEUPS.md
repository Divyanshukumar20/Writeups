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

















