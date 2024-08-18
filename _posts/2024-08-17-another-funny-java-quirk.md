---
layout: post
title: Another Funny Java Quirk
---

```
jshell> Arrays.asList("1", "2")
$7 ==> [1, 2]

jshell> Arrays.asList('1', '2')
$8 ==> [1, 2]

jshell> Arrays.asList(new String[]{"1", "2"})
$9 ==> [1, 2]

jshell> Arrays.asList(new char[]{'1', '2'})
$10 ==> [[C@65b3120a]

jshell> Arrays.asList(new int[]{1, 2})
$11 ==> [[I@79fc0f2f]

jshell> Arrays.asList(new Integer[]{1, 2})
$12 ==> [1, 2]
```
