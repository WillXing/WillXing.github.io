---
layout: post
title:  "Signed int"
author: "Will Xing"
date: 2020-07-13T00:00:00Z
---

Signed int represent both negative and positive numbers.

## 2's complement form

[[java]] Use this implementation for signed int. And by default, int in java is signed.

The first bit will tell number is negative or positiove, `1` means negative and `0` means positive.

If the bit number is `n`, the range of 2's complement form number will be [$-2^{n - 1}$, $2^{n -1} - 1$]

For example, if we have a `3` bit 2's complement form:
  1. The smallest number is -4, in binary `1 00`
  2. Note smallest is not `1 11`, `1 11` actually is `-0`, which is the biggest number in negative range.
  3. The biggest number is 3, in binary `0 11`

## Binary operation with signed number

Usually when doing binary operation to get the last bit of an integer, you can `&` with 1 and move 1 bit to right. Like this `int bit = n & 1`.

When you got a signed negative number, when you rich the most left bit, you will always get the sign position bit.

Let's look at following code:

```java
public static class Test {
  public static void printAllBits(int n) {
    while (n != 0) {
      int bit = n & 1;
      System.out.println(bit);
      n = n >> 1;
    }
    System.out.println("done");
  }
}
```

When pass in a *positive* number, this code is ok.

But when pass in *negative* number, the while loop will never end, because `n` will never become 0, it will be `-1` and always be `-1`, which binary present is `1` value is more like `1(sign) 0`

You can't use move right operation to pop out sign.

So it will print endless `1` in the end.

The correct code will be some thing like this:

```java
public static class Test {
  public static void printAllBits(int n) {
    for (int i = 0; i < 32; i++) {
      int bit = n & 1;
      System.out.println(bit);
      n = n >> 1;
    }
    System.out.println("done");
  }
}
```
