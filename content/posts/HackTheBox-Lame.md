+++
title = "HackTheBox - Lame"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-06-26"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++

Lame is one of the oldest machines, which is tagged as easy, usually this machine is preferred by one who is intrested in OSCP.


![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593436220827_Screenshot+2020-06-29+at+6.40.09+PM.png)


**Tools:**

* Portscan - Nmap and masscan
* Vulnerability Scan - Searchsploit
* Exploit Tool - Metasploit

**Steps to Reproduce:**

As initial recon i have started scanning all ports available using masscan

![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593434806476_Screenshot+2020-06-25+at+5.57.03+PM.png)


After scanning open ports use nmap to scan version and service details available in those ports


![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593434857988_Screenshot+2020-06-25+at+5.59.26+PM.png)


As you can see vsftpd is vulnerable version, i know this because i have came across the same version in someother box.

Instead of going to that method i thought of trying some other method, so while searching i found distccd is vulnerable to [command execution](https://www.exploit-db.com/exploits/9915) from exploitDB and [rapid7](https://www.rapid7.com/db/modules/exploit/unix/misc/distcc_exec)

So without any waiting i opened metasploit and try to exploit 


![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593435226296_Screenshot+2020-06-25+at+6.08.57+PM.png)


BOOM!!

This whole process took me around 10-15 mins

For better interaction use TTY shell using 


    python -c 'import pty; pty.spawn("/bin/sh")'

Without any delay i searched for user.txt


![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593435347633_Screenshot+2020-06-25+at+6.11.00+PM.png)


**Privilege Escalation:**

Since this is old machine there are lots of way to escalate to root

This box is using vulnerable version of linux kernal, by searching with the version there are mutliple exploits available


    uname -a 

But I am not going to use that method

While searching through box i found nmap was available in the machine

![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593435666618_Screenshot+2020-06-29+at+6.12.20+PM.png)


 
I know this version is vulnerable to privilege escalation as i have experienced this in some other [vulnhub](http://vulnhub.com) box


    nmap —interactive
    
![](https://paper-attachments.dropbox.com/s_D09D767BCE9F7AE704629770039DBB101E55727722CF66B59D8FCFFAED088617_1593435801297_Screenshot+2020-06-29+at+6.33.16+PM.png)


Thats it for now, this box can also be exploited through vsfpd which i will share in my upcoming post.

Thank you, Have a nice day
