+++
authors = ["xkaynit"]
title = "FCSC2024 - Welcome Admin(1)"
date = "2024-04-28"
description = "FCSC 2024 write up for Welcome Admin(1) challenge"
tags = [
    "xkaynit",
    "web",
]
# series = ["Theme Demo"]
+++
# FCSC 2024
## Welcome Admin 1/2

1. **Introduction**

This writeup is about the Welcome Admin 1/2 challenge of the France CyberSecurity Challenge 2024. Here is the description of the challenge :

*Au coeur d'un réseau labyrinthique, là où la lumière des écrans peine à éclairer les recoins les plus sombres, une demande spéciale est lancée dans les abîmes, un appel discret, attendu seulement par ceux qui connaissent les profondeurs. Seul un véritable expert pourra répondre à l'appel, cryptiquement formulé : "Un expert en SQL est demandé à la caisse numéro 3."*

Link to the challenge : https://welcome-admin.france-cybersecurity-challenge.fr/

2. **Analysis**

When you go on the link you arrive in a Website with a Login pages like below. 

![image](https://gist.github.com/assets/104362418/8f831ee7-a6e0-4f47-b52a-9b63bfd19216)

We can imagine the vulnerability is a SQL injection because the statement of the challenge talk about a SQL expert. As a reminder, SQL injection can enable an attacker to deliberately interfere with the database in order to access its sensitive information. So now I will try to exploit the vulnerability. First of all, to verify that is really an injection I enter an apostrophe in the password field and if an Internal Server Error occured it is good. 

![image-1](https://gist.github.com/assets/104362418/d34caf66-361c-4acf-a3e3-eef1d7fe0184)

The apostrophe is a way to know if a SQL injection is possible. An apostrophe give the possibility of inject data into the query without disrupting it. Now I will try to exploit the vulnerability and display the flag.

3. **Solution**

I've tested all the most common and popular SQL injections. The **' OR 1=1--** triggers the vulnerability and displays the flag as in the screenshot below.

![image-2](https://gist.github.com/assets/104362418/25a113c9-3e7e-4ff3-a95c-513b61848b68)

FLAG : **FCSC{94738150696e2903c924f0079bd95cd8256c648314654f32d6aaa090846a8af5}**