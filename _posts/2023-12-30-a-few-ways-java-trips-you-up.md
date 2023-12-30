---
layout: post
title: A few Ways Java Trips You up
---

One thing is the modulo operation. For example, if you ask Java what -3 % 50 is, it'll respond with -3 but the actual correct answer is 47. If, however, you ask Python what -3 % 50 is, it will correctly respond that it is 47.

To circumvent this problem, if I have to find x % y and x is less than 0, I repeatedly add y to x until I get a positive value, and once I have gotten the positive value then I ask Java what x % y is. (I am assuming y is greater than 0). In Java code:

```
int remainder(int x, int y) {
    while(x < 0) {
        x += y;
    }
    return x % y;
}

```
Or, perhaps:

```
int remainder(int x, int y) {
    if (x < 0) {
        double absX = -1.0 * x;
        int q = (int)Math.ceil(absX/y);
        x = x + q*y;
    }
    return x % y;
}
```

One case when knowing this is useful is when, for example, writing the snake game using Java. Sometimes, we want perhaps to allow the snake to move through the left edge of the screen and appear back in from the right edge of the screen. To do that, we would like column -1 in an imaginary grid to be the right most column in that same grid. So, for example, if our grid has 50 columnsand the snake moves through the left edge of the screen, we would like the snake's head to appear in column 49 (which is really the 50th column if the first is column 0). Hence, we would like -1 % 50 to be 49 rather than -1.

So, the modulo operator in Java simply works wrong and does not work the way it is supposed to.

Next, varargs. Unlike the modulo operator, I cannot claim that varargs does not work as intended. Rather, it is easy to miss a detail about how it works and so it is not really a Java quirk.

Let's recall that varargs is simply a way to parameterise the number of arguments to a method. Here is an example:

```
void printNumbers(int... numbers) {
    for(int n : numbers) {
        System.out.println(n);
    }
}
```

Because I hve used varargs in the signature of this method, any of the following invocations of it are acceptable to the Java compiler:

```
printNumbers(1);
printNumbers(10, 20);
printNumbers(100, 200, 300);
printNumbers();
```

So, the last invocation of the method takes no arguments. As a Java developer, I am conditioned to dread the possibility of having a NullPointerException being raised in that case but what actually happens is that the compiler replaces the no-argument invocation with an invocation that takes a zero-length array of numbers and so no NullPointerException is going to be raised.

For clarity, printNumbers(); is equal to printNumbers(new int[0]{}); and printNumbers(1, 2, 3); is equal to printNumbers(new int[]{1, 2, 3});

Last I'd like to point out is this:

```
int i = 1;
i = i++;
System.out.println(i);
```

This is not the way to write code, but it is still a strange thing that once tripped me up. Perhaps I had another variable, say j, whose value I was trying to set to i++, or something like that.

Anyways. It is easy to think that after those two lines of code have executedthe value of i is 2 but in actuality, the vaule of i after those two lines of code remains 1.

Here is another way this pattern of writing code may trip one up. Here is some code:

```
int i = 1;
int j = -1;
j = i++;
System.out.println(i);
System.out.println(j);
```

Funny enough, after this code finishes executing, i will be 2 and j will be 1 (-1 + 2).

I like Java. I am not trying to bash it here. And probably, regardless of what language one uses there are things in the language that are easy to make mistaken assumptions about. Hence, my message here is rather that regardless of what language we use we need to make sure that our code does what we expect it to do and, more difficultly, to make sure that it does not do what we intend it not to do.

So, instead of bashing Java, I am rather promoting the idea of making sure your methods are small and extensively tested.
