+++
title = "HackTheBox - Beep"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-06-23"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++

![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592925476840_Screenshot+2020-06-23+at+8.47.09+PM.png)


**Tools:**


- Portscan - Nmap and Masscan
- Vulnerability Scan - Searchsploit and ExploitDB

As intial recon i just started with masscan by scanning all tcp and udp ports which resulted me with 

![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592925776043_Screenshot+2020-06-23+at+8.48.46+PM.png)


As you can see above image there are lots of open ports, so i just took those stuffs and scanned for versions using nmap


![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592925891681_Screenshot+2020-06-23+at+8.54.38+PM.png)


By seeing above result i just know that there are multipe ways to exploit this box.

As there are lots of port i decided to go one by one , While accessing 80 port browser throws SSL error which means we have use https protocol


![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592926071422_Screenshot+2020-06-23+at+8.57.47+PM.png)


As usual i just try to find exploit for elastix using searchsploit


![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592926395798_Screenshot+2020-06-23+at+9.02.10+PM.png)


So the challenging part is i didn’t find any version of elastix, so only way it to attempt all possible exploits,

Lets ignore XSS vulnerabilities as we have to gain access to machine i started with LFI. 

[ExploitDB](https://www.exploit-db.com/exploits/37637) resulted me with exploit code, while analysing the code i found a intresting part of code

    #LFI Exploit: /vtigercrm/graph.php?current_language=../../../../../../../..//etc/amportal.conf%00&module=Accounts&action

As you can see the exploit can be used to read amportal.conf file from server,


![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592926737571_Screenshot+2020-06-23+at+9.08.53+PM.png)


From above response i thought that version should be 2.2.0, so without any delay i started checking the result.

There are lots of stuff available, using “view page source” will give a better view

Things which we care about is 

![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592997268987_Screenshot+2020-06-24+at+4.44.24+PM.png)


 As you can see above file there are some passwords
 
I also referred etc/passwd file, which says there is a user called “fanis” 

![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592926777134_Screenshot+2020-06-23+at+9.09.34+PM.png)


So i try to ssh fanis with the passwords which we got previously but no luck, out of no where i just thought why not “root”


![](https://paper-attachments.dropbox.com/s_66C140379216529AC483768061914AA3843255DE739BD21AB922AB7413A0C96E_1592997754032_Screenshot+2020-06-24+at+4.52.19+PM.png)


**Takeaway:**

Sometimes foolish guess works

