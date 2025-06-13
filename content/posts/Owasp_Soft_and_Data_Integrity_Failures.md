+++
authors = ["xkaynit"]
title = "OWASP TOP 10 - Software and Data Integrity Failures"
date = "2025-05-02"
description = "Eighth OWASP top 10 vulnerabilities - Software and Data Integrity Failures"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Eighth OWASP top 10 vulnerabilities - Software and Data Integrity Failures

## Introduction

This article is the eighth of a series of ten, for more information about the serie please refer to the first article. The first part will introduce what the vulnerability does and the impact of it on website, the second will show the vulnerability with some practical examples. Finally, the last part will described the best practices to mitigate and delete vulnerable components. This article is focus on [software and data integrity failures](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/).

## Software and Data Integrity Failures

This is a new category in the top 10 which appeared with the increase of updates, critical data exchanges and CI/CD pipelines. If this is done without verifying the integrity of the new downloaded package or there is no means to verify the data integrity, this can lead to malicious behavior. An insecure CI/CD pipeline which has no verifications of the package can expose the application too. 

## Practical example 

Due to the nature of the vulnerability, only two examples will be available for this category :

- Assuming Ubuntu is your favorite Linux distribution(my case :D) and you want to update the packages on your operating system. Two commands are necessary but if you want to test an untrusted package, you have to disable the verification of the downloaded package. If the package contains malicious code, an attacker can take the control of your computer for example. This is why it is important to always know what you download and to let enable the package verification.

- If you have some CI/CD pipeline to optimize your integration and your deployment but you do not check the integrity of all the downloaded package during the setup phase, this is not a good practice. Some package can be very close in the names and a mistake can be done by writting it. This can lead to a bad integration and the consequences can be technical or financial in such cases.

## Means of prevention

It is important to implement the following recommendations to prevent from this type of vulnerability :

- Use digital signatures or similar mechanisms to verify the software or data is from the expected source and has not been altered. 

- Ensure that there is a review process for code and configuration changes to minimize the chance that malicious code or configuration could be introduced into your software pipeline.

- Ensure that your CI/CD has proper configuration, and access control to ensure the integrity of the code flowing in the build and deploy processes.

- Ensure that unsigned or unencrypted serialized data is not sent to unstrusted clients without some form of integrity check or digital signature to detect tampering or replay of the serialized data.

## Conclusion

Integrity is an important part that security teams and developers need to take into account for the packages used in the project or the application they built. Giving to the customer a signature or a mechanism to check the integrity of the application is considered has mandatory nowadays.

