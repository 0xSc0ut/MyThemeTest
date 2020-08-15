---
layout: default
title:  "HackTheBox - Mirai"
date:   2020-07-02
categories: HackTheBox
---

# HackTheBox - Mirai

Welcomes you with another box, ‘Mirai’


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593697198313_Screenshot+2020-07-02+at+7.09.48+PM.png)


Its a Linux based machine with Easy severity. Getting initial shell was really easy and escalating to root won’t take long but getting root flag needs some tricks.

**Tools:**


- PortScan - Masscan and Nmap
- Diskspace Utility - [df](https://www.tecmint.com/how-to-check-disk-space-in-linux/)
- Vulnerability Scanner -Searchsploit
- Directory Search - [FUFF](https://github.com/ffuf/ffuf)

**Initial Recon:**

As always start with scanning all ports with masscan or nmap usually i use both to faster results


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593697550461_Screenshot+2020-07-02+at+7.15.42+PM.png)


Based on results, search for vulnerabilities using searchsploit


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593697636029_Screenshot+2020-07-02+at+7.16.10+PM.png)


 
By accessing port 80, empty page gets loaded. So our next step would be scanning directories. 

Here i have used FUFF, you can use any tools like gobuster,dirb,etc.


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593697728925_Screenshot+2020-07-02+at+4.00.29+PM.png)


As you can see there is directory named /admin

**Shell:**

We didn’t get anything from swfobject.js except a prase


    var x = "Pi-hole: A black hole for Internet advertisements."

But /admin didn’t disappoint us 


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593697910760_Screenshot+2020-07-02+at+7.21.31+PM.png)


A quick google search gave me default credentials like [refer](https://blog.cryptoaustralia.org.au/instructions-for-setting-up-pi-hole/)


    Username: pi
    Password: rasberry

But it doesn’t work ..!!

I started brute forcing passwords again rockyou.txt 


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698171512_Screenshot+2020-07-02+at+7.25.50+PM.png)


Meanwhile lets see if there is some other way to achieve shell, so i try to ssh using credentials which we get 

BOOM!!


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698254325_Screenshot+2020-07-02+at+4.05.57+PM.png)


Usually flag will be in /home/username/user.txt but it was not there,so i used Find command to find user.txt


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698466359_Screenshot+2020-07-02+at+7.30.56+PM.png)


**Privilege Escalation:**

If you have read my previous post i would have mentioned that  my first testcase for privilege escalation is uname -a and sudo -l 


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698616790_Screenshot+2020-07-02+at+7.33.24+PM.png)


As you see escalating won’t be a big deal


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698660014_Screenshot+2020-07-02+at+7.34.07+PM.png)


As i said earlier getting into root is easy but retrieving flag is what need some effort


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698381663_Screenshot+2020-07-02+at+5.14.44+PM.png)



As you can see USB stick files are deleted, lets check disk utility through which we can get some intel


    df -h

-h means human readable format


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593698902533_Screenshot+2020-07-02+at+7.38.14+PM.png)


As you can see /media/usbstick is available in filesystem /dev/sdb, lets check sdb


    cat /dev/sdb


![](https://paper-attachments.dropbox.com/s_09823AB2D868F212637B54E71520EDBB2DD0EC7D446C0E27B27DD067C3AB5B01_1593699074849_Screenshot+2020-07-02+at+7.40.59+PM.png)


Thank you for reading , Have a nice day

<div id="hyvor-talk-view"></div>
<script type="text/javascript">
    var HYVOR_TALK_WEBSITE = 961; // DO NOT CHANGE THIS
    var HYVOR_TALK_CONFIG = {
        url: '{{ page.url | absolute_url }}',
        id: '{{page.id}}'
    };
</script>
<script async type="text/javascript" src="//talk.hyvor.com/web-api/embed"></script>
