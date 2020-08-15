+++
title = "Shellter Labs - Omni Corp"
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

![](https://paper-attachments.dropbox.com/s_5BFC514CDB77BAE41D266A3929E09BF4A037693D98B0F9DBA01B00747655E671_1559026954596_Screen+Shot+2019-05-28+at+12.32.20+PM.png)


As the comment says i have downloaded the attachment..!

Which contained index.php, After having a quick look on the code i just noticed this interesting part of code
 

![](https://paper-attachments.dropbox.com/s_5BFC514CDB77BAE41D266A3929E09BF4A037693D98B0F9DBA01B00747655E671_1559027103837_Screen+Shot+2019-05-28+at+12.34.20+PM.png)


So by just seeing the code, I thought that in order to reach the flag i have to 

1.Set username with non empty string 
2.ID must be ’0’ to pass the if check

I thought it is simple, but when i accessed the url this is what i got

![](https://paper-attachments.dropbox.com/s_5BFC514CDB77BAE41D266A3929E09BF4A037693D98B0F9DBA01B00747655E671_1559027325300_Screen+Shot+2019-05-28+at+12.38.24+PM.png)


What??????

So Its a simple thing in php ‘==’ compares the type of the variable rather than its value, whereas ‘===’ compares the actual value in a variable.
So in our case i have used corresponding data type which gave me the flag

![](https://paper-attachments.dropbox.com/s_5BFC514CDB77BAE41D266A3929E09BF4A037693D98B0F9DBA01B00747655E671_1559027782649_Screen+Shot+2019-05-28+at+12.44.03+PM.png)


**Takeaway:**

Learnt about [php type juggling](https://www.php.net/manual/en/language.types.type-juggling.php)

