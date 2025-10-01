+++
authors = ["xkaynit"]
title = "Shutlock 2025 - Ice Cream"
date = "2025-06-30"
description = "Shutlock 2025 - Ice Cream"
tags = [
    "xkaynit",
    "post",
    "web"
]
# series = ["Theme Demo"]
+++

# Shutlock 2025

## Ice Cream 

1. Introduction 

The purpose of this article is a writeup for Ice Cream challenge which was one of the web challenges for the Shutlock CTF 2025. Here is the description of it :

*Bienvenue dans le challenge Ice Cream !*

*Tu dois trouver l’ingrédient secret des glaces de "PAPI glace", le plus grand vendeur de glace de tout Cannes*.

*You have to generate a token and launch a dynamic instance to start the challenge.* 

2. Analysis

The first step when I am in front of the interface is to analyse it and what I am able to do. The screenshot below shows the interface with a list of ingredients and it is possible to go through each with the corresponding description. Another interesting thing is the Json Web Token(JWT) available when a request is done, a role is associated with the default one : **"simple_utilisateur"**. 

![Challenge web interface](https://gist.github.com/user-attachments/assets/8a69c950-5f9c-4179-8aa7-4b9e36e57407)

Once I have take a look at the general informations available, it is time to go further. When I click on an ingredient, some parameters appears in the URL such as **dir** and **file**. The first thing to do when I see this is to try a directory traversal attack with replacing the current **dir** value by **/** to be at the root of the application.

![Path traversal](https://gist.github.com/user-attachments/assets/2371fa26-f508-4726-8888-9bb019ae5719)

I have now access to other files, let us go deeper into that. Some files seems to be interesting :

- admin.php and admin.txt -> admin.php is here to create user token depending on some informations(code explanation below) and admin.txt contains admin token

- auth.php -> this file allows to search for the other roles available linked to JWT

- secrets.txt -> the secret used to generate the token
 
- role.txt -> the roles availables

All text files are inaccessible without the correct rights. However, many important informations can be found in **admin.php** files because this is the file which generate the user token.

```
function is_admin($jwt, $secret, $adminRole) {
    $payload = verify_jwt_hs256($jwt, $secret);
    if (!$payload) return false;
    return isset($payload['role']) && strtoupper(trim($payload['role'])) === strtoupper(trim($adminRole));
}
```

 The first information I see it is the need to have the correct role($adminROle) and secret($secret) to access to text files otherwise a token with **"simple_utilisateur"** role is created. Another important information to keep in mind is a **debug** endpoint exists to execute command but I need to have an admin token to use it. Now I can start to seek for the expected role. 

![SQL Injection](https://gist.github.com/user-attachments/assets/f8511b2b-9d31-4f1d-a5c8-1c9b78d1a7a0)

I quickly identified an SQL Injection on the input of auth.php with the returned message when I fill the input with an apostrophe character. With searching and trying some of the payload available [here](https://github.com/payloadbox/sql-injection-payload-list), I listed all the **auth** table and got the correct role(PAPI-JE-SUIS-UN-ADMIN). I can now focus on the secret. For this steps, I used **jwt_tool** available on **Exegol** which can brute-force secret and tamper token. 

![Token brute-force](https://gist.github.com/user-attachments/assets/70ebeafc-ec03-42bf-95cd-7e115f96caf4)

With using **rockyou** wordlists during few seconds a match is established :**icecream**. I have know all the informations to create the expected token with the same tool. In **admin.php** file I learn about the existence of the **debug** endpoint, this is the next step after created the token. 

![Debug endpoint](https://gist.github.com/user-attachments/assets/ded09311-8f63-41ef-be98-1416c582d350)

Firstly, I try a **ls** to show the files in the current directory. I remembered that the text in an ingredient file talks about an hidden file, so to be sure I added **-a** option to display this type of files.

![Flag in hidden file](https://gist.github.com/user-attachments/assets/49db2031-4945-49e1-a9b9-877836814169)

By browsing, going in the **secrets** folder and using **cat** command the flag is display and this is the end of this challenge ! 

3. Solution

The flag is display when the correct command on the hidden file is done. This approach of the challenge is new for me because it involves in many steps to retrieve the flag in a **"black box"** environment type. This was a very enriching experience and let me go deeper on the analysis phase for the next challenges !
