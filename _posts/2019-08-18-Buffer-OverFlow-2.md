---
date: 2019-08-18 23:48:05
layout: post
title: Buffer OverFlow-Stack2
subtitle: "Created by: Mrx-Exploit"
description: >-
  Created by: Mrx-Exploit
image: >-
  /assets/img/BoF/stack2/stack2.jpg
optimized_image: >-
  /assets/img/BoF/stack2/stack2.jpg
category: blog
tags:
  - pwn
  - buffer-overFlow
author: Hazem Hesham
paginate: true
---


# Code Review
I will not review all the code because there is one thing only new of last post, So i think you should to go back and the see it :D  
So let's see What's the new part  

![image](/assets/img/BoF/stack2/1.png)

What that mean idk ? - So let's run the program

![image](/assets/img/BoF/stack2/2.png)

I got it he need need me to setup the "GREENIE", So let's setup it

![image](/assets/img/BoF/stack2/3.png)

Nothing happened !! So let's resetup it with big number

![image](/assets/img/BoF/stack2/4.png)

He printed `43` that's mean he taked the `C`
so What will happen if we gived him the address let's see what will happen

![image](/assets/img/BoF/stack2/5.png)

Damn thats so easy  
And we did it  

