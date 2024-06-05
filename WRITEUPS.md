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
Here everything is right but the thing here is License which is little bit weird.The license string doesn't look like license.This mixture of uppercase lowercase letters,number.May be this is a base64 encoded data.
We can decode this string using `base64` command with `-d` parameter to decode.First we print the string using `echo` command and then use `base64 -d`

```shell
┌──(rinshu㉿kali)-[~/Downloads]
└─$ echo cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9 | base64 -d
picoCTF{the_m3tadata_1s_modified}  
```
And here we got our flag as `**picoCTF{the_m3tadata_1s_modified}**`


