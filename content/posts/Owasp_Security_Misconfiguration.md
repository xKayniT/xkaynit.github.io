+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Security Misconfiguration"
date = "2025-03-30"
description = "Fifth OWASP top 10 vulnerabilities - Security Misconfiguration"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Fifth OWASP top 10 vulnerabilities - Security Misconfiguration

## Introduction

This article is the fifth of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical example. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [security misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/).

## Security Misconfiguration

A Security Misconfiguration happens when the parameters of an application or a system are bad configurated or when some basics configuration are missing. An application/A system can be vulnerable if : 

- Unecessary features/components are enabled or installed(unecessary ports, services, accounts...).

- Default accounts and passwords are enabled and unchanged.

- Error handling give too many informations to users.

- The server does not send security headers or directives.

## Practical example 

The [lab](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-in-error-messages) for this topic will focus on information disclore. More precisely on the third points of the last part, **error handling**. The goal of this lab is to disclose the version of a vulnerable framework. Let's try to solve it :

![shop view](https://gist.github.com/user-attachments/assets/c18a1d37-3de0-4ac7-845f-edfa08c91d3a)

I arrived on a page like above, the only thing I can do is to click on **View details** for a product.

![shop URI](https://gist.github.com/user-attachments/assets/f5bb417d-b4b8-4b24-ad28-8182a56f39a0)

In the URI, products are referenced by Id as it is possible to see with the parameter **ProductId**. 

![URI modified](https://gist.github.com/user-attachments/assets/27b22247-4b91-4523-9c33-ecaf0e223730)

The first thing I can do is modifying the **ProductId** with a big number in order to be sure that there is no article. The **Not Found** result is expected because none entry exist in the database for this article. But what happend if I replace the number by a word, a letter ? Let's see :

![lab solved](https://gist.github.com/user-attachments/assets/9efe3a54-4beb-401f-86be-759d1cace20a)

Bingo ! Indeed, no error handling seems to be in place for another type than integer so I can see the raw error as return from the framework. The framework version is displayed at the bottom of the screenshot.

## Means of prevention

Many means of prevention exists started from secure installation processes which includes : 

- Setup a minimal platform without any unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.

- Think about a segmented application architecture which provide effective and secure separation between components.

- A task to review and updates the configurations according to security notes or patches should be a part of the patch management process.

- An automated process to verify the effectiveness of the configurations and settings in all environments.

## Conclusion

Security misconfiguration is a very common vulnerability due to the surface of it can be applied. A misconfiguration can be the default credentials not changed, an error statement return too many information(practical example) or some unused components. All these elements must be analyzed by security teams to reduce potentials risks.