---
date: 2019-08-19 23:48:05
layout: post
title: Buffer OverFlow-Stack3
subtitle: "Created by: Mrx-Exploit"
description: >-
  Created by: Mrx-Exploit
image: >-
  /assets/img/BoF/stack3/bof.jpg
optimized_image: >-
  /assets/img/BoF/stack3/bof.jpg
category: blog
tags:
  - pwn
  - buffer-overFlow
author: Hazem Hesham
paginate: true
---


# Code Review

I will not review all the code because there is one thing only new of last post, So i think you should to go back and the see it :D 

![image](/assets/img/BoF/stack3/sourceCode.png)

But the new thing here there is new function called 'win' you should to call it to print the "code flow successfully changed\n"  
What ever let's run it and see what will happen with us. 

![image](/assets/img/BoF/stack3/1.png)

Nothing happened because our input in his buffer so let's give him `'A'*100`

![image](/assets/img/BoF/stack3/3.png)

Now i should to create pattern, So my best way to create it is `gdb-peda`

![image](/assets/img/BoF/stack3/4.png)

After that run the program and put this pattern as input  
Now debug time  
`gdb-peda$ x/xg $rsp`  
`gdb-peda$ pattern offset 0x63413163` Now we know the offset `64`, So know we need to know the addr of win function let's go back and use r2 or `objdump`or `gdb`  
what ever i will use `gdb` idk why but the important thing is the address  
So to get the functions address in the `gdb` you just need to type `info functions` and boom we got it `0x08048424` So now attack time  
Let's do our payload with `python python -c 'print "A" * 64 + "\x24\x84\x04\x08"'`  

![image](/assets/img/BoF/stack3/done.png)

Damn thats so easy  
And we did it  