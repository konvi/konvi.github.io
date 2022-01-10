---
layout: post
title:  "OWASP in Java/Spring"
date:   2022-01-09 09:14:00 -0700
categories: owasp
---

OWASP Top 10 Security Risks:
![OWASP Top 10](https://owasp.org/www-project-top-ten/assets/images/mapping.png "OWASP Top 10")

Learn more:
- [OWASP Top Ten](https://owasp.org/www-project-top-ten/).
- [Java Spring Developers @ OWASP] (https://owasp.org/dev-pages/java/spring/index.html)

## A01:2021 Broken Access Control (A05:2017)

It is all about Authorization.
1. "Insecure direct object reference": url allows to access something I shouldn't be able to access.
2. "Missing function level access control"

## A02:2021 Cryptographic Failures (A03:2017 Sensitive Data Exposure)

1. Maybe we don't need to collect this information at all?

## A03:2021 Injection (A01:2017)

Good Explanation is in "Learning the OWASP Top 10" by Caroline Wong:
>In this case, a web application is supposed to control the commands, and the user is supposed to provide the data. But in an injection attack, instead of specifying data, the user is able to specify a command.  

Example: 
Concatenation instead of PreparedStatement
{% highlight sql %}
"select * from users where username = '" + username + "'"
where username = "xyz' or '1' = '1"
#=> selects all users, because the clause is always true
{% endhighlight %}

Solution:
Use PreparedStatements / Spring Data/JPA

## A03:2021 Injection (A07:2017 Cross-Site Scripting XSS)

It is also an injection: user input is command, instead of being a data. User input on a web page contains `<script>` tag.

>As a general rule, unless the application is using ThymeLeaf, Java/JSP applications can be susceptible to XSS (A3). It is recommended that all output be encoded using a library level function to ensure that user input doesn't get sent to the browser in a way that will be executed.

## A05:2021 Security Misconfiguration (A04:2017 XML External Entities XXE)

XML should contain data, but malisios document causes XML processor to run out of memory.

## A04:2021 Insecure Design (NEW)


## A05:2021 Security Misconfiguration (A06:2017)

1. Error Handling - don't reveal details to the end user

## A06:2021 Vulnerable and Outdated Components (A09:2017 Using Components with Known Vulnerabilities)

Quite clear.

## A07:2021 Indentification and Authentication Failures (A02:2017 Broken Authentication)

Limit login attempts, two-factor authentication.
Broken Application Logic 

Use Spring Security defaults or at least know what you are changing and why.
Spring provides function level access control (A7) through annotations to controllers or services.


## A08:2021 Software and Data Integrity Failures (A08:2017 Insecure Deserialization)

=> denial of service, remote code execution, ... 

## A09:2021 Security Logging and Monitoring Failures (A10:2017 Insufficient Logging & Monitoring) 

## A10:2021 Server-Side Request Forgery (SSRF)