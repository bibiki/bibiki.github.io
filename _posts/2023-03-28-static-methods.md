---
layout: post
title: Static Method in Java
---

"What does static keyword do to a method in Java?" I was asked this question in an interview and my immediate answer was to the tune of "static members, and hence static methods, are shared across objects of the class that contains the static method." However, this is somewhat innacurate, as my interviewer said it is.

In the course of writing this post, and since ChatGPT is all the rage last few weeks, I thought I'd ask it, too, the same question. After its initial answer, I doubled down as follows:

![ChatGPT's answer]({{site.url}}/assets/images/static-method-chat-gpt.png)

Let's see where the confusion comes from. Often, the significance of the static keyword is introduced along with the singleton pattern, and we get used to thinking in terms of there being only one of that which is declared static. And if at all times there is only one of a static member, then it must be shared across all instances of objects of the same class.

But, just how is the method shared? Or, rather, how are non-static methods in Java not shared among various objects of a class? If a method is a blueprint that describes some behavior, then a non-static method is also shared among objects of the same class as there is a single specification for them, too, regardless of the particular instance the method will be invoked on.

In that respect, a method, as code that specifies some behavior, is always shared across all objects of some class, and may not be not shared, whether it is declared to be static or not. On the other hand, state may or may not be shared. And saying that static state is shared across all objects of a class is accurate. It being shared is its distinct feature, compared to non-static state, which is not shared. [I can actually nitpick to the point that even this is not so. See Nitpicking after conclusion of this post.]

So, the question asked in those terms is somewhat ambiguous, and an answer to it that makes use of the idea of the method being shared or non-shared across objects of the class is even more so since it then leads to the necessity to define what it means for a member to be shared and if both behavior specification, behavior execution, and state may be shared. To further clarify, if a method named someMethod() is static and requires five seconds to execute and it is invoked more than once in one second, how many times is its entirety executed? It is not like the JVM will say "alright, the method is already executing so all invocations of it while it is executing will be catered to with this same execution of it." Or, to look at it another way, if it has no access to any static state, is there anything shared across all cases of its execution? And, if there is nothing shared across all of its invocations, then how is the method shared across all objects of the class?

I suppose, a better way to bring forth the significance of static keyword when applied to a method is to focus on what the difference between a static method and a non-static method in Java is. And the straightforward answer to that question would be that static methods do not have access to non-static state of different objects, that static methods have access to the static state declared in the class, and that static methods may be invoked on a class without requiring an object of the class being instantiated.

On the other hand, a non-static method will have access to the static state declared in the class **in addition to having access to the non-static state that is associated with the particular instance that the non-static method is associated with.**

In conclusion, the static keyword applied to some field has different meaning than when applied to a method in a class. When applied to a field, it specifies that the field belongs to the class the field is in. This may be described, and usually is described, as "is shared across all objects of the class." When applied to a method, it specifies how the method is invoked and that it has access to the static state of the class in addition to the non-static state of its own arguments, but not to the non-static state declared in the class.

Nitpicking
==========

In the conclusion part of the post I said that static state belongs to the class it is defined in. That is to imply that static state does not belong to the objects of the class. And if it does not belong to the objects then the idea of static state being shared across all objects of the class makes no sense. It is only upon chosing to think of the state of an object as encompassing the state of its class that static state of the class begins to seem shared across all the objects of a class. Beyond that, it is access modifiers (public, protected, default, and private) that determine who has access to some state, and not the static keyword. If some static field is public then all instances of that class have ownership of that state just as much as other classes and their objects do. And in this sense, static state is shared across more than just various objects of the class. Anyways, enough with nitpicking.
