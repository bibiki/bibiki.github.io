---
layout: post
title: Recursion
---

I really like recursion and I think everyone should get familiar with it. One way to describe recursion is to say that recursion describes recursion. LOL. I know. Another way to describe it is as follows: recursion is either zero steps or one step and a smaller recursion.

These descriptions are very simple, but map is not territory and moving from description to understanding of what recursion is is difficult. But moving from experience of recursion to understanding of it is luckily easy.

There is rarely any discussion of recursion I have seen that does not use the example of finding the factorial of a positive integer. As a reminder, the factorial of some positive integer n is the product of integers from 1 to n. For example, the factorial of 4 is 1\*2\*3\*4. Or, factorial of 5 is 1\*2\*3\*4\*5. Or, factorial of 5 is 5\*(factorial of 4). Generally, factorial of n is n\*(factorial of n - 1). So, factorial is defined in terms of factorial. Or recursion describes recursion.

To bring forth all the questions that need to be answered when using a recursive process to calculate the factorial of some number n I will just write the code for it. Here is some python code for that:

```
def factorial(n):
    if n == 1:
        return 1
    else:
        return n*factorial(n-1)
```

The else branch in that function is exactly what I described earlier when I said "factorial of n is n*(factorial of n - 1)" but there was no mention there of anything that resembles the if branch. However, without that if branch, the factorial function would call itself with n - 1, and it would continue to do so for n = 1, n = 0, n = -1, and so on to n = negative infinity. Now, if the definition of the factorial of some number included the product of all integers from negative infinity up to n, that would not have been a problem. But the definition of factorial of n is the product of all positive integers from 1 up to n.

So, the definition of factorial of n may be restated as: for n some positive integer, if n = 1, the factorial of n is 1; else, it is the product of n and the factorial of n - 1.

This resembles my description of recursion as "either zero steps, or one step and another recursion." The "zero steps" corresponds to when n is 1. Nothing is done, 1 is returned. And "one step and a smaller recursion" corresponds to multiplying n with the smaller recursion that the factorial of n - 1 is.

Now, just as the process of calculating the factorial of n and the description of recursion share the same structure, there are many other processes that conform to the same structure. Here, for example, is code for finding the sum of all positive integers from 1 up to n.

```
def sum(n):
    if n = 1:
        return 1
    else:
        return n + sum(n-1)
```

The only difference between the code for factorial and the code for sum is that factorial multiplies n with factorial of n - 1 whereas sum adds n with the sum of n - 1. So, the structure is the same in both functions. The only difference is the operation used to combine n with the smaller recursion for n - 1. Now we can separate the general structure from the particular operation... let us define a function recur that takes two arguments: n and the operation to combine n and the smaller recursion.

```
def recur(n, op):
    if n == 1:
        return 1
    else:
        return op(n, recur(n-1))
```

With recur function defined, we can rewrite factorial and sum as follows:

```
def factorial(n):
    return recur(n, multiply_two_numbers)

def sum(n):
    return recur(n, add_two_numbers)

% where multiply_two_numbers and add_two_numbers are defined as follows
def multiply_two_numbers(a, b):
    return a*b

def add_two_numbers(a, b):
    return a+b
```

So, there is something called recursion and there are processes that conform to it. So far, the calculation of factorial of n and the calculation of the sum of first n positive integers were described as recursive processes. Because the two processes share the same structure, the factorial(n) and sum(n) functions admited to one-line code definitions. But, there are other processes that conform to the same structure.

For example the square of positive integers n. Let's point out that n = 1 + (n-1). Therefore, n^2 = 1 + 2\*(n-1) + (n-1)^2. So, the recursive definition of squaring a positive integer n is as follows: if n = 1, return 1; else, add 1 and 2\*(n-1) to the smaller recursion of (n-1)^2. This follows the same structure as factorial of n except that the operation that combines n with the smaller recursion for n-1 is different than multiply_two_numbers. Because our operation for that will have to add to the smaller recursion the value of 1 + 2\*(n-1)=2\*n - 1, let us call the required operation add_two_n_minus_one. Here is the code:

```
def add_two_n_minus_one(n, value_of_smaller_recursion):
    return 2*n - 1 + vallue_of_smaller_recursion

def square(n):
    return recur(n, add_two_n_minus_one)
```

This is great. Three different processes, the same structure. And there are countless other processes that conform to the same structure. And some other processes that differ only slightly. For example, since 1*x=x, we do not really need to recur down to 1 when calculating the factorial of n. However, we do need to recur down to 1 when calculating the sum of all numbers from 1 to some positive integer n. So, the ```if n == 1:``` could be ```if n == 2:``` for the calculation of factorial. The value of n for which the recursion should stop is usually called the base case. So, different processes that otherwise share a structure may have different base cases. Let us allow for a parameter to our recur function to paramtereize the base case.

```
def recur(n, op, base_case):
    if base_case(n):
        return n
    else:
        op(n, recur(n-1, op, base_case))
```

Let's stop here for a minute. This recur function implements recursion itself. This can be used to instruct others how to implement a recursive solution to some problem. Whenever we want to implement a recursive solution to some problem we need to think of two things: 1, the base case. In other words, we need to specify when recursion should stop and what the result of that should be. And 2, we need to specify the operation that combines n with the smaller recursion corresponding to n-1. That is the op argument to the recur function.

For completeness sake, here is the implementation of factorial function that ends recursion at n = 2 as well as the implementation of the sum function that calculates the sum of all positive integers from 1 up to n using this newer version of recur.

```
def end_recursion_at_2(n):
    return n == 2

def multiply_two_numbers(a, b):
    return a*b

def factorial(n):
    return recur(n, multiply_two_numbers, end_recursion_at_2)

def end_recursion_at_1(n):
    return n == 1

def add_two_numbers(a, b):
    return a+b

def sum(n):
    return recur(n, add_two_numbers, end_recursion_at_1)
```

Now, I named this post 'Recursion' but this has rather been an exercise in abstraction. I began with the implementation of a recursive function to calculate the factorial of n and ended up with a function that implements recursion itself. Nothing is what it seems.

The way the last version of recur is coded is somewhat rigid. It assumes that n is an integer and that the smaller recursion is applied to n-1. However, we may as well apply recursive algorithms on lists. In that case, the smaller recursion will be applied on another list that has one element fewer than the original list has. For example, we might want to calculate the sum of the numbers in a given list like [1 3 5 7 9]. Without writing the code, the recursive algorithm would be to add 1 to the sum of the smaller list that [3 5 7 9] is. For the base case, we may chose to test if the input list is empty and if it is, we return 0. You might want to write that function. If you chose to, the first implementation of factorial should guide you. The last version of recur assumes that n is a number so it is not as easily followed as the first version of factorial function.
