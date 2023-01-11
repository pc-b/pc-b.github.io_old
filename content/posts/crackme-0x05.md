---
title: "Crackme 0x05"
date: 2023-01-11T11:38:41-05:00
draft: false
tags: ["binary exploitation","reverse engineering"]
---
crackme0x05, this one is very similar to crackme0x04, so this should be a quick one

<!--more-->

### Ghidra
![image](/img/5/p1.png)
Our main function, which is identical to crackme0x05. Lets take a look at check.
![image](/img/5/p2.png)
Ok, this looks very similar as well, but this time we call a function called parell, if the sum of our input is 0x10 (16)
![image](/img/5/p3.png)
We can see that it is doing an and between local_8, our input and 1. This & represents an and gate, which would return 1 if all the bits of our input are the same as the bits of 1 So what does this mean? Let's take a look at this

{{< highlight text>}}
for example lets try 97 as our input:

num | bits
-----
97: | 1100001
 1: | 0000001
an and gate goes through all the bits and returns 1 if 1 exists in both 97 and 1.
at the weight of 1, both 97 and 1 have a 1, so the output of the and gate is 1.
so this password does NOT work, because to work, the bitwise and of our 
input and 1 has to be equal to 0.

a working example would be this:

num | bits
-----
88: | 1011000
 1: | 0000001

when we and 88 & 1, we get 0 as there are not 1's in any positions in both 88 or 1. 
This password also sums to 16, so lets try it out
{{</highlight >}}
![image](/img/5/p4.png)
**Nice! Works great.**