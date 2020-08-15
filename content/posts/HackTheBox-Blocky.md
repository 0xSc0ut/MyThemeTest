+++
title = "HackTheBox - Blocky"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-06-30"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++

Here we go another box !!


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593612113100_Screenshot+2020-07-01+at+7.31.41+PM.png)


Blocky is Linux based machine which is tagged with “Easy” severity.  A good environment to play and learn for OSCP related stuffs.

It was a quite easy machine with proper practice on an average it may take around 1hr (In my perspective as i m a beginner) to solve.

**Tools:**


- PortScan - Masscan and Nmap
- Directory Search - Gobuster or FUFF
- Decompiler - JD-GUI

**Steps to Reproduce:**

**Initial Recon:**

As always my initial recon starts with scanning ports and all other version details.


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593612644378_Screenshot+2020-07-01+at+7.40.24+PM.png)


 
On Accessing 80 port we get something like this


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593612725063_Screenshot+2020-07-01+at+7.41.41+PM.png)


Without any delay i started searching directories


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593612756509_Screenshot+2020-07-01+at+7.41.31+PM.png)


Woww!!, We got some beautiful results like phpmyadmin, wordpress and plugins.

**Getting Into Server:** 

Lets try one by one, starting with phpmyadmin


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613221838_Screenshot+2020-07-01+at+7.49.52+PM.png)


I tried default password which resulted in failure, so lets hold this for a while and lets check other like wordpress and plugins


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613321430_Screenshot+2020-07-01+at+7.51.52+PM.png)


Default wordpress site where default credentials didn’t work


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613368044_Screenshot+2020-07-01+at+7.52.10+PM.png)


Ahh!! appears to be something interesting

I downloaded it and Open both jars in [JD-GUI](http://java-decompiler.github.io/)

I found some credentials over there


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613470257_Screenshot+2020-07-01+at+7.54.12+PM.png)


Nice..!!!

Just tried those credentials with phpmyadmin and wordpress, we were into phpmyadmin


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613666696_Screenshot+2020-07-01+at+7.57.01+PM.png)


So usual way of getting reverse shell using phpmyadmin query didn’t work [(refer here)](https://blog.netspi.com/linux-hacking-case-studies-part-3-phpmyadmin/) as “--secure-file-priv “ which don’t allow uploading file content.


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593613868461_Screenshot+2020-07-01+at+8.00.51+PM.png)


OK another Failure !!

Lets try to ssh with those credentials,


- Username:root and Password: 8YsqfCTnvxAUeduz…… -  Not working
- Username:notch and Password: 8YsqfCTnvxAUed…….. - working


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593614068293_Screenshot+2020-07-01+at+7.20.57+PM.png)


**Privilege Escalation:**

As soon as entering into server my first commands would be uname -a and sudo -l


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593614239930_Screenshot+2020-07-01+at+8.07.01+PM.png)


As you can see user notch have ALL permissions

Soooooo….!!


![](https://paper-attachments.dropbox.com/s_24ADEB8E7B2D849B4D545FE6D77C81784FE75ECF0DC1B318B780A096ABC1947A_1593614332497_Screenshot+2020-07-01+at+8.08.05+PM.png)


Thanks for reading so far, Have a nice day !!
