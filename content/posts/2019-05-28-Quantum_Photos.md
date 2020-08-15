---
layout: default
title:  "Shellter Labs - Quantum Photos"
date:   2019-05-28
categories: main
---

# Shellter Labs - Quantum Photos

By just seeing the url i thought that it would be an path traversal attack

![](https://paper-attachments.dropbox.com/s_C015D4DB976B99B96D3E063620ED6D69A30CDDB4E3F00CD6222B4A5F90684419_1559037502580_Screen+Shot+2019-05-28+at+3.27.42+PM.png)


So i try to find etc/passwd file, before that i have to verify that is there is a possibility of path traversal.

![](https://paper-attachments.dropbox.com/s_C015D4DB976B99B96D3E063620ED6D69A30CDDB4E3F00CD6222B4A5F90684419_1559037589896_Screen+Shot+2019-05-28+at+3.29.39+PM.png)


There we go path traversal is possible

So i tried for etc/passwd by ‘../../../../etc/passwd’ and i got it

But the challenge is to find the flag

After some internet surfing found a new thing called https://php.net/manual/pt_BR/wrappers.php.php, These are used to access the internal files

[http://lab.shellterlabs.com:32841/index.php?page=php://filter/convert.base64-encode/resource=index.php](http://lab.shellterlabs.com:32841/index.php?page=php://filter/convert.base64-encode/resource=index.php)

This returned us the "index.php" content in base64 format


![](https://paper-attachments.dropbox.com/s_C015D4DB976B99B96D3E063620ED6D69A30CDDB4E3F00CD6222B4A5F90684419_1559039478086_Screen+Shot+2019-05-28+at+3.52.04+PM.png)


Thats it, what we need to do is decode the content

![](https://paper-attachments.dropbox.com/s_C015D4DB976B99B96D3E063620ED6D69A30CDDB4E3F00CD6222B4A5F90684419_1559039683621_Screen+Shot+2019-05-28+at+4.04.23+PM.png)


**References:**

[https://www.idontplaydarts.com/2011/02/using-php-filter-for-local-file-inclusion/](https://www.idontplaydarts.com/2011/02/using-php-filter-for-local-file-inclusion/)

[https://github.com/qazbnm456/awesome-security-trivia/blob/master/Tricky-ways-to-exploit-PHP-Local-File-Inclusion.md](https://github.com/qazbnm456/awesome-security-trivia/blob/master/Tricky-ways-to-exploit-PHP-Local-File-Inclusion.md)


<div id="hyvor-talk-view"></div>
<script type="text/javascript">
    var HYVOR_TALK_WEBSITE = 961; // DO NOT CHANGE THIS
    var HYVOR_TALK_CONFIG = {
        url: '{{ page.url | absolute_url }}',
        id: '{{page.id}}'
    };
</script>
<script async type="text/javascript" src="//talk.hyvor.com/web-api/embed"></script>
