+++
authors = ["xkaynit"]
title = "FCSC2024 - Fifty shades of white(Junior)"
date = "2024-05-04"
description = "FCSC 2024 write up for Fifty shades of white(Junior) challenge"
tags = [
    "xkaynit",
    "reverse",
]
# series = ["Theme Demo"]
+++

# FCSC 2024
## Fifty Shades Of White(Junior)

1. **Introduction**

This writeup is about the Fifty Shades Of White(Junior) challenge of the France CyberSecurity Challenge 2024. Here is the description of the challenge :

*Le grand Walter White a écrit un programme lui permettant de restreindre l’accès à ses données “professionnelles”. Il distribue des licenses au compte-gouttes, mais vous avez néanmoins récupéré une license qu’il a générée pour son fils ! Son système propose deux niveaux de licenses : celle que vous avez récupérée est la moins privilégiée, et vous souhaitez obtenir une license “admin”.*

*Le programme ci-joint vérifie entre autres le niveau de privilèges de la license, et vous récompense si vous présentez une license “admin”.*

File : fifty-shades-of-white, license-walter-white-junior.txt

2. **Analysis**

Firstly I'm going to analyse what is in the text file. Here is the content : 

TmFtZTogV2FsdGVyIFdoaXRlIEp1bmlvcgpTZXJpYWw6IDFkMTE3YzVhLTI5N2QtNGNlNi05MTg2LWQ0Yjg0ZmI3ZjIzMApUeXBlOiAxCg== 

It's seems to be a base64 content according to the two equal signs at the end of the string. It's possible to decode the chain like below :

Name: Walter White Junior
Serial: 1d117c5a-297d-4ce6-9186-d4b84fb7f230
Type: 1

Now, I know the content of the license file and the analysis of the fifty-shades-of-white file can start. I open the file with ghidra and see the different functions defined in this file :

![image](https://gist.github.com/assets/104362418/b11d9b13-73bc-4252-8d14-5b62c7247acd)

If I click on the **check** function, I can display this function and analyse what she does :

![image-1](https://gist.github.com/assets/104362418/0fc266eb-aba4-443f-b1f8-1cdb62b82d47)

In this function, we can see 3 if loops which is the result of the validate function which take the license parameter in entry. Below is an explanation of each loop and the condition to pass in it:
- The first loop check if a license is provided when the program is started.
- The second loop verify that the type of the license is equal to 1.
- The last loop call the function **show_flag** if the type is equal to 0x539.

3. **Solution**

As we can see, 0x stand for hexadecimal number. So, with a converter, I know that 0x539 give 1337 in decimal. Now, I will try to modify the type field in the license file with 1337 and see what appends. 

![image-2](https://gist.github.com/assets/104362418/138edde7-7ac6-48ab-a500-ab721780773c)

With 1337 in license type, I passed into the third loop so the flag is display.

FLAG : **FCSC{2053bb69dff8cf975c1a3e3b803b05e5cc68933923aabdd6179eace1ece0c41a}**