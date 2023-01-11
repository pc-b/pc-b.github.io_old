---
title: "Crackme 0x02"
date: 2023-01-11T10:57:19-05:00
draft: false
tags: ["binary exploitation","reverse engineering"]
---
I've recently started doing the [Modern Binary Exploitation](https://github.com/RPISEC/MBE/) challenges, and figured i'd do writeups on them to consolidate my learning as I went.

<!--more-->

### Start
Marking crackme0x02 as executable with `chmod +x crackme0x02` and running it provides with with a prompt to enter a password:
![prompt](/img/3/p1.png)

### Ghidra
Opening this up in Ghidra, we can open up the function list on the left menu
![list](/img/3/p3.png)
And we can see that it contains the main function if we scroll down:
![main](/img/3/p4.png)
This program has not been stripped of it's debug symbols, so we can still see function names thankfully. Inside of the main function we have this:
![main](/img/3/p5.png)
Taking a look at our decompiled main, it looks like if the password we enter is equal to `0x52b24` we have found the correct password.
![main](/img/3/p6.png)
 This is a hexadecimal number, so lets convert it to binary and give it a try!
 ![main](/img/3/p7.png)
Great, so this works but how does the assembly code work? I decided to try tracing the assembly code and this is what I came up with:

Below is the original ASM code, then my stack
{{< highlight c>}}
0804842b c7 45 f8        MOV        dword ptr [EBP + local_c],0x5a
                5a 00 00 00
08048432 c7 45 f4        MOV        dword ptr [EBP + local_10],0x1ec
                ec 01 00 00
08048439 8b 55 f4        MOV        EDX,dword ptr [EBP + local_10]
0804843c 8d 45 f8        LEA        EAX=>local_c,[EBP + -0x8]
0804843f 01 10           ADD        dword ptr [EAX]=>local_c,EDX
08048441 8b 45 f8        MOV        EAX=>local_c,dword ptr [EBP + -0x8]
08048444 0f af 45 f8     IMUL       EAX,dword ptr [EBP + local_c]
08048448 89 45 f4        MOV        dword ptr [EBP + local_10],EAX
0804844b 8b 45 fc        MOV        EAX,dword ptr [EBP + local_8]
0804844e 3b 45 f4        CMP        EAX,dword ptr [EBP + local_10]
{{</highlight >}}

{{< highlight c>}}
mov [EBP + local_c], 0x5a
local_c = 0x5a
local_c = 90

mov [EBP + local_10], 0x1ec
local_10 = 0x1ec
local_10 = 492

mov EDX, [EBP + local_10]
EDX = local_10
EDX = 492

lea EAX=>local_c, [EBP + -0x8]
EAX = *local_c
EAX = 90

add [EAX]=>local_c, EDX
local_c = local_c + EDX
local_c = 90 + 492
local_c = 582

mov EAX=>local_c, [EBP + -0x8]
EAX = local_c
EAX = 582

imul EAX, [EBP + local_c]
EAX = EAX * (EBP + local_c)
EAX = 582 * 582

mov dword ptr [EBP + local_10],EAX
local_10 = EAX
local_10 = 338724

mov EAX,dword ptr [EBP + local_8]
EAX = local_8

cmp EAX,dword ptr [EBP + local_10]
EAX == local_10
input == 338724
{{</highlight >}}

### Conclusion
After doing both static analysis with Ghidra and a stack trace of the assembly code, we can see that the password 338724 works, and we have successfully cracked this program!