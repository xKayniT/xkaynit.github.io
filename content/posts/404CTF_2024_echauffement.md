+++
authors = ["xkaynit"]
title = "404CTF - Echauffement"
date = "2024-05-15"
description = "404CTF write up for echauffement challenge"
tags = [
    "xkaynit",
    "reverse",
]
# series = ["Theme Demo"]
+++

# 404CTF 2024
## Echauffement

1. **Introduction**

This writeup is about the echauffement challenge of the 404CTF 2024. Here is the description of the challenge :

*Un bon échauffement permet non seulement d'éviter des blessures, mais aussi de conditionner son corps et son esprit au combat qui va suivre. Ce crackme devrait constituer un exercice adéquat..*


File : echauffement.bin

2. **Analysis**

The first step in the analysis consist in decompilate the binary file to understand how it works. We can do this with the decompiler of your choice, which for me it's Ghidra ([ghidra website](https://ghidra-sre.org/)). The main function is composed with the following code :

    puts("Vous ne devinerez jamais le mot …");
    void buf;
    fgets(&buf, 0x40, stdin);
    if (secret_func_dont_look_here(&buf) == 0)
    {
        puts("Wow, impressionnant ! Vous avez …");
    }
    else
    {
        puts("C'est bien ce que je pensais, vo…");
    }
    return 0;

An other function appears with the code : **secret_func_dont_look_here**, so I'm going directly to see what it contains.

    int32_t rax_1 = strlen(secret_data);
    int32_t var_10 = 0;
    for (int32_t i = 0; i < rax_1; i = (i + 1))
    {
        if (((*(arg1 + i) * 2) - i) != *(secret_data[i]))
        {
            var_10 = 1;
        }
    }
    return var_10;

I have now the operation to reverse the characters of the flags : (((arg1 + i) * 2) - i) != *(secret_data[i]), and the content when I double click on secret_data variable.

3. **Solution**

So I can find the calculation to retrieve secret_data original value. The reverse operation is (secret_data[i]+1)/2 starting from (((arg1 + i) * 2) - i) != *(secret_data[i]). Below is the script to find original value : 

    # Input data found when double click on secret_data variable
    hex_list = [
    '68', '5F', '66', '83', 'A4', '87', 'F0', 'D1', 'B6', 'C1', 'BC', 'C5',
    '5C', 'DD', 'BE', 'BD', '56', 'C9', '54', 'C9', 'D4', 'A9', '50', 'CF',
    'D0', 'A5', 'CE', '4B', 'C8', 'BD', '44', 'BD', 'AA', 'D9'
    ]

    # Store the result for each iteration
    ascii_result = []

    # For loop to go through the list
    for i, hex_value in enumerate(hex_list):
        # Transform hexa in decimal
        decimal_value = int(hex_value, 16)
        # Apply the operation to the previous decimal value
        result = (decimal_value + i) // 2
        # Transform decimal result value in char
        ascii_char = chr(result)
        ascii_result.append(ascii_char)
    # Reassembly all obtain results
    ascii_string = ''.join(ascii_result)
    print(ascii_string)

FLAG : **404CTF{l_ech4uff3m3nt_3st_t3rm1n3}**