# Syskron Security CTF 2020
Total Points: 400 
  Placing:   458th


#### Table of Contents
- [Forensics](#forensics)
	- [Redacted News](#re)
- [Packet-Analysis](#packet)
	- [DoS Attack](#dos)
- [Web](#web)
	- [Security Headers](#head)

# Forensics
## <a name="re"></a> Redacted News
![date](https://img.shields.io/badge/date-10.21.2020-brightgreen.svg)
![forensics category](https://img.shields.io/badge/category-Forensics-lightgrey.svg)
![score](https://img.shields.io/badge/score-100-blue.svg)

### Description
```
Oh, this is a news report on our Secure Line project. But someone removed a part of the story?!
```

### Attached files
- 2020-10-1-secureline.png

### Solution
I found a tool that would find the solution for us in an instant called fotoforensics (http://fotoforensics.com)

After I uploaded the image, I used the hidden pixels tool to find the cached information

### Alternative Solution
Convert the png file into a jpg
```
convert 2020-10-1-secureline.png 2020-10-1-secureline.jpg
```

### Flag
```
syskronCTF{d0-Y0u-UNdEr5TaND-C2eCh?}
```


# <a name="packet"></a> Packet Analysis

## <a name="dos"></a> DoS attack
![date](https://img.shields.io/badge/date-10.21.2020-brightgreen.svg)
![packet-analysis category](https://img.shields.io/badge/Category-Packet%20Analysis-lightgrey)
![score](https://img.shields.io/badge/score-100-blue.svg)

### Description
```
One customer of Senork Vertriebs GmbH reports that some older Siemens devices repeatedly crash. We looked into it and it seems that there is some malicious network traffic that triggers a DoS condition. Can you please identify the malware used in the DoS attack? We attached the relevant network traffic.

Flag format: syskronCTF{name-of-the-malware}
```

### Attached files
- dos-attack.pcap

### Solution
I feel like you don't need to analyze the pcap file if the name and the description exposes the answer but let's analyze the pcap file.
![image-20201024191946907](C:\Users\joshu\AppData\Roaming\Typora\typora-user-images\image-20201024191946907.png)
This is a representation of a single machine DoS'ing another machine. In this case 172.16.31.155. If we follow the UDP stream:
![image-20201024192257977](C:\Users\joshu\AppData\Roaming\Typora\typora-user-images\image-20201024192257977.png)
It is just layers upon layers of messages flooding UDP traffic to the target. Now that we know that this is a DoS attack, we need to know what the name of this attack is. The description provides information about a company named Siemens. So I searched up "Siemen DoS vulnerability" which brought a lot of articles about Siemens recent vulnerability from a DoS attack.
**Article:** https://securitybrief.com.au/story/claroty-reveals-dos-vulnerability-in-siemens-protocol
In the reading, you'll see the name "Industroyer" popping out in the article, so when I tried that, it worked!


### Flag
```
syskronCTF{Industroyer}
```

# <a name="web"></a> Web

## <a name="head"></a> Security Headers
![date](https://img.shields.io/badge/date-10.21.2020-brightgreen.svg)
![web category](https://img.shields.io/badge/category-web-lightgrey.svg)
![score](https://img.shields.io/badge/score-100-blue.svg)

### Description
```
Can you please check the security-relevant HTTP response headers on www.senork.de. Do they reflect current best practices?
```

### Solution
When they asked for HTTP headers I should've instantly gone to burpsuite to see how I can tinker with the headers. But I instantly went to using curl which wasn't any help; it only gave me the http source code.
```
> curl -I https://senork.de
HTTP/2 301
server: nginx
date: Sat, 24 Oct 2020 23:07:17 GMT
content-type: text/html
content-length: 178
location: https://www.senork.de/
strict-transport-security: max-age=31536000; includeSubdomains; preload
```
But after aimlessly using curl, I finally decided to use burpsuite and see what'll happen
After intercepting the website, I placed it into the repeater and sent it again. Which lead to the flag being exposed. Playing back the request to the server did something that lead to it responding with the flag.

### Flag
```
SyskronCTF{y0u-f0und-a-header-flag}
```
