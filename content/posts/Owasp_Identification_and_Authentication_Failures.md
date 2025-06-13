+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Identification and Authentication Failures"
date = "2025-04-28"
description = "Seventh OWASP top 10 vulnerabilities - Identification and Authentication Failures"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Seventh OWASP top 10 vulnerabilities - Identification and Authentication Failures

## Introduction

This article is the seventh of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical example. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [identification and authentication failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/).

## Identification and Authentication Failures

This vulnerability differs from [broken access control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) in the fact that identication/authentication is the way used to check the user identity where access control is linked to the permission of a user in an application. There may be a weakness in the authentication if the application :

- Permits automated attacks such credentials stuffing(attackers has a list of valid username/password).

- Permits brute force, weak or well-known passwords.

- Uses plain text, encrypted or weakly hashed passwords(linked to [cryptographic failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)).

- Has missing or ineffective multi-factor authentication.

- Expose session identifier in the URL or do not properly invalidate it on logout.

## Practical example 

The lab chooses in this example show an ineffective [multi-factor authentication](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass). The goal of this lab is to bypass the MFA with using the victim credential. Let's try to solve it :

![MFA implementation](https://gist.github.com/user-attachments/assets/271632eb-718a-4e41-b866-9a34418e29a6)

The picture above describes the implementation of the MFA in the application. There are three steps, the login form where the user can fill it with his credentials(**/login endpoint**), verification code(**/login2 endpoint**) with email in a second time which result in a successful login if the code is correct(**/my-account endpoint**). 

![verification code](https://gist.github.com/user-attachments/assets/54074be8-81ef-499e-a663-29acafd7fad9)

After fill the login form with good credentials, it is possible to bypass the verification with going through **/my-account endpoint**.

![lab solved](https://gist.github.com/user-attachments/assets/81eac729-64d1-4c24-94df-b6753652ded7)

The MFA is bypassed and the lab is solved ! No check is done for the verification code step. 

## Means of prevention

Many ways exists to prevent this type of attack :

- Implement multi-factor authentication when possible, this is the best mean to prevent from automated attacks(credential stuffing, brute-force...).

- Do not ship or deploy any applications with default credentials.

- Implement weak passwords check with testing new or changed passwords with rockyou databases for example.

- Ensure registration, credential recovery and API returns the same messages for all outcomes.

- Use a server-side and built-in session manager that generates a new random Session ID with high entropy after login.

## Conclusion

The identification and authentication part is a very sensible component in all applications. If the design is not well thought out, this can lead to failures in both part as shown with the lab in the example. Multi-factor authentication is a must-have feature that all applications shall have nowadays.