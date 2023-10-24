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
