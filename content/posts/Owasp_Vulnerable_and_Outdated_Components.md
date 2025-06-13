+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Vulnerable and Outdated Components"
date = "2025-04-22"
description = "Sixth OWASP top 10 vulnerabilities - Vulnerable and Outdated Components"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Sixth OWASP top 10 vulnerabilities - Vulnerable and Outdated Components

## Introduction

This article is the sixth of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical example. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [vulnerable and outdated components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/).

## Vulnerable and Outdated Components

This type of vulnerability is directly linked to the previous one of the OWASP top ten(Security Misconfiguration) for the vulnerable components. The following lists contains proofs that can indicates your infrastructure have vulnerable or outdated components :

- If you do not know the versions of all components you use(both client and server side)

- If the software is out of date or unsupported. For example the OS, the web server or the DBMS. 

- If you do not scan for vulnerabilities regurlaly and subscribe to security bulletins related to the components you use.

- If software developers do not test the compatibility of updated, upgrade or patched libraries.

## Practical example 

In this part, I'm going to introduce two practical examples :

- The first example is linked to another [post](https://xkaynit.github.io/posts/owasp_security_misconfiguration/#practical-example) on the OWASP top 10. The lab done in this article display the version of the framework used and this is not the last version of it. The focus here is not on the messages errors displayed but on the framework version. This is not the last released version and this can lead to potential vulnerabilities. 

- In a second time, an application runnning components with the same privileges but not the same roles can have a big impact. Indeed, if one component is compromised, all the application can be vulnerable because the privileges are the same in all the application. 

## Means of prevention

Fortunalety, some ways exists in order to prevent your application to be vulnerable : 

- Remove unused dependencies, unnecessary features, components, files, and documentation.

- Continuously inventory the version of both client and server side components. Furthermore, take a regular look of the security notice for the components in use.

- Ensure you have only official packages in use with signature to reduce the chance of including a modified or malicious components. 

- Monitor for libraries and components that are unmaintained or do not create security patches for older versions. 

## Conclusion

Thinking about a management process of the vulnerabilities is mandatory in an application lifecycle to be sure that there is no vulnerable or outdated components. Security teams and developers should be aware of what they used to develop the application. 
