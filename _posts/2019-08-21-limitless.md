---
date: 2019-8-21 23:48:05
layout: post
title:  Limitless from PicoCTF - Binary Exploitation
subtitle: "Created by: Mrx-Exploit"
description: >-
 Created by: Mrx-Exploit
tags: 
 - picoCTF 
 - pwn 
 - source_code_review 
image: >-
 /assets/img/binary-exploitation/limitless/banner.png
optimized_image: >-
 /assets/img/binary-exploitation/limitless/banner.png
category: blog
author: Hazem Hesham
paginate: true
---

# Source Code Review

In this section we will listen to the source code hahaha let's see the main func

![image](/assets/img/binary-exploitation/limitless/discover.png)

there are two variable ( index and value ) and one array , there are two input and in the end they called another func Hmmm That's look's normal

![image](/assets/img/binary-exploitation/limitless/indexing.png)

Damn something wrong here What is that he takes value and put it in the array with index so wierd

![image](/assets/img/binary-exploitation/limitless/win.png)

There is anther func called win all its function is to read what is inside flag.txt and then print it look's easy  

# Pwn

Let's try to test this program

![image](/assets/img/binary-exploitation/limitless/aaa.png)

Segmentation fault !!! Why !!?  
ops i forgot it's take int value hahaha let's try it again  

![image](/assets/img/binary-exploitation/limitless/normal.png)

That's looks normal right now :D  
Let's get win func addr then try to set it in the first let's see what will happen

![image](/assets/img/binary-exploitation/limitless/winfunc.png)

Awesome let's change it to pass it in the program let's do that with python

![image](/assets/img/binary-exploitation/limitless/p32.png)

Awesome now time to try it

![image](/assets/img/binary-exploitation/limitless/addr.png)

it's doesn't work you know why buecause it's takes int again Hmmmm so what will happen if we change it to Decimal so let's change it with python again easy to do with 0xDiablos :D

![image](/assets/img/binary-exploitation/limitless/hextodecimal.png)

So easy let's try it now but in the first let's create flag.txt

![image](/assets/img/binary-exploitation/limitless/createflag.png)

Done let's try it now

![image](/assets/img/binary-exploitation/limitless/done.png)

Cool we got it Daaaaaaaamn i told you it's will be easy with 0xDiablos  

# Automated Script

I did script to automate this mission and there is comment Per line so amazing [HERE](https://github.com/Mrx-Exploit/CTF-Scripts/tree/master/Binary%20Exploitation/limitless)  
