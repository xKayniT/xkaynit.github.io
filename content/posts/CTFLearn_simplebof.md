+++
authors = ["xkaynit"]
title = "CTFLearn - simplebof"
date = "2024-08-07"
description = "CTFLearn write up for simplebof challenge"
tags = [
    "xkaynit",
    "binary",
]
# series = ["Theme Demo"]
+++

# CTFLearn
## Simple bof

1. **Introduction**

This writeup is about the simple bof challenge of the CTFlearn website, it's a very interesting challenge to see how a buffer overflow works and to understand it in detail. Here is the description of the challenge :

*Want to learn the hacker's secret? Try to smash this buffer!*

*You need guidance? Look no further than to Mr. Liveoverflow. He puts out nice videos you should look if you haven't already*

***nc thekidofarcrania.com 35235***
**link : https://ctflearn.com/challenge/1010**

File : bof.c

2. **Analysis** 

A buffer overflow is *an anomaly whereby a program writes data to buffer beyond the buffer's allocated memory, overwriting adjacent memory locations*. Every program in a computer have dedicated memory area and if a program want to write at an address not reserved for it, the system can't properly work(Look at the crowdsite incident). I really wanted to make a write-up about this challenge because the way to resolve is, in my opinion, the better to understand buffer overflow. 

Let's start with the code analysis and more precisely variables declarations :

```
  char padding[16];
  char buff[32];
  int notsecret = 0xffffff00;
  int secret = 0xdeadbeef;

  memset(buff, 0, sizeof(buff));
  memset(padding, 0xFF, sizeof(padding));
```

Four variables are created, two arrays and two ints. The two arrays have her values setted by the memset function. Let's have a look at what memset function take in parameters : **void memset(void str, int c, size_t n)**. The first parameter is the entry string, here the two arrays. Second parameter is corresponding to the value using to fill the memory block and last parameter stand for the size of what is wanted to change in the variable(in our case padding and buff). **notsecret** value is set with 0xffffff00 and **secret** with 0xdeadbeef. Now, we can focus on the rest of the code :

```
if (secret == 0x67616c66) {
    puts("You did it! Congratuations!");
    print_flag(); // Print out the flag. You deserve it.
    return;
  } else if (notsecret != 0xffffff00) {
    puts("Uhmm... maybe you overflowed too much. Try deleting a few characters.");
  } else if (secret != 0xdeadbeef) {
    puts("Wow you overflowed the secret value! Now try controlling the value of it!");
  } else {
    puts("Maybe you haven't overflowed enough characters? Try again?");
  }
```

As you can see in the code, there is four statements :

- First if check that the secret variable is equal to **0x67616c66**(which is equal to glaf in char)
- Second if check that the notsecret variable is not equal to **0xffffff00**(default value)
- Third if check that the secret value is not equal to **0xdeadbeef**(default value)
- The last else is returned when all the previous conditions are not respected

So, to print the flag value, we have to set the secret varaible at **0x67616c66**. Now we know how the program works overall and what we have to do, it's time to launch and see what happens.

The first array appear when the program is started is the following : 

![image](https://gist.github.com/user-attachments/assets/c98b9901-437e-4160-8213-ca2a1ec200ff)

We find the differents variables that were defined earlier, such as **buff**, **padding**, **notsecret** and **secret**. We already know the length of each variables according to the given code so we can try to input 48 characters(the size of **buff** is 32 and **padding** is 16) and see the behavior.

![image-1](https://gist.github.com/user-attachments/assets/30050a97-b1ff-48e9-a1b0-6803b0ac9763)

With the character "a" 48 times(61 is the hexa value), the value in the **buff** and **padding** are modified by this input. The next four characters is the secret value(**0xdeadbeef**) and the next four again is notsecret(**0x00ffffff**). One important thing to notice is on the last screenshot, the bytes are store in **little endian** because the least significant bit is stored on the left for **notsecret** value(more information [here](https://www.geeksforgeeks.org/little-and-big-endian-mystery/)).

Now that we have the correct lenght, we have to modify the secret value to trigger the first if condition and print the flag. We know that when "a" character is sent the ASCII char is display(61) on the array and the **secret** value needs to be **0x67616c66**, let's tanslate it in ASCII char : glaf. 

![image-2](https://gist.github.com/user-attachments/assets/545802fb-1197-433a-b308-de173d2e8e1a)

The input on the screenshot above trigger the if condition because **"flag"**(reversed because of little endian) is present after 48 times the character "a". As said earlier, the size of the **buffer+padding** is 48 and the if condition wait for the correct string after the buffer and padding. Futhermore, you can see the notsecret value(**0x00ffffff**) after the secret value which means that if the input text was too big, not secret value will be erased.

## Conclusion

Like I said earlier, this challenge was really interesting for me to show how a buffer overflow works in practice because many challenges need knowledges in buffer overflow. Thank for your reading and feel free if you have some questions ! :)
