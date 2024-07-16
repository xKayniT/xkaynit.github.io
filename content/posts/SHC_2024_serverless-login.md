+++
authors = ["xkaynit"]
title = "SHC2024 - Serverless login"
date = "2024-05-13"
description = "SHC 2024 write up for Serverless login challenge"
tags = [
    "xkaynit",
    "web",
]
# series = ["Theme Demo"]
+++

# SHC 2024
## serverless-login

1. **Introduction**

This writeup is about the serverless-login challenge of the Swiss Hacking Challenge 2024. Here is the description of the challenge :

*Imagine you're throwing a party. You could buy all the food, drinks, and decorations, prepare everything yourself, and then clean up afterwards. That's like running your own servers. You have total control, but it's a lot of work and expense.
Now, imagine instead you decide to hold your party at a restaurant. They handle the food, drinks, and clean-up. You just pay for what you consume. That's like serverless computing. You don't worry about the infrastructure; you just focus on having a great party (or in this case, building a great app). However, there seems to have been a misunderstanding about the term serverless...*

This challenge need to launch on a private instance. 


2. **Analysis**

When I start the challenge and click on the instance, I arrive in the page below :

![image](https://gist.github.com/assets/104362418/751231a1-e1ad-45bd-bcd5-a57dd21d9e55)

As we can see, it's a login page. I tried differents things like an apostrophe to know if it's an SQL injection but without results. After this phase, I have started to analyse the source code of the page and I found a **python script**. A login function is present in the script composed with the following code :

    db = sqlite3.connect("database.db")
    username = document.querySelector("#username").value
    password = document.querySelector("#password").value
    passord_hash = hashlib.sha256(password.encode("utf-8")).hexdigest()
    data = db.execute("SELECT * FROM login WHERE username=? and password=?", (username, passord_hash)).fetchone()
    if data is not None:
        master_key = int.from_bytes(bytes(password, "utf-8"))
        cipher = aes.aes(master_key, 128, mode="CTR", padding="PKCS#7", iv=IV)
        decrypted_flag = "".join([chr(x) for x in cipher.dec(FLAG)])
        toggle("flag")
        document.querySelector("#flag-text").innerHTML = f"Congratluations! You solved the challenge. Here's your flag: <b>{decrypted_flag}</b>"
    else:
        toggle("failed")`

This function load a database which can be downloaded. With a SQL database viewer, I can see the contained entry which is "admin" for user and "11a4a60b518bf24989d481468076e5d5982884626aed9faeb35b8576fcd223e1" for the password hash. 

3. **Solution**

With a Google Dork(Google Keyword Research), it's possible to know the value of corresponding hash. **intext:11a4a60b518bf24989d481468076e5d5982884626aed9faeb35b8576fcd223e1** can find the hash in text of all websites referenced in Google(more precisions here https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06). It's matched with the string "python". Finally, we have the username,password and the flag that appears when you fill the login form. 

![image-1](https://gist.github.com/assets/104362418/e2b45399-9933-43a0-b56f-5fd7aea038c2)

FLAG : **shc2024{wh0_N33d5_4_53RV3r_4nYw4Y2?}**