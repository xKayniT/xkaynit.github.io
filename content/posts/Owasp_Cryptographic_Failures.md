+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Cryptographic failures"
date = "2025-03-22"
description = "Second OWASP top 10 vulnerabilities - Cryptographic failures"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Second OWASP top 10 vulnerabilities - Cryptographic failures

## Introduction

This article is the second of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical example. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [Cryptographic failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/).

## Cryptographic failures

This category is focusing on the data exchange and the data at rest. In some domains such as banks or health there are laws which specified the need of extra protection depending on the types of data manipulated. For example, credits cards numbers or health records need a better protection compared to user basket data on a sport website. Below are some questions that a developper or security team should ask themselves :

- Is any data transmitted in clear text ? This is the case for all insecured protocols likes HTTP, FTP. Verify all internal traffic and considered that all external internet traffic is hazardous.

- Are some old or weak cryptographic algorithms/protocols used by default ? 

- Are default crypto keys in use, weak crypto key generated or re-used ? Are crypto keys checked into source code repositories ?

- Is encryption not enforced(headers for security, HSTS...) ? 

- Are deprecated hash functions such as MD5 or SHA1 in used ? 

## Practical example

This type of vulnerabilities can be show in many scenario, the following list describe some of them :

- A company store many databases but the critical information such as the password is stored with MD5 hash function. This is a weak hash function and this can lead to information leaks. 

- An organisation have a new secured website(HTTPS) but for practical reason she still uses the old version(HTTP). If a login form is used in the old version of the website, this can lead to credentials leaks if an attacker intercept traffic. 

## Means of prevention

Many solutions should be used in this case :

- Identify which data is stored or exchange and if some data are under a law control caused by the sensitivity of it(bank or health domains). 

- Make sure that all sensitive data at rest are encrypt.

- Do not use legacy protocol such as FTP or SMTP for transporting sensitive data.

- Ensure to use up-to-date and strong algorithms(SHA256, AES256...).

- Encrypt all data in transit with secure protocol(TLS).

- Use salted hashing function for sensitive information.

## Conclusion

To summarize, this article show the importance of the data encryption. The usage of weak algorithms must be revoked and algorithms recommended by national public agency(ANSSI,NIST...) should be used whenever it is possible.