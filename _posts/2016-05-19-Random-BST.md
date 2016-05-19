---
layout: post
title: Randomized Binary Search Trees
published: true
---
This post will be about using Randomization for constructing [Binary Search Trees (BST)](https://en.wikipedia.org/wiki/Binary_search_tree) whose expected height would be $$O(\log n)$$. This post is inspired from a question I was asked in my B.Tech Grand Viva.

The problem we are solving here is this:
Given $$n$$ distict integers in an array, construct a BST in expected $$O(n \log n)$$ time, whose expected height would be $$O(\log n)$$.

There are deterministic ways to construct height balanced BSTs. Popular methods taught in undergraduate curriculum are [AVL trees](https://en.wikipedia.org/wiki/AVL_tree) and [Red-Black Trees](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree). But these data structures involve complex operations which are quiet difficult to remember.

One could also construct the required tree by sorting the array in $$O(n \log n)$$ and using the following simple recursive algorithm:
    Use Medain of the array as the root of the tree.
    

