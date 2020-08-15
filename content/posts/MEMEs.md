+++
title = "Shellter Labs - MEMEs"
description = ""
type = ["posts","post"]
tags = [
    "ShellterLabs",
    "CTF",
    "Bug",
]
date = "2019-05-28"
categories = [
    "ShellterLabs",
    "CTF",
]
series = ["ShellterLabs"]
[ author ]
  name = "0xSc0ut"
+++

As a initial recon i just tried /robots.txt, which pop me with below

![Always check the content of robots.txt file](https://paper-attachments.dropbox.com/s_FAD8684AC53D8DDD2B0F1F311D2C6EF4EBC3EACAE13F27CEDFD2C70F1F62F2F0_1559023395997_Screen+Shot+2019-05-28+at+11.33.04+AM.png)


When i try to view the content of /memeviewer, it listed me a file directory

![File Directory](https://paper-attachments.dropbox.com/s_FAD8684AC53D8DDD2B0F1F311D2C6EF4EBC3EACAE13F27CEDFD2C70F1F62F2F0_1559023480816_Screen+Shot+2019-05-28+at+11.34.18+AM.png)


It is obvious that i found out the flag which is available in flag.txt

![flag.txt](https://paper-attachments.dropbox.com/s_FAD8684AC53D8DDD2B0F1F311D2C6EF4EBC3EACAE13F27CEDFD2C70F1F62F2F0_1559023543233_Screen+Shot+2019-05-28+at+11.35.06+AM.png)


**TakeAways:**

Always have a look at config files or files that are accessible, it may contain some sensitive informations.
