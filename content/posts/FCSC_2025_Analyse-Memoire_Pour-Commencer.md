+++
authors = ["xkaynit"]
title = "FCSC - Analyse mémoire - Pour commencer"
date = "2025-04-18"
description = "FCSC write up for Analyse Mémoire challenge"
tags = [
    "xkaynit",
    "forensic",
]
# series = ["Theme Demo"]
+++

# FCSC 2025
## Analyse mémoire - Pour commencer 1/2

1. **Introduction**

This writeup is about the Analyse mémoire - Pour commencer challenge of the France CyberSecurity Challenge 2025. Here is the description of the challenge :

You are preparing to analyze a memory dump and you are noting some information about the machine before diving into the analysis:

    username,
    machine name,
    non-local IPv4 address of the machine.

The flag is in the format FCSC{<username>:<machine name>:<IPv4 address>} where:

    <username> is the name of the user using the machine,
    <machine name> is the name of the analyzed machine, and
    <IPv4 address> is the non-local IPv4 address of the machine.

For example: FCSC{Arthur:Computer-of-rct:192.168.1.150}.

File : analyse-memoire.tar.gz
Link : [Analyse mémoire - Pour commencer](https://hackropole.fr/fr/challenges/forensics/fcsc2025-forensics-analyse-memoire-1/)

2. **Analysis**

This is the first forensic challenge on this blog. In this post, I am going to introduce a tool that I discovered during the CTF, **volatility**. This is a tool designed for memory analysis for Windows or Linux. As for me, I used **Exegol** with the 2 and 3 version of it. The first thing to do is to identify the Operating System of the capture we have :

```
volatility2 imageinfo -f analyse-memoire.dmp

[...]
          Suggested Profile(s) : Win10x64_17134, Win10x64_10240_17770, Win10x64_14393, Win10x64_18362, Win10x64, Win2016x64_14393, Win10x64_16299, Win10x64_19041, Win10x64_17763, Win10x64_10586, Win10x64_15063 (Instantiated with Win10x64_15063)

[...]
```

Once I now that is a Windows memory dump, I can use the corresponding flags of the framework to find the required information. **Volatility** brings many options to find informations about the dump. The following command display all the process in the opened sessions, as it is possible to see, one user catches my attention :

```
volatility3 -f analyse-memoire.dmp windows.sessions.Sessions

[...]
1	-	4412	sihost.exe	DESKTOP-JV996VQ/userfcsc-10	2025-04-01 22:10:52.000000 UTC
1	-	4452	svchost.exe	DESKTOP-JV996VQ/userfcsc-10	2025-04-01 22:10:52.000000 UTC
1	-	4504	svchost.exe	DESKTOP-JV996VQ/userfcsc-10	2025-04-01 22:10:52.000000 UTC
1	-	4576	taskhostw.exe	DESKTOP-JV996VQ/userfcsc-10	2025-04-01 22:10:52.000000 UTC
[...]
```

The screenshot gives to me the hostname and the username but this is not the only way to retrieve it. The next step is to find the local IPv4 address of the computer. For this task, I use **windows.netscan.NetScan** which displays the network objects presents in the image :

```
volatility3 -f analyse-memoire.dmp windows.netscan.NetScan

[...]
Offset	Proto	LocalAddr	LocalPort	ForeignAddr	ForeignPort	State	PID	Owner	Created
0xa50a29071730	TCPv4	10.0.2.15	65014	20.190.160.67	443	CLOSED	7232	msedge.exe	2025-04-01 22:13:53.000000 UTC
0xa50a296962c0	TCPv4	10.0.2.15	51477	204.79.197.219	443	CLOSED	7232	msedge.exe	2025-04-01 22:13:52.000000 UTC
0xa50a297956f0	TCPv4	10.0.2.15	52503	91.199.221.3	80	CLOSED	5408	msedge.exe	2025-04-01 22:14:07.000000 UTC
0xa50a297cd270	TCPv4	10.0.2.15	58828	185.89.210.248	443	ESTABLISHED	7232	msedge.exe	2025-04-01 22:13:46.000000 UTC
0xa50a2988aa70	TCPv4	10.0.2.15	49745	199.232.210.172	80	CLOSED	2948	svchost.exe	2025-04-01 22:13:27.000000 UTC
0xa50a299804a0	TCPv4	10.0.2.15	49743	52.177.176.186	443	CLOSED	9112	svchost.exe	2025-04-01 22:12:55.000000 UTC
0xa50a29a37520	TCPv4	10.0.2.15	57919	185.89.208.19	443	CLOSE_WAIT	7232	msedge.exe	2025-04-01 22:13:46.000000 UTC
0xa50a29bd1a20	TCPv4	10.0.2.15	59258	52.108.8.254	443	CLOSED	6720	SearchApp.exe	2025-04-01 22:15:17.000000 UTC
0xa50a29c1fa20	TCPv4	10.0.2.15	49690	150.171.27.12	443	CLOSED	6008	SearchApp.exe	2025-04-01 22:11:00.000000 UTC
0xa50a29e5fa20	TCPv4	10.0.2.15	49718	2.21.146.43	443	CLOSED	9112	svchost.exe	2025-04-01 22:12:54.000000 UTC
0xa50a29eaea60	TCPv4	10.0.2.15	49709	100.68.20.103	443	ESTABLISHED	1800	rundll32.exe	2025-04-01 22:11:15.000000 UTC
0xa50a29f8b2a0	TCPv4	10.0.2.15	57158	35.214.168.80	443	ESTABLISHED	7232	msedge.exe	2025-04-01 22:13:47.000000 UTC
0xa50a2a0d6ae0	TCPv4	10.0.2.15	65055	52.222.169.27	443	CLOSE_WAIT	7232	msedge.exe	2025-04-01 22:13:43.000000 UTC
[...]
```

3. **Solution**

I have now all the informations needed to retrieve the flag. **Volatility** is a very powerful tool to analyse a memory dump. In addition, it was the first time that I used **Exegol** in a CTF and this was very easy to use and to deploy, I recommend it !

## Analyse mémoire - Pour commencer 2/2

1. **Introduction**

Here is the description of the second part of the challenge : 

The memory dump was taken while a user was working on a highly sensitive document. If the workstation was compromised, this document may have been stolen. Can you find:

    the name of the document editing software,
    the name of the document.

The flag is in the format FCSC{<software name>:<document name>} where:

    <software name> is the name of the editing software’s executable, and
    <document name> is the name of the document being edited by the user (without the file path).

For example: FCSC{calc.exe:My accounts 2025.txt}.

2. **Analysis**

The second step of the challenge focuses on the application used by the user when it was working on a sensitive document. With **volatility**, it is possible to list all the process during the memory dump. **windows.sessions.Sessions** flag help me to identify the targeted application which was soffice.exe. Started from this clue, I grep all the files link to soffice.exe application and finally found the result below : 

```
volatility3 -f analyse-memoire.dmp windows.cmdline.CmdLine | grep "soffice.exe"

[...]
8968resssoffice.exe	"C:\Program Files\LibreOffice\program\soffice.exe" -o "C:\Users\userfcsc-10\Desktop\[SECRET-SF][TLP-RED]Plan FCSC 2026.odt"
9048	soffice.bin	"C:\Program Files\LibreOffice\program\soffice.exe" "-o" "C:\Users\userfcsc-10\Desktop\[SECRET-SF][TLP-RED]Plan FCSC 2026.odt" "-env:OOO_CWD=2C:\\Users\\userfcsc-10\\Desktop"
[...]
```

3. **Solution**

All the informations above can be used for the flag. **Volatility** linked to **Exegol** gave to me the opportunity to discover these two tools. They are very powerful and I will use it in other CTF if needed.
