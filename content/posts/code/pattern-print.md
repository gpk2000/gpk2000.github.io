---
title: "Tackling with Pattern Printing"
date: 2021-07-04T18:33:23+05:30
description: "Strengthen your looping skills using pattern printing"
draft: true
tags: [C, fundamentals]
---

**Target Audience**: Learners facing difficulties in implementing
pattern printing.

# Why learn Pattern Printing?

Pattern printing is one of the topics many learners face difficulties with, most of the difficulties
that I have seen people facing are not due to fact that they cannot write `for` loops but is due
to not properly understanding the logic behind it. In this article, I will take few examples and
explain how to tackle with each of them with source code in C programming language!!

# Outcomes

After going through this article I hope the reader will be able to write many pattern printing
programs (even complex ones) without any difficulties.

Let's start!!

---

## Example-1

**Statement**: Given an integer `n` that must be read through `stdin` (`scanf` in C) print
the following pattern

**Input**: 5

**Output**

```
    *
   **
  ***
 ****
*****
```

### Approach

- Before jumping directly into implementing this let us understand how we can represent this 
pattern in form of a matrix (visualizing this way is easier to construct the final logic).


![example-1](/ex1.png)


- If we look at the above image we see that we definitely need two `for` loops
One to loop over each row of the matrix, and other which loop which actually 
prints the characters to the screen.
- Now this is most important part of any pattern printing program. The **Logic** part, all we need to do is to find a way to represent the number of 'spaces' and 'stars' in terms of `n` and `i`. 
- If we look carefully at the above image
    * for i = 0, we have 4 spaces and 1 star
    * for i = 1, we have 3 spaces and 2 star
    * for i = 2, we have 2 spaces and 3 star
    * for i = 3, we have 1 space and 4 stars
    * for i = 4, we have 0 spaces and 5 stars
- Can you see the logic here? Whenever I am row `i` I have `n-i-1` spaces and `i+1` stars. Now it is easy to implement this. Let's code it.

```c
void example_pattern_1(int n) {
    // looping over rows
    for (int i = 0; i < n; i++) {
        // looping over column
        // * spaces = n-i-1
        // * stars = i+1
        // printing spaces
        for (int j = 0; j < n-i-1; j++) {
            printf(" ");
        }
        
        // printing stars
        for (int j = 0; j < i+1; j++) {
            printf("*");
        }
        
        // row ended so print a newline
        printf("\n");
    }
}

```

---

## Example-2

