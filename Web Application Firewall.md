# AWS WAF - Web Application Firewall — 

One of the best practices to identify SQL injection attacks is having a web application firewall (WAF).  

A WAF operating in front of the web servers monitors the traffic which goes in and out of the web servers,  
and identifies patterns that constitute a threat.  
Essentially, it is a barrier put between the web application and the Internet.  

We can define aws managed as well as custom rules.  WAF uses these defined rules to check for the suspicious traffic  

A WAF will keep monitoring the applications and the GET and POST requests it receives to find and block malicious traffic.  

WAFs provide efficient protection from a number of malicious security attacks such as:  

- SQL injection
- Cross-site scripting (XSS)
- Session hijacking
- Distributed denial of service (DDoS) attacks
- Cookie poisoning
- Parameter tampering



**Reference:**  
1. https://www.ptsecurity.com/ww-en/analytics/knowledge-base/how-to-prevent-sql-injection-attacks/

