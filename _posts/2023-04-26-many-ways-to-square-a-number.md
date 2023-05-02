---
layout: post
title: Many Ways to Square a Number
---

n^2 = n\*n

Here is the simplest way to write code to find the square of n.

```
def square(n):
    return n*n
```

Let's now complicate it, just for the fun of it. I'll continue to use python for the code and will assume that n is alsways non-negative.

Since n\*n is like saying 'have n occurances of n and add them together' we can write the code using a loop, as follows.

```
def square(n):
    rez = 0
    counter = 1
    while i <= n:
        rez += n
        counter++
    return rez
```

But, we may as well write this using two loops. Think of it as counting n times up to n. That is like counting up to n squared.	

```
def square(n):
    i = 0
    j = 0
    rez = 0
    while i < n:
        while j < n:
            rez += 1
            j++
        i++
    return rez
```

Since the square of n is equal to the sum of the first n odd positive integers, then this will work also.

```
def square(n):
    rez = 0
    counter = 1
    while counter <= n:
        rez += (2*counter - 1)
        counter++
    return rez
```

There is this nice method to quickly multiply two numbers that is sometimes refered to as Russian Peasant's Method (but is actually found in math documents from 1650 BC). It is based on the idea that a\*b=(2\*a)*(b/2). If b is odd, however, b/2 is not an integer and we want to stick to using integers only. So, the method recognizes two cases: b is odd or b is even. Let's multiply 7 by 7 to illustrate the idea:

```
7*7=7*6 + 7
7*6 + 7=14*3+7
14*3 + 7=14*2 + 14 + 7
14*2 + 14 + 7=28*1 + 14 + 7=28 + 14 + 7=49
```

So, basically, the idea is that when b is even, we rewrite a\*b as (2\*a)\*(b/2). When b is odd, we rewrite a\*b as a\*(b-1) + a. Let's see what this looks like in code.

```
def square(n):
    rez = 0
    a = n
    b = n
    while b >= 1:
        if b%2 == 1:
            rez += a
            b -= 1
        else:
            a *= 2
            b /= 2
    return rez
        
```

And that's all. My intention was to show that that which solves the problem is different from the code that implements the solution. Every one of these functions calculates the square of an integer but they are all different. One way these functions differ is the number of steps performed to calculate the square of n. There is nothing faster than n\*n but this last way to square an integer n is very interesting and is faster than the other ways. The same idea is used to calculate the nth fibonaci number in a logarithmic number of steps, whereas a recursive function to calculate fibonaci numbers takes 2^n steps. That is a huge improvement in performance. To see how that is done you may consult the book Structure and Intepretation of Computer Programs, exercise 1.19.
