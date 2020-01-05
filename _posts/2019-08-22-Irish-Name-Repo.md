---
date: 2019-8-22 23:48:05
layout: post
title:  "Irish-Name-Repo 1*3 - PicoCTF 2019"
subtitle: "Created by: X-Billy"
description: >- 
  Created by: X-Billy
image: >- 
  /assets/img/WebCTF/Irish-Name-Repo/banner.png
optimized_image: >- 
  /assets/img/WebCTF/Irish-Name-Repo/banner.png
tags: 
 - picoCTF 
 - websec 
 - sqli 
category: blog
author: Abdullah Hassan
paginate: true
---

# Irish-Name-Repo 1

> Points: 300  
> Difficulty: Medium

Let's see what will happen with the first one  
There are Admin login, So what!!  
For sure we will try SQli  
Let's try Basic payload `admin'or'1'='1 --`  

![image](/assets/img/WebCTF/Irish-Name-Repo/1/1.png)

Flag is picoCTF{s0m3_SQL_93e76603} 
Easy to hack :D  
Let's see what's next  

---

# Irish-Name-Repo 2

> Points: 350  
> Difficulty: Medium

It's look like the first one, So let's go to the Admin Login Then let's try our Basic Payload `admin'or'1'='1 --`

![image](/assets/img/WebCTF/Irish-Name-Repo/2/1.png)

So let's see our source code probably find something important  
We got `debug mode 0`  
So what will happen if we change it from 0 to 1 let's see what will happen  

![image](/assets/img/WebCTF/Irish-Name-Repo/2/info.png)

Now we can see the SQL query  
Aha, But it's looks normal but I think there is Filtration in the or  
So What will happen if we did that `admin'--`  
  
Flag is `picoCTF{m0R3_SQL_plz_83dad972}`  
Easy to hack again :P  
I hope to find something hard in the next one  

---
# Irish-Name-Repo 3

> Points: 400  
> Difficulty: Medium

It's look like the first one, So let's go to the Admin Login, Hmm there is something different in this one it's takes password only  
So let's see what will happen if we did debug 1 again

![image](/assets/img/WebCTF/Irish-Name-Repo/3/2.png)

Hmmm, I think it's takes our input then do Some Rot13  
then let encode our payload from admin'or'1'='1 to nqzva'be'1'='1  
  

flag is `picoCTF{3v3n_m0r3_SQL_ef7eac2f}`
Its work now but it's looks easy again :D
