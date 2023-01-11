---
title: "Crackme 0x04"
date: 2023-01-11T11:38:38-05:00
draft: false
tags: ["binary exploitation","reverse engineering"]
---
With these crackmes, I think that i'll try to get through them as fast as possible, so starting right off I opened this in ghidra.

<!--more-->

### Ghidra
![image](/img/4/p1.png)
Looks like our main function calls another function called check with the input we gave it, so lets double click on check to jump to that function.
![image](/img/4/p2.png)
We can see a whole bunch of code, with one return statement and one break. The return statement only will happen if our string's length is 0. However, the break executes when the variable `local_c == 0xf` which is equivalent to 15 binary. Just looking over the code, this looks like we are getting the sum of all our input and checking if it is equal to 15. We get the value of the first character every iteration of the loop `local_11 = param_1[local_10];`. local_10 seems to be our loop counter, and local_c our sum. So let's try inputting some numbers that sum to 15.
![image](/img/4/p3.png)
Great, works as we'd expect. However, it seems to only care if the sum is 15 and does not care about what's after the sum is 15. Let's try inputting some funny stuff after to see if it cares.
![image](/img/4/p4.png)
Nice, it does not care at all! That one was not too bad!