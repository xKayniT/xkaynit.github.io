+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Security Logging and Monitoring Failures"
date = "2025-07-01"
description = "Nineth OWASP top 10 vulnerabilities - Security Logging and Monitoring Failures"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Nineth OWASP top 10 vulnerabilities - Security Logging and Monitoring Failures

## Introduction

This article is the nineth of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical examples. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [security logging and monitoring failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/).

## Security Logging and Monitoring Failures

This category is coming from the community survey and earned a place from the 2017 Top 10. She focuses on the detection and response time to attacks/breachs. Logging and Monitoring can be very impactful and useful for accountability, visibility and forensics. Security Logging and Monitoring Failures is regularly put behind others categories because it can be expensive in money and time to setup a good and clear loggind/monitoring system. Breach cannot be detected if :

- Logins or failed logins are not logged.

- Warning and errors generate unclear log messages.

- Logs of applications and APIs are not monitored for suspicious activity.

- Logs are not stored with redundancy.

- Security application tools do not trigger alerts.

- Logs are not recorded in real-time or near real-time.

## Practical example

I found no labs related to this vulnerability, this is why there are only two examples here :

- Your company works with a third-party cloud provider for the authentication of your e-commerce website. If a breach occured in it and the cloud provider alerts you few days after the incident, you will not be able to answer properly to it in a reasonable time. This is why it is important to ensure your company works with well-known and trusted provider.

- For the second one, let us imagine we are on a **red team** exercise. As a remember, a **red(offensive) team** exercise is when a company want to know the detection and response time of the **blue(defensive) team** with doing some (emulated)attacks on the system/network. The goal of the **red team** is to be silent and not to be detected, where the **blue team** objective is to detect him as earlier as she can. However, if your application or system do not have logging or monitoring features, this complicate a lot the work of the defensive team because she has no ways to identify that an intrusion happened. 

## Means of prevention

Fortunalety, developers can implement some controls to prevent from this type of attacks with the following things : 

- Be sure that all tries to login, access-control and server-side input validation failures are logged by the application.

- Ensure that generated logs are in a format which a SIEM can easily consume and analyse.

- Ensure data logs are correctly encoded in order to prevent injection or attacks on logging and monitoring systems.

- Establish a recovery plan and make some exercises of incident response after an attack to try your detection and response time. 

## Conclusion

Nowadays, logging and monitoring is mandatory for each applications or teams who wants to be secured. The detection and response time are both important to have the smallest possible impact on the system and network. Additionnaly, many solutions exists to treat logs streams such as ELK(Elastic Search suite) or Wazuh for example. 