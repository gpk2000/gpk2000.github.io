---
title: "Beautiful Math in SICP Part-1"
date: 2021-07-04T10:39:44+05:30
draft: false
description: "Math in SICP (Structure and Interpretation of Computer Programs)"
katex: true
markup: "mmark"
tags: [python, sicp, math]
---

## Square roots by Newton's Method

$$\sqrt x = \text{the } y \text{ such that } y \geq 0 \text{ and } y^2 = x$$

The above definition is a valid way of defining square-root. But how do we tell
the computer to calculate it? We Use Newton's method of successive
approximations. The algorithm is as follows:-

* We start by guessing the $$\sqrt x$$ is $$y$$.
* Now we check how close the square of the $$y$$ is to $$x$$.
* If the guess is close enough we terminate the program (usually we compare
the absolute difference between the **square** of $$y$$, $$x$$ and compare it with
eps which is small, say $$10^{-3}$$ for example.
* If the guess was not good, then we need to improve it. We can perform simple
math to get better approximation, which is by averaging $$y$$ with $$\frac{x}{y}$$
* Repeat this process until you terminate out in step-3.


```py
EPS = 1e-3

def average(x, y):
    return (x + y ) / 2

def square(x):
    return x * x

def improve(guess, x):
    return average(guess, x / guess)

def isGoodEnough(guess, x):
    return abs(square(guess) - x) <= EPS

def sqrtIter(guess, x):
    if isGoodEnough(guess, x):
	return guess
    return sqrtIter(improve(guess, x), x)

def sqrt(x):
    return sqrtIter(1.0, x)

print(sqrt(2))      # 1.4142156862745097
print(sqrt(4))      # 2.0000000929222947
```

### Exercises

- Does the above procedure work for very small/large numbers? i.e is the
procedure `good-enough`? good enough? A way to resolve this issue is to notice
how does guess changes in its value. If it changes too less then you can
terminate. Implement this idea by yourself and test it.
- Use the `sqrt` code as a motivation to write a procedure that calculates cube
root of a number. The improve logic for cube root is:-

$$
\frac{x/y^2 + 2y}{3}
$$

---

## Ackermann's Function

The Ackermann's function $$A(x, y)$$ (where $$x$$, $$y$$ are non-negative integers) is defined as follows:-

$$A(x, y) = \begin{cases}y + 1 & x = 0 \\A(x - 1, 1) & y = 0 \\A(x - 1, A(x, y - 1)) & x > 0, y > 0\end{cases}$$


```py
def A(x, y):
    if x == 0:
        return y + 1
    if y == 0:
        return A(x - 1, 1)
    
    return A(x-1, A(x, y-1))
    
print(A(1, 10))     # 12
print(A(2, 4))      # 11
print(A(3, 3))      # 61
```

Why describe such a simple function? [Here](https://www.youtube.com/watch?v=i7sm9dzFtEI)

---

## Calculating sin(x)

The sine of an angle (specified in radians) can be computed by making use of the
approximation $$\sin x \approx x$$ if $$x$$ is sufficiently small and the trigonometric identity
to reduce the size of the argument of sin.

$$\sin x = 3 \sin \frac{x}{3} - 4 \sin^3 \frac{x}{3}$$


```py
import math

EPS = 0.01
PI = math.pi

def calc(x):
    return 3*x - 4*(x**3)

def sine(angle):
    if abs(angle) <= EPS:
        return angle
    else:
        sinOneThirdAngle = sine(angle/3)
        return calc(sinOneThirdAngle)

print(sine(PI/2))  # 0.999999999940162
```

---


## Fast Exponentiation

We can compute $$a^n$$ in fewer steps (less than $$n$$) by sucessive squaring. Here is the recursive definition:-

$$exp(a, n) = \begin{cases}exp(a, n - 1) \times a &\text{if } n \ \% \ 2 = 1 \\(exp(a, \frac{n}{2}))^{2} & \text{otherwise}\end{cases}$$

```py
def exp(a, n):
    if n == 0:
        return 1
    elif n % 2 == 0:
        return exp(a, n//2) ** 2
    else:
        return a * exp(a, n - 1)

print(exp(2, 20))      # 2^20
```

---

## Greatest Common Divisor

The idea of this algorithm is based on the observation that, if $$r$$ is the
remainder when $$a$$ is divided by $$b$$, then the common divisors of $$a$$, $$b$$ are precisely the same as common divisors of $$b$$ and $$r$$. Thus, we can use the equation below to successively reduce the problem of computing GCD.

$$gcd(a, b) = gcd(b, a \ \% \ b)$$


```py
def gcd(a, b):
    if b == 0:
        return a
    else:
        return gcd(b, a % b)

print(gcd(3, 4))            # 1
print(gcd(13232, 3432))     # 8
```

**Lame's Theorem:** If Euclid algorithm (above one) requires $$k$$ steps to compute the GCD of some pair, then the smaller number in that pair must be greater than or equal to the $$k^{th}$$ Fibonacci number.

We can use the above theorem to get an order of growth estimate for Euclid's
algorithm which is $$O(\log ({\min_{a,b}}))$$

---

## Primality Testing

#### Method-1: Finding the smallest divisor greater than 1

We can test whether a number is prime or not as follows: $$n$$ is prime if and only if $$n$$ is its own smallest divisor greater than 1. It is a fact that if $$n$$ is not prime then it must have a divisor less than or equal to $$\sqrt n$$. This means we only run the algorithm from 1 to $$\sqrt n$$ which gives is $$O(\sqrt n)$$ time complexity 


```py
def findDivisor(num, testDivisor):
    if testDivisor ** 2 > num:
        return num
    elif num % testDivisor == 0:
        return testDivisor
    else:
        return findDivisor(num, testDivisor+1)

def smallestDivisor(num):
    return findDivisor(num, 2)

def testPrime(num):
    if num == 1:
        return False
    else:
        return smallestDivisor(num) == num


print(testPrime(1))     # False
print(testPrime(19))    # True
print(testPrime(13535)) # False
```

#### Method-2 The Fermat Test

**Theorem (Fermat's Little Theorem)**: If $$n$$ is a prime number and
$$a$$ is any positive integer less than $$n$$, the $$a$$ raised to $$n^{th}$$
power is congruent to $$a \mod n$$. 

Two numbers are said to be congruent to each other if they have the
same remainder when divided by $$n$$.

This algorithm falls under the category of Probabilistic Algorithms which
means that we are not 100% sure that our algorithm gives the correct result. But
we can say it is probably correct. In this theorem if $$n$$ does not satisfy the
theorem then we are certain that $$n$$ is not a prime. But if it does satisfy the
theorem then we can say that it is probably prime. And indeed there are some
numbers which satisfy the theorem but are not prime. They are called [Carmichael Numbers](https://en.wikipedia.org/wiki/Carmichael_number) which are rare. In general this algorithm is reliable if tested against
many random numbers less than $$n$$. The time complexity of primality check once
using Fermat test is $$O(\sqrt n)$$.

```py
from random import randint

def modExp(a, n, m):
    # calculates a^n mod m in O(log n)
    # see fast exponentiation
    if n == 0:
        return 1
    elif n % 2 == 0:
        return (modExp(a, n//2, m) ** 2) % m
    else:
        return (a * modExp(a, n - 1, m)) % m

def fermatTest(n):
    if n == 1:
        return False
    randomVal = randint(1, n) + 1
    return modExp(randomVal, n, n) == randomVal % n

def testFermatNTimes(n, times):
    for i in range(times):
        if not fermatTest(n):
            return False
    return True

print(testFermatNTimes(13, 5))          # True
print(testFermatNTimes(1423411, 10))    # False
print(testFermatNTimes(1, 5))           # False
```

