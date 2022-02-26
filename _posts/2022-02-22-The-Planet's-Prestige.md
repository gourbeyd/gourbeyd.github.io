---
layout: post
title: "The Planet's Prestige"
date: 2022-02-15
image: ../../assets/img/Posts/Gallery.png
categories: [Tryhackme, Easy]
tags: [rkhunter, nano, sqlmap, sqlinjection, searchsploit, CMS, wfuzz, linux]
---

<!-- > CoCanDa, a planet known as 'The Heaven of the Universe' has been having a bad year. A series of riots have taken place across the planet due to the frequent abduction of citizens, known as CoCanDians, by a mysterious force. CoCanDa’s Planetary President arranged a war-room with the best brains and military leaders to work on a solution. After the meeting concluded the President was informed his daughter had disappeared. CoCanDa agents spread across multiple planets were working day and night to locate her. Two days later and there’s no update on the situation, no demand for ransom, not even a single clue regarding the whereabouts of the missing people. On the third day a CoCanDa representative, an Army Major on Earth, received an email.

- [encryptomatic .eml viewer](https://www.encryptomatic.com/viewer/)
- [MsgEml](https://msgeml.com/)

![image](https://user-images.githubusercontent.com/58165365/154682900-4f55c88b-3a6f-40a6-9708-3c757e5e73f6.png)

![image](https://user-images.githubusercontent.com/58165365/154682975-bbd8dce2-6073-4efb-8b5c-d626db32db0d.png)

What is the email service used by the malicious actor? (1 points)

![image](https://user-images.githubusercontent.com/58165365/154679539-5df98e97-18f5-436f-9da4-f66bc75779b2.png)

`emkei.cz`

What is the Reply-To email address? (2 points)

![image](https://user-images.githubusercontent.com/58165365/154679410-76dd6f26-ec3c-4d85-a785-eba34c762158.png)

`negeja3921@pashter.com`

What is the filetype of the received attachment which helped to continue the investigation? (1 points)

```bash
➜  file PuzzleToCoCanDa.pdf
PuzzleToCoCanDa.pdf: Zip archive data, at least v2.0 to extract, compression method=deflate
```

To confirm its actually a zip file, we can use a tool called `xxd` to check the Hex signature/Magic bytes of the file.

```bash
➜  xxd PuzzleToCoCanDa.pdf | head
00000000: 504b 0304 1400 0000 0800 2085 3952 080f  PK........ .9R..
00000010: c628 2031 0000 e648 0000 1e00 0000 5075  .( 1...H......Pu
00000020: 7a7a 6c65 546f 436f 4361 6e44 612f 4461  zzleToCoCanDa/Da
00000030: 7567 6874 6572 7343 726f 776e ed7a 6754  ughtersCrown.zgT
00000040: 134a bb6e e85d 4441 8a14 11a4 8526 4d42  .J.n.]DA.....&MB
00000050: 8ba0 80a0 f412 e922 4a95 de7b 5140 2210  ......."J..{Q@".
00000060: 0101 e95d 3a91 de83 4847 915e a4f7 5e12  ...]:...HG.^..^.
00000070: 6a68 c98d fb3b 5f3d e7ae 7dee 5d67 afef  jh...;_=..}.]g..
00000080: ae75 f764 3df9 31e5 9d79 de99 79cb 24d8  .u.d=.1..y..y.$.
00000090: 09ec 3ce0 aaaa 928a 1200 0f0f 0fe0 85fb  ..<.............
```

If we compare the value's with this [list of file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures), we can acertain that it is indeed a zip file.

![image](https://user-images.githubusercontent.com/58165365/154681110-9baaccf2-851a-4adf-8201-edd9e4931b9b.png)

`.zip`

What is the name of the malicious actor? (2 points)

Unzipping the contents of the zip file, we get:

```bash
➜  unzip PuzzleToCoCanDa.pdf
Archive:  PuzzleToCoCanDa.pdf
  inflating: PuzzleToCoCanDa/DaughtersCrown
  inflating: PuzzleToCoCanDa/GoodJobMajor
  inflating: PuzzleToCoCanDa/Money.xlsx
  ➜  tree
.
├── 3EPrcBh52dtr4XdJB8tdZvLVzVYiSJ.zip
├── A Hope to CoCanDa.eml
├── PuzzleToCoCanDa
│   ├── DaughtersCrown
│   ├── GoodJobMajor
│   └── Money.xlsx
└── PuzzleToCoCanDa.pdf
```

Looking at the file information, we get:

```bash
➜  file DaughtersCrown
DaughtersCrown: JPEG image data, JFIF standard 1.01, resolution (DPI), density 120x120, segment length 16, baseline, precision 8, 822x435, components 3
➜  file GoodJobMajor
GoodJobMajor: PDF document, version 1.5, 1 pages
➜  file Money.xlsx
Money.xlsx: Microsoft Excel 2007+
```

Using a tool called [exiftool]() we can try get metadata on the files.

```bash
➜  ./exiftool GoodJobMajor
ExifTool Version Number         : 12.21
File Name                       : GoodJobMajor
Directory                       : PuzzleToCoCanDa
File Size                       : 28 KiB
File Modification Date/Time     : 2021:01:26 11:14:22-05:00
File Access Date/Time           : 2021:01:26 11:14:22-05:00
File Inode Change Date/Time     : 2022:02:18 06:41:42-05:00
File Permissions                : -rw-r--r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.5
Linearized                      : No
Author                          : Pestero Negeja
Producer                        : Skia/PDF m90
Page Count                      : 1
```

On the `GoodJobMajor` file we get a name under the Author field.

`Pestero Negeja`

What is the location of the attacker in this Universe? (2 points)

![image](https://user-images.githubusercontent.com/58165365/154681615-691615ef-28a3-4a96-8f6b-6ab5f63050f4.png)

If you want to locate hidden cells in Excel:

![image](https://user-images.githubusercontent.com/58165365/154693263-3f64e05d-93a8-452f-956c-38187318c1a2.png)

![image](https://user-images.githubusercontent.com/58165365/154694500-1c08b7d0-3273-4e72-baf1-5ff01a19181d.png)

![image](https://user-images.githubusercontent.com/58165365/154691848-fac6cf71-790a-4c8c-9574-ae417666bceb.png)

`The Martian Colony, Beside Interplanetary Spaceport`

What could be the probable C&C domain to control the attacker’s autonomous bots? (2 points)

`pashter.com` -->
