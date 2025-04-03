+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Injection"
date = "2025-03-24"
description = "Third OWASP top 10 vulnerabilities - Injection"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

## Third OWASP top 10 vulnerabilities - Injection

## Introduction

This article is the third of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical example. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [injection](https://owasp.org/Top10/A03_2021-Injection/).

## Injection

Wikipedia defines _[Code Injection](https://en.wikipedia.org/wiki/Code_injection) as a computer security exploit where a program fails to correctly process external data, such as user input, causing it to interpret the data as executable commands._ An injection can be of differents types such as for example SQL or XSS injection. Below are some elements which can make your application vulnerable :

- User input are not validated, filtered or sanitized by the application. None control is done on data.

- Dynamic queries or non-parameterized calls without context-aware escaping are used directly in the interpreter especially for SQL injection.

- Hostile data is directly used or concatenated. This point is linked to the first one.

## Practical example

I am going to show two types of injection here. The first one will talk about SQL injection which is a direct interaction with the database in a user input and how to detect it. The second is oriented on XSS injection which allow the user to execute command on both client or server side. 

**SQL Injection :**

This is a very common types of injection. This happens when the user input is not properly validated and cause a non-expected interaction with the database. Most of the times this vulnerability appears on login form and can cause a lot of damages. It is possible to use a set of tests to all entry point in a application in order to detect a SQL injection :

- The single quote character **'** can help to identify it if an error is returned.

- Use some boolean conditions like **OR 1=1** to detect some anomalies.

- Some tools can help you to identify this such as Burp Scanner or SQLMap.

Now that we know how to detect SQL injection we can try to detect it with an exercise. I take [juice shop](https://github.com/juice-shop/juice-shop) as a lab which is designed by OWASP to learn web vulnerabilities. The first step is to use the techniques cited above in order to check if there are some vulnerabilities, the scope of this part is the login page. Let's try the single quote :

![sql injection](https://gist.github.com/user-attachments/assets/df1785ad-e7fe-44a3-be80-921430f6f1a2)

As it is possible to see, the error returned seems to be abnormal. Indeed, this is the behavior of an SQL injection. The next thing to do here is to find the correct payload, I will not show you how to resolve this lab because I want to let you try by yourself(hint : [SQL injection payload](https://github.com/payloadbox/sql-injection-payload-list?tab=readme-ov-file)).

**XSS injection :**

Cross-site scripting is another common types of injection. The aim of it is to execute command on a user input which can lead to potential malicious behavior. There are three mains types of XSS attacks([for more information](https://portswigger.net/web-security/cross-site-scripting#how-to-find-and-test-for-xss-vulnerabilities)) :

- Reflected XSS where the malicious script comes from the current HTTP request.

- Stored XSS where the malicious script comes from the website's database.

- DOM-based XSS where the vulnerability exists client-side code rather than server-side code.

To detect the presence of a XSS vulnerability, it is possible to execute some functions like **alert()** or **print()** on user input field. The example below show how to detect it with solving a portswigger lab :

![search bar](https://gist.github.com/user-attachments/assets/5755d973-58a7-4b94-b960-dfb7a369f6d3)

![xss result](https://gist.github.com/user-attachments/assets/768e0671-62c1-472f-98f2-7690237f2974)

In the first screenshot I am trying to identify if a cross-site scripting vulnerability exist in the website. The payload filled should be interpreted as executable code if a vulnerability is present. The result is shown in the second screenshot and confirm the vulnerability. This is a reflected XSS because it comes from the current HTTP request if I take a look on network tab on a browser.

![reflected xss](https://gist.github.com/user-attachments/assets/01f5239b-798c-45cf-bb17-961774fadc9d)

## Means of prevention

Hopefully, many ways can be set in place to prevent from injections :

- Safe API can be a solution because it provides a parameterized interface. 

- Always validated, filtered and sanitized user input. 

- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection.

## Conclusion

Injection is a very widespread vulnerability nowadays although it easy to detect and patch. All developers shall be aware of how it works and the impact that it can have on a company. 