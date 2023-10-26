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
