1. Get basic information from the endpoint 

```
$ nmap -A -Pn -sC -sV <endpoint>
```

2. Perform directory traversal to find out restricted areas which has been left open:
```
$ dirb <endpoint> <bruteforce-dictionary>
```

3a Found admin url
    - in case of Base64 try to bypass it over variation in http request method (e.g. PUT instead of POST)
    - try sql injections 