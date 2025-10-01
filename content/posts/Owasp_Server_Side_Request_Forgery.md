+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Server Side Request Forgery"
date = "2025-07-05"
description = "Tenth OWASP top 10 vulnerabilities - Server Side Request Forgery"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Tenth OWASP top 10 vulnerabilities - Server Side Request Forgery

## Introduction

This article is the tenth(and the last !) of the serie, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical examples. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [Server Side Request Forgery](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/) also known as SSRF.

## Server Side Request Forgery

This category is coming from the community survey too and this is a new category. SSRF occurs when a web application is fetching a remote resource without validating the user-supplied URL. So an attacker can craft a request to an unexpected destination, even with all the protection set in place. Since the server send the request there are fewer right issues on the fetch URL. In the same time, fetching an URL in a web application is common and this increase the incide of SSRF. 

## Practical example

I will discuss two examples from Portswigger labs in this part. The first is a very common example of a local SSRF vulnerability and the second use an interesting feature of Burp suite. Lets try to solve the [first one](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost).

In this lab, the vulnerability is located on the store check feature which no verification is done on the URL. Below is the original request :

![Original request](https://gist.github.com/user-attachments/assets/de78a312-4482-4c58-b4df-115b08d606f8)

The result is returned in the answer. I tried to modify the URL with the one in the statement. 

![Modified request](https://gist.github.com/user-attachments/assets/9aca3200-8946-472e-80d9-887ca6af7f27)

The request to **http://localhost/admin** is done by the server so it is possible to access to it. The last step is to delete carlos user with a **POST** request on the correct URL(**http://localhost/admin/delete?username=carlos**)

The [second example/lab](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-backend-system) is more advanced in SSRF exploitation. In a first time we need to scan the internal network in order to find an admin interface and finally to delete **carlos** user. Let's take a look at the request sent when a user want to check the stock of a product. The original request is done in the same way than the previous lab but with an IP address here. With the **Intruder** feature of Burp Suite, it is possible to scan the network in order to identify where is the admin panel.

![Intruder setup](https://gist.github.com/user-attachments/assets/1019e94f-c026-4361-9ea8-0452ec8bbb33)

Once the attack begin, I can see the response sent by the IP address and seeking for an 200 HTTP code status(OK).

![200 code status](https://gist.github.com/user-attachments/assets/9e818e24-9b2a-4baa-95fb-dc128fde7ae9)

The server on 192.168.0.69 sent a 200 response code, this is our target ! In the content of the response two URL are displayed, with sending a POST request to the correct URL(**http://localhost/admin/delete?username=carlos**), the lab is solved !

## Means of prevention

Developers and security team can prevent this vulnerability by implementing some or all the following defense in depth controls :

**From Network layer**

- Establish a segmented network of resources/applications to reduce the impact of SSRF.

- Always use, when it is possible, "deny by default" firewall policies or network access control rules to block all but not essential intranet traffic. 

**From Application layer**

- Sanitize and validate all client-supplied input data.

- Enforce the URL schema, port, and destination with a positive allow list.

- Do not send raw response to clients.

- When it is possible, do not use a deny list or regular expression. 

## Conclusion

Although SSRF is the last ranked vulnerability, this is an important one and the impact of it can be important without any controls. As shown in the two labs example, it is possible to sca the internal network and to access to internal server only with the server exposed on internet. Security team and developers should be aware of this type of vulnerability and the existing ways to protect against it. This the last article of this series of ten, it was very interesting for me to discover all the attacks and configuration describe because it represents a large majority of the weaknesses of current websites. 