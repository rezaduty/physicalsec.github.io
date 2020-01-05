---
date: 2019-08-15 23:48:05
layout: post
title: HackTheBox-lacasadepapel
subtitle: "Created by: Mrx-Exploit"
description: >-
  Created by: Mrx-Exploit
image: >-
  /assets/img/hackthebox/Lacasadepapel/banner.png
optimized_image: >-
  /assets/img/hackthebox/Lacasadepapel/banner.png
category: blog
tags:
  - hackthebox
  - ftp
  - web
  - ssl_Cert
  - privilege_escalation
author: Hazem Hesham
paginate: true
---

# Enumeration

> Nmap Scan

First thing, We will start with nmap scan command `nmap -sV 10.10.10.131` Basic command

![image](/assets/img/hackthebox/Lacasadepapel/nmap.png)

Here we got `21,22,80` I think that's enough we don't need to do full scan until now

> VSFTP enumeration

I think the version is so old, So I will search if there any exploit for it or not
Yep i got exploit in metasploit `use unix/ftp/vsftpd_234_backdoor`
So let's give it try :D 

![image](/assets/img/hackthebox/Lacasadepapel/msf.png)

Hmmm, here i tryied to do `nc 10.10.10.131 6200`

> Psy shell

When i tried `nc 10.10.10.131 6200`
I got psy shell, So i should to get repo talking about psy command `https://psysh.org/`

![image](/assets/img/hackthebox/Lacasadepapel/pspy.png)

I tried show to get what's inside Tokyo 

![image](/assets/img/hackthebox/Lacasadepapel/tokyo.png)

Hmmm, There is CA file in naibrobi USER to get it copy and paste line number 4 easy

![image](/assets/img/hackthebox/Lacasadepapel/priv.png)

> Web enumeration

So here we have http and https are open.  
  
Here i tried gobuster but i didn't got something, So let's see https.  
  
I should to create pk12 file to see https contant, So let's see How can i create it
  
I got nice repo for that `https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs`  
  
To shorten the time to create pk12 you need to do these command  
`openssl req -new -x509 -key private.key -out publickey.cer -days 365`  
`openssl pkcs12 -export -out final_certificate.pfx -inkey private.key -in publickey.cer`  
  
when i create the pk12 file i put it in my firefox browser and finaly you can see https contant  

![image](/assets/img/hackthebox/Lacasadepapel/priv area.png)

So here i have two private area let's see season-1 area
I got avi file damn, Hmm
Then i got something in the source code what's it? that's avi file encoded with base64 

![image](/assets/img/hackthebox/Lacasadepapel/Base64.png)

So what happen if i change this to `Base64(../../../../../etc/passwd)` Becaues i think it's will be LFI there
It's working wow i got passwd file, So now i have list of users let's try to get user.txt from berlin `Base64(../../../../home/berlin/user.txt)`  
> NOTE: I love berlin ❤️ 

![image](/assets/img/hackthebox/Lacasadepapel/lfiuser.png)

![image](/assets/img/hackthebox/Lacasadepapel/userFlag.png)

So we got user.txt  
After a lot of time searching how i get user shell i got id_rsa from `Base64(../../../../home/berlin/.ssh/id_rsa)`  
ssh time `ssh -i id_rsa professor@10.10.10.131`  
Why professor because it's not working with my love berlin 😂   

# Privilege Escalation

Now i have user shell 

![image](/assets/img/hackthebox/Lacasadepapel/professor.png)

Basic thing `ls -la` . 

![image](/assets/img/hackthebox/Lacasadepapel/ls.png)

So What is that let's `cat memcached.ini`

![image](/assets/img/hackthebox/Lacasadepapel/nobody.png)

Wow i think if i change this file and change memcached.js i will get shell.
So let's change these files. 

![image](/assets/img/hackthebox/Lacasadepapel/nobodychanged.png)

![image](/assets/img/hackthebox/Lacasadepapel/beforeRoot.png)

So i think now time to listen 

![image](/assets/img/hackthebox/Lacasadepapel/rootShell.png)

Is that the end? Damn it's so easy 😂 let's try to cat root.txt 

![image](/assets/img/hackthebox/Lacasadepapel/root.png)

Damn is so easy, wow 
