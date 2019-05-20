# REST API
API research and learning by @Anakein
-------
# Sites vulnerabilities and search vulns:

A collaborative list of great resources about RESTful API architecture, development, test, and performance. Feel free to contribute to this on-going list. [Awesome REST](https://github.com/marmelab/awesome-rest#public-rest-apis-to-use-in-tests)
 

Some of our tools can be accessed programatically using this API. The tools can be started, stopped and queried for output in a machine-friendly format (JSON). The following tools have support for API: Web Server Scan, Find Subdomains, Find Virtual Hosts, Network Scan OpenVAS, URL Fuzzer, SQLi Scan, XSS Scan, WordPress Scan, Drupal Scan. 
[API REFERENCE](https://pentest-tools.com/api_reference)


With its increasing popularity, REST APIs pose major security challenges. 
[rest-api-security-vulnerabilities](https://dzone.com/articles/rest-api-security-vulnerabilities)

Blog post on the topic.

http://www.sempf.net/post/Cracking-and-Fixing-REST-APIs

Tools wise, SmartBear, the company that makes SoapUI, now has a security tool.

https://smartbear.com/product/ready-api/secure/overview/

Of course, Burp and ZAP work fine too, but the SmartBear tool has API specific stuff.

-------
# LAB
`bWAPP`, or a buggy web application, is a free and open source deliberately insecure web application.
It helps security enthusiasts, developers and students to discover and to prevent web vulnerabilities.
bWAPP prepares one to conduct successful penetration testing and ethical hacking projects.

What makes bWAPP so unique? Well, it has over 100 web vulnerabilities!
It covers all major known web bugs, including all risks from the OWASP Top 10 project.
[Download bWAPP](https://sourceforge.net/projects/bwapp/files/bWAPP/)

## Install for Windows
How to install bWAPP in Windows?
[install_for_windows](https://www.youtube.com/watch?reload=9&v=F3QcgmCuEC0)

## Install for Kali
How To Install bWAPP- Kali Linux?
[install_for_kali_linux](https://www.crackitdown.com/2018/05/how-to-install-bwapp-kali-linux.html)

bee:bug

-------
# REST API, pentester's perspective
REFERENCE: https://www.youtube.com/watch?v=zW8QF3x3oSU

## REST API 101

REST - representational state transfer

Data usually is sent as JSON

HTTP methods have a meaning (usually):

```
     GET - List(collection), retrieve data (element)    
     PUT - replace (all data is changed)  
     PACTH update    
     POST - create (new element)   
     DELETE
```

## REST API Pentest

```
Get endpoints
Get docs
Get keys/credentials
Get sample calls !!
```

## REST API Bug bounty

```
Sometimes no known endpoints
Sometimes no docs
Sometimes no keys/credentials
sometimes no sample calls !!
```
    
## Findings endpoints

```
/
/api/
/v1/
/v1.0/
/v1.1/
/api/v1/
/api/v2
```
```
Example:

https://yoursite.io</api/v1/>
Content-Type: text/plain
Content-Lenght: 45
Content:

<Community Platform API v1>
```

```
Example 2:

https://yoursite.com</api/v1/>
Content-Type: application/json
Content-Lenght: 169
Content:
{"meta":
    {"message":
         "This is a list of all endpoints avaible at this version",
    "endpoints": [
         "http://anothersite.com/<api/v1/health_check>/*
     ]}
"data": null}

```
```
/ping
/health
/status
...
Dictionaries for <directories> and <filenames> will help
```
A wordlist of API names used for fuzzing web application APIs. https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/api

```
Example 3: ping

https://yoursite.com</api/v1/ping>
Content-Type: application/json;charset-utf-8
Content-Lenght: 26
Content:

{"response":"<pong>","et":0}
```
```
Example 4: /health

https://yoursite.com</api/v1/health>
Content:

{"status":"good"}

----------------------------
https://yoursite/api/<health>
Content:
{"ok":false,"error":"unknown_method","req_method":"<health>"}
```

OSI Model 7 layer
![OSI7](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)
