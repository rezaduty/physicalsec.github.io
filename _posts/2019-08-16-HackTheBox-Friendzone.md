---
date: 2019-08-16 23:48:05
layout: post
title: HackTheBox-Friendzone
subtitle: "Created by: Mrx-Exploit"
description: >-
  Created by: Mrx-Exploit
image: >-
  /assets/img/hackthebox/Friendzone/banner.png
optimized_image: >-
  /assets/img/hackthebox/Friendzone/banner.png
category: blog
tags:
  - hackthebox
  - ftp
  - web
  - smb
  - dns
  - privilege_escalation
author: Hazem Hesham
paginate: true
---

# Enumeration 

> Nmap Scan

First thing, We will start with nmap scan command `nmap -sV 10.10.10.123` Basic command

![image](/assets/img/hackthebox/Friendzone/NmapScan.png)

Here we got `80,21,53,445,443` I think that's enough we don't need to do full scan

> FTP enumeration

Here i will try to login with anonymous command `ncftp 10.10.10.123` But if you don't have ncftp you can get it using this command `sudo apt-get install ncftp`

![image](/assets/img/hackthebox/Friendzone/ncftp.png)

Login failed so let's see web interface.

> Web enumeration

So here we have http and https are open.
Here i tried gobuster but i didn't got something, So let's see SMB.

![image](/assets/img/hackthebox/Friendzone/webpage.png)

> SMB enumeration

Here we have port 445 so let's use smbclient to know if there any files we had access on it or not
Command: `smbclient -L \10.10.10.123`

![image](/assets/img/hackthebox/Friendzone/smbclient.png)

Aha we have general and Development oki Let's see what's will happen if we mount general
Command: `mount -t cifs //10.10.10.123/general /mnt/general` Note: Here i created general folder before.

![image](/assets/img/hackthebox/Friendzone/mount.png)

Yes, I got Creds.txt .

![image](/assets/img/hackthebox/Friendzone/Credits.png)

> Rabbit Hole

Here i got a lot of rabbit holes haha.

[1] I tried these Credits to take ssh.
[2] I tried to find login page on web interface.
[3] I tried these Credits again in ftp maybe works.

Here i got the thinking to use https but again it's not working but i got the right way name of box FriendZone i think there is any Zone Transfer Oki let's go out of rabbit hole.

> DNS Zone Transfer

After many attempts i got it.
Command: `dig axfr @10.10.10.123 friendzone.red` 

![image](/assets/img/hackthebox/Friendzone/zonez.png)

Let's put all of these in /etc/hosts file

> Web enumeration again

Let's see what inside administrator1.friendzone.red.
Yes, We got login page it's try our Credits

![image](/assets/img/hackthebox/Friendzone/login.png)

It's works, and now we can see dashboard.

![image](/assets/img/hackthebox/Friendzone/dashboard.png)

I got this word let's try it in our url. 

> Rabbit Hole again

Here i tried a lot of think again Like.

[1] I can upload something in uploads.friendzone.red. and i can see from dashboard !!
[2] I can bypass upload file !!

And finaly i remembered i have write permissions on Development so let's mount it and upload our shell there So let's go out of rabbit hole.

![image](/assets/img/hackthebox/Friendzone/dev.png)

Upload Reverse Shell

When i mount Development Folder i put it there my php Reverse Shell.
I went back in the dashboard and i tried to find my reverse shell, So i will try to visit `/etc/Development/shell.php`.
But again it's not working but i got the idea let's clear the extention from the file and boom it's working and now we have www-data shell. 

![image](/assets/img/hackthebox/Friendzone/rev-shell.png)

![image](/assets/img/hackthebox/Friendzone/www-data.png)

So let's see if we can see user.txt or not, Yep we Can. 

![image](/assets/img/hackthebox/Friendzone/user.png)

# Privilege Escalation

Here i got two ways to get root.txt
Let's see what's first one.
Iam uploaded pspy in target box then i use it
So i got python script called: reporter.py 

![image](/assets/img/hackthebox/Friendzone/reporter.png)

 Then i found something there is cronjob is running but i don't have write permissions in this script, But i have in os lib and it's use os lib so if i add anything in os will be run when reporter.py run
So let's add this command 

![image](/assets/img/hackthebox/Friendzone/root.png)

I created pspy file and i made it hidden just for camouflage it.
Then when reporter.py run u will get the root.txt in pspy file.

> Second way

There is another Credits in the `/var/www/` there is file name mysql_data.conf cat the file.  

![image](/assets/img/hackthebox/Friendzone/ssh.png)

Then use this Credits to login with ssh.  

![image](/assets/img/hackthebox/Friendzone/sshDone.png)

And try again to add our last command then Boom root.txt :D  

I hope u enjoyed with this writeup. You can follow me on the twitter @Mrx_Exploit  

Thanks for reading.  