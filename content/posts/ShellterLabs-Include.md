+++
title = "Shellter Labs - Include"
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

By accessing the URL, it gave the same page which we have seen in "memes" challenge

So i have accessed robots.txt file which listed me

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559049124851_Screen+Shot+2019-05-28+at+11.33.04+AM.png)


When i have accessed /tafacil.txt it showed me 

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559049191134_Screen+Shot+2019-05-28+at+6.42.44+PM.png)


So then further accessing it gave me some clue

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559049256585_Screen+Shot+2019-05-28+at+6.43.39+PM.png)


The learning in previous challenge helped me to perform LFI and retrieve the content of memeviewer.php

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559050489207_Screen+Shot+2019-05-28+at+7.04.27+PM.png)


On seeing this i thought that i have got the flag but on decoding the content it said

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559050544507_Screen+Shot+2019-05-28+at+7.05.31+PM.png)


So i checked for index.php which gave me nothing but the same one, so i traversed back and got some thing, i thought that at least now i got the flag but it showed me

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559050721976_Screen+Shot+2019-05-28+at+7.08.30+PM.png)


After accessing memesbrasileirossaoosmelhores.txt, Finally got the flag

![](https://paper-attachments.dropbox.com/s_895DBAE88D746DC070A81F8B4D6B21D94B95EB77CC5BF216734618A9A8F20DD3_1559050784723_Screen+Shot+2019-05-28+at+7.09.12+PM.png)
