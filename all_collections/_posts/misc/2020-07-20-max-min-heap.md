---
layout: post
title:  "Max/Min Heap"
author: "Will Xing"
categories:
  - blog
date: 2020-07-20T00:00:00Z
---

It's a binary tree structure which will always have the smaller one or bigger one at the parent node. Also means the root node is always the smallest or biggest.

## How it implemented

Let's take max heap here as example.

### Insert

The insertion will always insert to the end of the tree, which the tree is more like a complete binary tree in my understanding.

What heap will do is to compare the new inserted node with it's parent, if it's bigger than parent, then swap them. Continue doing it, until it's smaller than parent, or reach the top. This will happene each time when insert a new value, which have the time complixity `O(logn)`

This operation can summarized as bubbling up.

### Remove

The remove should always remove the root of the tree, which because the root is the biggest of the set.

There are 3 steps happening when remove:

1. Swap the root with the last element in the tree
2. Remove the last element (which is the old root element that we want)
3. Bubble down the root to the right position

Bubble down operation is, compare with both children node, swap with the smallest, keep doing this until node itself bigger than both children or there's no more child to swap. and the time complixity is also `O(logn)`
