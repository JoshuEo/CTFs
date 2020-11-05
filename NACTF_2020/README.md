# Network Academy CTF 2020
Total Points: 2251
  Placing:    232/968

- [General Skills](#general)
  - [Grep 0](#grep0)
  - [Grep 1](#grep1)
  - [Arithmetic](#ari)
  - [Zip Madness](#zip)
- [Web](#web)
  - [Missing Image](#image)
- [Reverse Engineering](#rev)
  - [Generic Flag Checker 1](#gen1)
  - [Generic Flag Checker 2](#gen2)



# <a name="general"></a> General Skills
## <a name="grep0"></a> Grep 0
![date](https://img.shields.io/badge/date-11.01.2020-brightgreen.svg) ![General category](https://img.shields.io/badge/Category-General-lightgrey.svg) ![score](https://img.shields.io/badge/score-50-blue.svg)

### Description
```
Sophia created this large, mysterious file. She might have said something about grap.. grapes? Find her flag!
```

### Attached files
- flag.zip

### Solution
This zip file contains a looooot of text! (81.7 MB to be exact!) It just said:
```
Maybe Here???
Nop
Definitely not here
Look somewhere else
```
Repetetively and we know that there is a flag in this gigantic text file. How the title says it all, but they want us to use "grep". Grep is a powerful tool used to find patterns within text and display them for you. These patterns my utilize regular expressions, wildcards, etc to meet your needs (for more information, look at their man page! ![Man Page](https://man7.org/linux/man-pages/man1/grep.1.html))
Now that we understand what grep can do, let's put that to practice. So the description gave us a little hint that our flag contained a string related to grapes (But they really mean grep). 
After some testing around I noticed that "gra", "gre", "grep", "grape", and many other combinations weren't working so I decided to truncate the pattern into "gr" which worked!
We can use this simple command that will search for "gr" in the text file:
```
grep "gr" flag.txt
```

### Flag
```
nactf{gr3p_1s_r3ally_c00l_54a65e7}
```


## <a name="grep1"></a> Grep 1
![date](https://img.shields.io/badge/date-10.30.2020-brightgreen.svg) ![General category](https://img.shields.io/badge/Category-General-lightgrey.svg) ![score](https://img.shields.io/badge/score-200-blue.svg) 

### Description
```
Elaine hid a REGULAR flag among more than 1,000,000 fake ones! The flag was an EXPRESSION of her love for nactf, so the first 10 characters after "nactf{" only have the characters 'n', 'a', 'c', and the last 14 characters only have the characters 'c', 't' and 'f'. There are 52 characters in total, including nactf{}.
```

### Attached files
- flag.zip

### Solution
Ok this challenge has a similar topic to Grep 0 except that this one will be a bit more complex. They gave us a 72.3 MB file that contains many counterfiet flags with the format "nactf{....}". Similar to grep 0, we are looking for a specific pattern to find the actual flag. They've given a hint about regular expressions when they ALL CAPS the "regular" and "expression" in the description. In this challenge, we have to formulate a regular expression to find patterns according to the description of the real flag.
- First 10 characters after nactf{ only have the characters 'n', 'a', 'c'
- Last 14 characters only have the characters 'c', 'f', and 'f'
- There are 52 characters in total including nactf{}

So this is what I did at first:
```
grep "nactf{[nac][nac][nac][nac][nac][nac][nac][nac][nac][nac]?????????????????????[ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf][ctf]}" flag.txt
```
After doing some research, I found this cheat sheet: [Link](https://www.rexegg.com/regex-quickstart.html) that helped me solve this problem (and make it look less messy)

- []: One of the characters in the brackets should match
- {}: How many times it should be iterated
- .: Literally any character except a line break

```
grep "nactf{[nac]{10}.{21}[ctf]{14}}" flag.txt
```
This was the correct pattern but I couldn't get it to work for some reason, so I decided to use sublime's regular expression search on the text file. After some debugging and playing around, I found the flag!

This took some time for me but it was worth it!

### Flag
```
nactf{caancanccnxfynhtjlgllctekilyagxctftcffcfcctft}
```

## <a name="ari"></a> Arithmetic
![date](https://img.shields.io/badge/date-11.02.2020-brightgreen.svg) ![general category](https://img.shields.io/badge/Category-General-lightgrey.svg) ![score](https://img.shields.io/badge/score-150-blue.svg) 

```
### Description
Ian is exceptionally bad at arthimetic and needs some help on a problem: x + 2718281828 = 42. This should be simple... right?
```
```
HINT: What does uint32_t mean?
```

### Solution
I was really stuck on this problem mainly because the math didn't add up (get it?!) They're asking us to input a number that stands true to the comparison. But what threw me off was that you CAN'T input negative numbers. A normal person would think its:

x = -2718281786

So I was stuck. Until I looked at the hint. "What does uint32_t mean?".

uint_32 is the same as an unsigned integer. Ok so that explains why we couldn't use negatives. But when I searched up "uint_32 vulnerability" I found a CVE talking something called "integer overflow". What's that you ask?

Integer overflow occurs when an arithmetic operation (+/-/*//) attempts to create a numeric value that is outside of the range that can be represented with a given number of digits.

Since we know that uint_32 is an integer, we know that its maximum value is 2^32-1 which is 4294967295.
> Tip: There is also uint_16 (short) and uint_64 (long) which have different max values as well!

So an integer overflow occurs when you go over the limit of its expected value so the value turns into 0.

After this aha moment, I did the maths:

4294967296 - 2718281828 + 42 = 1576685510

> The reason why I added one to uint_32's maximum value (4294967295) is because we also need to account for when the value turns to 0

So once I entered the number, I received the flag!


```
> nc challenges.ctfd.io 30165
Enter your number:
1576685510
flag: nactf{0verfl0w_1s_c00l_6e3bk1t5}
```

### Flag
```
nactf{0verfl0w_1s_c00l_6e3bk1t5}
```


## <a name="zip"></a> Zip Madness
![date](https://img.shields.io/badge/date-11.03.2020-brightgreen.svg) ![General category](https://img.shields.io/badge/Category-General-lightgrey.svg) ![score](https://img.shields.io/badge/score-175-blue.svg)

### Description
```
Evan is playing Among Us and just saw an imposter vent in front of him! Help him get to the emergency button by following the directions at each level.
```

### Attached files
- flag.zip

### Solution
The whole gist of this challenge is:
1. Unzip file
2. Read direction.txt (Either says left or right)
3. Unzip the corresponding side (There are 2 files <number>left.zip or <number>right.zip)
  a. The number starts from 1000 down to 1 which is where the flag is
4. Repeat the process over and over again until you reach 1

This requires some scripting knowledge which I previously didn't have. I chose to learn a little bit of bash scripting so I formulated this script:
```
#!/bin/bash

unzip flag.zip
for ((direct=1000; direct>=1; direct--))
do
        read -r side < direction.txt
        unzip -o "$direct$side.zip"
done
```
So basically, what this script does is:
1. Initially unzip the flag.zip file
2. Loop from 1000 to 1 decrementing
3. While going down the numbers, read the direction.txt file
  a. Assign it a value "side"
4. unzip the zipped file based on its "side" and "direct"
  a. The "-o" is to overwrite the direction.txt file so I won't have to type "y" all the time to overwrite it
5. Repeat

After running that command, it unzipped EVERYTHING all the way until the flag was found! (flag.txt)
This was my first time trying bash scripting so it took a lot of debugging and googling to solve this problem.

### Flag
```
nactf{1_h0pe_y0u_d1dnt_d0_th4t_by_h4nd_87ce45b0}
```



# <a name="web"></a> Web

## <a name="image"></a> Missing Image
![date](https://img.shields.io/badge/date-11.01.2020-brightgreen.svg) ![web category](https://img.shields.io/badge/category-Web-lightgrey.svg) ![score](https://img.shields.io/badge/score-75-blue.svg) 

### Description
```
Max has been trying to add a picture to his first website. He uploaded the image to the server, but unfortunately, the image doesn't seem to be loading. I think he might be looking in the wrong subdomain...
```

### Link (Probably doesn't work anymore)
- https://hidden.challenges.nactf.com/

### Solution
When visiting the link, we are greeted with this generic site:
![image-20201101175554898](C:\Users\joshu\AppData\Roaming\Typora\typora-user-images\image-20201101175554898.png)
As referenced in the title and the description of the challenge, we know that we are looking for some sort of image. After inspecting the page and going to the Network tab + reloading the page to see what type of traffic is going in. I see a 404 in the flag.png file:
![image-20201101180841364](C:\Users\joshu\AppData\Roaming\Typora\typora-user-images\image-20201101180841364.png)
In the description, we know that the image is being requested in the wrong subdomain. We see that it requests at http://challenges.nactf.com/flag.png.
But wait, the site we are on has a hidden subdomain. So once I added the flag.png and the subdomain in my search header, I found the flag.
http://hidden.challenges.nactf.com/flag.png

### Flag
```
nactf{h1dd3n_1mag3s}
```


# <a name="rev"></a> Reverse Engineering

## <a name="gen1"></a> Generic Flag Checker 1
![date](https://img.shields.io/badge/date-11.01.2020-brightgreen.svg) ![Reverse Engineering category](https://img.shields.io/badge/Category-Reverse%20Engineering-lightgrey.svg) ![score](https://img.shields.io/badge/score-75-blue.svg) 

### Description
```
Flag Checker Industries™ has released their new product, the Generic Flag Checker®! Aimed at being small, this hand-assembled executable checks your flag in only 8.5kB! Grab yours today!
```

### Files
- gfc1 (ELF File)

### Solution
I'm pretty new to reverse engineering so I won't be have the intelligence of explaining what is going on. But this challenge was not difficult to solve. When running the "file" command, we knew that it was an elf executable.
```
> file gfc1
gfc1: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, BuildID[sha1]=7d4bbad2b6eeb736abec4fd52079781dcc333781, stripped
```
**ELF (Executable Linkable Format)**: Similar to binary files but with additional information such as possible debug info, symbols, distinguishing code from data within the binary.

All I had to do to find this flag was cat the file
OR display strings using the string command
```
cat gfc1
OR
strings gfc1
```
This is the operation is called static analysis.
**Static Analysis**: A method of computer program debugging that is done by examining the code without executing the program.
We didn't execute this program, we only displayed the contents of them.
> Tip: This is the single way to analyze executable files. If you were to analyze larger and more complex files, you would need a tool for static analysis. I recommend [Ghildra](https://ghidra-sre.org/) *You can find the flag using this tool as well!

> Ghildra is a "software reverse engineering suite of tools developed by NSA's Research Directorate in support of the Cybersecurity mission"

### Flag
```
nactf{un10ck_th3_s3cr3t5_w1th1n_cJfnX3Ly4DxoWd5g}
```

## <a name="gen2"></a> Generic Flag Checker 2
![date](https://img.shields.io/badge/date-11.01.2020-brightgreen.svg) ![web category](https://img.shields.io/badge/Category-Reverse%20Engineering-lightgrey.svg) ![score](https://img.shields.io/badge/score-150-blue.svg)

### Description
```
Flag Checker Industries™ is back with another new product, the Generic Flag Checker® Version 2℠! This time, the flag has got some foolproof math magic preventing pesky players from getting the flag so easily.
```
```
HINT: When static analysis fails, maybe you should turn to something else...
```

### Files
- gfc2 (ELF File)

### Solution
Alright now this challenge needs a different approach as to the preceding challenge. When we try catting the file, we just see a lot of data, but no flag. There is no use scouring through the data because you won't find the flag there. If you see the hint, they are asking us to take a different approach of reverse engineering. So if static analysis (analysis without running the executable) doesn't work, then the opposite alternative would be dynamic analysis (analysis while running the executable). There are 2 command line tools called "ltrace" and "strace" that you should know.

**ltrace vs strace**

strace is a *system call and signal tracer*. It is primarily used to trace system calls (function calls made from programs to the kernel)
ltrace is a *libary call tracer* and it is primarily used to trace calls made by programs to library functions. It can also trace system calls and signals, like strace.
For more info: https://blog.packagecloud.io/eng/2016/03/14/how-does-ltrace-work/#:~:text=strace%20is%20a%20system%20call%20and%20signal%20tracer.&text=As%20described%20in%20our%20previous,calls%20and%20signals%2C%20like%20strace%20.

In this challenge, we will use "ltrace" but it's good to understand both (there is also another tool called ptrace!).

> How to install ltrace: sudo apt-get install ltrace

Here are the results after running and tracing the executable
```
> ltrace ./gfc2
puts("what's the flag?"what's the flag?
)                                                                        = 17
fgets(flag
"flag\n", 64, 0x7fad1600c980)                                                             = 0x7ffc03648310
fmemopen(0, 256, 0x555fb9e4c015, 59)                                                            = 0x555fbaf83c00
fprintf(0x555fbaf83c00, "%0*o24\n%n", 28, 026602427217, 0x7ffc036481f8)                         = 31
fseek(0x555fbaf83c00, 0, 0, 0)                                                                  = 0
__isoc99_fscanf(0x555fbaf83c00, 0x555fb9e4c022, 0x7ffc036481fc, 0)                              = 1
fclose(0x555fbaf83c00)                                                                          = 0
strncmp("flag", "nactf{s0m3t1m3s_dyn4m1c_4n4lys1s"..., 56)                                      = -8
puts("nope, not this time!"nope, not this time!
)                                                                    = 21
+++ exited (status 1) +++
```
We found the flag! But part of it is still obscured. So after looking through the man page of ltrace, I used the "-s" parameter to specify the maximum string size to print.

```
> ltrace -s 123 ./gfc2
puts("what's the flag?"what's the flag?
)                                                                        = 17
fgets(flag?
"flag?\n", 64, 0x7f6ff99d9980)                                                            = 0x7ffd2392a490
fmemopen(0, 256, 0x55f000a7b015, 58)                                                            = 0x55f001be8c00
fprintf(0x55f001be8c00, "%0*o24\n%n", 28, 026602427217, 0x7ffd2392a378)                         = 31
fseek(0x55f001be8c00, 0, 0, 0)                                                                  = 0
__isoc99_fscanf(0x55f001be8c00, 0x55f000a7b022, 0x7ffd2392a37c, 0)                              = 1
fclose(0x55f001be8c00)                                                                          = 0
strncmp("flag?", "nactf{s0m3t1m3s_dyn4m1c_4n4lys1s_w1n5_gKSz3g6RiFGkskXx}", 56)                 = -8
puts("nope, not this time!"nope, not this time!
)                                                                    = 21
+++ exited (status 1) +++
```
Nice!

### Flag
```
nactf{s0m3t1m3s_dyn4m1c_4n4lys1s_w1n5_gKSz3g6RiFGkskXx}
```
