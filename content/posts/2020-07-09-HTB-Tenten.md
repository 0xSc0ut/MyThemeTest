---
layout: default
title:  "HackTheBox - Tenten"
date:   2020-07-09
categories: HackTheBox
---

# HackTheBox - Tenten

Welcome back !!


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594362275790_Screenshot+2020-07-10+at+11.46.01+AM.png)


Tenten is **Linux** based **Medium** severity machine, Though it is tagged as medium, with proper google search you can get into shell and escalating to root is not a big deal.

**Tools:**


- PortScan - Masscan and Nmap
- Directory Search - FFUF
- Password Cracker - JohnTheRipper
- Cryptographic - Steghide
- Wordpress Scanner - Wpscan

**Initial Recon:**

As always start with port scanning. Here i have used Masscan and Nmap, you can use any other tools out there,


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594364756654_Screenshot+2020-07-10+at+12.34.33+PM.png)


From above results we came to know there is a **wordpress 4.7.3** application running on port 80. While accessing port 80, a blog post appeared


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594364908695_Screenshot+2020-07-10+at+12.38.13+PM.png)


Lets scan directories available in **10.10.10.10.**
For this i have used FFUF as it is faster and provides better functionalities, its not mandatory to use FFUF anyother tools like gobuster, dirb ,etc will also works.


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594365135926_Screenshot+2020-07-10+at+12.42.06+PM.png)


There are some stuffs which can help us, I have searched every possible things but didn’t find anythin useful.

So!!! our next steps is to use **wpscan**, I have gone through one post which you can use it as reference [link](https://blog.sucuri.net/2015/12/using-wpscan-finding-wordpress-vulnerabilities.html)


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594375505866_Screenshot+2020-07-10+at+3.34.42+PM.png)


There is a job-manager with version **0.7.25**

Lets enumerate users using wpscan


    wpscan —url http://10.10.10.10 —enumerate u


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594375622529_Screenshot+2020-07-10+at+3.36.50+PM.png)


So there is a wordpress user called **takis**

I try to login to wordpress account but it was a fail

**Shell:**

Based on information that i have got , I started surfing through internet about those stuffs and find a good read content which helped me a lot.

[**[CVE-2015-6668] CV filename disclosure on Job-Manager WP plugin**](https://vagmour.eu/cve-2015-6668-cv-filename-disclosure-on-job-manager-wordpress-plugin/)

I took sometime for me to understand the vulnerability, but i just started applying those stuff into this machine

When you click **Hello World** from the home page, you can see a folder structure like this


    http://10.10.10.10/index.php/2017/04/12/hello-world/

So i started enumerating /index.php folder


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594376302842_Screenshot+2020-07-10+at+3.47.34+PM.png)


Lets try to access every folder that are listed, among those /jobs gave something like this


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594376374641_Screenshot+2020-07-10+at+3.48.48+PM.png)


By clicking Apply Now i got a URL structure which i read from the post


    http://10.10.10.10/index.php/jobs/apply/8/

I started changing value from ’8’ in URL, and at ‘13’ i found a link like HackerAccessGranted


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594377282187_Screenshot+2020-07-10+at+4.04.32+PM.png)


From above reference post i downloaded the python code from github [link](https://gist.github.com/DoMINAToR98/4ed677db5832e4b4db41c9fa48e7bdef)


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594377603660_Screenshot+2020-07-10+at+4.09.51+PM.png)


By running the exploit code with above information gave us a URL which has a .jpg file.


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594377727272_Screenshot+2020-07-10+at+4.11.54+PM.png)


Lets just download it using wget


    wget http://10.10.10.10/wp-content/uploads/2017/04/HackerAccessGranted.jpg

I have used **steghide** tool to decode image content


    steghide --extract -sf HackerAccessGranted.jpg 

**No password** is required to decode the content, We got a **id_rsa** file.

In order to ssh we need password phrase, which we can get it using **ssh2john** python code.


    python ssh2john.py /path/to/id_rsa > crack.txt


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594378202874_Screenshot+2020-07-10+at+4.19.53+PM.png)


So the password is **superpassword,**

Now lets try to ssh , i tried to ssh root but it didn’t work. But no worries we **takis**


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594378354609_Screenshot+2020-07-10+at+4.22.24+PM.png)


**Privilege Escalation:**

For privilege escalation i always begin with checking kernal version which is,


    Linux tenten 4.4.0-62-generic #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

For this version i found a privilege escalation exploit, [link](https://www.exploit-db.com/exploits/41458) i don’t know why it didn’t work.

My next step is to verify sudo level permission 


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594378553956_Screenshot+2020-07-10+at+4.25.43+PM.png)


So takis has permission to execute /bin/fuckin with root privilege,


![](https://paper-attachments.dropbox.com/s_C70574DEBC90EC96EB0688717D1E9D856AA78F8ADBBEB44F6D9BAD2E994A4926_1594378641100_Screenshot+2020-07-10+at+4.27.11+PM.png)


Thank you !! Have a nice day !

<div id="hyvor-talk-view"></div>
<script type="text/javascript">
    var HYVOR_TALK_WEBSITE = 961; // DO NOT CHANGE THIS
    var HYVOR_TALK_CONFIG = {
        url: '{{ page.url | absolute_url }}',
        id: '{{page.id}}'
    };
</script>
<script async type="text/javascript" src="//talk.hyvor.com/web-api/embed"></script>
