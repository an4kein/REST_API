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

----------------------------
https://yoursite.com/health
Content:

{"status":"UP","status":{"status":"UP","Tn7 admin":"UP","ims":"UP"}}

```

## OSI Model 7 layers

![OSI7](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)

## Spring Boot Actuator

Spring Boot makes is easy to create stand-alone, production-grade Spring based Application that you can "just run". We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most spring Boot applications need very little spring configuration. 

Spring Boot Actuator is a sub-project of Spring Boot. It adds several production grade services to your application with little effort on your part. In this guide, you’ll build an application and then see how to add these services.
https://spring.io/guides/gs/actuator-service/

### Features

```
Create stand-alone Spring application
Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
Provide opinionated `started` POMs to simplify your Maven configuration
Automatically configure Spring whenever possible
Provide production-ready features such as metrics health checks and externalized configuration
```
### Interesting endpoints:
```
/actuator
/health
/trace
/logfile
/metrics
/heapdump (Spring MVC)
   - Example:
   Tested domain: yousite.com
   
   /heapdump           :200 , size: 11707186 [11707177]
   Content-Dosposition: attachment:
   filename="<heapdump2017-05-00-47-live1546516215946541654.hprof.gz>"
```
## Finding docs:

```
/api-docs
/application.wadl  -> sucessor of WSDL web service
/doc
/docs
/swagger-ui.html
/swagger.json
```

## Finding Sample Calls
```
Still no docs?
Error messages to the rescue!

GET /api/public/files/109/test.pdf
[...]

HTTP/1.1 404 NOT FOUND
[.....]

{ "message":
"The resquested URL was not found on the server.
If you entered the URL manually please check your spelling and try again.
You have requested this URI
[/api/public/files/109/test.pdf]
but did you mean

/api/public/files/<intifile_id>/<string:filename> or
/api/public/countries/<string(length=2):code> or
/apt/admin/files/types?" )
```

```
GET /REST/v1/auth HTTP/1.1
[...]

HTTP/1.1 405 Method Not Allowed
Allow:  DELETE, OPTIONS, POST
[...]
```

```
POST /REST/v1/auth HTTP/1.1
Content-Type: application/json
[...]
{}

HTTP/1.1 400 Bad Request
[...]

Could not found a <'username'> parameter
```

```
POST /REST/v1/auth HTTP/1.1
[...]
<{ 'username' : 'test' }>

HTTP/1.1 400 Bad Request
[...]

Could not found a <'password'> parameter
```

```
POST /REST/v1/auth HTTP/1.1
[...]
{ <'username' : 'test', 'password' : 'test'> }

HTTP/1.1 401 Unauthorized
[...]

Invalid credentials.
```

### Brute force parameter names!
https://github.com/maK-/parameth

### Analyse JS code
https://github.com/zseano/JS-Scan

### Dissect mobile app 
Apk-Scan for Android apps hard-coded URL's
https://blog.nviso.be/2013/03/29/apkscan-beta-released/

Secure, Unified, Powerful and Extensible Rust Android Analyzer https://superanalyzer.rocks/
https://github.com/SUPERAndroidAnalyzer/super

Reverse Engineering APIs from Android Apps — Part 1
https://medium.com/@thomas_shone/reverse-engineering-apis-from-android-apps-part-1-ea3d07b2a6c

## Finding Keys

```
Check mobile application
  - Google Play Crawler > https://github.com/nviennot/playdrone

Check GitHub (truffleHog to the rescue)
  - Scan public repos of a company
  - Scan public repos of a company devs
  
#1 Jolokia > https://github.com/rhuss/jolokia
Example: https://example.com/jolokia/write/Tomcat:port=19880,type=Connector/xpoweredBy/true
X-Powered-By:Servlet/3.1 JSP/2.3(Apache Tomcat/8.0.20 Java/Oracle Corporation/1.8.0_60-b27)

#2 REST API wrongly placed
  - A form
  - Putting ID and solving CAPTCHA
  - sECURED (no way to brute force ID) 
  - A mobile app with the same feature
  - No CAPTCHA
  - No rate limiting
  - Brute force & profit report to client !
```

Summary
 - Find endpoints
 - Find docs
 - Find sample calls
 - Find keys
 - Fuzz

--------

# API Security Testing For Hackers

## Overview of API Attack Vectors

### Common API Security Issues

API Bugs are a common source of news we hear about security breaches

Security issues and vulnerabilities in APIs typically fall into a few distinct categories.

- [Access Controls](#access-controls)
    - [Authorization](#authorization)
    - [Authentication](#authentication)
- [Rate Limiting](#rate-limiting)
- [Input Validation](#input-validation)
- [Restricting HTTP Methods]
- [3rd Party API abuse]
- [Other application logic errrors]

## Access Controls

### Authorization

Involves proving that you are who you say you are.

### Authentication

Involves granting access to resources based on your indentify.

Access control schemes tend to follow a pattern.

- Client makes a request to something that requires authentication
- Server process auth request, checks for things like
    - If an account/session exists
    - If the requested resource in within access scope of the client
    - If any previous cookies or tokens were used in the request
    - If the way the route to the resource was valid
    - If any othe conditions the server expects have been met.
- If successful, server returns a token, session id, or other identifier to mark the session.
- Further authenticated requests will follow  a similiar pattern throughout the session.

### Access Control Bugs

- Common ways of testing for access control bugs include:
    - Enumerating potentially restricted endpoints
    - Modifying session tokens
    - Reusing older session tokens
    - Attempt to bypass restrictions on access with IDOR
    - Modifying the resquest with additional parameters like "&admin=True" or others
    - Modifying referrer headers that the application may expect
    
### Access Coontrol Bug - Panera Bread

Exposed API endpoint allowed for complete access to client information

Sequetial numbering of client records allowed for easy enumeration of the entire user database.

Resulted in tens of millions of client records being leaked

![panerabread_leaked](https://raw.githubusercontent.com/e-anakein/REST_API/master/images/panera-2.png)

## Input Validation

Input validation is a very importante attack vector to consider when hunting for bugs.

It's  crucial to remember that input is *anything* that the server takes in, from the user, 3dr party apps, and other internal mechaninms.

### Common places to test in an API
    - within the request header
    - Parameters within the URL
    - Parameters within the request
    - File uploads (PUT/DELETE requests)
    - Different request methods

## Input validation Bugs

Depending on the architeture of the application, specific parts of a request may be processed in ways that are unsafe.

These include:

- Improver parameterization of requests within application logic
- Lack of input sanitization/escaping unsafe characters
- Improver handling of parameters
- Insufficient controls for data types passed (File Upload Bugg, Unicode bugs)

## Input validation Bug - German eID System

Requests to the German eID System required signing of requests to be valid.

The application did not account for the same parameter occurring multiple times.

This allowed for an attacker to get their initial request signed and processed, while the application parsed the last parameter in the request. This means that they would be able to change their identify entirely within the system.  https://www.bleepingcomputer.com/news/security/german-eid-authentication-flaw-lets-you-change-identity/   -  https://www.youtube.com/watch?v=kaATyYmpiIE

## Input Validation - Fuzzing

```
Things you can fuzz for:

Remote Code Execution > RCE
Cross Site Scripting > XSS
Local/Remote File Inclusion > LFI/RFI
SQL/NoSQL Injection
Request Splitting
Deserialization
XXE and other templated languages fun
Encoding errors with Junk characters, Control charecter, Emoji, etc
File upload vulnerabilities
SSRF

Tools you can use:
Burp 
Enumeration Tools (Gobuster, dirb, Dirsearch, etc)
Custom Scripts

PROTIP: Speed up your fuzzing by making HEAD requests to API endpoints

```
## Rate Limiting

APIs are built  for applications that typically expect a high load and large amount of arbitrary requests. This aspect can be abused to very quickly enumerate the endpoint in ways that are unintentional.

Common ways to test Rate Limiting

Make requests in varying states of authentication.

- as an unauthenticated user
- as an authenticated user
- as a developer
- as a bot
- with a deactivated account
- with bogus credentials

An API with improperly implemented rate limiting can be used to make an abnormal amount of requests to enumerate the application and potentially cause other issues.

Source: https://www.youtube.com/watch?v=ijalD2NkRFg&feature=youtu.be
