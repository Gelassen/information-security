### Port and ip fingerprinting
```$ nmap -A -Pn -sC -sV <endpoint>```
Use shodan.io or censys.io in case web application firewall (WAF) usage or ```nmap --script=http-waf-fingerprint,http-waf-detect -p443 <endpoint>```

### SQL injections
Use ```sqlmap```

