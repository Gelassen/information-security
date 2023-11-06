1. Get basic information from the endpoint 

```
$ nmap -A -Pn -sC -sV <endpoint>
```

2. Perform directory traversal to find out restricted areas which has been left open:
```
$ dirb <endpoint> <bruteforce-dictionary>
```

3. Investigate discovered artifacts

3a. Found admin url
    - in case of Base64 try to bypass it over variation in http request method (e.g. PUT instead of POST)
    - try sql injections ```$ sqlmap -u <endpoint>```

3b. Found exposed ```.git``` folder
    - dump the website, e.g. ```$ ./gitdumper.sh -h <endpoint>/.git dest-dir <path-to-folder-on-local-filesystem>``` from <a href="https://github.com/Gelassen/information-security/blob/main/README.md">GitTools</a>, and check history of commits for security holes in the software 

3c. Investigate html and js source code for clues to security holes (passwords, no public API methods, db table names, etc.)

3d. Go deeper with other penetration techniques

4. Based on fingerprint stage results search for recent vulnerabilities (and possible already published exploits)
Nmap command launch and sql injections should give us information about endpoint operating system and database used by the backend. This information might be used for search recently discovered vulnerabilities, e.g. in <a href="https://nvd.nist.gov/vuln/full-listing/2023/11">US National Vulnerability Database</a>. This knowledge might be used to write own exploit to hack the system or even to use a developed one. 

Tools like Nessus or alternative solutions might help to automate such search. 

5. Focus on the post popular attack vectors 
<a href="https://owasp.org/www-project-web-security-testing-guide/stable/">OWASP testing guide</a> contains many vectors of attacks which is also updated every year. Over passed two weeks I touched only 5%-15% of this techniques and it is important to prioritize where will you put your efforts search.  That's why it might be interesting to view several InfoSeq reports to identify the most common hacks in your target region and align your threat model to them. 

To be continued...

 