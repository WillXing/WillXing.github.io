---
layout: post
title:  "Happens Before Rule"
author: "Will Xing"
categories:
  - java
date: 2020-07-13T00:00:00Z
---

When executing program, computer usually will reorder the command for the performance beneficial.

My understanding of `Happens before` rule is, it implemented in [JMM]({% post_url java/2020-07-13-JMM %}) to guarantee the order of the command and reordering will have the same executing result.

Just like the name, it will make sure the command executing one after another when it will influence on the result.

But still not limit JMM to not reordering for performance beneficial.

For example, it's ok to reorder variables declaration:

```java
int i = 1;
String s = "after";
```

It might create string `s` in memory ealier than `a`.

But when the first command need to happen before second:

```java
int i = 1;
int j = i + 1;
```

In this case, `happens before` will make sure `i` got assigned first.
