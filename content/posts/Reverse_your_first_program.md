+++
authors = ["xkaynit"]
title = "Reverse your first program using ghidra"
date = "2024-07-15"
description = "Reverse your first program using ghidra post"
tags = [
    "xkaynit",
    "post",
    "reverse",
]
# series = ["Theme Demo"]
+++

# Reverse your first program using ghidra

## Introduction

This article is here to help anyone want to start in reverse some programs but don't know where to start. **Reverse engineering** consist in deconstruct/decompilate(in our case) code or executable file in order to understand how it works and to identify potentials vulnerabilities which can be exploit by an attacker after analysis. As an example, I took a file from https://crackmes.one/ which includes a lot of resources to start in reverse engineering. Many softwares exists and allow you to decompilate executable code like BinaryNinja for example, I personnaly use Ghidra to do this but the features is pretty the same for all. 

## Ghidra installation

In this parts, I'm going to show you how you can download, install and launch Ghidra for the first time. Ghidra is a software reverse engineering(SRE) suite of tools developed by NSA. There is differents ways to download and install Ghidra, in this article I will install it with the snap command available in Linux. Snap is a store with many applications packaged and with all of their dependencies, so it means we just have to install a software with snap and no more action is required. We could find all the way to install it on the following link : https://htmlpreview.github.io/?https://github.com/NationalSecurityAgency/ghidra/blob/Ghidra_11.1.1_build/GhidraDocs/InstallationGuide.html. 

Ghidra take approximatively 2 Go of storage and it can be installed with the command below :

**sudo snap install ghidra**

As explain in the previous paragraph, no additional action is necessary so ghidra can be start with only write **ghidra** in a command line terminal and the software started :

![image](https://gist.github.com/user-attachments/assets/9a82bb3b-1659-4fe3-a91c-8d131c865a4b)

This screenshot indicate that ghidra is successfuly installed and ready to use. The last thing to do is to create a new project to start reverse your first program ! Go in File > New Project, after you have to choose if you want to shared you projet or not(in my case not) and the name of it. Now you are ready for the next step !

## Reverse

In the previous screenshot, you can see that four elements is present in the **Tool Chest**. In this article, we are only focus on the first element, the dragon. If you move your mouse over, the displayed name is **Code Browser**, it allow you to analyse a lot of executable program, click on it.

![image-1](https://gist.github.com/user-attachments/assets/1f601fdd-b99f-4ebf-a21f-91fbe1576f75)

The screenshot above show the code browser application launched but no file is currently imported. Click on File > Import File and load the file you want to analyse(an ELF file in my case). A window like below should appear :

![image-2](https://gist.github.com/user-attachments/assets/1c0a2981-c28e-4269-b49b-05d796456e0e)

In this window you can find some informations about the file you import, the format, the language(here you can see that it's C language with the gcc compiler) and project name too. Choose OK and a message box named **Analyze?** should be displayed, answer Yes. 

![image-3](https://gist.github.com/user-attachments/assets/babfc1ec-7d40-498b-8b22-be8b8436848f)

Once analysis option window open, let the options currently checked and don't modify the **Analyzers** unless if you have some knowledges of the options you wish to modify or if you have special requirements. You can now analyse the file.

![image-4](https://gist.github.com/user-attachments/assets/b5063b8c-3352-42b4-b247-786596230474)

When you can see this windows you have successfuly imported your file and the analysis can start ! 

## Analysis

At this step, the file is imported and ready for analysis. In the previous screenshot, you have many informations, let me give you some useful informations for a better comprehension.

![image-5](https://gist.github.com/user-attachments/assets/ee10dfef-8772-4443-8429-2361da5f7286)

The screenshot is composed of 3 square, the red square show the structure of the code like imported librairies, classes and **functions** defined. Futhermore, you can see that only a main function is interesting here because all the others functions concern the compiler. The middle blue square named listing display the different call between functions and addresses used, this give the possibility to look at what's inside the program and understand more precisely the call order sequence. Finally, the green square is the most important because it shows the **decompilate and understandeable** code. It's the start point of our analysis. Now that a little bit of explanations is set, we can go more deeper in the analysis !

As I explained previously, the right part of the CodeBrowser window show the code before compilation. In the case of the crackmes example, only the main function is interesting, let's go to analyse it !

![image-6](https://gist.github.com/user-attachments/assets/c3a0b73b-e549-4e72-95e8-d95472e039da)

Now, I will list all the importants points present in the code :

- The first if loop check that a string is filled in argument after the program is started(example: ./my-executable-code password) 
- An array named piVar2 is created with the following value in hexadecimal : [0x6e,0x60,0x5e,0x6d,0x60,0x6f], the next for loop increasing by 5 the hexadecimal value of each elements in the array, now the array is equal to : [0x73,0x65,0x63,0x72,0x65,0x74]
- A __s2 array variable is created with the same characters value as piVar2 for each elements(example : __s2[0] = piVar[1] or __s2[0] = 0x73 etc...).Futhermore, the last character is an "\0" which is corresponding to an end of string. 
- The result of strcmp is stored in iVar1 variable. The strcmp stand for stringcompare and take two arguments which are our second argument as a char type and __s2 variable. If the strcmp function find no differences between the two given strings 0 is return, if one difference is find 1 is returned etc..
- The last if loop check that if the result of strcmp is 0, "Welcome" is returned and returned "Denied" if the result is not 0. 

We know that our second parameter need to be equals to __s2 variable(see strcmp function) and __s2 take the piVar2 value after the loop. So, we have the corresponding value of piVar2 after the loop which is : [0x73,0x65,0x63,0x72,0x65,0x74] equals to **"secret"** in characters(from hexadecimal, cyberchef website can help you here). This is the answer and you have to enter this string as the second parameter. 

Congratulations, you have reverse you first executable program using ghidra !

## Conclusion

If you have any questions in the way I have decompilate the program or understand it feel free to ask me ! This article show you how you can reverse and decompilate an executable program. Somes programming and compilation knowledges can be needed to clearly understand some steps of this article !

