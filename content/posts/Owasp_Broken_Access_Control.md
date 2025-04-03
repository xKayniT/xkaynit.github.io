+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Broken Access Control"
date = "2025-03-19"
description = "First OWASP top 10 vulnerabilities - Broken Access Control"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# First OWASP top 10 vulnerabilities - Broken Access Control

## Introduction

This article is the first of a series of ten. The aim of this serie is to describe each vulnerabilities contained in the [OWASP top 10 2021](https://owasp.org/www-project-top-ten/). Open Web Application Security Project(OWASP) is an international organisation with the goal of improving web security applicatios. Each article will be composed with three parts. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability in practice with a portswigger lab related to access control. Finally, the last part will described the best practices to mitigate and delete vulnerable components. The first article is focus on [Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/).

## Broken Access Control

Access control define the permissions of a user into a website. When these permissions are not well-defined or broken, user can do things normally unauthorized for him such as switch to another account. This can lead to unauthorized information disclosure, modification of some information and it is a threat for integrity of user data. Common acces control vulnerabilities include notably :

- Violation of the principle of least privilege or deny by default. Some resources with particular roles or users are accessible to anyone.

- Bypassing access control checks by modifying URL, internal application state, or the HTML page, or by using an attack tool modifying API requests.

- Acting as a user without being logged in or acting as an admin when logged in as a user.

- Force browsing to authenticated pages as an unauthenticated user or to privileged pages as a standard user.

## Practical example

As I said earlier, I'm going to use a [portswigger lab](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality) which is designed to practice vulnerabilities related on access control. I will try to use one of the method cited above. The first thing to do is to login on the platform. In the lesson provide with this lab we learn that a robots.txt file can disclose some interestings URI. Let's try to access to this file :

![robots file](https://gist.github.com/user-attachments/assets/0f05f375-ca74-4e0c-b30d-0fbecb485bb4)

On the robots file, a URI location is specified. As it is possible to see, the URI is related to an administrator panel. If I try to go on this URI we have the possibility to delete some user account :

![admin panel](https://gist.github.com/user-attachments/assets/91f9b398-75f9-42df-9974-79b701b2801c)

With deleting user carlos, the lab is solved ! The goal of a robots file is to display some directories directly to [web google crawlers](https://developers.google.com/search/docs/crawling-indexing/robots/intro). Here the file is not a problem although display the administrator panel on the robots file is not a good practice, an attacker could brute-force directories names. The major problem here is that the panel is accessible for anyone without any control. 

## Means of prevention

Differents means of prevention exists, there are directly linked with common access control vulnerabilities described in the previous part. You can find below a non-exhaustive list of it : 

- Deny by default access to a resource except if it is a public resource.

- Disable web directory listing and ensure important files(e.g .git, backup files...) are not in the web roots.

- Log access control failures with sending alerts to administrator in such cases(e.g repeated failures, trying to access an unauthorized resources many times).

- If your application have an API, set a rate limit in order to prevent the use of automated tools(same thing with the access control).

## Conclusion

This article show the first vulnerability of the OWASP top 10 2021. Access control is a big part of a web application because it is directly linked to users rights. Developpers and security team should be aware of this risk and apply the means of prevention when it is possible.