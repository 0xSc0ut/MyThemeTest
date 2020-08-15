+++
title = "HackTheBox - Curling"
description = ""
type = ["posts","post"]
tags = [
    "HackTheBox",
    "CTF",
    "Bug",
]
date = "2020-07-06"
categories = [
    "HackTheBox",
    "CTF",
]
series = ["HackTheBox"]
[ author ]
  name = "0xSc0ut"
+++

As part of #Hackthebox writeup,  Curling is our next one


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594117588229_Screenshot+2020-07-07+at+3.55.29+PM.png)


Curling is an **Linux** based machine with severity tagged as **Easy.**  This machine needs basic level of enumeration and getting shell requires traditional way of exploitation.

**Tools:**


- PortScanning - Masscan and Nmap
- Directory Search - Gobuster or FFUF
- Vulnerability Scanner - Searchsploit
- Hexdump reverse - xxd

**Initial Recon:**

Once we get IP address lets start scanning ports using masscan and nmap


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118146378_Screenshot+2020-07-07+at+4.05.33+PM.png)


Nothing special, let’s see whats there in port 80


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118256691_Screenshot+2020-07-07+at+4.07.20+PM.png)


Ahaha !! 

Lets try some default passwords, which didn’t work.

Meanwhile lets scan all directories using gobuster.


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118402714_Screenshot+2020-07-07+at+4.09.50+PM.png)


As you can see we found something useful, while accessing **/administrator** we got joomla


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118545716_Screenshot+2020-07-07+at+4.12.14+PM.png)


**Shell:**

In **view page source** of index page gave us a clue


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118701606_Screenshot+2020-07-07+at+4.13.33+PM.png)


Ahaha!! another clue


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594118753157_Screenshot+2020-07-07+at+4.15.34+PM.png)


Lets decode this, While decoding with base64 we got something like **“Curling2018!”**

Lets try to login with **username** “**floris” (which we see in blog post) and password “Curling2018”**


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594119155250_Screenshot+2020-07-07+at+4.22.26+PM.png)


We are in!!

While playing around web application , usually i try to find some file uploads for easy shell exploitation and as i expected there was one


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594119370886_Screenshot+2020-07-07+at+4.26.00+PM.png)


I just modified exploit code in **/error.php** like below


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594119491149_Screenshot+2020-07-07+at+4.27.19+PM.png)


Lets access **10.10.10.150/templates/protostar/error.php**


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594119737613_Screenshot+2020-07-07+at+4.32.06+PM.png)


**Getting user.txt:**

While having a look at **/home/floris** directory we find something like **“password_backup”,** but we cann’t view it so lets move this to our machine using netcat


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594121018395_Screenshot+2020-07-07+at+4.53.13+PM.png)


While reading those contents with **Strings,** we get something like


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594121161923_Screenshot+2020-07-07+at+4.55.03+PM.png)


I try to decode hexdump using xxd


    xxd -r password_backup > pass.txt

**‘File’** gives file type,


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594125780756_Screenshot+2020-07-07+at+6.12.46+PM.png)



    mv pass.txt pass.bz2
    bzip2 -d pass.bz2
    mv pass pass.gz
    gunzip -d pass.gz
    mv pass pass.bz2
    bzip2 -d pass.bz2
    mv pass pass.tar
    tar -xvf pass.tar

After all these steps we got **password.txt** file


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594125955362_Screenshot+2020-07-07+at+6.15.23+PM.png)


I just try to ssh with above password and BOOM!!


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126076864_Screenshot+2020-07-07+at+6.17.47+PM.png)


**Privilege Escalation:**

**Method 1:**

We got something like **/admin-area**, having a deep look gave something like


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126555533_Screenshot+2020-07-07+at+6.25.10+PM.png)


Whereas **report** , default login page code


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126643887_Screenshot+2020-07-07+at+6.27.15+PM.png)


I modified input content with file protocol to read root flag


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126758621_Screenshot+2020-07-07+at+6.28.44+PM.png)


Here you go 


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126785647_Screenshot+2020-07-07+at+6.29.33+PM.png)


**Method 2:**


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594126859166_Screenshot+2020-07-07+at+6.30.45+PM.png)


After long time of surfing i found exploit using snapd


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594127778662_Screenshot+2020-07-07+at+6.46.09+PM.png)


Refer:


https://github.com/initstring/dirty_sock


As i am using second version, transfer **dirty_sockv2.py** into /tmp folder


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594127747763_Screenshot+2020-07-07+at+6.44.06+PM.png)


Successfully exploitation works


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594127855498_Screenshot+2020-07-07+at+6.47.17+PM.png)


just follow the steps,


![](https://paper-attachments.dropbox.com/s_B3FF802273E2D519A69F0406B1A3CAD2CB372A8470F1152B728E4CD69E2CBB34_1594128084791_Screenshot+2020-07-07+at+6.51.03+PM.png)


Thank you, Have a nice day!!

