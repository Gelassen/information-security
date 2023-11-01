# information-security
This repository has been created by demand to get practical skills in penetration testing by request from a potential client to establish information security for the government organisation (<a href="https://gelassen.github.io/blog/2023/10/21/case-study-information-security-of-government-organisation.html">https://gelassen.github.io/blog/2023/10/21/case-study-information-security-of-government-organisation.html</a>). 

# Attacks

## Obtain private address of the organisation to further spoof it
1. Exchange server has to expose its internal IP on each email
2. Misconfigureed DNS
3. Memory leaks
4. Cracking WiFi
5. Plug-in into a webinar or conference room to sniff traffic
6. Phishing emails with special script to gather informaition from PC and network

<a href="https://security.stackexchange.com/questions/111308/how-to-get-an-internal-network-address-range-during-a-footprinting-stage">https://security.stackexchange.com/questions/111308/how-to-get-an-internal-network-address-range-during-a-footprinting-stage</a>

Mitigation steps:

1. Careful audit for DNS configuration 
2. Search for memory leaks of corporate software for exposed sensitive information
3. Investigate further

## Exploit User-Agent to run executable code on the backend

## Analyse web resource for leaked credentials and other valuable information 
1. Check source code of the page
2. Check backend potenial paths for possibles hidden directories:
  - chekc sitemap and /robots.txt for a clue
  - write a tool or use an existing one to brute force common names for a site
https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information

Mitigation steps:

https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information

## By pass Basic Auth
Basic Auth might be by passed due misconfugiration on the server side. It is done by using not intended Http request (e.g. PUT insead of POST) or totaly artificial Http Request: 
```
    Authentication must be implemented with a <Limit VERB VERB VERB> directive. As this only enforces authentication for the listed verbs. If the restrictions only cover GET and POST for example you can bypass this with ver juggling (use PUT instead of POST)

    If you cannot juggle a verb you may be able to juggle anyway by using a mangled verb such as GETS instead of GET. However this is only possible if the application handler allows unknown verbs or does a bad job validating known verbs (such as the php script handler).
```
Source https://security.stackexchange.com/a/143741/153546

Mitigation steps: 

Properly configure web server or in general replace basic auth with reliable solution (it has many others drawbacks and can not be considered as a secure auth method)

## Gather information about target's software (fingerprinting)
Cookies, html pages, headers and few others things might disclose information about the software used on the backend. 

Mitigation steps: 

1. HTTP headers
  Check the configuration and disable or obfuscate all HTTP-headers
  that disclose information the technologies used. Here is an interest-
  ing article about HTTP-headers obfuscation using Netscaler: http://
  grahamhosking.blogspot.ru/2013/07/obfuscating-http-header-us-
  ing-netscaler.html
2. Cookies
  It is recommended to change cookie names by making changes in the
  corresponding configuration files.
3. HTML source code
  Manually check the contents of the HTML code and remove every-
  thing that explicitly points to the framework.

https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20OWASP%20testing%20guide%20v4.pdf (Fingerprint Web Application Framework chapter)

https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework

## Site structure traversal
At least on my training resource it is common to see public web server folders and urls paths which should not be acessible for public. 

Use special software or write your own to brute force possible pathes

Mitigation steps:
With help of specialized software analyse holes and proper confugure backend

## CLFR attacks

HTTP Splitting/Smuggling chapter <a href="https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20OWASP%20testing%20guide%20v4.pdf">OWASP testing guide</a>

## Attack on uploader feature of web app
Uploading some custom user's file might lead to open security breach for malware.

1. Write a custom script (e.g. php like) which will upload file on server and returns links on it
2. Find out this just uploaded script on the backend by traversing urls
3. Upload a custom shell like b375k and explore web server unauthorised zones

Mitigation steps:
1. Control on file extensions would not help much, but make sure user's uploaded files would not be accessible by user *directly* via *direct* URL is a right strategy. This can be done either by storing uploaded files outside of the web root or configuring the web server to deny access to the uploads directory.
2. System generated file names 

https://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Secure%20file%20upload%20in%20PHP%20web%20applications.pdf

Test Upload of Malicious Files chapter
https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files

## Attack on .git files
If .git of the backend is available for public, it is possible to dump this files by anyone and go through version history and whole code to gather credentials, security holes, etc. 

Mitigation steps:
Remove any .git folders from backend. Existing or own script/plugin might help to automate this

## SQL injection

1. Check the database type by error message (by doing intentional error query) 
/api/v2/?id=[x]
2. Check amount of columns in query for future union query by iterating over ```order by``` values until the point server reply valid response:
/api/v2/?id=1' order by 3--+
3. Find out special tables for target database to query tables in db. e.g. for sqlite:
/api/v2/?id=1' union select 1,1,1 from sqlite_master
4. Pocking specific table to find out right column names:
/api/v2/?id=1'  or id='1' union select username,password from users-- -+

Mitigation steps:
https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
