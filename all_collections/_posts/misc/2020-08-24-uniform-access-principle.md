---
layout: post
title:  "Uniform access principle"
author: "Will Xing"
categories:
  - misc
date: 2020-08-24T00:00:00Z
---

    All services offered by a module should be available through a uniform notation, which does not betray whether they are implemented through storage or through computation.

I took a bit time to understand this principle while reading this defination.

And it's actually so easy to understand by code.

```
Dog.name = "Oscar";
print(Dog.name) // Oscar
print(Dog.name()) // Oscar
```

Basically it says, no matter the value is store in any place, a class should be able to retrieve it by either attribute/function/method...