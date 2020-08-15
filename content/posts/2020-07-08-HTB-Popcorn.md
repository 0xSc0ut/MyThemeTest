---
layout: default
title:  "HackTheBox - Popcorn"
date:   2020-07-08
categories: HackTheBox
---


# HackTheBox Popcorn

Welcome back to another new post on [#Hackthebox](https://www.hackthebox.eu/),


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594206912446_Screenshot+2020-07-08+at+4.42.19+PM.png)


**Popcorn** is an **Linux** based based machine with **Medium** Severity.Though this machine is tagged as Medium every exploits where straight forward, proper google search will lead you to right path.

**Tools:**


- PortScan - Masscan and Nmap
- Directory Search - FFUF
- Vulnerability Scanner - Searchsploit / ExploitDB
- Proxy - Burp Suite

**Initial Recon:**

As always my initial recon starts with scanning all port using masscan and extracting relevant informations for open ports using nmap.


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213339406_Screenshot+2020-07-08+at+6.32.07+PM.png)


Usual stuff nothing interesting, but lets go for port 80


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213399774_Screenshot+2020-07-08+at+6.33.10+PM.png)


So we left with scanning directories, i used FFUF you can use any similar tools.


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213495899_Screenshot+2020-07-08+at+6.34.45+PM.png)


**Initial Shell as www-data :**

From those results **torrent** dragged my attention first so lets start with that,


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213746368_Screenshot+2020-07-08+at+6.38.57+PM.png)


Ahaha !! a web application - **Torrent Hoster**

Tried some default password which didn’t work, so i just created a account using **Register** option.

![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213846832_Screenshot+2020-07-08+at+12.45.27+PM.png)



![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213908313_Screenshot+2020-07-08+at+12.46.01+PM.png)


With above credentials just logged in and we are in to home page,

After some googling i found a very useful video [Link](https://www.youtube.com/watch?v=7r-gf_LoTuQ), So torrent hoster has a file upload action using which we can get initial shell.

Since this application just accepts torrent file, 


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594214305062_Screenshot+2020-07-08+at+12.59.00+PM.png)


I just uploaded some random file which i downloaded from google


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594214560863_Screenshot+2020-07-08+at+6.52.22+PM.png)


Our major target is uploading php content to Torrent **screenshot** option,


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594214714302_Screenshot+2020-07-08+at+6.54.26+PM.png)


Capture the request using **Burp Suite** and tamper it according to your wish,

![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594213818101_Screenshot+2020-07-08+at+3.38.22+PM.png)


Lets listen using netcat in our machine,


    nc -lvp 1234

After listening click on **Image not Found** button,


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594214936294_Screenshot+2020-07-08+at+6.58.19+PM.png)


BOOM !! we are in


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594214974830_Screenshot+2020-07-08+at+3.40.42+PM.png)


**User Flag:**

Since **www-data** had permission for reading **user.txt ,** lets just read it 


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594215046416_Screenshot+2020-07-08+at+3.44.29+PM.png)


**Privilege Escalation:**

By searching exploit for kernel version , i found one in exploitDB - [Dirty Cow Attack](https://www.exploit-db.com/exploits/40839)


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594215187938_Screenshot+2020-07-08+at+7.01.50+PM.png)


So i just downloaded the file in my local machine and transferred it to **/tmp** folder, Compile the file which we have downloaded using **gcc compiler** and run the exploit.

In simple this exploit overrides /etc/passwd with root account.


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594215459978_Screenshot+2020-07-08+at+7.03.44+PM.png)

****
Exploit may take some time to work so wait!!

Since exploit was successful lets just ssh to the username and password which we override.


![](https://paper-attachments.dropbox.com/s_6D367084CA0DE606197231FA4F4A8853B31AE2560D5BE253189668880D00B3A8_1594215620182_Screenshot+2020-07-08+at+7.10.10+PM.png)


Thanks for reading, Have a nice day !!

<div id="hyvor-talk-view"></div>
<script type="text/javascript">
    var HYVOR_TALK_WEBSITE = 961; // DO NOT CHANGE THIS
    var HYVOR_TALK_CONFIG = {
        url: '{{ page.url | absolute_url }}',
        id: '{{page.id}}'
    };
</script>
<script async type="text/javascript" src="//talk.hyvor.com/web-api/embed"></script>
